# 📘 Belajar Express.js Dasar

Express.js adalah framework web untuk Node.js yang cepat, ringan, dan fleksibel. Cocok digunakan untuk membangun REST API maupun aplikasi web.

---

## 🛠️ Persiapan

### 1. Instal Node.js
Download dari: [https://nodejs.org](https://nodejs.org)

### 2. Inisialisasi Proyek
```bash
mkdir belajar-express
cd belajar-express
npm init -y
npm install express
🚀 Hello World Express
js
Copy
Edit
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Halo dari Express!');
});

app.listen(port, () => {
  console.log(`Server berjalan di http://localhost:${port}`);
});
```
## 📦 Paket Tambahan Penting
### 1. morgan – Logging Request
Fungsi: Menampilkan log setiap request (HTTP method, URL, status, dll).

Instalasi & Penggunaan:

```
npm install morgan
```
```js
const morgan = require('morgan');
app.use(morgan('dev'));
```
### 2. dotenv – Kelola Variabel Environment
Fungsi: Membaca konfigurasi dari file .env.

Instalasi & Penggunaan:

```
npm install dotenv
```
File .env:
```
PORT=3000
SECRET_KEY=mysecret
```
Di index.js:

```js
require('dotenv').config();
const port = process.env.PORT;
```
### 3. cors – Mengizinkan Akses dari Origin Lain
Fungsi: Mengizinkan frontend dari domain berbeda untuk mengakses server ini.

Instalasi & Penggunaan:

```bash
npm install cors
```
```js
const cors = require('cors');
app.use(cors()); // Mengizinkan semua origin
```
Contoh lebih aman:

```js
app.use(cors({
  origin: 'http://localhost:3001'
}));
```
### 4. body-parser (Built-in) – Parsing Body Request
Fungsi: Membaca data dari body request (JSON atau form).

Penggunaan (tanpa install tambahan):

```js
app.use(express.json()); // Untuk aplikasi JSON
app.use(express.urlencoded({ extended: true })); // Untuk form (urlencoded)
```
Contoh penggunaan:

```js
app.post('/user', (req, res) => {
  console.log(req.body);
  res.send('Data diterima!');
});
```
📂 Struktur Project Sederhana
pgsql
```
belajar-express/
├── node_modules/
├── .env
├── index.js
├── package.json
```
✅ Contoh Route Tambahan



```js
app.get('/about', (req, res) => {
  res.send('Ini halaman About');
});

app.get('/user/:name', (req, res) => {
  res.send(`Halo, ${req.params.name}`);
});

app.get('/search', (req, res) => {
  res.send(`Query: ${req.query.q}`);
});
```
🎯 Selanjutnya
🔐 Autentikasi (JWT, bcrypt)

🛢️ Koneksi database (MongoDB, MySQL)

🔄 CRUD API

📦 Struktur folder MVC

### 📚 Referensi
Express.js Official Docs,
Node.js,
npm


### 🚀 Penutup
Selamat belajar dan eksplorasi Express.js! Cocok untuk membuat API ringan, backend frontend modern, atau fullstack project.

---
