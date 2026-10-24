NOVA AI V5 — PAKET AWAL BERBASIS V4
====================================

ISI PAKET
- index.html: antarmuka NOVA, riwayat lokal, konteks lokal, pencarian Wikipedia, retry/error handling.
- functions/api/chat.js: backend Cloudflare Pages yang memanggil OpenRouter tanpa membocorkan API key ke browser.
- .dev.vars.example: contoh nama variabel rahasia; tidak berisi key sungguhan.
- wrangler.toml: konfigurasi dasar Cloudflare Pages.

PENTING
1. Paket ini memakai Cloudflare Pages Functions. Jangan hanya membuka index.html lewat file manager/Chrome; endpoint /api/chat hanya tersedia setelah dipasang pada hosting yang menjalankan Functions.
2. Buat repository GitHub baru atau gunakan repository pribadi, unggah isi folder ini, lalu hubungkan repository ke Cloudflare Pages. Build command bisa dikosongkan; output directory gunakan . sesuai konfigurasi proyek. Pastikan fungsi `functions/api/chat.js` ikut ter-deploy.
3. Di Cloudflare Dashboard: Workers & Pages > proyek Pages > Settings > Variables and Secrets (atau pengaturan Environment Variables) > tambahkan `OPENROUTER_API_KEY` sebagai Secret, bukan plaintext variable. Masukkan key di sana. Jangan tulis key asli di HTML, JavaScript, GitHub, README, atau chat.
4. Tambahkan `OPENROUTER_MODEL` sebagai variable biasa jika ingin mengganti model. Default: `openrouter/free`; model dan ketersediaannya mengikuti OpenRouter.
5. Setelah menambahkan secret, redeploy proyek jika diminta. Tes situs melalui URL HTTPS hasil deployment.
6. Untuk penggunaan lokal dengan Wrangler, salin `.dev.vars.example` menjadi `.dev.vars`, isi key hanya di `.dev.vars`, dan jangan pernah mengunggah `.dev.vars`. File contoh ini sengaja tidak berisi key.

KEAMANAN DAN BIAYA
- API key tidak dikirim ke frontend. Namun siapa pun dapat menggunakan endpoint publik jika situs dibuka umum, sehingga pemilik key bisa menanggung biaya atau batas pemakaian.
- Sebelum publikasi luas, pasang pembatasan/rate limiting di Cloudflare (misalnya WAF rate limiting rules yang sesuai paket) dan pantau penggunaan OpenRouter. Backend ini membatasi ukuran dan jumlah pesan, tetapi bukan pengganti rate limiting berbasis pengguna/IP.
- Gunakan batas pengeluaran/pemberitahuan di OpenRouter jika tersedia untuk akun/model yang digunakan. Jangan menganggap model gratis selalu tersedia atau selalu tanpa batas.
- Riwayat chat disimpan di localStorage browser pengunjung. Tombol Hapus menghapus riwayat dan ringkasan lokal pada browser itu saja. Jangan masukkan data rahasia.

CATATAN MIGRASI
- Riwayat lokal menggunakan key `nova-chat-v4`, sehingga percakapan V4 yang tersimpan pada browser yang sama tetap dapat terbaca.
- Pencarian memakai Wikipedia dan dilakukan dari browser pengunjung. Hasil pencarian dikirim ke backend sebagai konteks untuk jawaban.
- Ini adalah peningkatan V4 dengan backend serverless; uji sebelum dibagikan publik.
