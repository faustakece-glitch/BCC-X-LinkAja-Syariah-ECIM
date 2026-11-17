# Simulasi Monte Carlo Pertumbuhan Pengguna LinkAja Syariah 2027 sampai 2032

Proyeksi pengguna aktif bulanan (MAU) LinkAja Syariah dalam dua kondisi: **tanpa solusi (baseline)** dan **dengan tiga pilar solusi Nava by LinkAja**. Keduanya dihitung dengan simulasi Monte Carlo 10.000 putaran. Proyek ini adalah bagian analisis kelayakan pada Jakarta Business Case Competition (JBCC) 2026 dengan klien LinkAja.

## Ringkasan

| | Do nothing | Dengan solusi Nava | Selisih |
|---|---|---|---|
| MAU 2032 (juta, median) | 1,08 | 1,55 | +0,40 juta (median berpasangan) |
| Pertumbuhan MAU per tahun (CAGR Des 2026 sampai 2032, median) | 4,3% (P10 sampai P90: 2,4% sampai 6,4%) | 10,7% (7,8% sampai 13,6%) | +6,4 poin per tahun |
| Total pertumbuhan MAU 2032 vs Des 2026 | +26% | +75% (51% sampai 101%) | |
| Kenaikan MAU 2032 dibanding do nothing | tidak ada | | **+39%** (rentang P10 sampai P90: 19% sampai 56%) |
| Pengguna terdaftar 2032 | 12,2 juta | 13,5 juta | +10,5%, sekitar 1,3 juta |

Cara membaca: rentang P10 sampai P90 mencakup 80% hasil simulasi. Angka adalah hasil model berdasarkan asumsi yang tertulis di dokumen ini, bukan prediksi pasti dan bukan angka resmi LinkAja. Angka yang paling dapat dipertahankan adalah persentase kenaikan dan kelipatan, bukan level MAU absolut, karena MAU Syariah saat ini tidak dipublikasikan.

![Do nothing vs solusi](output/solusi/grafik_utama.png)

Grafik menunjukkan MAU dengan indeks Desember 2026 = 100 per simulasi, agar ketidakpastian level awal yang sama di kedua dunia tidak mengaburkan efek solusi. Garis adalah median dan pita adalah P10 sampai P90. Pada indeks ini P10 solusi berada di atas P90 do nothing di setiap tahun (2032: 151 melawan 141). Sebelum 2027, garis historis adalah regresi saturasi eksponensial (R2 0,99, hanya 5 titik) pada pengguna terdaftar, dengan asumsi rasio aktif tetap karena data MAU historis Syariah tidak tersedia; periode 2023 sampai 2025 adalah estimasi, bukan data. Detail ada di Bagian 2.4.

Dokumen ini terdiri dari dua bagian. **Bagian 1** menjelaskan pembuatan baseline: alasan, faktor, driver, probabilitas, bentuk matematika, dan hasil. **Bagian 2** menjelaskan bagaimana tiga pilar solusi mempengaruhi faktor-faktor tersebut dan seberapa besar pengaruhnya.

---

# Bagian 1. Baseline: Kondisi Tanpa Solusi

## 1.1 Alasan dan tujuan baseline

Baseline adalah gambaran masa depan jika LinkAja Syariah tetap berjalan seperti sekarang di dalam aplikasi LinkAja, dengan pemasaran rutin, tanpa aplikasi Nava, KOL, aktivasi offline, maupun program loyalitas. Fungsinya sebagai pembanding: dampak solusi dihitung sebagai selisih antara kondisi dengan solusi dan kondisi baseline pada simulasi yang sama.

Baseline dibuat sebagai distribusi, bukan satu angka, karena tiga alasan:

1. **Data publik tidak lengkap.** MAU LinkAja Syariah tidak dipublikasikan, dan pengguna terdaftar Syariah untuk 2023 sampai 2025 tidak ditemukan di sumber publik.
2. **Masa depan tidak pasti.** Kompetisi e-wallet, kondisi ekonomi, regulasi, dan gangguan layanan dapat mengubah lintasan.
3. **Pertumbuhan sudah melandai.** Pengguna terdaftar Syariah tercatat 1,6 juta (akhir 2020), 5,8 juta (November 2021), 7,5 juta (Juli 2022), dan lebih dari 9 juta (Agustus 2026). Dari 2022 ke 2026 pertumbuhannya sekitar 4,5% sampai 6,9% per tahun (hitungan sendiri dari angka tersebut). Pola cepat lalu melandai ini sesuai dengan kurva S pada teori difusi inovasi (Bass, 1969).

Hasil baseline pada Bagian 1 dilaporkan dengan batas ekstrem P5 dan P95. Perbandingan dengan solusi pada Bagian 2 memakai P10 dan P90 agar pita tidak terlalu lebar. Tiga garis baseline:

| Garis | Arti | Dasar pengambilan |
|---|---|---|
| **P50** | Median, dipakai sebagai baseline | Kelanjutan tren dengan pertumbuhan yang terus melandai |
| **P5** | Batas bawah | Hanya 5% simulasi berada di bawahnya |
| **P95** | Batas atas | Hanya 5% simulasi berada di atasnya |

Metrik utama adalah **MAU**, karena yang dinilai adalah orang yang benar-benar memakai layanan. Pengguna terdaftar adalah stok kumulatif yang memuat banyak akun tidur. Sebagai gambaran, MAU LinkAja total sekitar 5 juta dari lebih dari 92 juta terdaftar, atau sekitar 5,4%.

## 1.2 Mengapa Monte Carlo

Metode Monte Carlo menaksir distribusi hasil dengan menjalankan model berulang kali memakai bilangan acak (Metropolis & Ulam, 1949). Metode ini cocok di sini karena ketidakpastian datang dari banyak sumber sekaligus (titik awal, laju pertumbuhan, makro, kompetisi, kejadian khusus) dan sebagian besar tidak punya deret waktu cukup untuk diestimasi secara statistik klasik. Setiap putaran menarik satu dunia yang mungkin, dan 10.000 dunia itu membentuk sebaran hasil.

Model Bass penuh tidak diestimasi karena hanya tersedia lima titik data untuk tiga parameter, yang rawan overfitting. Sebagai gantinya, laju pertumbuhan dibuat meluruh secara eksponensial menuju laju jangka panjang, yang meniru bentuk kurva S tanpa memaksa estimasi yang tidak didukung data.

## 1.3 Faktor yang dipertimbangkan

| Faktor | Mengapa penting | Jangkar data |
|---|---|---|
| Titik awal (pengguna terdaftar dan rasio aktif) | Menentukan level, dan rasio aktif paling menentukan level MAU | Kasus: 10 juta+; Kontan Agustus 2026: 9 juta+; rasio aktif LinkAja total 5,4% dan Muslim Pro 12,5% (5 juta aktif dari 40 juta terdaftar) |
| Pertumbuhan pengguna terdaftar | Menentukan kolam pengguna yang bisa menjadi aktif | Riwayat Syariah 4,5% sampai 6,9% per tahun; basis LinkAja total sekitar 1,5% per tahun (87 juta pada 2022 ke 92 juta pada 2026) |
| Pertumbuhan pengguna aktif | Metrik utama | MAU LinkAja total sekitar +5% (2026); MAU Syariah naik hampir 20% pada 2024 |
| Tekanan kompetisi e-wallet | Menekan pertumbuhan terdaftar dan MAU | LinkAja tertinggal dari ShopeePay, GoPay, DANA, OVO pada survei merek |
| Kondisi makro (konsumsi rumah tangga) | Mempengaruhi frekuensi transaksi | Pertumbuhan konsumsi rumah tangga 4,35% sampai 5,11% per kuartal (2022 sampai 2026), simpangan baku 0,24 poin |
| Musiman Ramadan | Lonjakan transaksi pada bulan tertentu | CEO memproyeksikan volume transaksi naik sekitar 10% bulan ke bulan pada Ramadan dan Lebaran 2026 |
| Kejadian khusus | Gangguan layanan, insiden keamanan, perubahan regulasi, fitur baru pesaing | Perluasan kebijakan MDR QRIS 0% mulai Oktober 2026 |
| Noise bulanan | Variasi acak yang tidak terjelaskan | Asumsi |

## 1.4 Driver, rentang, dan dasarnya

Setiap driver diberi label **FAKTA** (angka terpublikasi), **TURUNAN** (hitungan dari angka terpublikasi), atau **ASUMSI** (pertimbangan tim tanpa data langsung). Rentang berbentuk (minimum / modus / maksimum).

| Driver | Arti | Rentang | Label | Dasar |
|---|---|---|---|---|
| R0 | Pengguna terdaftar awal, Desember 2026 (juta) | 9,5 / 10,0 / 10,8 | ASUMSI | Mencakup dua sumber yang berbeda (9 juta dan 10 juta) |
| a0 | Rasio MAU terhadap terdaftar awal | 5% / 8% / 12,5% | ASUMSI | Batas bawah LinkAja total; batas atas Muslim Pro |
| g1 | Pertumbuhan terdaftar tahun 1 | 3% / 5,5% / 8% | TURUNAN | Riwayat 2022 ke 2026 |
| g_inf | Pertumbuhan terdaftar jangka panjang | 0,5% / 1,5% / 2,5% | TURUNAN | Laju basis LinkAja total |
| lam_g | Kecepatan peluruhan g | seragam 0,2 sampai 0,6 | ASUMSI | Tanpa data langsung |
| h1 | Pertumbuhan MAU tahun 1 | 3% / 8% / 15% | TURUNAN | MAU LinkAja total +5%; MAU Syariah +20% (2024) |
| h_inf | Pertumbuhan MAU jangka panjang | 0% / 2% / 4% | ASUMSI | Tanpa data langsung |
| mu_h | Kecepatan peluruhan h | seragam 0,2 sampai 0,7 | ASUMSI | Tanpa data langsung |
| kappa | Perubahan tekanan kompetisi (positif = memburuk) | -10% / 0 / +25% | ASUMSI | Modus 0 agar median sama dengan kelanjutan tren; ekor memburuk lebih panjang |
| beta | Sensitivitas MAU terhadap simpangan konsumsi | 0 / 1 / 3 | ASUMSI | Tanpa data langsung |
| A | Amplitudo lonjakan Ramadan | 2% / 6% / 12% | ASUMSI | Proyeksi transaksi +10% oleh CEO, dampak ke MAU diasumsikan lebih kecil |
| shock_rate | Kejadian khusus per tahun | 0,1 / 0,3 / 0,6 | ASUMSI | Tanpa data langsung |
| sigma_idio | Noise bulanan MAU | 0,5% / 1% / 2% | ASUMSI | Tanpa data langsung |

**Simpangan baku makro** berasal dari data: sepuluh observasi pertumbuhan konsumsi rumah tangga kuartalan 2022 sampai 2026 (rentang 4,35% sampai 5,11%) memberi simpangan baku 0,24 poin persentase, dibulatkan konservatif ke 0,30. Pada kondisi normal makro hampir tidak menggerakkan hasil. Ia berpengaruh besar hanya lewat resesi konsumsi.

Untuk parameter Syariah tidak ada deret waktu yang cukup untuk menaksir simpangan baku. Karena itu sebarannya berasal dari rentang yang terobservasi, bukan dari simpangan baku hasil estimasi.

## 1.5 Probabilitas dan sumber keacakan

| Sumber keacakan | Distribusi | Alasan pemilihan | Rujukan |
|---|---|---|---|
| Parameter dengan estimasi tiga titik | Segitiga (minimum, modus, maksimum) | Cocok bila hanya ada rentang dan nilai paling mungkin, tanpa data sampel | Malcolm dkk. (1959) |
| Parameter peluruhan | Seragam | Tidak ada informasi tentang nilai yang lebih mungkin | Prinsip ketidakpedulian |
| Pendaftar baru bulanan | Lognormal, rata-rata 1, simpangan baku 0,20 | Selalu positif dan bersifat multiplikatif | Praktik umum |
| Simpangan konsumsi makro | AR(1) dengan rho 0,5, inovasi Student t (derajat bebas 5) | Pertumbuhan konsumsi cenderung kembali ke rata-rata; Student t memberi ekor gemuk untuk guncangan besar | Praktik umum |
| Resesi konsumsi | Bernoulli, peluang 3% per tahun, penurunan seragam 2 sampai 4 poin | Kejadian langka tapi berdampak besar | Asumsi |
| Kejadian khusus | Proses Poisson, ukuran pemangkasan seragam 2% sampai 10%, pulih sekitar 12 bulan | Kejadian diskrit yang datang acak dalam waktu | Merton (1976) |
| Noise bulanan | AR(1) normal dengan phi 0,7 | Variasi yang bertahan beberapa bulan, bukan loncatan acak murni | Praktik umum |

Seed acak adalah 2026 sehingga hasil dapat direproduksi. Pengaruh tiap driver terhadap hasil diukur dengan korelasi peringkat Spearman, mengikuti praktik analisis sensitivitas global (Saltelli dkk., 2008).

## 1.6 Bentuk matematika

Waktu berjalan bulanan, t = 1 sampai 72 (Januari 2027 sampai Desember 2032), tahun ke-k = ceil(t/12).

```
(1) Laju pertumbuhan terdaftar:  g_k = g_inf + (g1 - g_inf) · exp(-lam_g · (k-1))
(2) Pengguna terdaftar:          R_t = R_(t-1) · [1 + r_t · e_t]
                                 r_t = (1 + g_k · (1 - kappa))^(1/12) - 1
                                 e_t ~ Lognormal(rata-rata 1, sigma 0,20)

(3) Laju pertumbuhan tren MAU:   h_k = h_inf + (h1 - h_inf) · exp(-mu_h · (k-1))
(4) Simpangan konsumsi:          d_k = rho · d_(k-1) + sigma_m · z_k - 1[resesi] · U(2%; 4%)
                                 z_k ~ Student t(5) dinormalkan, rho = 0,5, sigma_m = 0,30 poin
(5) Tren MAU:                    Tren_t = Tren_(t-1) · (1 + h_t)
                                 h_t = (1 + h_k·(1 - kappa) + beta · d_k)^(1/12) - 1

(6) Kejadian khusus:             S_t = S_(t-1) + (1 - S_(t-1))/12
                                 peluang 1 - exp(-shock_rate/12) per bulan: S_t <- S_t · (1 - u), u ~ U(2%; 10%)
(7) Noise bulanan:               eta_t = 0,7 · eta_(t-1) + sigma_idio · z_t,  z_t ~ N(0,1)
(8) Musiman Ramadan:             Phi_t = 1 + A · w_t   (w_t = fraksi hari bulan t yang jatuh pada Ramadan)
(9) MAU teramati:                MAU_t = min( Tren_t · S_t · Phi_t · exp(eta_t) ; 0,35 · R_t )

Kondisi awal: R_0 ~ Segitiga(9,5; 10,0; 10,8) juta,  Tren_0 = a0 · R_0
```

Struktur modelnya memisahkan komponen: pengguna terdaftar (1 sampai 2) tidak dipengaruhi makro, karena pendaftaran bersumber dari ekosistem BUMN. Makro dan kompetisi mempengaruhi MAU lewat laju tren (3 sampai 5). Kejadian khusus, noise, dan Ramadan hanya menggeser MAU yang teramati (6 sampai 9), bukan tren dasarnya. Dengan begitu tren dasar tetap bisa dibaca bersih, sementara lonjakan Ramadan tetap terlihat pada pola bulanan.

## 1.7 Hasil baseline

![Proyeksi baseline MAU](output/baseline/grafik_proyeksi_MAU.png)

Garis tebal biru adalah P50 tanpa musiman, garis emas tipis adalah P50 dengan lonjakan Ramadan, bayangan adalah P5 sampai P95, dan blok emas adalah bulan Ramadan.

**MAU rata-rata tahunan (juta)**

| Tahun | P5 | P50 (baseline) | P95 |
|---|---|---|---|
| 2027 | 0,63 | 0,88 | 1,18 |
| 2028 | 0,67 | 0,93 | 1,26 |
| 2029 | 0,70 | 0,98 | 1,33 |
| 2030 | 0,72 | 1,02 | 1,39 |
| 2031 | 0,74 | 1,05 | 1,44 |
| 2032 | 0,75 | 1,08 | 1,49 |

**Pengguna terdaftar posisi Desember (juta)**

| Tahun | P5 | P50 | P95 |
|---|---|---|---|
| 2027 | 10,16 | 10,61 | 11,16 |
| 2028 | 10,51 | 11,04 | 11,67 |
| 2029 | 10,77 | 11,39 | 12,11 |
| 2030 | 10,99 | 11,69 | 12,51 |
| 2031 | 11,19 | 11,95 | 12,87 |
| 2032 | 11,36 | 12,19 | 13,22 |

**Kelipatan MAU 2032 terhadap MAU awal** (dihitung per simulasi, sehingga tidak bergantung pada tebakan level awal): P5 1,11x, P50 1,27x, P95 1,47x, setara pertumbuhan sekitar 2,0%, 4,4%, dan 7,2% per tahun.

![Lintasan simulasi](output/baseline/grafik_lintasan_simulasi.png)

![Histogram MAU 2032](output/baseline/histogram_MAU_2032.png)

## 1.8 Kondisi bullish dan bearish

Bullish dan bearish didefinisikan secara konkret sebagai simulasi yang berada di ekor P95 dan P5 dari kelipatan MAU 2032. Tabel berikut menunjukkan rata-rata tiap driver pada kedua ekor tersebut.

| Driver | Bearish (P5 ke bawah) | Semua simulasi | Bullish (P95 ke atas) |
|---|---|---|---|
| Pertumbuhan MAU tahun 1 | 6,0% | 8,7% | 12,3% |
| Kecepatan peluruhan pertumbuhan | cepat (0,50) | sedang (0,45) | lambat (0,30) |
| Perubahan tekanan kompetisi (positif = memburuk) | +7,4% | +4,9% | +0,6% |
| Jumlah kejadian khusus dalam 6 tahun | 2,6 | 2,0 | 1,6 |
| Efek makro kumulatif pada pertumbuhan MAU (jumlah 6 tahun, poin persentase) | -7,3 | -1,2 | +0,3 |

**Bearish** terjadi ketika momentum MAU cepat habis, ada resesi konsumsi, kompetisi e-wallet memanas, dan gangguan datang beruntun. **Bullish** terjadi ketika momentum tahun pertama menyerupai lonjakan 2024, tidak ada resesi, posisi kompetitif bertahan, dan hampir tidak ada gangguan.

## 1.9 Pengujian baseline

**Uji kewajaran terhadap jangkar data** (keluaran `tabel_kewajaran_baseline.csv`):

| Ukuran | P10 | P50 | P90 | Jangkar |
|---|---|---|---|---|
| Pertumbuhan terdaftar 2027 | 3,8% | 5,2% | 6,7% | Riwayat 4,5% sampai 6,9% per tahun |
| Pertumbuhan tren MAU 2027 | 1,4% | 7,1% | 11,7% | MAU LinkAja total +5%; MAU Syariah +20% (2024) |
| Pangsa Syariah dari basis LinkAja 2032 | 11,4% | 12,1% | 12,9% | 8,6% (2022) dan 10,9% (2026) |

Sekitar 65% simulasi menghasilkan pertumbuhan terdaftar 2027 di dalam rentang riwayat. Median pertumbuhan MAU 2027 (sekitar 7%) berada di bawah kenaikan 2024 (sekitar 20%) karena pertumbuhan 2024 terjadi sebelum pelandaian; ini membuat baseline cenderung konservatif.

**Uji konvergensi.** Lima seed berbeda menghasilkan persentil baseline yang selisih relatifnya kurang dari 0,7%, sehingga 10.000 putaran cukup stabil (jalankan dengan opsi `--uji`).

## 1.10 Keterbatasan baseline

1. **MAU awal Syariah adalah asumsi.** Level MAU 2032 sangat bergantung pada rasio aktif awal. Itu sebabnya kelipatan dan persentase dipakai sebagai ukuran utama.
2. **Dua sumber saling tidak cocok** (9 juta dan 10 juta), dan data 2023 sampai 2025 tidak tersedia.
3. **Sebagian besar driver berlabel ASUMSI** dan pengaruhnya kecil menurut analisis sensitivitas, tetapi tetap harus dibuka sebagai asumsi.
4. **Tidak ada back-test formal**, karena hanya dua titik historis yang andal (2022 dan 2026).
5. **Tanggal Ramadan adalah perkiraan** dengan galat sekitar 2 hari, cukup untuk resolusi bulanan.

---

# Bagian 2. Implementasi Tiga Pilar Solusi

## 2.1 Posisi strategis dan batas yang dipakai

Tiga pilar solusi adalah (1) aplikasi Nava by LinkAja yang berdiri sendiri, (2) aktivasi awareness online dan offline lewat KOL, QRIS masjid, dan sponsor festival, serta (3) loyalty lewat Quest, Widget, dan Muhasabah.

Secara strategis, tujuannya memindahkan LinkAja Syariah dari kuadran **question mark** (pasar tumbuh cepat, pangsa kecil) menuju **star** (pasar tumbuh cepat, pangsa besar) pada matriks pertumbuhan dan pangsa BCG (Henderson, 1970). Argumennya: nilai sebuah produk bergantung pada kemampuan meraih pangsa terdepan sebelum pertumbuhan pasar melambat (BCG). Pasarnya memang tumbuh cepat: volume transaksi pembayaran digital naik 40,36% secara tahunan pada Agustus 2026 dan transaksi QRIS naik 67,22% (Bank Indonesia, dikutip Kontan, 2026). Posisi pangsa LinkAja Syariah masih kecil: hanya 10% responden survei internal yang tahu ada layanan Syariah padahal 70% mengenal LinkAja.

**Batas atas yang dipakai.** Rasio aktif solusi boleh mendekati rasio aplikasi Muslim terbaik di kategorinya, yaitu Muslim Pro dengan 12,5% (sekitar 5 juta aktif dari 40 juta terdaftar di Indonesia, Bisnis.com 2025), tetapi tidak melampauinya. Pada hasil, median rasio aktif 2032 berada di 12,4% dan batas 12,5% aktif pada sekitar 49% simulasi. Alasannya:

1. Menjadi star berarti menyamai pemimpin kategori, bukan melampauinya tanpa bukti.
2. Nava menggabungkan kebiasaan ibadah harian (waktu sholat, Al-Quran) dengan pembayaran, sehingga menyamai rasio aplikasi ibadah murni adalah target yang masuk akal.
3. Batas ini berasal dari satu pembanding, jadi diperlakukan sebagai plafon ambisi dan bukan sebagai prediksi.

## 2.2 Pilar, faktor yang dipengaruhi, dan besarnya

Setiap pilar menggeser faktor tertentu pada model. Besarnya diberi sebagai (minimum / modus / maksimum) dan berlaku penuh setelah efek terbangun.

| Pilar | Faktor model yang dipengaruhi | Besar efek |
|---|---|---|
| **1. Aplikasi Nava** | Rasio aktif terhadap terdaftar (pengali MAU), M1 | 1,05 / 1,20 / 1,45x |
| 1. Aplikasi Nava | Tambahan terdaftar dari toko aplikasi, U1 | 0 (sengaja konservatif) |
| **2. Aktivasi (KOL, masjid, festival)** | Tambahan pengguna terdaftar, U2 | 3% / 9% / 20% |
| 2. Aktivasi | Keaktifan pengguna baru relatif terhadap rata-rata, q | 0,5 / 0,8 / 1,0 |
| **3. Loyalty** | Retensi, pengali MAU, M3 | 1,02 / 1,08 / 1,18x |
| 3. Loyalty | Retensi yang menumpuk: tambahan pertumbuhan MAU per tahun, G3 (asumsi, belum ada data) | 0% / 1% / 2,5% per tahun |
| Semua pilar | Tumpang tindih (pengali efek gabungan), omega | seragam 0,7 sampai 1,0 |
| Semua pilar | Waktu efek terbangun: pilar 1 / 2 / 3 (bulan) | seragam 8 sampai 18 / 6 sampai 15 / 12 sampai 24 |

### Pilar 1: aplikasi Nava, mempengaruhi rasio aktif

Aplikasi standalone yang ringan dan bebas iklan mengganggu menangani tiga keluhan utama pada aplikasi Muslim saat ini: iklan mengganggu (78%), fitur terpecah (70%), dan UI buruk (46%), menurut survei internal (n=50, mahasiswa Gen Z, bersifat indikatif). Pengaruhnya dimodelkan sebagai pengali rasio aktif, karena aplikasi yang lebih mudah dipakai mengubah pendaftar menjadi pengguna aktif (Davis, 1989, tentang kemudahan penggunaan dan pemakaian). Plafonnya rasio Muslim Pro, yaitu sekitar 1,56 kali rasio awal 8%. Pengali modus 1,20x hanya mengambil sekitar sepertiga dari ruang menuju plafon itu.

### Pilar 2: aktivasi, mempengaruhi pengguna terdaftar

Celah awareness adalah dasar utamanya: 70% responden mengenal LinkAja tetapi hanya 10% tahu ada layanan Syariah. Kanal yang dipakai:

- **KOL.** Jajak pendapat Tirto terhadap 1.238 responden usia 16 sampai 45 tahun menunjukkan 66,48% percaya atau sangat percaya pada informasi influencer.
- **QRIS masjid.** Terdapat 317.461 masjid terdaftar di SIMAS Kementerian Agama (per 28 Agustus 2026). Sekretaris Jenderal DMI menyatakan masjid yang memakai QRIS untuk ZISWAF dapat meningkatkan pendapatan hingga 10 kali lipat (Fortune IDN). Ini pernyataan pejabat organisasi, bukan studi, sehingga hanya dipakai sebagai indikasi arah.
- **Festival.** Jakarta Fair Kemayoran memiliki 6,1 juta pengunjung dalam 32 hari, dan survei Populix (2025) menemukan 78% responden menghadiri acara langsung demi hiburan dan pengalaman baru.

Besarnya dikalibrasi dengan pangsa basis: setiap tambahan 1 poin pangsa opt-in dari basis LinkAja (sekitar 100 juta pada 2032) setara sekitar 1 juta pengguna terdaftar, atau sekitar 9% dari pengguna terdaftar Syariah. Modus +9% setara menambah sekitar 1 poin pangsa.

### Pilar 3: loyalty, mempengaruhi retensi

Tinjauan empiris gamifikasi menemukan bahwa mayoritas studi melaporkan efek positif, tetapi besarnya sangat bergantung pada konteks dan pengguna (Hamari dkk., 2014). Data NP Digital (2024) menunjukkan penambahan progress bar menaikkan retensi dari 35% ke 48%, sedangkan elemen reward saja hanya dari 34% ke 36%. Karena itu kekuatan Quest ada pada misi yang punya progres, bukan pada poin. Duolingo melaporkan bahwa pengguna widget memiliki retensi jauh lebih baik bahkan setelah mengendalikan fakta bahwa mereka lebih berkomitmen sejak awal (blog Duolingo, tanpa angka).

Hasil NP Digital berasal dari agensi dan Duolingo dari blog perusahaan, bukan penelitian peer review. Karena itu efek pilar 3 sengaja dibuat jauh lebih kecil daripada angka sumbernya: modus pengali MAU hanya 1,08x.

**Retensi yang menumpuk (G3).** Pengali M3 hanya menggeser level. Bila Quest, Widget, dan Muhasabah mengurangi pengguna yang berhenti memakai aplikasi, selisihnya menumpuk tiap tahun, sehingga pertumbuhan Nava tetap di atas do nothing setelah efek level selesai. Ini dimodelkan sebagai tambahan pertumbuhan MAU per tahun (segitiga 0% / 1% / 2,5%) yang berjalan sejak efek pilar 3 terbangun dan berhenti bekerja bila pilar 3 tidak berjalan. **Besar G3 adalah asumsi tim: tidak ada data retensi LinkAja maupun studi yang bisa dijadikan kalibrasi langsung.** Batas bawah 0% berarti retensi bisa saja tidak menumpuk. Efek ini tetap dibatasi plafon rasio aktif 12,5%.

## 2.3 Bentuk matematika solusi

Efek solusi diterapkan pada **lintasan simulasi baseline yang sama** (bilangan acak yang sama), sehingga selisihnya murni berasal dari solusi, bukan dari keberuntungan acak.

```
Rampa efek pilar i:        r_i(t) = 1 - exp(-t / tau_i),   t = 1..72 bulan, tau_i ~ Seragam
Terdaftar dengan solusi:   R'_t   = R_t · (1 + U2 · r_2(t) + U1 · r_1(t))
Faktor pilar 1:            f1(t)  = 1 + (M1 - 1) · r_1(t)
Faktor pengguna baru:      f2(t)  = 1 + q · (U2 · r_2(t) + U1 · r_1(t))
Faktor pilar 3:            f3(t)  = (1 + (M3 - 1) · r_3(t)) · (1 + G3)^( r_3(t) · t/12 )     (retensi menumpuk)
Kenaikan MAU:              L_t    = omega · ( f1(t) · f2(t) · f3(t) - 1 )      (omega = 1 bila hanya satu pilar aktif)
Batas rasio aktif:         c_t    = min( max(0,125 ; MAU_t / R_t) ; 0,35 )
MAU dengan solusi:         MAU'_t = min( MAU_t · (1 + L_t) ; c_t · R'_t )

M1, U1, U2, q, M3, G3 ~ Segitiga(min, modus, maks);  omega ~ Seragam
```

Efek dibangun bertahap karena adopsi tidak instan: kampanye butuh waktu untuk menjangkau, dan kebiasaan butuh waktu untuk terbentuk. Pilar 3 dibuat paling lambat karena loyalty baru bekerja setelah ada pengguna dan kebiasaan. Faktor tumpang tindih omega mengakui bahwa efek ketiga pilar tidak sepenuhnya saling menambah, karena menyasar pengguna yang sama.

## 2.4 Hasil

Seluruh perbandingan memakai P10 sampai P90 (80% hasil simulasi) dan, pada grafik, MAU berindeks Desember 2026 = 100 per simulasi.

![Do nothing vs solusi](output/solusi/grafik_utama.png)

**MAU berindeks (Des 2026 = 100)**

| Tahun | Do nothing P10 | Do nothing P50 | Do nothing P90 | Solusi P10 | Solusi P50 | Solusi P90 | P10 solusi di atas P90 do nothing |
|---|---|---|---|---|---|---|---|
| 2027 | 100 | 104 | 106 | 112 | 118 | 124 | ya |
| 2028 | 104 | 110 | 116 | 128 | 140 | 154 | ya |
| 2029 | 107 | 115 | 123 | 137 | 153 | 171 | ya |
| 2030 | 110 | 119 | 130 | 143 | 162 | 184 | ya |
| 2031 | 112 | 123 | 136 | 148 | 169 | 193 | ya |
| 2032 | 114 | 126 | 141 | 151 | 175 | 201 | ya |

Pada simulasi yang sama, solusi menghasilkan MAU lebih tinggi daripada do nothing di 100% dari 10.000 simulasi pada setiap tahun.

**Lintasan acak.** Enam puluh dunia acak yang sama digambar dua kali: tanpa solusi (kiri) dan dengan solusi (kanan). Garis putus-putus adalah P10 dan P90, garis tebal adalah median.

![Lintasan acak](output/solusi/grafik_lintasan_acak.png)

**MAU rata-rata tahunan (juta)**

| Tahun | Do nothing P10 | Do nothing P50 | Do nothing P90 | Solusi P10 | Solusi P50 | Solusi P90 |
|---|---|---|---|---|---|---|
| 2027 | 0,68 | 0,88 | 1,12 | 0,78 | 1,00 | 1,28 |
| 2028 | 0,72 | 0,93 | 1,19 | 0,92 | 1,21 | 1,47 |
| 2029 | 0,75 | 0,98 | 1,25 | 1,02 | 1,34 | 1,57 |
| 2030 | 0,78 | 1,02 | 1,31 | 1,08 | 1,43 | 1,64 |
| 2031 | 0,80 | 1,05 | 1,36 | 1,13 | 1,50 | 1,70 |
| 2032 | 0,82 | 1,08 | 1,40 | 1,18 | 1,55 | 1,74 |

Pada level absolut, pita do nothing dan solusi masih sedikit saling menimpa karena level awal MAU Syariah tidak diketahui dan berlaku sama di kedua dunia. Itu sebabnya perbandingan utama memakai indeks dan simulasi berpasangan.

**Kenaikan MAU terhadap do nothing pada simulasi yang sama (median, rentang P10 sampai P90)**

| Tahun | Kenaikan |
|---|---|
| 2027 | +13% (9 sampai 19) |
| 2028 | +27% (18 sampai 39) |
| 2029 | +33% (20 sampai 47) |
| 2030 | +36% (21 sampai 52) |
| 2031 | +38% (20 sampai 54) |
| 2032 | +39% (19 sampai 56) |

![Kenaikan dari waktu ke waktu](output/solusi/grafik_kenaikan_per_waktu.png)

**Ringkasan 2032**

| Ukuran | Do nothing | Dengan solusi |
|---|---|---|
| MAU, median (juta) | 1,08 | 1,55 |
| Tambahan MAU, median berpasangan (juta) | | 0,40 |
| Pertumbuhan MAU per tahun (CAGR Des 2026 sampai 2032) | 4,3% | 10,7% |
| Pertumbuhan MAU tahun 1 | | 18,2% |
| Pengguna terdaftar (juta) | 12,2 | 13,5 (+10,5%, tambahan 1,28 juta) |
| Pangsa Syariah dari basis LinkAja | 12,1% | 13,4% |
| Rasio aktif terhadap terdaftar | 9,4% | 12,4% |
| Simulasi yang kena batas rasio aktif 12,5% | | 49% |
| Peluang MAU solusi melewati P90 do nothing | | 70% |

Catatan cara membaca: "tambahan MAU" dan "kenaikan" dihitung sebagai median selisih per simulasi (berpasangan), yaitu ukuran dampak yang bersih dari keberuntungan acak. Pada grafik utama angka adalah rata-rata tahunan 2032, sedangkan pada grafik lintasan angka adalah bulan Desember 2032, sehingga selisihnya sekitar satu poin indeks.

![Distribusi MAU 2032](output/solusi/grafik_distribusi_2032.png)

Histogram kanan menunjukkan sebaran kenaikan pada simulasi yang sama. Kenaikan terendah di seluruh simulasi masih sekitar 3%, dan kenaikan P10 sebesar 19%.

## 2.4a Pertumbuhan per tahun dan cara membaca "+39%"

Angka +39% adalah selisih terhadap do nothing, bukan pertumbuhan Nava itu sendiri. Pertumbuhan Nava terhadap kondisi hari ini lebih besar: MAU naik sekitar 75% dari Desember 2026 ke 2032, atau sekitar 10,7% per tahun, melawan 26% atau 4,3% per tahun tanpa solusi (Tabel 12). Sebagai pembanding, pertumbuhan 7% per tahun selama 6 tahun menghasilkan +50%, dan selama 7 tahun +60%.

| Tahun | Do nothing (median) | Dengan Nava (median) | Dengan Nava (P10 sampai P90) |
|---|---|---|---|
| 2027 | 3,8% | 17,5% | 11,8% sampai 24,1% |
| 2028 | 6,1% | 19,1% | 12,1% sampai 25,6% |
| 2029 | 4,9% | 9,5% | 4,6% sampai 14,4% |
| 2030 | 3,9% | 5,6% | 2,5% sampai 10,1% |
| 2031 | 3,3% | 3,8% | 1,6% sampai 8,0% |
| 2032 | 2,8% | 2,9% | 1,0% sampai 6,8% |

Pertumbuhan tahunan adalah perubahan MAU rata-rata tahunan terhadap tahun sebelumnya; 2027 dibandingkan dengan MAU Desember 2026. CAGR 2027 sampai 2032 saja (tanpa tahun loncatan pertama) adalah 8,3% untuk Nava dan 4,0% untuk do nothing.

**Bentuk pertumbuhan.** Pertumbuhan Nava paling besar pada 2027 sampai 2028 (sekitar 18% sampai 19%), saat efek pilar 1 dan 2 terbangun, lalu melandai. Retensi yang menumpuk (G3, Bagian 2.2) membuat Nava tetap tumbuh lebih cepat daripada do nothing pada 2030 sampai 2031 (5,6% dan 3,8% melawan 3,9% dan 3,3%), tetapi pada 2032 selisihnya hampir hilang (2,9% melawan 2,8%). Penyebabnya: efek pilar 1 dan 2 adalah kenaikan level sekali jalan, dan plafon rasio aktif 12,5% sudah menahan hasil pada sekitar 49% simulasi pada 2032. Plafon itu berasal dari satu pembanding (Muslim Pro) dan melonggarkannya adalah keputusan tim, bukan hasil data.

**Audit asumsi yang tampak konservatif** (Tabel 15, satu asumsi diubah per baris, median kenaikan 2032 acuan 39,7%): melonggarkan batas rasio aktif ke 15% menjadi 44,1%, tanpa batas rasio aktif 45,6%; menghapus diskon tumpang tindih pilar 44,8%; memberi lintas jual dari basis LinkAja (U1) 41,7%; ketiganya sekaligus 56,5%. Ketiga pelonggaran itu belum punya data pendukung, sehingga angka acuan tetap dipakai. Angka acuan dapat diganti bila LinkAja memberi data MAU Syariah, retensi, atau hasil uji lintas jual.

**Titik impas biaya** (Tabel 14). Nava menambah sekitar 1,8 juta tahun-pengguna selama 2027 sampai 2032 (median; P10 sampai P90: 1,2 sampai 2,5 juta). Pertanyaan "apakah biaya sepadan" dijawab dengan titik impas, bukan persentase kenaikan: pada biaya total Rp25 miliar (angka ilustrasi, bukan biaya proposal), tiap MAU tambahan harus menghasilkan pendapatan bersih sekitar Rp14 ribu per tahun (median). Level MAU absolut bergantung pada asumsi MAU awal, jadi titik impas harus dihitung ulang dengan angka klien.

## 2.5 Kontribusi tiap pilar

![Kontribusi pilar](output/solusi/grafik_kontribusi_pilar.png)

Kenaikan MAU 2032 bila satu pilar berjalan sendiri (median, rentang P10 sampai P90):

| Pilar | Kenaikan MAU 2032 |
|---|---|
| Pilar 1, aplikasi Nava | 19,5% (8 sampai 32) |
| Pilar 2, aktivasi | 8% (5 sampai 12) |
| Pilar 3, loyalty (termasuk retensi menumpuk) | 15% (7 sampai 21) |
| **Gabungan** (setelah tumpang tindih) | **39%** (19 sampai 56) |

Pilar 1 memberi kontribusi terbesar, karena ia menggeser rasio aktif untuk seluruh basis pengguna, sedangkan pilar 2 hanya menambah pengguna baru dan pilar 3 menaikkan retensi (efeknya meningkat setelah retensi menumpuk, dari 8% menjadi 15%). Jumlah ketiganya sedikit di atas efek gabungan karena tumpang tindih dan batas rasio aktif.

**Sensitivitas** (korelasi peringkat Spearman dengan kenaikan MAU 2032): pengali rasio aktif pilar 1 sebesar 0,39, tambahan pengguna pilar 2 sebesar 0,24, faktor tumpang tindih 0,22, retensi menumpuk pilar 3 (G3) sebesar 0,17, pengali level pilar 3 sebesar 0,16, dan keaktifan pengguna baru 0,06. Waktu efek terbangun hampir tidak berpengaruh pada hasil akhir 2032, tetapi mempengaruhi seberapa cepat dampak terlihat.

## 2.6 Uji ketangguhan

1. **Terhadap kasus baik do nothing.** Median MAU 2032 dengan solusi (1,55 juta) berada di atas P90 do nothing (1,40 juta), dan pada indeks, P10 solusi (151) berada di atas P90 do nothing (141) di setiap tahun. Jadi keberatan "baseline terlalu rendah" tidak menggugurkan argumen. Pada level absolut, P10 solusi (1,18 juta) hanya sedikit di atas median do nothing (1,08 juta), karena level awal yang tidak pasti berlaku di kedua dunia.
2. **Pangsa dari basis LinkAja** 13,4% pada 2032 menambah sekitar 2,5 poin dari 10,9% pada 2026. Pada baseline, pangsa itu 12,1%.
3. **Pertumbuhan MAU tahun pertama** sebesar 18,2% masih di bawah kenaikan MAU Syariah 2024 (hampir 20%).
4. **Konvergensi.** Lima seed berbeda menghasilkan kenaikan median dengan selisih relatif sekitar 0,5% (jalankan dengan `--uji`).

Titik yang dapat dipertanyakan: batas rasio aktif 12,5% aktif pada sekitar 49% simulasi, artinya pada sebagian dunia hasil solusi dibatasi oleh patokan Muslim Pro. Batas itu berasal dari satu data pembanding.

## 2.7 Peluang solusi berhasil

Hasil di atas menjawab "berapa dampaknya bila pilar berjalan seperti dirancang". Pertanyaan yang berbeda adalah "berapa peluang solusi ini benar-benar berhasil". Jawabannya perlu memasukkan kemungkinan sebuah pilar tidak berjalan sama sekali, misalnya aplikasi sepi unduhan atau kampanye tidak menghasilkan pendaftar.

**Metode.** Peluang dihitung dari simulasi yang sama dalam dua lapis:

1. **Ketidakpastian besar efek.** Tiap pilar punya rentang efek (Bagian 2.2). Lapis ini sudah ada pada seluruh hasil sebelumnya.
2. **Risiko eksekusi.** Pada tiap simulasi, tiap pilar gagal total (efeknya nol) dengan peluang awal f. Pilar 3 hanya berjalan bila aplikasi (pilar 1) berjalan, karena Quest, Widget, dan Muhasabah berada di dalam aplikasi Nava.

Peluang berhasil adalah persentase dari 10.000 simulasi yang memenuhi definisi berhasil (galat simulasi sekitar 0,5 poin persentase). Besar efek pilar tidak diubah untuk mendapatkan angka tertentu; yang diubah hanya rancangan eksekusi, dan itu dilaporkan terpisah.

**Definisi berhasil** (dilaporkan bersamaan agar tidak bergantung pada satu ambang): lebih tinggi dari do nothing pada simulasi yang sama; kenaikan MAU 2032 minimal 10%, 20%, atau 30%; dan MAU 2032 melewati P90 do nothing (mengalahkan kasus terbaik status quo).

**Nilai f.** Tidak ada data kegagalan pilar untuk LinkAja, sehingga f disajikan sebagai rentang 0% sampai 40% dan angka utama dipakai pada 30%. Rujukan kasarnya adalah literatur peluncuran produk baru: klaim populer bahwa 80 sampai 90% produk baru gagal disebut "urban legend" oleh makalah di *Journal of Product Innovation Management*, yang menaksir kegagalan sekitar 40% (dikutip blog Oregon State PACE), dan estimasi yang lebih hati-hati berada di sekitar 25% sampai 45% (McTague, 2023, blog); survei Tenet 2026 melaporkan 28% peluncuran produk tidak memenuhi harapan manajemen. Semuanya berasal dari produk konsumen dan sumber sekunder, bukan aplikasi keuangan, sehingga hanya dipakai sebagai kisaran, bukan kalibrasi.

### Tanpa mitigasi

Setiap pilar berjalan satu kali dan gagal total dengan peluang f, saling bebas.

| Peluang tiap pilar gagal | Lebih tinggi dari do nothing | Kenaikan minimal 10% | Kenaikan minimal 20% | Kenaikan minimal 30% | Melewati P90 do nothing | Kenaikan median |
|---|---|---|---|---|---|---|
| 0% | 100% | 98% | 89% | 75% | 70% | +39,4% |
| 10% | 99% | 89% | 77% | 61% | 61% | +34,8% |
| 20% | 95% | 81% | 66% | 49% | 54% | +29,5% |
| **30% (angka utama)** | **90%** | **72%** | **56%** | **38%** | **46%** | **+24,0%** |
| 40% | 83% | 62% | 46% | 29% | 40% | +16,8% |

### Dengan mitigasi

Rancangan eksekusi yang dimasukkan ke proposal mengubah struktur risikonya:

- **Pilar 1, uji coba bertahap.** Aplikasi diluncurkan sebagai uji coba dengan kriteria lanjut atau berhenti. Bila peluncuran pertama gagal, aplikasi diperbaiki dan diluncurkan ulang satu kali. Asumsi: peluncuran ulang gagal lagi dengan peluang 50% (bukan 30%, karena penyebab kegagalannya mungkin sama), dan efek pilar 1 serta 3 tertunda 9 bulan.
- **Pilar 2 dan 3, kanal paralel.** Aktivasi memakai tiga kelompok kanal yang saling lepas (KOL online, QRIS masjid, festival), dan loyalty memakai tiga fitur (Quest, Widget, Muhasabah). Pilar gagal total hanya bila ketiganya gagal, ditambah peluang 10% semuanya gagal karena sebab bersama (misalnya proposisi nilai yang lemah). Dengan f awal 30%, peluang pilar gagal menjadi sekitar 12%.

Mitigasi ini hanya sah bila benar-benar ada di rencana implementasi proposal (uji coba dengan kriteria berhenti, anggaran untuk peluncuran ulang, lebih dari satu kanal). Ketiga angkanya (10%, 50%, 9 bulan) adalah asumsi tim, bukan data. Bukti pendukungnya berupa pola umum: perusahaan yang memakai riset dan proses bertahap lebih jarang gagal (24% pada 20 perusahaan makanan terbesar dibanding 88% pada perusahaan kecil, Blackburn, dikutip PACE), tetapi itu produk makanan dan bukan kalibrasi untuk aplikasi.

| Peluang pilar gagal awal | Lebih tinggi dari do nothing | Kenaikan minimal 10% | Kenaikan minimal 20% | Kenaikan minimal 30% | Melewati P90 do nothing | Kenaikan median | Target dengan keyakinan 80% |
|---|---|---|---|---|---|---|---|
| 20% | 99% | 89% | 78% | 61% | 61% | +34,7% | +18,3% |
| **30% (angka utama)** | **98%** | **86%** | **73%** | **57%** | **58%** | **+33,1%** | **+14,1%** |
| 40% | 97% | 81% | 68% | 51% | 55% | +30,8% | +10,7% |

Untuk pembanding, tanpa mitigasi target dengan keyakinan 80% hanya +7,2% pada f = 30%.

![Peluang berhasil](output/solusi/grafik_peluang_berhasil.png)

### Membaca hasilnya

- **Peluang berhasil di atas 80%** tercapai pada dua definisi: solusi lebih baik dari do nothing (98%) dan kenaikan MAU minimal 10% (86%), dengan f awal 30% dan mitigasi.
- **Target yang bisa dijanjikan dengan keyakinan 80%** adalah kenaikan MAU minimal sekitar 14% pada 2032 (rentang 11% sampai 18% untuk f awal 40% sampai 20%). Ini cara yang lazim menyatakan target: nilai yang dilampaui dengan peluang 80%, bukan nilai tengah.
- **Kenaikan minimal 20%** berpeluang sekitar 73%, dan angka tengah (median +33%) hanya menjadi target dengan peluang sekitar 50%. Menaikkan peluang ini ke 80% memerlukan efek pilar yang lebih besar atau risiko eksekusi yang lebih kecil dari yang dapat dipertanggungjawabkan oleh data yang ada, sehingga tidak dilakukan.
- **Kenaikan median turun** dari 39,4% menjadi 33% karena sebagian simulasi harus meluncurkan ulang aplikasi (efek tertunda) dan sebagian pilar tetap gagal.
- Pada baris f = 0% dengan mitigasi, peluang sebab bersama 10% tetap berlaku, sehingga baris itu tidak ditampilkan di tabel.

Pada f awal 30% dengan mitigasi, ketiga pilar berjalan pada sekitar 66% simulasi (kenaikan median 39%), hanya pilar 2 berjalan pada sekitar 12% (median 8%), dan tidak ada pilar berjalan pada sekitar 2%. Rincian ada di `tabel_kondisi_pilar_mitigasi.csv`.

**Keterbatasan metode.** Kegagalan dibuat biner dan sebagian besar saling bebas. Pada kenyataannya pilar bisa gagal sebagian dan berkorelasi (anggaran dan tim yang sama), yang membuat hasil lebih bergelombang daripada model ini.

## 2.8 Bahasa yang aman untuk proposal

- Angka utama: kenaikan MAU sekitar **39%** pada 2032 dibanding do nothing (rentang 19% sampai 56%), atau pertumbuhan MAU sekitar 10,7% per tahun melawan 4,3% tanpa solusi.
- Pada kasus buruk solusi (P10) hasilnya tetap di atas kasus baik do nothing (P90) bila dibandingkan dalam indeks.
- Target yang dapat dijanjikan: dengan rancangan mitigasi dan peluang pilar gagal awal 30%, solusi menaikkan MAU minimal sekitar 14% pada 2032 dengan keyakinan 80% (peluang lebih baik dari do nothing sekitar 98%, kenaikan minimal 10% sekitar 86%, minimal 20% sekitar 73%).
- Sajikan persentase dan kelipatan, bukan level absolut, sampai angka MAU Syariah dari klien tersedia.

## 2.9 Keterbatasan dan daftar verifikasi

1. **Semua rentang efek pilar adalah asumsi tim.** Jangkarnya tidak langsung: rasio Muslim Pro adalah batas atas, bukan bukti efek Nava; tidak ada data konversi KOL atau QRIS masjid untuk LinkAja.
2. **Bukti retensi berkualitas rendah.** NP Digital adalah data agensi dan Duolingo adalah blog perusahaan. Dukungan akademiknya (Hamari dkk., 2014) menyatakan efek bergantung konteks.
3. **Pernyataan DMI "hingga 10 kali lipat"** adalah pernyataan pejabat organisasi, bukan studi.
4. **Linimasa diasumsikan** Nava aktif sejak Januari 2027 dengan anggaran terjaga. Bagian implementasi proposal masih perlu mengisi linimasa sebenarnya.
5. **Retensi menumpuk (G3) adalah asumsi tanpa data.** Rentang 0% / 1% / 2,5% per tahun belum bisa dikalibrasi dengan data LinkAja; tanpanya (G3 = 0) kenaikan median 2032 sekitar 34%. Perlu data retensi dari klien atau uji coba sebelum dipakai sebagai angka tunggal.
6. **Tambahan pendaftar dari aplikasi Nava (U1)** dibuat nol agar konservatif. Bila itu dianggap terlalu rendah, nilainya dapat dinaikkan.
7. **Survei internal (n=50)** adalah mahasiswa Gen Z dan bersifat indikatif, bukan representatif nasional.
8. **Dampak finansial belum dihitung.** Konversi tambahan MAU menjadi pendapatan butuh pendapatan per pengguna aktif dari klien.
9. **Perlu dicek ke sumber primer:** sepuluh observasi konsumsi rumah tangga (diambil dari pemberitaan BPS, bukan tabel resmi), angka MAU Syariah dari klien, dan selisih 9 juta (Kontan) dengan 10 juta (kasus) pengguna terdaftar.
10. **Peluang gagal pilar (f) dan parameter mitigasi adalah asumsi.** Tidak ada data kegagalan pilar untuk LinkAja; rujukannya literatur peluncuran produk konsumen (sumber sekunder). Sepakati nilai f dengan klien atau mentor sebelum dipakai sebagai angka tunggal.
11. **Riwayat pada grafik utama.** Garis sebelum 2027 adalah regresi pada lima titik data pengguna terdaftar, dengan asumsi rasio aktif tetap. Tahun 2023 sampai 2025 adalah estimasi, bukan data.

---

# Bagian 3. Cara Menjalankan

Persyaratan: Python 3.8 atau lebih baru.

```bash
git clone https://github.com/USERNAME/linkaja-syariah-baseline-montecarlo.git
cd linkaja-syariah-baseline-montecarlo
pip install -r requirements.txt
python src/baseline_montecarlo.py
python src/simulasi_solusi.py
```

Opsi (berlaku untuk kedua skrip): `--n` jumlah simulasi (default 10000), `--seed` seed acak (default 2026), `--outdir` folder keluaran. Skrip solusi juga punya `--uji` untuk uji konvergensi lima seed.

**Versi notebook.** `notebook/simulasi_linkaja_syariah.ipynb` berisi seluruh kode dan hasil dalam satu alur (cara kerja, baseline, solusi, pilar, pertumbuhan per tahun, peluang berhasil, uji kestabilan, uji ketahanan) dan bisa dijalankan dari atas ke bawah di Jupyter atau Colab tanpa file lain.

**Di Google Colab:** unggah kedua file di folder `src` ke Colab, lalu jalankan `%run baseline_montecarlo.py` dan `%run simulasi_solusi.py`. Skrip solusi membutuhkan `baseline_montecarlo.py` di folder yang sama.

## Struktur repositori

```text
linkaja-syariah-baseline-montecarlo/
├── README.md
├── requirements.txt
├── .gitignore
├── notebook/
│   └── simulasi_linkaja_syariah.ipynb   versi notebook, semua kode dan hasil berurutan (Bagian 0 sampai 7)
├── src/
│   ├── baseline_montecarlo.py      simulasi baseline
│   └── simulasi_solusi.py          dampak tiga pilar
└── output/
    ├── baseline/                   3 grafik dan 4 tabel baseline
    └── solusi/                     6 grafik dan 15 tabel dampak solusi
```

---

# Referensi

**Teori dan metode** (judul, penulis, dan lokasi terbit telah dicocokkan dengan katalog penerbit atau repositori ilmiah)

Bass, F. M. (1969). A new product growth model for consumer durables. *Management Science, 15*(5), 215 sampai 227. https://doi.org/10.1287/mnsc.15.5.215

Hamari, J., Koivisto, J., & Sarsa, H. (2014). Does gamification work? A literature review of empirical studies on gamification. *Proceedings of the 47th Hawaii International Conference on System Sciences*, 3025 sampai 3034. https://doi.org/10.1109/HICSS.2014.377

Henderson, B. D. (1970). The product portfolio. *BCG Perspectives*. Boston Consulting Group.

Malcolm, D. G., Roseboom, J. H., Clark, C. E., & Fazar, W. (1959). Application of a technique for research and development program evaluation. *Operations Research, 7*(5), 646 sampai 669. https://doi.org/10.1287/opre.7.5.646

Merton, R. C. (1976). Option pricing when underlying stock returns are discontinuous. *Journal of Financial Economics, 3*(1 sampai 2), 125 sampai 144. https://doi.org/10.1016/0304-405X(76)90022-2

Metropolis, N., & Ulam, S. (1949). The Monte Carlo method. *Journal of the American Statistical Association, 44*(247), 335 sampai 341.

Saltelli, A., Ratto, M., Andres, T., Campolongo, F., Cariboni, J., Gatelli, D., Saisana, M., & Tarantola, S. (2008). *Global sensitivity analysis: The primer*. John Wiley & Sons.

Davis, F. D. (1989). Perceived usefulness, perceived ease of use, and user acceptance of information technology. *MIS Quarterly, 13*(3), 319 sampai 340. (Dikutip dari dokumen proposal; belum dicocokkan ulang.)

**Data dan pemberitaan**

- LinkAja Syariah 1,6 juta pengguna, akhir 2020: [Kontan](https://keuangan.kontan.co.id/news/belum-setahun-beroperasi-linkaja-syariah-sudah-punya-16-juta-pengguna)
- 5,8 juta pengguna Syariah, November 2021: [Katadata](https://katadata.co.id/finansial/korporasi/619f6a59a3681/4-program-kerja-sama-linkaja-syariah-dan-bsi-apa-keuntungan-nasabah)
- 7,5 juta pengguna Syariah, Juli 2022: [Bisnis.com](https://finansial.bisnis.com/read/20220828/563/1571405/linkaja-syariah-raih-penghargaan-internasional-begini-kinerjanya)
- Lebih dari 9 juta pengguna Syariah, Agustus 2026: [Kontan](https://keuangan.kontan.co.id/news/optimalkan-pasar-syariah-linkaja-berencana-kembangkan-aplikasi-khusus-syariah)
- Lebih dari 92 juta pengguna terdaftar LinkAja, semester I 2026: [Kontan](https://keuangan.kontan.co.id/news/linkaja-catatkan-pertumbuhan-kinerja-pada-semester-i-2026)
- MAU LinkAja sekitar 5 juta: [Bisnis.com](https://teknologi.bisnis.com/read/20260819/266/1997485/laba-bersih-linkaja-tumbuh-100-semester-i2026-pengguna-aktif-naik-tipis)
- Momentum Ramadan 2026: [Kontan](https://keuangan.kontan.co.id/news/momentum-ramadan-dan-hari-raya-2026-berpotensi-tingkatkan-transaksi-digital)
- MDR QRIS 0%: [Kontan](https://keuangan.kontan.co.id/news/bi-perluas-kebijakan-mdr-qris-0-mulai-oktober-2026-ini-respons-linkaja)
- Transaksi pembayaran digital Agustus 2026 naik 40,36% dan QRIS 67,22%: [Kontan](https://keuangan.kontan.co.id/news/transaksi-pembayaran-digital-agustus-2026-tembus-611-miliar)
- Pernyataan DMI tentang QRIS masjid dan jumlah masjid: [Fortune IDN](https://www.fortuneidn.com/sharia/angka-ziswaf-masjid-melonjak-10-kali-lipat-berkat-qris-00-4vfn9-rz6xfw)
- Widget dan retensi Duolingo: [Blog Duolingo](https://blog.duolingo.com/widget-feature)
- Konsumsi rumah tangga: [Bisnis.com Q3 2026](https://ekonomi.bisnis.com/read/20261109/12/2019555/pertumbuhan-ekonomi-547-dunia-usaha-masih-wait-and-see), [Fortune IDN Q4 2025](https://www.fortuneidn.com/news/bps-catat-pertumbuhan-ekonomi-ri-capai-5-11-sepanjang-2025-00-6cmtl-n2fp1d)
- Peluncuran produk baru: [Oregon State PACE](https://blog.pace.oregonstate.edu/why-new-food-products-fail) (merangkum makalah JPIM dan Blackburn), [McTague 2023](https://brucemctague.com/2023/01/30), [Tenet 2026 via SHNO](https://www.shno.co/marketing-statistics/product-launch-statistics); sumber sekunder dan produk konsumen.
- Data survei internal tim (n=50), data Tirto, Populix, NP Digital, Muslim Pro, SIMAS, dan Jakarta Fair berasal dari dokumen proposal tim.

---

Penulis: Fausta Yassar Putra Prakoso, Manajemen, Fakultas Ekonomi dan Bisnis, Universitas Indonesia.
Data bersumber dari pemberitaan publik dan dokumen kasus kompetisi. Angka proyeksi adalah hasil simulasi berdasarkan asumsi yang tertulis dan bukan angka resmi LinkAja.
