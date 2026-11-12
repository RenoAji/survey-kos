# Frontend Implementation Plan — Jasa Survei Kos Mahasiswa

## Stack

- **Framework**: React (Vite) atau plain HTML/CSS/JS (pilih sesuai preferensi)
- **Styling**: Tailwind CSS
- **Routing**: React Router (jika pakai React)
- **Maps**: Google Maps Embed API (iframe, gratis, tanpa backend)
- **State**: useState / localStorage (tidak ada backend)
- **Payment display**: Tampilkan QR code statis (gambar QRIS, bukan integrasi nyata)

---

## Halaman & Komponen

### 1. Landing Page (`/`)

**Tujuan**: Perkenalan layanan, CTA ke dua layanan, tampilkan testimoni.

**Sections**:
- **Hero** — Judul, tagline, 2 tombol CTA ("Carikan Kos + Survei" & "Survei Kos Pilihan")
- **Cara Kerja** — Stepper/timeline alur layanan (ringkas, 4–5 langkah)
- **Keunggulan** — 4 card: hemat waktu, harga terjangkau, foto+video, laporan lengkap
- **Testimoni** — Carousel/grid ulasan pelanggan (data statis)
- **FAQ** — Accordion pertanyaan umum
- **Footer** — Kontak WhatsApp, Instagram, TikTok

---

### 2. Halaman Pilih Layanan (`/layanan`)

**Tujuan**: User memilih salah satu dari dua layanan.

**Komponen**:
- 2 card besar:
  - **Carikan Kos + Survei** — deskripsi singkat, harga estimasi, tombol "Pilih"
  - **Survei Kos Pilihan** — deskripsi singkat, harga estimasi, tombol "Pilih"

**Routing**:
- "Pilih" Layanan 1 → `/layanan/carikan-kos`
- "Pilih" Layanan 2 → `/layanan/survei-pilihan`

---

### 3. Layanan 1 — Carikan Kos + Survei (`/layanan/carikan-kos`)

**Alur**: Form kriteria → Rekomendasi kos → Pilih kos → Pembayaran → Konfirmasi

#### Step 1: Form Kriteria Kos

Field:
- Nama / WhatsApp (untuk pengiriman hasil)
- Anggaran (Rp) — slider atau input angka
- Jarak maksimal ke kampus — dropdown (< 500m, < 1km, < 2km, bebas)
- Fasilitas yang diinginkan — checkbox: AC, WiFi, Kamar Mandi Dalam, Parkir Motor, Dapur
- Jenis kos — radio: Putra / Putri / Campur
- Jumlah kos yang ingin disurvei — number input (1–5)
- Catatan tambahan — textarea

#### Step 2: Rekomendasi Kos

- Tampilkan daftar kos dari **data statis** (JSON hardcoded, 5–10 kos)
- Setiap card kos: nama, foto thumbnail, harga/bulan, jarak ke kampus, fasilitas badge, tombol "Pilih untuk Survei"
- Integrasikan **Google Maps Embed** untuk peta lokasi kos
- User bisa memilih 1–N kos sesuai input sebelumnya

#### Step 3: Pembayaran QRIS

- Ringkasan pesanan: kos dipilih, jumlah survei, total harga
- Harga dihitung: base price × jumlah kos (kalkulasi statis)
- Tampilkan **gambar QR Code QRIS** (placeholder/statis)
- Instruksi bayar
- Tombol "Konfirmasi Sudah Bayar" → pindah ke halaman konfirmasi

#### Step 4: Konfirmasi Pesanan

- Pesan sukses
- Nomor pesanan (generated random, e.g. `SRV-YYYYMMDD-XXXX`)
- Ringkasan: layanan, kos, nama, WhatsApp
- Instruksi: "Hasil survei akan dikirim ke WhatsApp kamu dalam 1×24 jam"
- Tombol "Kembali ke Beranda"

---

### 4. Layanan 2 — Survei Kos Pilihan (`/layanan/survei-pilihan`)

**Alur**: Form input link → Pembayaran → Konfirmasi

#### Step 1: Form Input Link

Field:
- Nama / WhatsApp
- Link Google Maps kos — input URL (validasi format `maps.google.com` / `goo.gl/maps`)
- Bisa tambah link (tombol "+ Tambah Kos", maks 5)
- Catatan tambahan — textarea

#### Step 2: Pembayaran QRIS

- Sama seperti Layanan 1 Step 3
- Harga dihitung berdasarkan jumlah link yang diinput

#### Step 3: Konfirmasi

- Sama seperti Layanan 1 Step 4

---

### 5. Halaman Testimoni (`/testimoni`) _(opsional, bisa jadi section di landing)_

- Grid card ulasan: foto avatar (placeholder), nama, rating bintang, teks ulasan
- Data statis (5–8 testimoni)
- Form tambah testimoni (tidak perlu submit ke backend — tampilkan toast "Terima kasih!" saja)

---

## Komponen Shared

| Komponen | Keterangan |
|---|---|
| `Navbar` | Logo, link navigasi, tombol WhatsApp |
| `Footer` | Kontak, sosmed, copyright |
| `StepIndicator` | Progress bar multi-step (Step 1/2/3) |
| `KosCard` | Card kos: foto, nama, harga, fasilitas, jarak |
| `QrisPayment` | Tampilan QR code + instruksi bayar |
| `TestimonialCard` | Card ulasan pelanggan |

---

## Data Statis (Mock Data)

Buat file `src/data/kos.json`:

```json
[
  {
    "id": 1,
    "nama": "Kos Melati",
    "alamat": "Jl. Raya ITS No. 12, Sukolilo, Surabaya",
    "harga": 700000,
    "jarak_kampus": "300m",
    "fasilitas": ["WiFi", "Kamar Mandi Dalam", "Parkir Motor"],
    "jenis": "Putri",
    "foto": "/images/kos1.jpg",
    "maps_embed": "https://www.google.com/maps/embed?pb=..."
  }
]
```

Buat file `src/data/testimoni.json` untuk data ulasan.

---

## Routing Summary

```
/                          → Landing Page
/layanan                   → Pilih Layanan
/layanan/carikan-kos       → Form Carikan Kos (multi-step)
/layanan/survei-pilihan    → Form Survei Pilihan (multi-step)
/konfirmasi                → Halaman konfirmasi pesanan
/testimoni                 → (opsional) Halaman khusus testimoni
```

---

## Kalkulasi Harga (Statis)

| Jumlah Kos | Harga per Kos | Total |
|---|---|---|
| 1 | Rp 50.000 | Rp 50.000 |
| 2 | Rp 45.000 | Rp 90.000 |
| 3+ | Rp 40.000 | Rp 40.000 × N |

> Angka ini placeholder — sesuaikan dengan keputusan tim.

---

## Urutan Implementasi

1. [ ] Setup project (Vite + React + Tailwind)
2. [ ] Buat komponen shared (Navbar, Footer, StepIndicator)
3. [ ] Landing Page (semua section)
4. [ ] Halaman Pilih Layanan
5. [ ] Form Layanan 1 — Step 1 (kriteria)
6. [ ] Halaman Rekomendasi Kos + data statis JSON
7. [ ] Step Pembayaran QRIS (shared component)
8. [ ] Halaman Konfirmasi
9. [ ] Form Layanan 2 — Step 1 (input link)
10. [ ] Section / Halaman Testimoni
11. [ ] Polish: responsif mobile, animasi transisi, validasi form

---

## Fitur yang Diabaikan (sesuai instruksi)

- Dasbor Admin
- Dasbor Surveyor
- Backend / API nyata
- Autentikasi user
- Integrasi QRIS nyata (hanya tampil QR statis)
- Pengiriman WhatsApp otomatis (hanya info instruksi)
