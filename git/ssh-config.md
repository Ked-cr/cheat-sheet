# SSH

## Apa itu SSH?

**SSH (Secure Shell)** adalah **protokol jaringan** yang digunakan untuk melakukan komunikasi antar komputer secara **aman** lewat jaringan (misalnya internet).

Dengan SSH, kamu bisa **login, mengontrol, dan transfer data** ke komputer lain seakan-akan kamu langsung duduk di depan komputer itu.

## Fungsi SSH

* 🔐 **Akses Jarak Jauh (Remote Login)**

  Bisa masuk ke server dari mana aja tanpa harus berada di lokasi fisik.
* 💻 **Menjalankan Perintah di Server**

  Kamu bisa mengelola server (misalnya update, install software, setting aplikasi).
* 📂 **Transfer File Aman**

  Bisa kirim dan ambil file lewat `scp` atau `sftp`.
* 🔑 **Autentikasi dengan SSH Key**

  Menggunakan kunci enkripsi (public & private key) supaya lebih aman daripada password.
* 🔄 **Integrasi dengan Tools Developer**

  Contoh: GitHub/GitLab pakai SSH biar kamu bisa push & pull code tanpa harus masukin password tiap kali.

## impementtasi ssh pada github

membuat key ssh di terminal

```bash
ssh-keygen -t ed25519 -C "emailkamu@example.com"
```

* `-C` = komentar (biasanya isi dengan email kamu).
* `-t ed25519` = tipe key modern.

menjalankan agen ssh dan menambahkan key ke client ssh

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

mendapatkan key ssh untuk ditambah ke github

```bash
cat ~/.ssh/id_ed25519.pub
```

tambah ke setting> key ssh di github

cek koneksi

```bash
ssh -T git@github.com
```

## mutiple akun github

setting ssh config

```bash
notepad C:\Users\{user}/.ssh/config
```

tambahkan config sesuaikan dengan nama file directory ssh key

```bash
# Akun utama (misalnya personal)
Host github.com-personal  // alias ssh
    HostName github.com  // host github
    User git //user git
    IdentityFile C:\Users\user/.ssh/id_ed25519   // direktory key

# Akun kedua (misalnya kerja/freelance)
Host github.com-work
    HostName github.com
    User git
    IdentityFile C:\Users\user/.ssh/id_ed25519_github2


# Akun ketiga(misalnya user)
Host github.com-user
    HostName github.com
    User git
    IdentityFile C:\Users\user/.ssh/id_ed25519_ghuser
```

pengunaan:

```bash
git clone git@{alias ssh}:{user name github}/{nama-repo}.git
```
