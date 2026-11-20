# Tugas 9 (Pekan 9) — Cloud Delivery & Deployment Model

**Materi terkait:** Karakteristik esensial cloud computing (NIST SP 800-145), Deployment Model (Public/Private/Hybrid/Community), Service Model (IaaS/PaaS/SaaS).

> Bagian 2 mata kuliah ini murni **analisis & desain di atas kertas** — tidak ada biaya cloud sama sekali, karena tugas berhenti di tahap rekomendasi, bukan implementasi nyata (implementasi baru mulai Tugas 11).

## Studi Kasus

Pemerintah ingin membangun **Sistem Data Kependudukan** (nama, NIK, alamat, data biometrik) yang:
- Sangat **rahasia** (data pribadi warga negara, diatur UU Perlindungan Data Pribadi).
- Diakses oleh **ribuan instansi** (dukcapil daerah, kepolisian, bank untuk verifikasi KYC) dengan pola akses yang **tidak rata** (lonjakan saat musim pemilu/pendaftaran sekolah).
- Butuh **skalabilitas** agar tidak downtime saat lonjakan akses, tapi **tidak boleh** datanya berada di infrastruktur yang tidak terkendali penuh oleh negara.

## Tugas Kelompok

#### 1. **Rekomendasikan Deployment Model** (Public / Private / Hybrid / Community Cloud) yang paling tepat untuk sistem ini. Jelaskan alasannya berdasarkan **karakteristik NIST** (on-demand self-service, broad network access, resource pooling, rapid elasticity, measured service) dikaitkan dengan kebutuhan kerahasiaan vs skalabilitas.

Menurut kami, pilihan yang tepat  adalah Hybrid Cloud karena sistem harus menjaga kerahasiaan data kependudukan, tetapi tetap mampu menangani lonjakan akses dari ribuan instansi.
  **Rekomenadi Deployment Model : Hybrid Cloud**
Berdasarkan kebutuhan pada studi kasus, Hybrid Cloud merupakan model deployment yang paling sesuai untuk Sistem Data Kependudukan. Alasannya, sistem ini harus mampu menjaga kerahasiaan data pribadi warga negara, tetapi juga perlu menangani akses dari ribuan instansi yang jumlahnya bisa meningkat pada waktu tertentu.

Hybrid Cloud menggabungkan Private Cloud dan Public Cloud sehingga keduanya dapat digunakan sesuai kebutuhan. Dalam sistem ini, Private Cloud digunakan untuk menyimpan dan mengelola data utama yang bersifat sensitif, seperti NIK, alamat, dan data biometrik. Dengan begitu, pemerintah tetap memiliki kendali terhadap penyimpanan dan pengelolaan data tersebut.

Sementara itu, Public Cloud dapat dimanfaatkan untuk mendukung layanan yang membutuhkan kapasitas lebih fleksibel, misalnya portal pengecekan status atau komponen layanan lain yang tidak menyimpan data sensitif. Pembagian ini memungkinkan sistem menangani peningkatan jumlah pengguna tanpa harus menempatkan seluruh data kependudukan pada infrastruktur public cloud.

Pemilihan Hybrid Cloud juga dapat dikaitkan dengan lima karakteristik esensial cloud computing menurut NIST SP 800-145 sebagai berikut, 

| Karakteristik NIST | Penerapan pada Sistem Data Kependudukan |
|---|---|
| On-demand self-service |	Instansi yang memiliki kewenangan dapat menggunakan sumber daya komputasi sesuai kebutuhan tanpa harus selalu melalui proses permintaan manual kepada pengelola infrastruktur. |
| Broad network access |	Sistem dapat diakses oleh berbagai instansi, seperti Dukcapil daerah, kepolisian, dan bank melalui jaringan dengan mekanisme keamanan yang sesuai. |
| Resource pooling |	Sumber daya komputasi dapat dimanfaatkan bersama oleh berbagai instansi, tetapi hak akses setiap instansi tetap dibatasi sesuai kewenangannya. |
| Rapid elasticity |	Kapasitas layanan dapat ditambah atau dikurangi sesuai kebutuhan. Hal ini berguna ketika terjadi peningkatan akses, misalnya saat musim pemilu atau pendaftaran sekolah. |
| Measured service |	Penggunaan sumber daya seperti penyimpanan, jaringan, dan komputasi dapat dipantau dan diukur sehingga pemerintah dapat mengetahui kebutuhan kapasitas serta mengelola penggunaannya dengan lebih efisien. |

#### 2. **Rekomendasikan Service Model** (IaaS / PaaS / SaaS) yang tepat untuk komponen-komponen berbeda dari sistem ini (boleh berbeda service model untuk komponen berbeda, mis. database inti vs portal publik untuk pengecekan status).

Setiap komponen dalam Sistem Data Kependudukan memiliki kebutuhan yang berbeda, sehingga Service Model yang digunakan juga perlu disesuaikan. Bagian yang mengelola data sensitif membutuhkan kontrol dan keamanan yang lebih ketat, sedangkan bagian aplikasi yang diakses banyak pengguna perlu lebih mudah dikelola dan mampu menyesuaikan kapasitas saat terjadi peningkatan akses.

Kombinasi IaaS, PaaS, dan SaaS :

| Komponen Sistem | Service Model | Alasan |
|---|---|---|
| Database utama kependudukan | laaS | Memberikan kontrol lebih besar terhadap server, penyimpanan, jaringan, dan keamanan data sensitif. |
| Penyimpanan data biometrik | laaS | Membutuhkan pengelolaan infrastruktur dan perlindungan data yang ketat. |
| Aplikasi pengelolaan data dan API | PaaS | Memudahkan pengembangan dan pengelolaan aplikasi tanpa harus mengatur seluruh infrastruktur server. |
| Portal pengecekan status | PaaS | 	Memudahkan pengembangan serta penyesuaian kapasitas ketika jumlah pengguna meningkat. |
| Email dan aplikasi kolaborasi internal | SaaS | 	Dapat langsung digunakan tanpa perlu membangun dan mengelola aplikasi sendiri. |

#### 3. Jelaskan **minimal 2 risiko spesifik** dari pilihan kalian dan mitigasinya (mis. jika Hybrid Cloud dipilih: risiko sinkronisasi data antara private & public segment).

1. Sinkronisasi Data
Perbedaan waktu pembaruan antara Private Cloud dan Public Cloud dapat menyebabkan data yang ditampilkan tidak sesuai dengan kondisi terbaru. Untuk mengatasinya, diperlukan API yang terstandarisasi, validasi data, serta pemantauan proses sinkronisasi secara berkala.

2. Kebocoran Data melalui API
Koneksi antara Private Cloud dan Public Cloud berpotensi menjadi celah keamanan apabila tidak dilindungi dengan baik. Pihak yang tidak memiliki izin dapat mencoba mengakses data kependudukan melalui API. Mitigasinya adalah menerapkan autentikasi, pembatasan hak akses, enkripsi komunikasi, dan pencatatan aktivitas akses (audit log).

3. Gangguan Koneksi Antar-Cloud
Gangguan jaringan dapat menyebabkan komunikasi antara Private Cloud dan Public Cloud terhambat sehingga beberapa layanan tidak dapat diakses sementara. Untuk mengatasinya, perlu disediakan koneksi cadangan, pemantauan jaringan, dan mekanisme pemulihan layanan (failover).

#### 4. Bandingkan singkat dengan **satu alternatif yang kalian tolak** — kenapa alternatif itu kurang tepat (mis. kenapa Public Cloud murni ditolak, atau kenapa Private Cloud murni dianggap kurang skalabel).
Alternatif yang dipertimbangkan tetapi tidak dipilih adalah Public Cloud murni. Model ini sebenarnya memiliki keunggulan dalam hal fleksibilitas dan skalabilitas karena kapasitas sumber dayanya dapat disesuaikan dengan kebutuhan pengguna.

Namun, Public Cloud murni kurang sesuai untuk Sistem Data Kependudukan karena sistem ini menyimpan informasi yang sangat sensitif, seperti NIK, alamat, dan data biometrik warga negara. Jika seluruh data ditempatkan pada infrastruktur Public Cloud, pemerintah memiliki keterbatasan dalam mengendalikan infrastruktur secara langsung. Hal ini dapat meningkatkan risiko keamanan dan kebocoran data.

Oleh karena itu, Hybrid Cloud lebih dipilih karena data utama tetap disimpan dalam Private Cloud yang berada di bawah kendali negara, sedangkan Public Cloud digunakan untuk mendukung layanan yang membutuhkan fleksibilitas dan skalabilitas. Dengan demikian, kebutuhan keamanan dan kemampuan sistem dalam menangani lonjakan akses dapat tetap terpenuhi.
## Struktur Submission

```
tugas-09-cloud-delivery-deployment/
├── README.md      # Analisis lengkap (isi dengan poin 1-4 di atas)
├── JURNAL.md
└── bukti/          # (opsional) diagram pendukung
```

## Rubrik Penilaian (Tugas 9)

| Komponen | Bobot | Kriteria |
|---|---|---|
| Ketepatan rekomendasi deployment model | 30% | Argumen berbasis karakteristik NIST, bukan sekadar "private cloud lebih aman" tanpa detail |
| Ketepatan rekomendasi service model | 25% | Pemetaan komponen sistem ke IaaS/PaaS/SaaS masuk akal |
| Analisis risiko & mitigasi | 25% | Risiko spesifik ke konteks data kependudukan (bukan risiko generik) |
| Perbandingan dengan alternatif yang ditolak | 20% | Argumen penolakan jelas dan berdasar |

## Batasan Penggunaan AI (Level 2)

Kebijakan **Level 2 (AI Assisted Idea Generation & Structuring)** berlaku — lihat [`../RUBRIK-UMUM.md`](../RUBRIK-UMUM.md). Boleh bertanya ke AI gambaran umum karakteristik NIST/deployment model untuk brainstorming; **tidak boleh** meminta AI menuliskan rekomendasi/analisis akhir yang tinggal ditempel ke `README.md`. Catat pemakaian AI di "Log Penggunaan AI" pada `JURNAL.md`.

- Jawaban yang terasa seperti "template" (bisa dipakai untuk sistem cloud apa saja tanpa menyebut spesifik kependudukan/data pribadi/regulasi) akan dinilai rendah pada komponen ketepatan rekomendasi.
- `README.md` wajib membahas minimal satu skenario tandingan yang kalian buat sendiri (mis. "bagaimana jika instansi daerah tidak punya infrastruktur private cloud sendiri?") beserta jawabannya.
