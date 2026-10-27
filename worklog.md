# Worklog

---
Task ID: 1
Agent: main (Z.ai)
Task: Membangun Platform Belajar Mekanisme Biokimia Sistem Indra interaktif (visualisasi 2D jalur sinyal saraf + kuis evaluasi otomatis)

Work Log:
- Klasifikasi tugas: Type 3 (Interactive Web Development) → memuat skill fullstack-dev, jalankan init script (Next.js 16 + Tailwind 4 + shadcn/ui + zustand + framer-motion).
- Membuat data materi `src/data/pathways.ts`: 6 jalur (Optik 10 langkah, Integumen 6, Olfaktori 11, Gustatori 11, Vestibular 10, Koklear 13 = 61 langkah) sesuai urutan kaskade dari materi user, lengkap dengan fase scene, molekul kunci, dan fakta.
- Membuat bank soal `src/data/quiz.ts`: 37 soal (5-6 per jalur + 6 soal integratif campuran) dengan pembahasan; fungsi buildExamSet (12 soal acak) dan shuffle.
- Store progres `src/lib/progress-store.ts` (zustand persist localStorage): progres langkah per jalur, jalur selesai, rekor skor kuis, reset.
- Kit scene `src/components/scenes/scene-kit.tsx` (SceneFrame, P phase-dim, Lbl, Ion, IonFlow SMIL, ZoomBox) + keyframes SVG di globals.css.
- 6 scene SVG animasi 2D: OptikScene (mata+rodopsin→all-trans, transducin/PDE/cGMP, kanal CNG tertutup, grafik hiperpolarisasi, vesikel glutamat), IntegumenScene (termometer 40°, gelombang panas, TRPV, ion Na/Ca, gauge potensial, AP berjalan), OlfaktoriScene (hidung, odorant, GPCR→Gα-GTP→ACIII→cAMP, CNG, Cl⁻ keluar, AP ke bulbus), GustatoriScene (lidah, T1R2+T1R3, PLCβ2, IP₃→ER→Ca²⁺, TRPM5, CALHM1/3→ATP→P2X), VestibularScene (lift naik beranimasi, otolith bergeser, striola, tip links tegang, MET, K⁺, VGCC, eksositosis), KoklearScene (gelombang suara, timpani bergetar, stapes, spiral koklea + gelombang perilimfe, organ Corti, MET, VGCC, glutamat).
- Komponen platform: FlowCascade (metro line 61 node auto-scroll), PathwayView (scene + caption fase + kontrol play/pause/restart/prev/next/kecepatan 0.5-2x + detail langkah + CTA selesai), PathwayGrid (kartu progres), HomeView (hero, statistik, cara pakai), QuizView (pilih mode, sesi soal dengan feedback instan + pembahasan, layar hasil + tinjauan + rekor skor).
- page.tsx: shell navigasi tab (Beranda/Jalur Indra/Kuis), AnimatePresence, footer sticky; layout.tsx metadata Indonesia + lang="id".
- Perbaikan lint: menghilangkan setState-in-effect (interval pakai idxRef, kuis auto-start via prop initialQuizId + remount nonce).
- **Bug kritis ditemukan & diperbaiki via Agent Browser**: `answered = selected !== null` membuat tombol "Periksa Jawaban" tergantikan begitu opsi dipilih → jawaban tak pernah terekam (skor selalu 0). Fix: pisahkan state `checked`; verified 5/5 = 100% di browser.
- Verifikasi Agent Browser end-to-end: beranda, kartu jalur, playback otomatis kaskade (langkah maju, fase berganti), CTA tuntas, kuis per jalur (skor benar), ujian campuran 12 soal, progres persist (2/6), responsif mobile 390px, footer sticky, tanpa error konsol.
- Aksesibilitas: aria-label pada tab ikon mobile, role/aria pada kaskade & kuis.
- Lint final: 0 error, 0 warning. dev.log bersih (200 OK).

Stage Summary:
- Aplikasi web berjalan di port 3000 (route `/`), semua fitur inti terverifikasi via browser.
- Artefak: src/data/{pathways,quiz}.ts, src/lib/progress-store.ts, src/components/scenes/* (7 file), src/components/platform/* (7 file), src/app/{page,layout,globals.css}.
- Keputusan penting: tema gelap ilmiah dengan aksen per jalur (violet/oranye/hijau/pink/teal/amber), ion berkode warna konsisten antar scene, animasi partikel SMIL + dim per fase.

---
Task ID: 2
Agent: main (Z.ai)
Task: Menjawab "Kenapa ngga ada file index.html" — membuat versi standalone index.html dari Platform Belajar Mekanisme Biokimia Sistem Indra

Work Log:
- konteks: sesi sebelumnya (Task 1) membangun platform sebagai aplikasi Next.js (tanpa index.html karena Next.js merender via server). User mengharapkan file index.html.
- Dibuat /home/z/my-project/download/index.html (92 KB, satu file mandiri, offline, tanpa dependensi eksternal): CSS + 6 scene SVG data-driven + mesin animasi + kuis + navigasi.
- 6 jalur: Optik (10 langkah, hiperpolarisasi/glutamat turun), Integumen (6, TRPV), Olfaktori (13, inset zoom silium, Cl- keluar unik), Gustatori (13, T1R2+T1R3→IP3→TRPM5→CALHM1/3→ATP→P2X), Vestibular (11, lift naik, tilt bundel via swap v-b0/v-b1), Koklear (17, M-I-S→organ Corti→eksositosis). Total 70 langkah sesuai urutan kaskade materi user.
- Mesin: highlight per langkah (data-step + class on/done/off), mover ion tween rAF sepanjang polyline, pulse, autoplay 3.4s/speed, kecepatan 1x/1.5x/2x, keyboard ←/→/Space, chip rantai reaksi klikabel, daftar langkah klikabel.
- Kuis: 26 soal (4-5 per jalur) + pembahasan; kuis campuran 10 soal acak & kuis per jalur 5 soal; feedback instan, hasil + grade A-E + tinjauan jawaban keliru + rekor localStorage.
- Validasi: node --check 3 blok skrip OK; uji headless browser (agent-browser): beranda, 6 scene pada langkah tengah, playback, kuis end-to-end sampai layar hasil (5/10 terdeteksi benar).
- Bug ditemukan & diperbaiki: (1) template literal v-defl vestibuular terputus (syntax error) — diganti ARW(); (2) typo p.name → pname.name di renderQ (kuis kosong); (3) label meluber kanvas (v-stim) dipindah end-anchor; (4) label tumpang tindih integumen/gustatori dirapikan; (5) id highlight yang hilang (o-ray, o-rho, o-pde, g-tast) ditambahkan.
- Next.js app dari Task 1 masih berjalan di port 3000 (HTTP 200) — kedua versi tersedia.

Stage Summary:
- Deliverable utama: /home/z/my-project/download/index.html (mandiri, buka langsung di browser).
- Deliverable cadangan: aplikasi Next.js di port 3000 (fitur sama, plus progres persist).
- Keputusan: satu file tanpa framework agar bisa dibuka offline; warna aksen per jalur konsisten dengan versi Next.js.
