# ZYANV4 - Dark Purple Redesign (Vercel Static)

Landing page ZYANV4 dengan animasi scroll, 65 kamus JSON statis, dan URL per bahasa. Tidak membutuhkan PHP, API key, API translation, Node backend, atau build step saat deploy.

## Struktur untuk upload ke Vercel

```
index.html
vercel.json
locales/
    id.json
    en.json
    ja.json
    ms.json
    fil.json
    my.json
    ... (65 file JSON total)
```

**Upload seluruh struktur foldernya**, bukan hanya `index.html`. File `locales/*.json` harus tetap dalam folder `locales` pada root project.

## Deploy Vercel

1. Buat project baru di Vercel dari folder ini (Git repository atau Vercel CLI).
2. Framework Preset: `Other`. Tidak perlu perintah build.
3. Pastikan root project berisi `index.html` dan `vercel.json`, dan outputnya memakai root proyek (tanpa memindahkan `index.html` ke folder lain).
4. Deploy. URL `/` akan diarahkan ke `/ja`. URL bahasa seperti `/id`, `/en`, `/my`, dan `/ph` dapat langsung dibuka dan di-refresh.

Vercel memproses pengalihan dan rewrites dari `vercel.json`, lalu halaman mengambil kamus menggunakan `/locales/KODE.json` (GET). Perpindahan bahasa memperbarui URL dengan `history.pushState` tanpa reload; tombol Back/Forward mengambil bahasa dari URL.

**Perhatikan pemetaan khusus:**

| URL | Bahasa | File |
|---|---|---|
| `/ja` | Jepang - default | `ja.json` |
| `/id` | Indonesia | `id.json` |
| `/en` | Inggris | `en.json` |
| `/my` | Melayu/Malaysia | `ms.json` |
| `/ph` | Filipino/Filipina | `fil.json` |
| `/mm` | Myanmar/Burma | `my.json` |
| `/zh-cn` | Mandarin Sederhana | `zh-CN.json` |
| `/zh-tw` | Mandarin Tradisional | `zh-TW.json` |
| `/pt-br` | Portugis Brasil | `pt-BR.json` |
| `/es-mx` | Spanyol Meksiko | `es-MX.json` |

URL `/ms` diarahkan ke `/my`, `/fil` ke `/ph`, dan `/my-mm` ke `/mm`. Semua bahasa lain menggunakan kode bahasa kecil seperti `/ko`, `/ar`, `/fr`, dan sebagainya.

## Terjemahan

Setiap file bahasa memiliki **96 key**, mengikuti struktur `data-i18n` yang sudah ada. Untuk mengubah teks, edit value pada file JSON terkait dan deploy ulang. Contoh `locales/ja.json` untuk bahasa Jepang. Semua key terisi dan diuji bisa dimuat untuk 65 opsi. **Catatan mutu:** terjemahan awal pada sebagian bahasa (khususnya bahasa dengan penutur lebih sedikit) belum semuanya dilokalkan penuh: beberapa deskripsi produk masih memakai kalimat Inggris. Periksa file JSON sebelum dipakai secara resmi dalam pemasaran multibahasa. Kamus id, en, ja, zh-CN dari proyek sebelumnya dipertahankan; isi penting bahasa ms, fil, ko, zh-TW, th, vi, hi, ar, es, fr, de, pt-BR dan ru sudah diperluas.

Tidak ada permintaan terjemahan real-time, PHP, maupun API key. Browser hanya mengambil file JSON milik website sendiri dan memakai cache HTTP/Map.

## Animasi

Pada perangkat yang mendukung animasi: hero muncul halus saat halaman terbuka; kartu kategori, judul section, preset, produk editing, statistik, social, dan footer muncul saat berada di viewport (IntersectionObserver). Animasi diputar sekali per elemen. Jika user mengaktifkan `prefers-reduced-motion: reduce`, efek masuk dimatikan demi aksesibilitas. Tanpa JavaScript elemen tetap terlihat.

## Pengujian

- 65/65 file JSON ada, semua 96 key terisi.
- 65/65 bahasa lolos uji perubahan `lang` dan DOM dengan Chromium (mock fetch membaca JSON lokal).
- Pemetaan `/ja`, `/id`, `/en`, `/my`, `/ph`, `/mm`, `/ar`, `/zh-tw` lolos pemeriksaan client-side.
- Mobile 390px: tidak ada overflow horizontal.
- URL direct reload di deployment Vercel menggunakan konfigurasi rewrites; belum diuji lewat deployment Vercel sungguhan.

`index.html` masih menggunakan Google Fonts sebagai aset font melalui CDN. Tampilan tetap mempunyai fallback font bila CDN tidak tersedia. Gambar produk sudah tertanam langsung di HTML.

### Catatan SEO

Setiap path bahasa menyajikan HTML statis yang sama, lalu JavaScript mengganti teks. Ini bekerja untuk pengunjung, tetapi SEO multibahasa tingkat lanjut membutuhkan prerender/HTML terpisah per locale untuk metadata dan konten awal crawler.
