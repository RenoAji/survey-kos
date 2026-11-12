# Frontend Implementation Plan — Jasa Survei Kos Mahasiswa

## Stack

- **Framework**: React (Vite) atau plain HTML/CSS/JS
- **Styling**: Tailwind CSS
- **Routing**: React Router
- **State**: useState (tidak ada backend)

---

## Alur Utama (Direvisi)

```
Layanan 1: Form kriteria kos → Generate pesan WA → Redirect ke WA Admin
Layanan 2: Form input link Google Maps → Generate pesan WA → Redirect ke WA Admin
```

Rekomendasi kos, pembayaran QRIS, dan konfirmasi **terjadi di WhatsApp** — tidak diimplementasikan di web.

---

## Halaman & Komponen

### 1. Landing Page (`/`)

**Sections**:
- **Hero** — Judul, tagline, 2 tombol CTA ("Carikan Kos + Survei" & "Survei Kos Pilihan")
- **Cara Kerja** — Timeline alur singkat per layanan:
  - Layanan 1: Isi form → Chat WA → Dapat rekomendasi → Bayar → Hasil survei dikirim
  - Layanan 2: Kirim link → Chat WA → Bayar → Hasil survei dikirim
- **Keunggulan** — 4 card: hemat waktu, harga terjangkau, foto+video, laporan lengkap
- **Testimoni** — Grid ulasan pelanggan (data statis)
- **FAQ** — Accordion pertanyaan umum
- **Footer** — Nomor WA admin, Instagram, TikTok

---

### 2. Halaman Pilih Layanan (`/layanan`)

**Komponen**:
- 2 card besar:
  - **Carikan Kos + Survei** — deskripsi, tombol "Mulai"
  - **Survei Kos Pilihan** — deskripsi, tombol "Mulai"

---

### 3. Layanan 1 — Carikan Kos + Survei (`/layanan/carikan-kos`)

**Alur**: Isi form → Klik "Hubungi Admin via WhatsApp" → Redirect ke `wa.me/...`

**Form fields**:
- Nama lengkap
- Nomor WhatsApp
- Anggaran (Rp) — input angka atau slider
- Jarak maksimal ke kampus — dropdown (< 500m, < 1km, < 2km, Bebas)
- Fasilitas yang diinginkan — checkbox: AC, WiFi, Kamar Mandi Dalam, Parkir Motor, Dapur
- Jenis kos — radio: Putra / Putri / Campur
- Jumlah kos yang ingin disurvei — number input (1–5)
- Catatan tambahan — textarea

**Submit**:
- Tombol "Hubungi Admin via WhatsApp"
- Klik → generate URL `wa.me/62XXXXXXXXXX?text=...` dengan pesan terformat dari isian form
- Buka di tab baru

**Contoh pesan yang di-generate**:
```
Halo, saya ingin pesan layanan *Carikan Kos + Survei* 🏠

*Nama*: Budi Santoso
*WhatsApp*: 08123456789
*Anggaran*: Rp 800.000/bulan
*Jarak ke kampus*: < 1km
*Fasilitas*: AC, WiFi, Kamar Mandi Dalam
*Jenis kos*: Putra
*Jumlah kos disurvei*: 2
*Catatan*: Preferensi dekat minimarket
```

---

### 4. Layanan 2 — Survei Kos Pilihan (`/layanan/survei-pilihan`)

**Alur**: Isi form → Klik "Hubungi Admin via WhatsApp" → Redirect ke `wa.me/...`

**Form fields**:
- Nama lengkap
- Nomor WhatsApp
- Link Google Maps kos — input URL (bisa tambah sampai 5, tombol "+ Tambah Kos")
- Catatan tambahan — textarea

**Submit**:
- Tombol "Hubungi Admin via WhatsApp"
- Generate URL WA dengan pesan terformat

**Contoh pesan yang di-generate**:
```
Halo, saya ingin pesan layanan *Survei Kos Pilihan* 📍

*Nama*: Siti Rahayu
*WhatsApp*: 08987654321
*Link Kos*:
1. https://maps.google.com/...
2. https://maps.google.com/...
*Catatan*: Minta foto kamar mandi juga
```

---

### 5. Testimoni (section di Landing Page)

- Grid card: nama, rating bintang, teks ulasan (data statis)
- Tidak perlu form input testimoni

---

## Komponen Shared

| Komponen | Keterangan |
|---|---|
| `Navbar` | Logo, link navigasi, tombol WA |
| `Footer` | Nomor WA, sosmed, copyright |
| `TestimonialCard` | Card ulasan pelanggan |
| `generateWAMessage(data)` | Fungsi util: terima data form → return URL `wa.me/...` |

---

## Routing Summary

```
/                        → Landing Page
/layanan                 → Pilih Layanan
/layanan/carikan-kos     → Form Layanan 1
/layanan/survei-pilihan  → Form Layanan 2
```

Tidak ada halaman konfirmasi, halaman rekomendasi, atau halaman pembayaran — semua dilanjutkan di WA.

---

## Urutan Implementasi

1. [ ] Setup project (Vite + React + Tailwind)
2. [ ] Buat komponen shared (Navbar, Footer)
3. [ ] Fungsi util `generateWAMessage`
4. [ ] Landing Page — semua section
5. [ ] Halaman Pilih Layanan
6. [ ] Form Layanan 1 (Carikan Kos) + redirect WA
7. [ ] Form Layanan 2 (Survei Pilihan) + redirect WA
8. [ ] Polish: responsif mobile, validasi form, animasi

---

## Fitur yang Diabaikan

- Dasbor Admin & Surveyor
- Backend / API
- Halaman rekomendasi kos (terjadi di WA)
- Halaman pembayaran QRIS (terjadi di WA)
- Halaman konfirmasi pesanan
- Autentikasi user
