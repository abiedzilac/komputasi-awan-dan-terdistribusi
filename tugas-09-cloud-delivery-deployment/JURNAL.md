# Jurnal Proses — Tugas 9

## Diskusi deployment model
- **Opsi yang dipertimbangkan dan kenapa akhirnya memilih salah satu:** 
  Kelompok kami sempat mempertimbangkan dua opsi utama, yaitu **Public Cloud murni** dan **Hybrid Cloud** (serta sempat menyinggung Private Cloud murni).
- **Public Cloud murni** awalnya dilirik karena menawarkan *rapid elasticity* dan skalabilitas tinggi yang sangat dibutuhkan untuk menghadapi lonjakan trafik musiman (seperti saat musim pemilu atau pendaftaran sekolah/PPDB). Namun, opsi ini akhirnya ditolak karena sistem menyimpan data yang sangat sensitif (NIK, alamat, dan data biometrik warga negara) yang dilindungi oleh UU Perlindungan Data Pribadi (UU PDP). Menempatkan seluruh data kependudukan di infrastruktur pihak ketiga membuat negara kehilangan kendali penuh atas keamanan dan kedaulatan datanya.
- **Hybrid Cloud** akhirnya dipilih sebagai solusi paling ideal. Dengan model ini, komponen inti dan database sensitif tetap disimpan di dalam **Private Cloud** yang dikelola penuh oleh pemerintah, sementara layanan pendukung yang bersifat publik dan rentan mengalami lonjakan beban (seperti portal pengecekan status dan antarmuka API akses instansi) ditempatkan di **Public Cloud** untuk mendapatkan elastisitas kapasitas tanpa mengorbankan kerahasiaan data inti.

## Diskusi service model per komponen
- **Database utama kependudukan & Penyimpanan data biometrik → IaaS (Infrastructure as a Service):**
  Dipilih IaaS karena kedua komponen ini menyimpan aset paling krusial dan sensitif. Dengan IaaS, tim infrastruktur pemerintah memiliki kendali penuh atas sistem operasi, konfigurasi jaringan (isolasi *subnet*/VPN), enkripsi penyimpanan tingkat rendah, serta kebijakan keamanan berlapis sesuai regulasi negara.
- **Aplikasi pengelolaan data, API, & Portal pengecekan status → PaaS (Platform as a Service):**
  Dipilih PaaS agar tim pengembang aplikasi pemerintah bisa fokus pada logika bisnis, integrasi layanan antar-instansi (Dukcapil, Kepolisian, Bank/KYC), dan fitur aplikasi tanpa terbebani urusan pemeliharaan server fisik maupun *patching* OS. Selain itu, PaaS mempermudah *auto-scaling* ketika akses portal pengecekan status melonjak drastis.
- **Email dan aplikasi kolaborasi internal → SaaS (Software as a Service):**
  Dipilih SaaS karena komponen ini hanya berfungsi sebagai alat komunikasi dan koordinasi operasional pegawai/admin antar-instansi, bukan tempat penyimpanan *master data* kependudukan. Menggunakan SaaS lebih efisien secara waktu dan biaya operasional karena siap pakai (*ready-to-use*).

## Log Penggunaan AI (Level 2)

> Wajib diisi sesuai kebijakan Level 2 di [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Tulis "Tidak memakai AI" pada baris pertama jika memang tidak dipakai. Hanya untuk brainstorming ide/outline — bukan untuk kode/analisis/teks akhir.

| Tanggal | Tool AI | Prompt yang diberikan | Ringkasan saran/ide AI | Bagaimana diolah jadi tulisan/kode sendiri |
|---|---|---|---|---|
| 2026-11-20 | ChatGPT (GPT) | Meminta bantuan menyusun struktur poin analisis service model, analisis risiko & mitigasi pada Hybrid Cloud, serta meminta perubahan format dari tabel ke bentuk paragraf/poin yang lebih mengalir. | AI memberikan kerangka pemetaan risiko utama pada arsitektur Hybrid Cloud (sinkronisasi data antar-segmen, keamanan API penghubung, dan stabilitas koneksi antar-cloud) beserta opsi format penyajian naratif. | Kelompok kami menyeleksi 3 risiko yang paling relevan dengan konteks pertukaran data kependudukan (NIK & biometrik), menyesuaikan istilah mitigasinya dengan kebutuhan akses instansi (Dukcapil/Bank/Kepolisian), dan menyusun ulang kalimatnya agar menyatu dengan keseluruhan laporan di `README.md`. |
