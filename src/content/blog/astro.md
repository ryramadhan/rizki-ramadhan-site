---
title: "Kenapa Pakai Astro untuk Membangun Personal Blog"
description: "Dari React + Vite ke Astro — cerita di balik keputusan rebuild portfolio dan apa yang saya pelajari dari prosesnya."
pubDate: 2026-04-28
tags: ["astro", "react", "web performance", "belajar"]
---

Portfolio ini awalnya dibangun pakai **React + Vite**. Jalan, tampilannya oke, tidak ada yang rusak. Tapi waktu saya mulai mikirin soal performa dan SEO, saya sadar ada yang kurang tepat — saya pakai React untuk sesuatu yang sebetulnya tidak butuh React.

Ini cerita kenapa saya akhirnya rebuild pakai Astro.

## Masalah dengan React untuk Portfolio

React itu framework yang bagus. Tapi React dirancang untuk **aplikasi interaktif** — dashboard, editor, social media feed. Portfolio saya? Isinya cuma teks, gambar, dan satu form contact.

Masalahnya, meskipun halaman About saya tidak punya satu pun interaktivitas, React tetap mengirim seluruh bundle JavaScript ke browser. User harus download dulu, browser parse dulu, baru konten muncul.

```
Halaman About di React:
1. Browser download HTML kosong → <div id="root"></div>
2. Browser download bundle JS (~40KB+)
3. React "hydrate" semua elemen
4. Baru konten muncul

Padahal... halaman About cuma teks. Tidak ada yang interaktif.
```

## Apa itu Astro?

Astro adalah web framework yang filosofinya sederhana: **kirim sesedikit mungkin JavaScript ke browser.**

Secara default, komponen Astro di-render jadi HTML murni saat build time. Tidak ada React runtime, tidak ada hydration, tidak ada JavaScript sama sekali — kecuali kamu memang minta.

```astro
---
// Ini jalan di server saat build, BUKAN di browser
const nama = "Riski"
---

<!-- Hasilnya pure HTML, 0 KB JavaScript -->
<h1>Halo, saya {nama}</h1>
```

## SSG — Halaman Sudah Jadi Sebelum User Datang

Astro pakai pendekatan **SSG (Static Site Generation)** — semua halaman dibuat jadi file HTML statis saat kamu jalankan `npm run build`.

Analoginya seperti ini: kalau React itu masak di tempat setiap ada pesanan, Astro itu masak semua menu dari pagi — pas pelanggan datang tinggal kasih, tidak perlu nunggu.

Efeknya ke SEO juga langsung kerasa. Bot Google yang datang ke website React sering dapat halaman kosong karena kontennya baru muncul setelah JavaScript selesai jalan. Dengan Astro, bot langsung dapat HTML lengkap — konten langsung terindex.

## Island Architecture — JS Hanya di Tempat yang Butuh

Konsep paling menarik dari Astro namanya **Island Architecture**. Idenya simpel: tidak semua bagian website butuh JavaScript. Jadi kenapa harus kirim JS ke semua bagian?

Di portfolio saya, pembagiannya seperti ini:

| Komponen | JavaScript | Alasan |
|----------|------------|--------|
| Hero | ❌ Tidak ada | Pure HTML, cuma teks dan gambar |
| About | ❌ Tidak ada | Static content |
| Projects | ❌ Tidak ada | Daftar project, tidak interaktif |
| Navbar | ✅ Vanilla JS | Butuh smooth scroll handler |
| Contact | ✅ Vanilla JS | Form perlu handle state (idle/sent) |

Hasilnya? Hampir seluruh halaman adalah HTML murni. JavaScript hanya jalan di dua tempat yang memang butuh.

## Perbandingan Syntax — Tidak Beda Jauh

Waktu pertama lihat syntax Astro, saya kira bakal bingung. Ternyata kalau sudah kenal React, Astro tidak asing sama sekali.

**Komponen React:**
```jsx
export default function Card({ title, desc }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <p>{desc}</p>
    </div>
  )
}
```

**Komponen Astro:**
```astro
---
const { title, desc } = Astro.props
---

<div class="card">
  <h2>{title}</h2>
  <p>{desc}</p>
</div>
```

Yang berubah:
- Tidak ada `export default function`, tidak ada `return()`
- Logika JS ditulis di dalam blok `---` (frontmatter)
- `className` jadi `class` biasa
- Hasilnya HTML murni, bukan virtual DOM

## Kapan Sebaiknya Pakai Astro?

Astro cocok kalau website kamu lebih banyak **menampilkan konten** daripada menangani interaksi kompleks.

✅ **Pakai Astro untuk:**
- Portfolio dan personal site
- Blog dan website artikel
- Landing page
- Dokumentasi

❌ **Pertimbangkan framework lain untuk:**
- Aplikasi dengan banyak state global (dashboard, editor)
- Real-time features (chat, notifikasi live)
- Aplikasi yang sangat bergantung pada client-side routing

## Kesimpulan

Rebuild portfolio dari React ke Astro bukan karena React jelek — tapi karena **alat yang tepat untuk pekerjaan yang tepat**. Portfolio adalah content-first website, dan Astro memang dirancang untuk itu.

Yang paling saya suka dari proses ini: saya jadi benar-benar paham konsep SSG, hydration, dan island architecture — bukan cuma hafal istilahnya, tapi ngerti kenapa konsep itu ada.

Kalau kamu lagi bangun portfolio atau blog, coba pertimbangkan Astro. Kurva belajarnya tidak securam yang dikira, apalagi kalau sudah familiar dengan HTML dan sedikit JavaScript.