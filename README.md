# Portfolio DevSecOps — Audit Keamanan Otomatis

Project praktik DevSecOps yang mengintegrasikan pemeriksaan keamanan otomatis menggunakan GitHub Actions, ShellCheck, dan Trivy. Project ini berfokus pada pemeriksaan skrip Bash, konfigurasi Kubernetes, dan kerentanan pada image container.

## Tujuan Project

Project ini dibuat untuk mempelajari penerapan keamanan dalam proses pengembangan dan deployment aplikasi (*DevSecOps*), sehingga potensi masalah keamanan dapat ditemukan lebih awal melalui proses otomatis.

## Teknologi yang Digunakan

- **Git dan GitHub** — Mengelola kode sumber dan riwayat perubahan.
- **GitHub Actions** — Menjalankan pemeriksaan keamanan secara otomatis.
- **ShellCheck** — Menganalisis skrip Bash untuk menemukan potensi kesalahan.
- **Trivy** — Memeriksa konfigurasi Kubernetes dan kerentanan image container.
- **Kubernetes** — Mengelola konfigurasi deployment aplikasi.
- **Nginx** — Web server yang digunakan dalam demonstrasi.

## Fitur Keamanan

### 1. Analisis Skrip Bash

ShellCheck digunakan untuk menganalisis skrip:

`scripts/linux-hardening.sh`

Pemeriksaan ini membantu menemukan kesalahan umum dan potensi masalah pada skrip Bash.

### 2. Pemindaian Konfigurasi Kubernetes

Trivy memeriksa file konfigurasi Kubernetes di direktori `k8s/` untuk menemukan masalah dengan tingkat keparahan HIGH dan CRITICAL.

**Hasil pengujian lokal:** Tidak ditemukan temuan HIGH atau CRITICAL pada file konfigurasi yang diperiksa.

Perintah yang digunakan:

```bash
trivy config --severity HIGH,CRITICAL --exit-code 1 k8s/
```

### 3. Pemindaian Kerentanan Image Container

Trivy digunakan untuk memeriksa image `nginx:1.28` terhadap kerentanan dengan tingkat keparahan HIGH dan CRITICAL.

Workflow saat ini menampilkan hasil pemindaian tanpa menggagalkan job hanya karena ditemukan kerentanan pada image. Oleh karena itu, workflow yang berhasil tidak otomatis berarti image tersebut bebas dari kerentanan.

### 4. Penguatan Keamanan Kubernetes

Deployment Kubernetes menerapkan beberapa pengaturan keamanan:

- Menjalankan container menggunakan pengguna non-root.
- Menonaktifkan peningkatan hak akses (*privilege escalation*).
- Menghapus Linux capabilities yang tidak diperlukan.
- Menggunakan sistem file root hanya-baca (*read-only root filesystem*).
- Menggunakan profil seccomp `RuntimeDefault`.
- Menyediakan direktori sementara yang dapat ditulis melalui volume `emptyDir`.

Pengaturan tersebut membantu mengurangi risiko keamanan pada container.

### 5. Otomatisasi dengan GitHub Actions

Workflow keamanan dijalankan secara otomatis ketika:

- Ada perubahan yang di-push ke branch `main`.
- Ada pull request yang menargetkan branch `main`.
- Workflow dijalankan secara manual melalui GitHub Actions.

## Struktur Project

```text
devsecops-portfolio/
├── .github/
│   └── workflows/
│       └── security-audit.yml
├── k8s/
│   ├── deployment.yaml
│   └── nginx-config.yaml
├── scripts/
│   └── linux-hardening.sh
└── README.md
```

## Cara Menjalankan Pengujian

### 1. Pemeriksaan Skrip Bash

```bash
shellcheck scripts/linux-hardening.sh
```

### 2. Pemindaian Konfigurasi Kubernetes

```bash
trivy config --severity HIGH,CRITICAL --exit-code 1 k8s/
```

### 3. Pemindaian Image Container

```bash
trivy image --severity HIGH,CRITICAL nginx:1.28
```

### 4. Deployment ke Kubernetes

Pastikan cluster Kubernetes aktif, kemudian jalankan:

```bash
kubectl apply -f k8s/
```

Periksa status deployment dan Pod:

```bash
kubectl get deployments,pods
```

Periksa log aplikasi jika diperlukan:

```bash
kubectl logs deployment/devsecops-demo
```

## Hasil yang Diharapkan

- Skrip Bash diperiksa menggunakan ShellCheck.
- Konfigurasi Kubernetes diperiksa menggunakan Trivy.
- Image container dianalisis untuk menemukan kerentanan.
- Pengaturan keamanan diterapkan pada Deployment Kubernetes.
- Proses audit keamanan dijalankan melalui GitHub Actions.

Hasil pemeriksaan bergantung pada versi image, konfigurasi, tingkat keparahan yang dipilih, dan waktu pemindaian.

## Pembelajaran yang Diperoleh

Melalui project ini, saya mempelajari:

- Penerapan pemeriksaan keamanan otomatis dalam workflow CI.
- Analisis statis pada skrip Bash.
- Pemeriksaan kesalahan konfigurasi keamanan Kubernetes.
- Pemindaian kerentanan image container.
- Penerapan pengaturan keamanan pada container.
- Validasi dan troubleshooting pipeline GitHub Actions.

## Pengembangan Selanjutnya

Beberapa peningkatan yang dapat dilakukan:

- Menambahkan pemindaian dependensi aplikasi.
- Menambahkan pemeriksaan kebocoran secret atau kredensial.
- Menghasilkan laporan keamanan dalam format yang dapat diarsipkan.
- Menambahkan aturan kebijakan keamanan Kubernetes.
- Mendokumentasikan langkah perbaikan untuk setiap kerentanan yang ditemukan.

## Catatan

Project ini dibuat untuk pembelajaran dan portfolio. Hasil pemindaian merupakan gambaran dari pemeriksaan yang dijalankan dengan konfigurasi tertentu dan tidak menjamin bahwa seluruh sistem bebas dari risiko keamanan.

