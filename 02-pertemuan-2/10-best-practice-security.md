# 10 - Hal-hal Penting untuk Vibecoders

Vibecoding memang sangat cepat, tapi kecepatan tanpa kontrol bisa berbahaya. Sebagai Vibecoder, Anda harus tetap memegang kendali atas kualitas kode. Berikut adalah panduan etika koding dengan AI.

## 1. Best Practice (Mencegah "Spaghetti Code")
AI sering kali memberikan solusi yang langsung jalan, tapi belum tentu rapi dalam jangka panjang. Sebagai arsitek, Anda harus menerapkan dua prinsip utama ini:

### A. Prinsip DRY (Don't Repeat Yourself)

```mermaid
graph LR
    subgraph "Tanpa DRY (Buruk)"
    A[Logic A] --> B[Controller 1]
    A2[Logic A] --> C[Controller 2]
    end
    subgraph "Dengan DRY (Bagus)"
    S[Service/Helper Logic A] --> D[Controller 1]
    S --> E[Controller 2]
    end
```

### B. Prinsip SOLID (Dasar Arsitektur Clean Code)

```mermaid
graph TD
    subgraph "SRP: Satu Class, Satu Tugas"
    C[Controller] --> |"Logic"| S1[RegisterService]
    C --> |"Logic"| S2[EmailService]
    C --> |"Logic"| S3[ImageService]
    end
```

- **Open/Closed**: Tambah fitur baru tanpa ngerusak kode lama.
- **Dependency Inversion**: Class tinggi gak boleh tergantung class rendah (pakai Interface).

### C. Pemisahan Komponen UI (Decomposition)
Jangan buat satu file HTML/Blade yang isinya ribuan baris. Pecah bagian-bagian kecil menjadi komponen agar mudah dikelola dan digunakan kembali.

```mermaid
graph TD
    subgraph "Monolithic (Buruk)"
    M[index.blade.php]
    end
    
    subgraph "Component-Based (Bagus)"
    I[index.blade.php] --> N[Navbar.blade.php]
    I --> H[Hero.blade.php]
    I --> C[CardProduct.blade.php]
    I --> F[Footer.blade.php]
    C --> B[Button.blade.php]
    end
```

> [!TIP]
> **Tugas Anda sebagai Vibecoder**: Saat melihat file tampilan sudah terlalu panjang, katakan ke AI: *"Tolong pecah section di file ini menjadi komponen Blade yang terpisah agar lebih modular."*

---

## 2. Security (Keamanan adalah Utama)

### Visualisasi: Authorization Check (Cek Kepemilikan)

```mermaid
sequenceDiagram
    participant U as User (ID: 5)
    participant L as Laravel/AI
    participant D as Database

    U->>L: GET /order/6 (Punya orang lain)
    L->>D: Ambil User_ID dari Order 6
    D-->>L: Order 6 milik User 10
    alt User_ID (5) != Owner_ID (10)
        L-->>U: 403 Forbidden (Ditolak)
    else Sama
        L-->>U: Tampilkan Data
    end
```

> [!CAUTION]
> **Jangan Asal Percaya**: AI mungkin lupa nambahin proteksi ini. Selalu double check kode controller Anda!

---

## 3. Optimization (Kinerja & Skalabilitas)
Aplikasi yang jalan tapi lemot akan ditinggalkan pengguna.

- **Database Query**: Cek apakah AI membuat query yang boros (seperti masalah **N+1 Query**). Minta AI menggunakan `eager loading` (contoh: `with('user')`).
- **Index Database**: Pastikan kolom yang sering dicari sudah diberi **Index** di migration.
- **Server-side vs Client-side**:
  - Gunakan **Server-side Rendering (Blade)** untuk halaman yang butuh SEO dan loading awal cepat.
  - Gunakan **Client-side Interactivity** untuk fitur dashboard yang butuh interaksi dinamis tanpa refresh.
- **Bom Waktu User**: Cek proses berat seperti looping besar. Sesuatu yang terasa cepat untuk 10 user bisa menjadi sangat lambat saat user mencapai 10.000.

---

## Visualisasi: Mentalitas Vibecoder

```mermaid
graph TD
    A[Generasi AI] --> B{Review Filter}
    B -- "Rapi?" --> C[Best Practice]
    B -- "Aman?" --> D[Security Check]
    B -- "Cepat?" --> E[Optimization]
    C & D & E --> F[Push ke Produksi]
```

> [!IMPORTANT]
> Pekerjaan Anda bukan lagi "menulis kode", tapi **"memastikan kode benar"**. Gunakan waktu yang Anda hemat dari mengetik manual untuk melakukan review keamanan dan optimasi.
