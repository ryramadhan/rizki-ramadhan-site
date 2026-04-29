# ryr.dev — Personal Portfolio

Portfolio pribadi dibangun dengan Astro JS + Tailwind CSS. Sebelumnya menggunakan React + Vite, di-rebuild ke Astro karena portfolio adalah *content-first website* — tidak butuh React runtime untuk menampilkan list project atau bio.

## Kenapa Astro?

- **Zero JS by default** — Hero, About, Projects dikirim sebagai pure HTML (0 KB JavaScript)
- **SSG** — HTML di-render di server, search engine langsung bisa baca konten tanpa menunggu hydration
- **Island Architecture** — JS hanya jalan di bagian yang memang butuh: Navbar (smooth scroll) dan Contact (form state)

| Komponen | JavaScript |
|----------|------------|
| Hero, About, Projects | ❌ None — static HTML |
| Navbar | ✅ Vanilla JS — smooth scroll, mobile hamburger toggle |
| Contact | ✅ Vanilla JS — form state |

## Tech Stack

- [Astro JS](https://astro.build) — Static site generation
- [Tailwind CSS](https://tailwindcss.com) — Styling
- Vanilla JS — Minimal interactivity
- Sharp — Image optimization

## Jalankan Lokal

```bash
npm install
npm run dev
```

Buka `http://localhost:4321`

## Struktur Project

```
src/
├── components/        # Navbar, Hero, About, Projects, Contact, Footer
├── content/           # Data & translations
├── layouts/           # Layout.astro
├── pages/             # index.astro
└── styles/            # Tailwind + dark theme
```

---

Built with Astro 🚀