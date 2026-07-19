# 🛒 Harga Sembako (harga-sembako)

[![Live Demo](https://img.shields.io/badge/Live_Demo-hargasembako.live-success?style=for-the-badge)](https://hargasembako.live)
[![Next.js](https://img.shields.io/badge/Next.js-14+-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue?style=flat-square&logo=typescript)](https://www.typescriptlang.org)
[![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)](./LICENSE)

**Harga Sembako** adalah *smart aggregator* yang melacak dan memvisualisasikan fluktuasi harga bahan pangan pokok di Indonesia — diperbarui otomatis setiap hari, disajikan lewat peta interaktif dan grafik historis yang terasa hidup, bukan sekadar tabel data pemerintah yang kaku.

Dibangun dengan fokus mendalam pada **Visual Storytelling**, **Micro-interactions**, dan **performa yang tetap ramah di perangkat kelas menengah-bawah** — karena target penggunanya adalah masyarakat umum yang mayoritas mengakses lewat HP dengan koneksi yang tidak selalu stabil.

---

## ✨ Key Features

* **Peta Interaktif Indonesia + Dropdown Provinsi** — pilih provinsi lewat peta SVG yang bisa diklik, atau dropdown untuk aksesibilitas dan kenyamanan di layar kecil. Keduanya sinkron ke satu *state*.
* **"Peta Timbangan" — Signature Interaction** — memilih provinsi menganimasikan ikon timbangan pasar tradisional yang miring sesuai arah tren harga komoditas terpilih, menyatukan metafora pasar dengan data nyata.
* **Fluid UI/UX** — **Framer Motion** untuk transisi *layout* mulus (`layoutId`) dan umpan balik *spring physics*.
* **Scroll-Driven Historical Charts** — grafik tren harga digambar murni dengan SVG, dianimasikan berdasarkan *scroll* pengguna dengan **GSAP ScrollTrigger**.
* **Automated Daily Data Pipeline** — Vercel Cron Jobs menarik & membersihkan data harga setiap hari, dengan validasi ketat dan *fallback* otomatis ke data valid terakhir kalau sumber data bermasalah.
* **Aggressive Caching & Global State** — kombinasi **TanStack React Query** dan **Zustand** untuk perpindahan filter tanpa *lag*.
* **Accessible by Default** — dibangun di atas **Headless UI**, kompatibel *screen reader* dan navigasi *keyboard*.
* **Performance-First Motion** — elemen animasi/3D premium selalu *lazy-loaded*, menghormati `prefers-reduced-motion`, dan otomatis turun ke versi statis di koneksi lambat.

---

## 🛠️ Tech Stack

| Layer | Teknologi |
| --- | --- |
| **Core** | Next.js 14+ (App Router), React, TypeScript (strict) |
| **Styling** | Tailwind CSS, `clsx` + `tailwind-merge` |
| **Animasi** | Framer Motion (micro-interactions), GSAP (scroll-driven chart) |
| **Peta** | `react-simple-maps` + `topojson-client` |
| **Aksen Premium** | Rive (maskot/karakter ringan), opsional React Three Fiber untuk 3D asli |
| **State & Fetching** | Zustand (UI state), TanStack React Query (server state) |
| **Validasi** | Zod |
| **Database** | Supabase (PostgreSQL) |
| **Automation** | Vercel Cron Jobs |
| **Deployment** | Vercel |

---

## 🏗️ System Architecture

Sistem ini dirancang untuk **tidak membebani** sumber data pemerintah dan tahan terhadap perubahan struktur data sepihak:

1. **Cron Job** (`/api/cron/sync`) memicu pengambilan data harian, dijadwalkan mengikuti waktu *cutoff* data resmi (bukan asal tengah malam).
2. Data mentah divalidasi lewat **Zod**, dinormalisasi lewat lapisan *adapter* yang terisolasi, lalu di-*insert* ke tabel riwayat di **Supabase**. Kalau validasi gagal, sistem otomatis memakai data valid terakhir — bukan menampilkan data kosong/rusak.
3. **Client-side** aplikasi (pengguna) **hanya berkomunikasi dengan Route Handler internal**, tidak pernah langsung ke sumber data eksternal, dioptimasi dengan *caching* agresif yang disesuaikan kadensa sinkronisasi harian.

> 💬 *Data bersumber dari Panel Harga Pangan Badan Pangan Nasional (Bapanas), disinkronkan otomatis setiap hari. Aplikasi ini adalah proyek independen dan tidak berafiliasi dengan Bapanas.*

---

## 🚀 Local Development Setup

### 1. Clone & Install

```bash
git clone https://github.com/username-lu/harga-sembako.git
cd harga-sembako
npm install
```

### 2. Environment Variables

Buat `.env.local` di root proyek:

```bash
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
CRON_SECRET=
```

### 3. Jalankan

```bash
npm run dev
```

Buka [http://localhost:3000](http://localhost:3000).

---

## 📁 Struktur Proyek (ringkas)

```
src/
├── app/                # Routing & Route Handlers (App Router)
├── features/           # Unit fitur mandiri: prices, chart, map-filter, data-sync, hero-motion
├── components/         # UI primitives lintas fitur
├── lib/                # Utils bersama (format, query keys, supabase client)
├── store/              # Zustand slices global
└── styles/globals.css  # Design tokens (warna, radius, shadow)
```

Detail lengkap konvensi folder, sistem desain, dan aturan kode ada di [`agent-guide.md`](guide/agent-guide.md).

---

## 🗺️ Roadmap

- [x] **Fase 1 (MVP):** harga terkini per provinsi, peta + dropdown, historis 7–30 hari, sync harian otomatis.
- [ ] **Fase 2:** Programmatic SEO (OG image dinamis), analytics ringan, maskot/aksen 3D, native affiliate monetization.
- [ ] **Fase 3:** migrasi VPS, predictive analytics, PWA + push notification, modul crowdsourcing harga.

---

## 🤝 Contributing

Proyek ini masih dikembangkan solo, tapi kontribusi terbuka. Sebelum membuka PR, baca `agent-guide.md` untuk memahami konvensi kode dan sistem desain yang dipakai — ini menjaga konsistensi codebase baik untuk kontributor manusia maupun AI coding agent.

## 📄 License

MIT — bebas dipakai, dimodifikasi, dan disebarluaskan dengan atribusi.
