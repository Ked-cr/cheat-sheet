# Fitur Login JWT

## Mengenal JWT

JWT (JSON Web Token) adalah metode untuk mengamankan API dengan mengeluarkan token setelah user berhasil login. Token ini dikirim bersama request dan divalidasi di setiap endpoint yang perlu autentikasi.

## Alur Kerja Login JWT

1. User kirim email & password ke endpoint login.
2. Server verifikasi password.
3. Jika valid, server membuat token JWT.
4. Server mengirim token ke user (biasanya di header atau body).
5. Di endpoint lain, token harus dikirim kembali untuk akses (biasanya via Authorization` header).

### Install Package

```bash
npm install express prisma @prisma/client bcryptjs jsonwebtoken dotenv
npx prisma init
```

### Use Case

Struktur Folder Sederhana

```bash
project-api/
├── prisma/
│   └── schema.prisma
├── src/
│   ├── controllers/
│   │   └── authController.js
│   ├── routes/
│   │   └── authRoutes.js
│   ├── middlewares/
│   │   └── authMiddleware.js
│   ├── app.js
│   └── server.js
├── .env
├── package.json
```

### 1. `.env`

```bash
DATABASE_URL="postgresql://user:password@localhost:5432/dbku"
JWT_SECRET=my_jwt_secret
PORT=3000
```

### 2. Prisma Setup

```bash
npx prisma init
# edit prisma/schema.prisma
npx prisma migrate dev --name init
npx prisma generate
```

### 3. `prisma/schema.prisma`

```bash
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id       Int      @id @default(autoincrement())
  email    String   @unique
  password String
  createdAt DateTime @default(now())
}
```

### 4. `authController.js`

```js
import { PrismaClient } from '@prisma/client';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';

const prisma = new PrismaClient();

export const login = async (req, res) => {
  const { email, password } = req.body;

  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) return res.status(400).json({ error: 'Email tidak ditemukan' });

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) return res.status(400).json({ error: 'Password salah' });

  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET, {
    expiresIn: '1h',
  });

  res.json({ token });
};

export const register = async (req, res) => {
  const { email, password } = req.body;

  const exists = await prisma.user.findUnique({ where: { email } });
  if (exists) return res.status(400).json({ error: 'Email sudah digunakan' });

  const hashed = await bcrypt.hash(password, 10);

  const newUser = await prisma.user.create({
    data: { email, password: hashed },
  });

  res.status(201).json({ message: 'User dibuat', user: { id: newUser.id, email: newUser.email } });
};
```

### 5. `authMiddleware.js`

```js
import jwt from 'jsonwebtoken';

export default function authMiddleware(req, res, next) {
  const authHeader = req.headers.authorization;
  if (!authHeader?.startsWith('Bearer '))
    return res.status(401).json({ error: 'Token tidak valid' });

  const token = authHeader.split(' ')[1];
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch {
    return res.status(401).json({ error: 'Token tidak valid' });
  }
}
```

### 6. `authRoutes.js`

```js
import { Router } from 'express';
import { login, register } from '../controllers/authController.js';
import authMiddleware from '../middlewares/authMiddleware.js';

const router = Router();

router.post('/login', login);
router.post('/register', register);
router.get('/profile', authMiddleware, (req, res) => {
  res.json({ message: 'Akses OK', user: req.user });
});

export default router;
```

### 7. `app.js`

```js
import express from 'express';
import dotenv from 'dotenv';
import authRoutes from './routes/authRoutes.js';

dotenv.config();
const app = express();

app.use(express.json());
app.use('/api/auth', authRoutes);

export default app;
```

### 8. `server.js`

```js
import app from './app.js';

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`🚀 Server jalan di http://localhost:${PORT}`);
});
```

## Unit Test

* **Register:**
  * `POST /api/auth/register`
  * Body: `{ "email": "a@mail.com", "password": "123456" }`
* **Login:**
  * `POST /api/auth/login`
  * Body: sama, akan menerima token JWT
* **Profile:**
  * `GET /api/auth/profile`
  * Header: `Authorization: Bearer <TOKEN>`
  * 

## Impelentasi Refres Token

## Konsep Refresh Token

| Token                   | Expired Cepat    | Disimpan Di                             |
| ------------------------- | ------------------ | ----------------------------------------- |
| **Access Token**  | 15 menit - 1 jam | Memory / Frontend                       |
| **Refresh Token** | 7 - 30 hari      | Disimpan di DB dan dikirim saat refresh |

### Langkah-langkah

1. Tambah field `refreshToken` di model Prisma.
2. Saat login:
   
   * Buat access token + refresh token
   * Simpan refresh token ke DB
3. Buat endpoint `POST /refresh-token`:
   
   * Validasi refresh token
   * Jika valid → generate access token baru
4. Logout → hapus refresh token

### 1. Update `schema.prisma`

```bash
model User {
  id            Int      @id @default(autoincrement())
  email         String   @unique
  password      String
  refreshToken  String?  // bisa kosong/null
  createdAt     DateTime @default(now())
}
```

lalu jalankan

```bash
npx prisma migrate dev --name add_refresh_token
```

### 2. Util `token.js` (generate token)

```js
import jwt from 'jsonwebtoken';

export const generateAccessToken = (userId) => {
  return jwt.sign({ userId }, process.env.JWT_SECRET, {
    expiresIn: '15m', // akses cepat
  });
};

export const generateRefreshToken = (userId) => {
  return jwt.sign({ userId }, process.env.JWT_REFRESH_SECRET, {
    expiresIn: '7d',
  });
};
```

### 3. Controller `authController.js`

```js
import { PrismaClient } from '@prisma/client';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import { generateAccessToken, generateRefreshToken } from '../utils/token.js';

const prisma = new PrismaClient();

export const login = async (req, res) => {
  const { email, password } = req.body;
  const user = await prisma.user.findUnique({ where: { email } });
  if (!user) return res.status(400).json({ error: 'Email tidak ditemukan' });

  const valid = await bcrypt.compare(password, user.password);
  if (!valid) return res.status(400).json({ error: 'Password salah' });

  const accessToken = generateAccessToken(user.id);
  const refreshToken = generateRefreshToken(user.id);

  await prisma.user.update({
    where: { id: user.id },
    data: { refreshToken },
  });

  res.json({ accessToken, refreshToken });
};

export const refreshToken = async (req, res) => {
  const { token } = req.body;
  if (!token) return res.status(401).json({ error: 'Token tidak ditemukan' });

  try {
    const payload = jwt.verify(token, process.env.JWT_REFRESH_SECRET);
    const user = await prisma.user.findUnique({ where: { id: payload.userId } });

    if (!user || user.refreshToken !== token)
      return res.status(403).json({ error: 'Refresh token tidak valid' });

    const newAccessToken = generateAccessToken(user.id);
    const newRefreshToken = generateRefreshToken(user.id);

    await prisma.user.update({
      where: { id: user.id },
      data: { refreshToken: newRefreshToken },
    });

    res.json({ accessToken: newAccessToken, refreshToken: newRefreshToken });
  } catch (err) {
    return res.status(403).json({ error: 'Token kadaluarsa atau rusak' });
  }
};

export const logout = async (req, res) => {
  const { userId } = req.user;

  await prisma.user.update({
    where: { id: userId },
    data: { refreshToken: null },
  });

  res.json({ message: 'Logout berhasil' });
};
```

### 4. Tambahkan Route `authRoutes.js`

```js
import { Router } from 'express';
import { login, register, refreshToken, logout } from '../controllers/authController.js';
import authMiddleware from '../middlewares/authMiddleware.js';

const router = Router();

router.post('/register', register);
router.post('/login', login);
router.post('/refresh-token', refreshToken);
router.post('/logout', authMiddleware, logout);

export default router;
```

### 5. Tambahkan `.env`

```bash
JWT_SECRET=your_access_token_secret
JWT_REFRESH_SECRET=your_refresh_token_secret
```

## 6. Cara Pakai dari Client (frontend)

### Login:

```json
POST /api/auth/login
{
  "email": "a@mail.com",
  "password": "123456"
}
```
Respons:

```json
{
  "accessToken": "...",
  "refreshToken": "..."
}
```
### Refresh:

```json
POST /api/auth/refresh-token
{
  "token": "<refreshToken>"
}
```

### Logout:

* Kirim `accessToken` ke header Authorization → hapus refresh token dari DB.

---

## 🧪 Saran Penyimpanan Token (di Frontend)

| Token         | Disimpan di                                      |
| --------------- | -------------------------------------------------- |
| Access Token  | Memory / HTTP-only cookie                        |
| Refresh Token | HTTP-only cookie / Local Storage (dengan resiko) |




















