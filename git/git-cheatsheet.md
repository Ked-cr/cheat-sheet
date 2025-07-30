# 📘 Git Command Cheat Sheet

Dokumen ini merupakan rangkuman perintah Git yang umum digunakan, disusun berdasarkan fungsi dan kategori untuk memudahkan pemahaman dan penggunaan Git dalam pengelolaan proyek versi kontrol.

---

## 📂 1. Konfigurasi Awal

| Perintah                                       | Penjelasan                                           |
|-----------------------------------------------|------------------------------------------------------|
| `git config --global user.name "Nama"`        | Mengatur nama pengguna Git secara global            |
| `git config --global user.email "email"`      | Mengatur email pengguna Git secara global           |
| `git config --global core.editor "editor"`    | Menentukan editor teks default untuk Git            |
| `git config --list`                           | Menampilkan semua konfigurasi Git yang ada          |

---

## 📁 2. Inisialisasi & Repository

| Perintah               | Penjelasan                                                   |
|------------------------|--------------------------------------------------------------|
| `git init`             | Membuat repository Git baru di direktori saat ini            |
| `git clone <url>`      | Menggandakan repository dari remote URL                      |

---

## 🗂️ 3. Status & Informasi

| Perintah                | Penjelasan                                                  |
|-------------------------|-------------------------------------------------------------|
| `git status`            | Melihat status file yang dimodifikasi, staged, atau baru    |
| `git log`               | Menampilkan riwayat commit                                  |
| `git log --oneline`     | Menampilkan riwayat commit singkat dalam satu baris         |
| `git show <commit>`     | Menampilkan detail perubahan dari commit tertentu           |

---

## 🧾 4. Menambahkan & Commit Perubahan

| Perintah                     | Penjelasan                                                       |
|-----------------------------|------------------------------------------------------------------|
| `git add <file>`            | Menambahkan file tertentu ke staging area                        |
| `git add .`                 | Menambahkan semua perubahan ke staging area                      |
| `git commit -m "pesan"`     | Membuat commit dengan pesan tertentu                             |
| `git commit -am "pesan"`    | Menambahkan dan commit file yang sudah pernah di-track sebelumnya|

---

## 🔄 5. Branching & Merging

| Perintah                      | Penjelasan                                                       |
|-------------------------------|------------------------------------------------------------------|
| `git branch`                  | Menampilkan semua cabang (branch)                                |
| `git branch <nama>`           | Membuat cabang baru                                              |
| `git checkout <branch>`       | Berpindah ke cabang tertentu                                     |
| `git checkout -b <nama>`      | Membuat dan langsung berpindah ke cabang baru                    |
| `git merge <branch>`          | Menggabungkan cabang tertentu ke cabang aktif saat ini           |
| `git branch -d <nama>`        | Menghapus cabang yang sudah tidak digunakan                      |

---

## 🔁 6. Remote Repository

| Perintah                                | Penjelasan                                                           |
|-----------------------------------------|----------------------------------------------------------------------|
| `git remote add origin <url>`           | Menghubungkan repository lokal ke remote repository                  |
| `git remote -v`                         | Melihat daftar remote repository yang terhubung                      |
| `git push -u origin <branch>`           | Push branch ke remote untuk pertama kalinya dan set default upstream|
| `git push`                              | Mengirim commit ke remote repository                                 |
| `git pull`                              | Mengambil dan menggabungkan perubahan dari remote repository         |
| `git fetch`                             | Mengambil update dari remote tanpa melakukan merge                   |

---

## 📌 7. Undo / Reset

| Perintah                         | Penjelasan                                                              |
|----------------------------------|-------------------------------------------------------------------------|
| `git reset <file>`              | Menghapus file dari staging area                                        |
| `git checkout -- <file>`        | Mengembalikan file ke kondisi commit terakhir                           |
| `git reset --soft <commit>`     | Reset ke commit tertentu, perubahan tetap disimpan di staging           |
| `git reset --hard <commit>`     | Reset ke commit tertentu dan hapus semua perubahan                      |
| `git revert <commit>`           | Membuat commit baru untuk membatalkan commit tertentu                   |

---

## 🧪 8. Stashing (Simpan Sementara)

| Perintah                    | Penjelasan                                                  |
|-----------------------------|-------------------------------------------------------------|
| `git stash`                | Menyimpan sementara semua perubahan kerja yang belum di-commit |
| `git stash list`           | Melihat daftar perubahan yang telah disimpan sementara       |
| `git stash apply`          | Menerapkan kembali perubahan yang disimpan sementara         |
| `git stash drop`           | Menghapus entri stash yang tidak lagi diperlukan             |

---

## 🧹 9. Pembersihan & Lain-lain

| Perintah                          | Penjelasan                                                 |
|-----------------------------------|------------------------------------------------------------|
| `git clean -f`                   | Menghapus file yang tidak dilacak (untracked)              |
| `git tag <nama>`                | Memberi tag atau label pada commit tertentu                |
| `git blame <file>`              | Menampilkan siapa yang mengubah setiap baris dan kapan     |

---

## 📚 Tips Tambahan

- Gunakan file `.gitignore` untuk mengecualikan file atau folder dari version control.
- Gunakan `git reflog` untuk melihat histori referensi commit, termasuk commit yang sudah di-reset.
- Hindari `git reset --hard` tanpa memahami konsekuensinya (berisiko kehilangan perubahan yang belum tersimpan).

---

## 📎 Referensi

- [Pro Git Book (Free)](https://git-scm.com/book/en/v2)
- [GitHub Git Cheatsheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Git SCM Official Website](https://git-scm.com)

---

> Dokumentasi ini bisa kamu kontribusikan, duplikasi, atau cetak untuk keperluan belajar. Semoga membantu! 🚀
