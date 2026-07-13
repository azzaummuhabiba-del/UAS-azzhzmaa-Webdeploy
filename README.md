# 🚀 UAS Web Deployment - azzhzmaa

<p align="center">
  <img src="https://img.shields.io/badge/UAS-2026-blue?style=for-the-badge" alt="UAS">
  <img src="https://img.shields.io/badge/STMIK_TAZKIA-Kelas_Sentul-green?style=for-the-badge" alt="STMIK Tazkia">
  <img src="https://img.shields.io/badge/Docker-Compose-blue?style=for-the-badge&logo=docker" alt="Docker">
  <img src="https://img.shields.io/badge/Nginx-Reverse_Proxy-green?style=for-the-badge&logo=nginx" alt="Nginx">
</p>

Repository ini dibuat untuk memenuhi tugas **Ujian Akhir Semester (UAS)** mata kuliah *Sistem Operasi (MITI.202)* dan *Jaringan Komputer (MITI.203)* di STMIK Tazkia. Projek ini mendemonstrasikan proses deployment aplikasi web ke server production (VPS) secara mandiri[cite: 1].

---

## 🔗 Link Akses Sistem
* **Website Utama (HTTPS):** [https://azzhzmaa.my.id](https://azzhzmaa.my.id)
* **Dashboard Monitoring:** [https://azzhzmaa.my.id:3007](https://azzhzmaa.my.id:3007)

---

## 🏗️ Arsitektur & Detail Port
Semua service berjalan di dalam satu server VPS menggunakan kontainerisasi[cite: 1]:
* **Aplikasi Web:** Berjalan di Port `8086` (Docker Container)
* **Monitoring:** Uptime Kuma di Port `3007`[cite: 1]
* **Reverse Proxy:** Nginx mengarah ke port aplikasi dengan SSL dari Certbot Let's Encrypt[cite: 1].
* **Otomatisasi:** GitHub Actions untuk auto-deploy setiap melakukan `git push` ke branch `main`[cite: 1].

---

## 🛠️ Langkah-Langkah Deployment (Full Setup)

### 1. Persiapan Awal VPS
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git nginx ufw -y
2. Install Docker & Docker Compose
Bash
curl -fsSL [https://get.docker.com](https://get.docker.com) -o get-docker.sh
sudo sh get-docker.sh
docker compose up -d
3. Konfigurasi Nginx Reverse Proxy
Bash
sudo nano /etc/nginx/sites-available/azzhzmaa.my.id
Isi file konfigurasi:

Nginx
server {
    server_name azzhzmaa.my.id;
    location / {
        proxy_pass http://localhost:8086;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
Aktifkan konfigurasi:

Bash
sudo ln -s /etc/nginx/sites-available/azzhzmaa.my.id /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
4. Setup SSL dengan Certbot (HTTPS)
Bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d azzhzmaa.my.id
📑 Runbook / Perintah Darurat VPS
Catatan operasional untuk diagnosis dan recovery saat demonstrasi live-recovery di depan dosen[cite: 1].

1. Cek Logs Aplikasi (Docker Logs)
Bash
docker compose logs -f web
2. Restart Container (Jika Terjadi Error 502)
Bash
docker compose restart web
3. Mengatasi Firewall Block (UFW)
Bash
sudo ufw allow 443/tcp
sudo ufw allow 3007/tcp
sudo ufw reload
👤 Developer
Nama: Azza

GitHub Username: azzhzmaa
