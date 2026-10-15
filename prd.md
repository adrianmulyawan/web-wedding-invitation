# PRODUCT REQUIREMENTS DOCUMENT (PRD)
## Project Name: Retro Photobox & Interactive Guest Photostrip Wedding Invitation

- **Document Version:** 3.0 (Full Interactive Guest Studio Edition)
- **Date:** 15 Oktober 2026
- **Status:** Production Ready (Frontend SPA)
- **Tech Stack:** HTML5, Tailwind CSS v3, Vanilla JavaScript (ES6+), HTML5 Canvas 2D API, MediaDevices API, Web Audio API
- **Target Platform:** Mobile-First Responsive Web Application (iOS Safari & Android Chrome Optimized)

---

## 1. Executive Summary & Product Vision
Undangan pernikahan digital umumnya hanya bersifat satu arah (membaca teks dan menggulir layar secara vertikal). **Retro Photobox Wedding Invitation v3.0** mentransformasi undangan pernikahan menjadi pengalaman bilik foto analog (*90s Vintage Photobooth*) yang interaktif dan memberi kenang-kenangan nyata bagi tamu.

Melalui modul unggulan **Interactive Guest Photobox Studio**, setiap tamu undangan dapat menggunakan kamera HP mereka masing-masing untuk mengambil 3 pose foto layaknya di mesin photobox fisik, mengeditnya dengan filter warna retro, memilih warna kertas strip, menempelkan serta menggeser stiker (*drag-and-drop*), dan mencetak informasi resmi pernikahan (**Nama Pengantin, Tanggal, & Lokasi Acara**) tepat di bagian bawah strip fotobox. Hasil strip foto tersebut dapat langsung diunduh ke HP tamu sebagai souvenir digital beresolusi tinggi (`.PNG`) dan ditempelkan ke Papan Gabus (*Corkboard*) Buku Tamu.

---

## 2. Problem & Solution Matrix

| Masalah pada Undangan Digital Konvensional | Solusi pada Retro Photobox Wedding v3.0 |
| :--- | :--- |
| Tamu hanya *scroll* cepat untuk melihat tanggal & lokasi lalu menutup halaman. | Pengalaman *gamification* sejak halaman sampul (masukkan koin 1¢, cermin kamera langsung, efek blitz, dan suara mekanis mesin cetak). |
| Tamu tidak mendapatkan kenang-kenangan personal dari undangan digital. | **Guest Photobox Studio**: Tamu dapat foto 3 pose dari HP sendiri dan mengunduh *Photostrip Souvenir* (`.PNG`) lengkap dengan footer nama pengantin, tanggal, dan lokasi acara. |
| Buku tamu (RSVP) hanya berisi daftar teks komentar yang monoton. | **Corkboard Polaroid Guestbook**: Ucapan tamu tampil sebagai kartu polaroid fisik lengkap dengan *thumbnail* hasil strip fotobox mereka dan *washi tape*. |
| Kendala *broken link* atau blokir *autoplay* pada file audio eksternal (`.mp3`). | **Pure Web Audio Synthesizer**: Efek suara mekanis (koin, beep, shutter, printer) dan melodi musik latar dihasilkan secara prosedural via *Web Audio API* tanpa file eksternal. |

---

## 3. End-to-End User Flow

```text
[1. Landing / Cover Mesin Photobox 3D]
 ├─ Sistem membaca query URL `?to=Nama+Tamu` -> Ditampilkan pada Viewfinder & Caption Default Strip Tamu
 ├─ (Opsional) Klik "🪞 Cermin" -> Cek penampilan lewat kamera depan HP
 └─ Klik "🪙 Insert Coin & Cetak"
      │
      ▼
[2. Mechanical Transition Sequence]
 ├─ Animasi koin 1¢ masuk ke slot + SFX denting koin
 ├─ Hitung mundur 3.. 2.. 1.. CHEESE! + Efek Blitz putih + SFX Shutter Kamera
 └─ Transisi ke halaman utama + Animasi kertas Photostrip meluncur keluar dari celah printer + Musik Vinyl berputar
      │
      ▼
[3. Eksplorasi Undangan Photostrip (Frame 01 - 04)]
 ├─ Top Studio Bar: Tamu dapat mengganti filter foto pengantin & warna kertas undangan utama
 ├─ Frame 01: Foto pasangan (4 pose interaktif) + Kutipan Ayat (QS. Ar-Rum: 21)
 ├─ Frame 02: Profil Mempelai Pria & Wanita dalam bingkai Polaroid + Washi Tape
 ├─ Frame 03: Love Story dalam bentuk gulungan klise film 35mm horizontal
 └─ Frame 04: Flip-Clock Countdown real-time + Jadwal Akad/Resepsi + Google Maps & Copy Alamat
      │
      ▼
[4. Interactive Guest Photobox Studio (Frame 05 - Core Feature)]
 ├─ Step 1 (Capture): Tamu menyalakan kamera HP -> Pilih "⚡ Auto 3-Pose" (otomatis hitung mundur 3x beruntun) ATAU "📸 Jepret Slot" manual ATAU "🖼️ Pilih Galeri HP"
 ├─ Step 2 (Customize): Pilih Filter Foto Tamu, Warna Kertas Strip, Ketik Nama/Catatan, dan Tambah Stiker
 ├─ Step 3 (Canvas Manipulation): Geser posisi stiker (touch/mouse drag), perbesar/perkecil, atau hapus stiker langsung di atas pratinjau kanvas
 └─ Step 4 (Export & Share):
      ├─ Klik "⬇️ Unduh Strip Fotobox (.PNG)" -> File gambar tersimpan di galeri HP tamu
      └─ Klik "📌 Tempel ke Buku Tamu" -> Melampirkan strip foto ke formulir RSVP
      │
      ▼
[5. Wedding Gift & Corkboard Polaroid RSVP]
 ├─ Salin nomor rekening amplop digital (BCA / Mandiri)
 └─ Kirim form RSVP & Doa -> Kartu Polaroid baru (berisi ucapan + foto strip tamu) tertempel di Corkboard
```

---

## 4. Functional Requirements

### 4.1. Modul Cover: 3D Photobox Cabinet
- **FR-01 (URL Guest Parser):** Sistem membaca parameter `?to=` dari URL dan mengaplikasikannya secara otomatis pada layar *viewfinder* sampul, *input field* nama pada form RSVP, dan *caption* default pada strip fotobox tamu.
- **FR-02 (Live Mirror Viewfinder):** Tombol Cermin mengaktifkan `navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' } })` dengan transformasi cermin `scaleX(-1)`.
- **FR-03 (Coin & Shutter Sequence):** Menekan tombol mulai memicu animasi CSS `@keyframes coinDrop`, mengubah indikator LED dari *Ready* ke *Print*, memutar hitung mundur 3 detik dengan *beep* audio, dan menampilkan *flash overlay*.

### 4.2. Modul Utama: Undangan Photostrip (Frame 01 – 04)
- **FR-04 (Global Strip Theme & Filter):** Floating bar di bagian atas mengontrol kelas CSS pada kontainer `#photostrip-paper`:
  - **Filter Foto Pengantin:** `filter-warm`, `filter-bw`, `filter-sepia`, `filter-90s`.
  - **Tema Kertas Undangan:** `strip-ivory` (`#FCFBF7`), `strip-midnight` (`#1A1918`), `strip-pink` (`#F4E1E4`), `strip-sage` (`#DCE3D8`).
- **FR-05 (Couple Pose Switcher):** Tombol pada Frame 01 mengganti foto pasangan melalui *array* 4 foto disertai efek blitz kamera.
- **FR-06 (Real-time Flip-Clock Countdown):** Menghitung selisih waktu menuju target `2026-10-24T08:00:00+07:00` setiap 1000ms untuk menampilkan Hari, Jam, Menit, dan Detik.

### 4.3. Modul Spesial: Interactive Guest Photobox Studio (Frame 05)
Berjalan sepenuhnya di sisi klien (*Client-Side*) menggunakan **HTML5 MediaDevices API** dan **HTML5 Canvas 2D Context**.

#### A. Kamera & Input Foto Tamu (3 Slot Pose)
- **FR-07 (Multi-Method Capture):** Tamu memiliki 3 metode untuk mengisi 3 frame pose (`Pose 1`, `Pose 2`, `Pose 3`):
  1. **Auto 3-Pose Burst Mode (`startAuto3PoseBooth`):** Menjalankan siklus hitung mundur `3.. 2.. 1..` otomatis sebanyak 3 kali berturut-turut dengan jeda antar pose 650ms, menjepret *video frame* ke masing-masing slot secara berurutan.
  2. **Manual Single Slot Capture (`captureSinglePose`):** Menjepret kamera ke slot pose yang sedang aktif, lalu memindahkan indikator slot aktif ke pose berikutnya (`(activePoseSlot + 1) % 3`).
  3. **Gallery File Upload (`handleGalleryUpload`):** Membaca file gambar dari penyimpanan HP menggunakan `FileReader` (`readAsDataURL`) untuk dimasukkan ke slot yang sedang dipilih.
- **FR-08 (Camera Facing Switcher):** Mendukung pergantian kamera depan (`facingMode: 'user'`) dan kamera belakang (`facingMode: 'environment'`). Jika menggunakan kamera depan, hasil tangkapan kanvas otomatis dicerminkan secara horizontal (`ctx.scale(-1, 1)`) agar tidak terbalik.
- **FR-09 (Smart Center-Crop 4:3):** Saat mengambil gambar dari *stream* video maupun unggahan galeri, sistem otomatis melakukan pemotongan tengah (*center-crop*) ke rasio `4:3` (`800x600px`) agar proporsi wajah dan objek tidak terdistorsi di dalam frame strip.

#### B. Kustomisasi Filter, Kertas, & Stiker Interaktif
- **FR-10 (Guest Strip Photo Filter):** Mengaplikasikan filter warna pada ketiga foto tamu di dalam kanvas:
  - `none` (Normal)
  - `warm` (Warm Vintage)
  - `bw` (Classic B&W)
  - `sepia` (Sepia 90s + *color tint fallback* untuk kompatibilitas Safari iOS)
  - `rose` (Soft Rose + *pink tint fallback*)
- **FR-11 (Guest Strip Paper Color Palette):** Mengubah kombinasi warna latar kertas (`stripBgColor`), warna teks utama (`stripTextColor`), dan warna aksen garis/badge (`stripAccentColor`) dengan 5 preset: *Classic Ivory, Midnight Black, Dusty Rose, Matcha Sage,* dan *Retro Terracotta*.
- **FR-12 (Interactive Sticker Engine on Canvas):**
  - Tamu dapat menambahkan stiker emoji (`❤️`, `✨`, `🌸`, `🎀`, `🕶️`, `👑`, `🥂`, `💍`) maupun stiker *badge* tipografi (`Just Married`, `Team Pengantin`).
  - **Hit-Detection & Dragging:** Mendukung event `mousedown`/`mousemove` dan `touchstart`/`touchmove` (`passive: false`) dengan kalkulasi rasio skala kanvas sehingga tamu bisa menggeser posisi stiker dengan jari di layar HP.
  - **Scale & Delete Controls:** Tombol `➕ Besar` (`scale * 1.15`), `➖ Kecil` (`scale * 0.85`), dan `🗑️ Hapus` untuk memodifikasi stiker yang sedang dipilih.

#### C. Spesifikasi Tata Letak Kanvas & Footer Resmi Pernikahan
- **FR-13 (High-Resolution Strip Layout - `600x1620px`):**
  - **Header Strip (`Y: 0 - 75px`):** Teks kode film `ANALOG PHOTOBOX • 35MM` di kiri dan `#RAMA-SINTA-2026` di kanan.
  - **Body 3 Frame Pose (`Y: 76 - 1249px`):** 3 kotak foto berukuran `520x365px` dengan jarak (`gap`) `26px`, dilengkapi *backing* hitam dan cap tanggal kamera kuarsa `'26 10 24 01/02/03` berwarna oranye menyala (`#FF6B2B`) di sudut kanan bawah tiap foto.
  - **Official Wedding Footer (`Y: 1261 - 1620px`):** Bagian bawah strip fotobox wajib memuat informasi pernikahan sebagai berikut:
    1. Garis pembatas horizontal dekoratif.
    2. **Nama / Catatan Kustom Tamu:** Contoh `★ SAHABAT & KELUARGA ★` (dapat diedit langsung oleh tamu via input teks).
    3. **Sub-header:** *The Wedding Of* (Font: *Playfair Display Italic*).
    4. **Nama Pengantin:** **Rama & Sinta** (Font: *Playfair Display Bold 46px*).
    5. **Tanggal Pernikahan:** **SABTU, 24 OKTOBER 2026** (Font: *Courier Prime Bold 20px*).
    6. **Lokasi & Alamat Acara:** **📍 Glass House — The Dharmawangsa Jakarta** beserta alamat lengkap *Jl. Brawijaya Raya No. 26, Kebayoran Baru, Jakarta Selatan*.
    7. **Footer Stamp:** *OFFICIAL DIGITAL PHOTOBOX SOUVENIR • THANK YOU FOR CELEBRATING WITH US*.
- **FR-14 (Clean PNG Export):** Saat tombol **Unduh Strip Fotobox (.PNG)** ditekan, sistem merender ulang kanvas dengan parameter `hideSelectionRing = true` (menyembunyikan garis seleksi stiker), mengonversinya menjadi `data:image/png`, dan memicu unduhan otomatis bernama `Photobox_Rama_Sinta_[NamaTamu].png`.

### 4.4. Modul Corkboard Polaroid Guestbook & Amplop Digital
- **FR-15 (Photostrip-Attached Polaroid RSVP):** Saat tamu mengirim ucapan doa dan konfirmasi kehadiran, sistem secara otomatis menyertakan gambar *thumbnail* dari strip fotobox yang baru saja dibuat oleh tamu di dalam kartu Polaroid ucapan mereka di papan gabus.
- **FR-16 (One-Click Copy Bank Account):** Menyalin nomor rekening pengantin menggunakan `navigator.clipboard.writeText` disertai notifikasi *Toast* melayang.

---

## 5. Design System & Typography

### 5.1. Color Palette (Tailwind Custom Tokens)
| Token | Hex Code | Usage |
| :--- | :--- | :--- |
| `cream` | `#F6F1E7` | Papan nama mesin, teks utama di atas latar gelap |
| `ivory` | `#FCFBF7` | Warna dasar kertas photostrip & kartu polaroid |
| `terracotta` | `#B84A39` | Aksen utama, tombol CTA, cap/badge retro |
| `charcoal` | `#1F1D1A` | Latar belakang utama halaman, teks utama di atas kertas terang |
| `gold` | `#C99E5A` | Aksen bingkai mesin photobox, angka countdown, tombol unduh |
| `sage` | `#6E7E6A` | Opsi tema kertas photostrip *Matcha Sage* |
| `dustypink` | `#E6CCD2` | Opsi tema kertas photostrip *Dusty Pink* |

### 5.2. Typography Hierarchy
- **Heading / Nama Pengantin (`font-serif`):** *Playfair Display* — Memberikan kesan klasik, elegan, dan editorial.
- **Metadata / Timestamp / Controls (`font-mono`):** *Courier Prime* — Memberikan nuansa cetakan mesin tik analog, informasi klise film 35mm, dan tombol kontrol studio.
- **Ucapan Buku Tamu & Stiker Badge (`font-hand`):** *Caveat* — Menyerupai tulisan tangan dengan spidol di atas kertas Polaroid.
- **Body Text (`font-sans`):** *Plus Jakarta Sans* — Memastikan keterbacaan informasi lokasi dan panduan pengguna tetap tajam di layar mobile.

---

## 6. Technical Safeguards & Edge Cases
1. **Camera Permission Denied / In-App Browser Fallback:**
   - Jika tamu membuka tautan melalui *in-app browser* yang memblokir `getUserMedia` atau menolak akses kamera, aplikasi tidak akan berhenti (*crash*). Sistem menampilkan pesan ramah dan mengarahkan tamu untuk menggunakan tombol **"🖼️ Pilih Galeri HP"** atau tetap berkreasi dengan foto *default*.
2. **Mobile Safari Canvas Filter Fallback:**
   - Karena beberapa versi WebKit/Safari iOS tidak menerapkan `ctx.filter` pada elemen `<canvas>`, fungsi `renderGuestPhotostripCanvas()` dilengkapi lapisan *tint overlay* (`ctx.fillRect` dengan warna semi-transparan) agar filter *Sepia* dan *Soft Rose* tetap terekam pada hasil unduhan `.PNG`.
3. **Touch Scroll Locking Saat Drag Stiker:**
   - Event listener `touchstart` dan `touchmove` pada elemen `<canvas>` menggunakan `{ passive: false }` dan hanya memanggil `evt.preventDefault()` saat jari menyentuh area stiker. Dengan demikian, layar HP tidak ikut tergulir saat tamu menggeser stiker, tetapi tetap bisa di-*scroll* normal saat menyentuh area luar stiker.

---