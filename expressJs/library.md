<p align="center">
  <img src="images/library/1754105740048.png" width="200" />
</p>

## **Daftar Isi**

* [Middleware Umum](#-1-middleware-umum)
  * [morgan](#11-morgan)
  * [cors](#12-cors)
  * [helmet](#13-helmet)
  * [compression](#14-compression)
* [Body Parsing](#2-Body-Parsing)
* [Authentication & Security](#3-authentication--security)
* [Database](#4-Database)
* [File Upload](#5-File-Upload)
* [Utility](#5-Utility)
* [Session & Cookies](#7-Session--Cookies)
* [Rate Limiting & Proteksi API](#8-Rate-Limiting--Proteksi-API)
* [Template Engine](#9-Template-Engine)
* [Real-time Communication](#10-Real-time-Communication)
* [Routing & Modularization](#11-Routing--Modularization)
* [Dokumentasi API](#12-Dokumentasi-API)
* [Task Scheduling](#13-Task-Scheduling)

3. **Authentication & Security**

## 1. **Middleware Umum**

Middleware adalah inti dari Express. Banyak library yang bisa digunakan untuk memproses request dan response.

### 1.1 `morgan`

📌 **Fungsi**: Logging HTTP request (monitoring request masuk ke server).

```bash
npm install morgan
```

**Contoh penggunaan:**

```js
import express from "express";
import morgan from "morgan";

const app = express();
app.use(morgan("dev")); // format log: method, status, response time

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000);
```

💡 **Penjelasan**:

* `dev`: Format log singkat untuk development.
* Ada format lain: `combined`, `tiny`, dll.

---

### 1.2 `cors`

📌 **Fungsi**: Mengizinkan akses dari domain berbeda (Cross-Origin Resource Sharing).

```bash
npm install cors
```

**Contoh penggunaan:**

```js
import express from "express";
import cors from "cors";

const app = express();
app.use(cors()); // mengizinkan semua domain
// app.use(cors({ origin: "https://example.com" })); // hanya domain tertentu

app.get("/", (req, res) => {
  res.send("CORS aktif");
});

app.listen(3000);
```

💡 **Penjelasan**: Tanpa `cors`, API tidak bisa diakses oleh frontend yang berbeda domain.

---

### 1.3 `helmet`

📌 **Fungsi**: Menambahkan header keamanan HTTP.

```bash
npm install helmet
```

**Contoh penggunaan:**

```js
import express from "express";
import helmet from "helmet";

const app = express();
app.use(helmet());

app.get("/", (req, res) => {
  res.send("Security headers aktif");
});

app.listen(3000);
```

💡 **Penjelasan**: Menambahkan header seperti `X-Content-Type-Options` dan `Content-Security-Policy` untuk mencegah serangan umum.

---

### 1.4 `compression`

📌 **Fungsi**: Mengompres response untuk mempercepat transfer data.

```bash
npm install compression
```

**Contoh penggunaan:**

```js
import express from "express";
import compression from "compression";

const app = express();
app.use(compression());

app.get("/", (req, res) => {
  res.send("Data dikompres dengan gzip");
});

app.listen(3000);
```

💡 **Penjelasan**: Mengurangi ukuran data response (gzip).

---

## 2. **Body Parsing**

Secara default Express hanya menangani request sederhana. Untuk parsing data body:

### 2.1 `body-parser` (built-in mulai Express v4.16)

📌 **Fungsi**: Parsing `JSON` dan `URL-encoded form`.

```js
import express from "express";

const app = express();
app.use(express.json()); // parsing JSON
app.use(express.urlencoded({ extended: true })); // parsing form

app.post("/data", (req, res) => {
  console.log(req.body);
  res.send("Data diterima");
});

app.listen(3000);
```

💡 **Penjelasan**: `express.json()` = JSON; `express.urlencoded()` = form-data.

---

## 3. **Authentication & Security**

### 3.1 `jsonwebtoken`

📌 **Fungsi**: Autentikasi menggunakan JWT (JSON Web Token).

```bash
npm install jsonwebtoken
```

**Contoh penggunaan:**

```js
import express from "express";
import jwt from "jsonwebtoken";

const app = express();
const secretKey = "rahasia123";

app.post("/login", (req, res) => {
  const token = jwt.sign({ user: "eko" }, secretKey, { expiresIn: "1h" });
  res.json({ token });
});

app.get("/protected", (req, res) => {
  const token = req.headers.authorization?.split(" ")[1];
  try {
    const decoded = jwt.verify(token, secretKey);
    res.json(decoded);
  } catch {
    res.status(401).send("Token tidak valid");
  }
});

app.listen(3000);
```

---

### 3.2 `bcrypt`

📌 **Fungsi**: Enkripsi password.

```bash
npm install bcrypt
```

**Contoh penggunaan:**

```js
import bcrypt from "bcrypt";

const password = "123456";
const hashed = await bcrypt.hash(password, 10);
console.log(hashed);

const match = await bcrypt.compare("123456", hashed);
console.log(match); // true
```

💡 **Penjelasan**: Hashing satu arah yang aman untuk password.

---

## 4. **Database**

### 4.1 `mongoose`

📌 **Fungsi**: ODM (Object Data Modeling) untuk MongoDB.

```bash
npm install mongoose
```

**Contoh penggunaan:**

```js
import mongoose from "mongoose";

await mongoose.connect("mongodb://localhost:27017/test");

const User = mongoose.model("User", { name: String });
const user = new User({ name: "Eko" });
await user.save();
```

---

### 4.2 `pg` (PostgreSQL)

📌 **Fungsi**: Query PostgreSQL.

```bash
npm install pg
```

**Contoh penggunaan:**

```js
import pkg from "pg";
const { Client } = pkg;

const client = new Client({ user: "postgres", password: "1234", database: "test" });
await client.connect();

const result = await client.query("SELECT NOW()");
console.log(result.rows);
```

---

### 4.3 `prisma`

📌 **Fungsi**: ORM modern mendukung PostgreSQL, MySQL, SQLite.

```bash
npm install prisma
npx prisma init
```

**Contoh penggunaan:**

```js
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

const user = await prisma.user.create({
  data: { name: "Eko" }
});
```

---

## 5. **File Upload**

### `multer`

📌 **Fungsi**: Upload file (image, doc, dll).

```bash
npm install multer
```

**Contoh penggunaan:**

```js
import express from "express";
import multer from "multer";

const upload = multer({ dest: "uploads/" });
const app = express();

app.post("/upload", upload.single("file"), (req, res) => {
  res.send(`File diupload: ${req.file.originalname}`);
});

app.listen(3000);
```

---

## 6. **Utility**

### 6.1 `dotenv`

📌 **Fungsi**: Load variabel environment dari `.env`.

```bash
npm install dotenv
```

**Contoh penggunaan:**

```js
import dotenv from "dotenv";
dotenv.config();

console.log(process.env.PORT);
```

---

### 6.2 `express-validator`

📌 **Fungsi**: Validasi input request.

```bash
npm install express-validator
```

**Contoh penggunaan:**

```js
import express from "express";
import { body, validationResult } from "express-validator";

const app = express();
app.use(express.json());

app.post("/register",
  body("email").isEmail(),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) return res.status(400).json(errors.array());
    res.send("Valid!");
  }
);

app.listen(3000);
```

---

💡 **Kesimpulan:**

* **Basic Middleware**: `morgan`, `cors`, `helmet`, `compression`
* **Body Parsing**: `express.json`, `express.urlencoded`
* **Auth & Security**: `jsonwebtoken`, `bcrypt`
* **Database**: `mongoose`, `pg`, `prisma`
* **Upload File**: `multer`
* **Utility**: `dotenv`, `express-validator`

## 7. Session & Cookies

### 7.1 express-session

📌 Fungsi: Menyimpan session user di server (untuk login state, shopping cart, dll.)

npm install express-session
Contoh penggunaan:

```js
import express from "express";
import session from "express-session";const app = express();app.use(session({
secret: "rahasia-super-aman",
resave: false,
saveUninitialized: true,
cookie: { secure: false } // true jika pakai HTTPS
}));app.get("/", (req, res) => {
req.session.views = (req.session.views || 0) + 1;
res.send(`Dilihat ${req.session.views} kali`);
});app.listen(3000);
```

💡 Penjelasan: Session tersimpan di server, bukan di browser.

### 7.2 cookie-parser

📌 Fungsi: Parsing cookie dari request.

```bash
npm install cookie-parser
```

Contoh penggunaan:

```js
import express from "express";
import cookieParser from "cookie-parser";const app = express();
app.use(cookieParser());app.get("/", (req, res) => {
res.cookie("user", "eko");
res.send("Cookie diset!");
});

```

## 8. Rate Limiting & Proteksi API

### 8.1 express-rate-limit

📌 Fungsi: Membatasi jumlah request dari satu IP.

```basic
npm install express-rate-limit
```

Contoh penggunaan:

```js
import rateLimit from "express-rate-limit";const limiter = rateLimit({
windowMs: 1 * 60 * 1000, // 1 menit
max: 5, // Maksimal 5 request
message: "Terlalu banyak request, coba lagi nanti."
});app.use("/api", limiter);
```

### 8.2 hpp (HTTP Parameter Pollution)

📌 Fungsi: Mencegah duplikasi query parameter berbahaya.

```
npm install hpp
```

Penggunaan:

```js
import hpp from "hpp";
app.use(hpp());
```

## 9. Template Engine

Jika butuh server-side rendering (SSR):

### 9.1 pug

📌 Fungsi: Template engine minimalis.

```bash
npm install pug
```

Contoh penggunaan:

```js
app.set("view engine", "pug");app.get("/", (req, res) => {
res.render("index", { title: "Halo", message: "Hello dari Pug!" });
});
```

### 9.2 ejs

📌 Fungsi: Template engine berbasis HTML.

```bash
npm install ejs
```

contoh penggunaan

```js
app.set("view engine", "ejs");app.get("/", (req, res) => {
res.render("index", { name: "Eko" });
});
```

## 10. Real-time Communication

### 10.1 socket.io

📌 Fungsi: WebSocket untuk chat & notifikasi real-time.

```bash
npm install socket.io
```

Contoh penggunaan:

```js

import express from "express";
import { createServer } from "http";
import { Server } from "socket.io";const app = express();
const server = createServer(app);
const io = new Server(server);io.on("connection", (socket) => {
console.log("User terhubung");
socket.emit("welcome", "Selamat datang!");
});
server.listen(3000);
```

## 11. Routing & Modularization

### 11.1 express-async-handler

📌 Fungsi: Menangani async/await error tanpa try-catch berulang.

```
npm install express-async-handler
```

Contoh penggunaan:

```js
import asyncHandler from "express-async-handler";app.get("/", asyncHandler(async (req, res) => {
const data = await fetchData();
res.json(data);
}));
```

## 12. Dokumentasi API

### 12.1 swagger-ui-express

📌 Fungsi: Menyediakan dokumentasi API otomatis.

```bash
npm install swagger-ui-express swagger-jsdoc
```

Contoh penggunaan:

```js
import swaggerUi from "swagger-ui-express";
import swaggerJsdoc from "swagger-jsdoc";const options = {
definition: {
openapi: "3.0.0",
info: { title: "API Express", version: "1.0.0" }
},
apis: ["./routes/*.js"]
};const specs = swaggerJsdoc(options);
app.use("/docs", swaggerUi.serve, swaggerUi.setup(specs));
```

## 13. Task Scheduling

### 13.1 node-cron

📌 Fungsi: Menjalankan task terjadwal.

```bash
npm install node-cron
```

Contoh penggunaan:

```js
import cron from "node-cron";cron.schedule("*/5 * * * * *", () => {
console.log("Jalan tiap 5 detik");
});
⚡ 14. Performance & Cluster
```

### 13.2 pm2

📌 Fungsi: Menjalankan Express dalam mode cluster.

```bash
npm install pm2 -g
```

```bash
pm2 start server.js -i max
```

💡 Memanfaatkan semua core CPU untuk meningkatkan performa.
