# 🐳 Docker Command Cheat Sheet

Dokumentasi ringkas perintah dasar Docker untuk membantu pengembangan dan deployment container secara efisien.

---

## 🔹 1. Docker Dasar

| Perintah | Deskripsi |
|----------|-----------|
| `docker --version` | Menampilkan versi Docker |
| `docker info` | Menampilkan informasi sistem Docker |
| `docker help` | Menampilkan daftar bantuan perintah |

---

## 🔹 2. Docker Image

| Perintah | Deskripsi |
|----------|-----------|
| `docker pull <image>` | Mengunduh image dari Docker Hub |
| `docker images` | Menampilkan daftar image lokal |
| `docker rmi <image>` | Menghapus image dari sistem |
| `docker build -t <name>:<tag> .` | Membangun image dari Dockerfile |

---

## 🔹 3. Docker Container

| Perintah | Deskripsi |
|----------|-----------|
| `docker run <image>` | Menjalankan container dari image |
| `docker run -it <image>` | Menjalankan container dengan terminal interaktif |
| `docker run -d <image>` | Menjalankan container secara background (detached) |
| `docker run --name <nama> <image>` | Menjalankan container dengan nama khusus |
| `docker start <nama>` | Menyalakan container yang sudah ada |
| `docker stop <nama>` | Mematikan container yang berjalan |
| `docker restart <nama>` | Merestart container |
| `docker rm <nama>` | Menghapus container |
| `docker ps` | Menampilkan container yang sedang berjalan |
| `docker ps -a` | Menampilkan semua container (berjalan dan berhenti) |
| `docker exec -it <container> bash` | Masuk ke dalam container (bash shell) |

---

## 🔹 4. Docker Volume & Network

### Volume
| Perintah | Deskripsi |
|----------|-----------|
| `docker volume ls` | Melihat semua volume |
| `docker volume create <nama>` | Membuat volume baru |
| `docker volume rm <nama>` | Menghapus volume |

### Network
| Perintah | Deskripsi |
|----------|-----------|
| `docker network ls` | Menampilkan daftar jaringan |
| `docker network create <nama>` | Membuat jaringan baru |
| `docker network connect <jaringan> <container>` | Menghubungkan container ke jaringan |
| `docker network disconnect <jaringan> <container>` | Memutus koneksi container dari jaringan |

---

## 🔹 5. Docker Compose

| Perintah | Deskripsi |
|----------|-----------|
| `docker-compose up` | Menjalankan semua service di `docker-compose.yml` |
| `docker-compose up -d` | Menjalankan secara background |
| `docker-compose down` | Menghentikan dan menghapus semua service |
| `docker-compose build` | Membangun kembali image service |
| `docker-compose logs` | Menampilkan log dari semua service |
| `docker-compose exec <service> bash` | Akses shell ke dalam container via service |

---

## 📚 Referensi Resmi

- [Docker CLI Reference](https://docs.docker.com/engine/reference/commandline/docker/)
- [Docker Compose CLI Reference](https://docs.docker.com/compose/reference/)

---

> 📝 Tips: Simpan file ini sebagai `README.md` dalam folder dokumentasi proyekmu atau sebagai catatan pribadi.

