# Spillover Guncangan Moneter Murni dan Informasi Bank Sentral AS terhadap Perekonomian Indonesia

Analisis *Bayesian Structural VAR* (BSVAR) untuk memisahkan dua jenis guncangan The Fed, yaitu **guncangan kebijakan moneter murni** dan **guncangan informasi bank sentral**, lalu menguji dampaknya terhadap pasar keuangan, kebijakan moneter, dan sektor riil Indonesia.

**Skripsi:** [Spillover Guncangan Moneter Murni dan Informasi Bank Sentral Amerika Serikat terhadap Perekonomian Indonesia (Universitas Andalas, 2026)](http://scholar.unand.ac.id/id/eprint/532061)
**Penulis:** Syafri Ismail · **Pembimbing:** Arie Sukma, S.E., M.Sc. · **Pendanaan:** Bantuan Penelitian Kebanksentralan (Banlit) Bank Indonesia 2026
**Kontak:** [LinkedIn](https://www.linkedin.com/in/syafri-ismail-29s/)

> **English summary.** This repository contains the MATLAB code, data, and outputs of an undergraduate thesis that applies the Jarociński & Karadi (2020) framework to Indonesia. A monthly BSVAR (Aug 2005 – Mar 2025, 7 variables, 12 lags) is identified with sign and zero restrictions using high-frequency Fed funds futures and S&P 500 surprises around FOMC announcements. Pure US monetary policy shocks mainly move Indonesian bond yields and equities in the short run, while central bank information shocks are followed by higher equity prices and industrial production. Over a 24-month horizon the two shocks together explain 8.5–14.5% of forecast-error variance in the Indonesian variables, with the largest share for the stock index (IHSG).

---

## 1. Pertanyaan Penelitian

Kenaikan ekspektasi suku bunga The Fed tidak selalu bersifat kontraktif. Kadang pasar membacanya sebagai sinyal bahwa bank sentral memiliki informasi yang lebih optimis tentang ekonomi. Jika kedua kasus dicampur, respons pasar yang tampak bertentangan dengan teori (misalnya IHSG naik saat ekspektasi suku bunga AS naik) menjadi sulit dijelaskan.

Penelitian ini menjawab tiga pertanyaan:

1. Bagaimana respons pasar keuangan, BI Rate, output, dan inflasi Indonesia terhadap masing-masing jenis guncangan?
2. Seberapa besar kontribusi tiap guncangan terhadap variasi variabel domestik?
3. Apakah pemisahan guncangan membantu menjelaskan anomali ketahanan pasar saham dan yield obligasi domestik?

## 2. Temuan Utama

**Respons terhadap guncangan (IRF, median dan interval kredibel posterior)**

| Guncangan | Pasar keuangan | BI Rate, IPI, IHK |
|---|---|---|
| **Moneter murni** (FFF naik, S&P 500 turun) | Yield obligasi 1 tahun naik dan IHSG turun pada horizon awal | Respons relatif terbatas dan mengandung ketidakpastian posterior |
| **Informasi bank sentral** (FFF naik, S&P 500 naik) | IHSG naik secara konsisten secara posterior. Penurunan awal yield hanya menunjukkan kecenderungan arah karena interval kredibelnya mencakup nol | IPI naik secara konsisten secara posterior. BI Rate merespons bertahap dan IHK kecil serta tertunda |

**Dekomposisi varians (FEVD, horizon 24 bulan)**

| Variabel | Moneter murni | Informasi bank sentral | **Total dua guncangan** |
|---|---:|---:|---:|
| Federal Funds Futures | 64,63% | 35,37% | 100% |
| S&P 500 | 64,36% | 35,64% | 100% |
| BI Rate | 4,06% | 6,32% | **10,38%** |
| Yield obligasi 1 tahun | 4,56% | 4,27% | **8,83%** |
| IHSG | 7,35% | 7,16% | **14,51%** |
| Indeks Produksi Industri (IPI) | 3,45% | 5,70% | **9,15%** |
| IHK | 3,38% | 5,14% | **8,52%** |

**Cara membaca.** Angka total adalah kontribusi *gabungan* kedua guncangan The Fed yang diidentifikasi. Sisa variasi berasal dari guncangan lain di luar kedua guncangan tersebut (domestik maupun eksternal lainnya).

**Kesimpulan.** Spillover kebijakan moneter AS ke Indonesia bersifat **heterogen, relatif sementara, dan paling kuat pada pasar saham**. Respons kebijakan domestik perlu mempertimbangkan jenis guncangan yang mendasarinya, bukan hanya arah suku bunga The Fed.

Visualisasi:

![Grafik IRF: respons variabel Indonesia terhadap guncangan moneter murni dan guncangan informasi bank sentral](output/grafik_irf.png)

![Kesimpulan utama](output/Kesimpulan.png)

## 3. Metodologi

### 3.1 Data

- **Frekuensi dan periode:** bulanan, Agustus 2005 – Maret 2025.
- **Blok eksternal (kejutan frekuensi tinggi):** perubahan harga *three-month federal funds futures* (FFF) dan S&P 500 dalam jendela 10 menit sebelum hingga 20 menit setelah pengumuman FOMC. Kejutan pada hari FOMC dijumlahkan per bulan, dan bulan tanpa FOMC bernilai nol. Sumber: repositori GitHub Marek Jarociński (diperbarui hingga 2025).
- **Blok internal (Indonesia):** sumber BI, BPS, dan Investing.com.

### 3.2 Kamus variabel

| Kode | Variabel | Definisi | Transformasi | Sumber |
|---|---|---|---|---|
| `FFF` | Federal Funds Futures | Agregasi kejutan intraday di sekitar FOMC | Persentase (%) | Jarociński |
| `SP500` | S&P 500 | Agregasi perubahan intraday di sekitar FOMC | Persentase (%) | Jarociński |
| `BI Rate` | Suku bunga kebijakan BI | BI Rate / BI 7-Day Reverse Repo Rate | Persentase (level) | BI |
| `Yield` | Imbal hasil SUN tenor 1 tahun | Imbal hasil obligasi pemerintah | Persentase (level) | [ISI: BI / Investing.com] |
| `IHSG` | Indeks Harga Saham Gabungan | Indeks bursa saham domestik | Log × 100 | [ISI: Investing.com] |
| `IPI` | Indeks Produksi Industri | Proksi output riil bulanan | Log × 100 | BPS |
| `IHK` | Indeks Harga Konsumen | Proksi tingkat harga | Log × 100 | BPS |

Data domestik dipertahankan dalam bentuk level atau log-level agar informasi jangka panjang tidak hilang. Prior Bayesian dipakai untuk mengendalikan risiko overfitting.

### 3.3 Spesifikasi model

| Komponen | Pilihan |
|---|---|
| Model | Bayesian Structural VAR, 7 variabel endogen (2 eksternal + 5 domestik) |
| Urutan variabel domestik | BI Rate → Yield → IHSG → IPI → IHK |
| Lag | 12 bulan (`p = 12`) |
| Prior | *Independent Normal–Inverted Wishart* dengan penyusutan bergaya Minnesota/Litterman |
| Hiperparameter prior | [ISI: nilai dari skrip, misalnya tightness, decay, prior untuk konstanta] |
| Estimasi | Gibbs sampler, 20.000 iterasi |
| *Burn-in* dan *thinning* | 4.000 iterasi dibuang, lalu setiap iterasi ke-4 disimpan |
| Sampel posterior akhir | 4.000 *draws* |
| Eksogenitas blok | Variabel eksternal tidak dipengaruhi variabel Indonesia (asumsi *small open economy*) |
| Diagnostik | *Trace plot*, autokorelasi sampel, kestabilan respons impuls |
| Perangkat lunak | MATLAB [ISI: versi dan toolbox] |

### 3.4 Identifikasi guncangan

Identifikasi memakai **restriksi tanda (*sign*) dan restriksi nol (*zero*)**. Restriksi nol berasal dari eksogenitas blok, dan restriksi tanda memisahkan dua guncangan berdasarkan pergerakan bersama FFF dan S&P 500. Kandidat matriks dampak dibentuk dari dekomposisi Cholesky dikalikan rotasi blok. Rotasi diambil dengan prior seragam atas ruang rotasi (algoritma distribusi Haar, Rubio-Ramirez dkk. 2010; Arias dkk. 2018).

| Variabel | Moneter murni (*co-movement* negatif) | Informasi bank sentral (*co-movement* positif) | Lainnya |
|---|:---:|:---:|:---:|
| Federal Funds Futures | + | + | 0 |
| S&P 500 | − | + | 0 |
| Variabel domestik Indonesia | tidak dibatasi | tidak dibatasi | tidak dibatasi |

Variabel domestik sengaja tidak dibatasi (pendekatan agnostik), sehingga respons Indonesia ditentukan oleh data.

## 4. Struktur Repositori

> Sesuaikan dengan isi folder yang sebenarnya.

```
.
├── data/        # [ISI: data gabungan, mis. data_bulanan.csv, plus kamus data]
├── scripts/     # [ISI: skrip MATLAB, urutan eksekusi di bagian 5]
├── output/      # Grafik IRF, validasi identifikasi, kerangka berpikir, kesimpulan
├── docs/        # [ISI: dokumen pendukung]
├── LICENSE
└── README.md
```

## 5. Cara Mereplikasi

**Prasyarat:** MATLAB.

1. Unduh atau *clone* repositori:
   ```bash
   git clone https://github.com/Syafri1/BSVAR-Monetary-Spillover-Indonesia.git
   ```
2. Buka MATLAB dan jadikan folder repositori sebagai *working directory*.
3. Jalankan main.m lalu komputer akan melakukan komputasi secara berurutan dengan memanggil fungsi-fungsi yang diperlukan
4. Hasil tersimpan di folder `output/`.

## 6. Sumber Data dan Atribusi Kode

Proyek ini mengadaptasi metodologi dan kode dari literatur terkemuka, lalu menyesuaikannya untuk Indonesia.

- **Kejutan eksternal:** data FFF dan S&P 500 dari repositori GitHub Marek Jarociński.
- **Data Indonesia:** BI Rate, yield Obligasi Indonesia 1 tahun, IHSG, IPI, dan IHK dari Bank Indonesia, BPS, dan Investing.com.
- **Kode estimasi:** diadaptasi dari skrip MATLAB paket replikasi *American Economic Journal: Macroeconomics* (Jarociński & Karadi, 2020).

### Kontribusi dan modifikasi dalam proyek ini

Dibandingkan paket replikasi aslinya:

- Kerangka diterapkan pada **ekonomi pasar berkembang kecil dan terbuka (Indonesia)**, bukan ekonomi AS.
- Model diperluas menjadi **7 variabel endogen** dengan blok internal Indonesia.
- Ditambahkan **eksogenitas blok** (restriksi nol) sehingga guncangan Indonesia tidak memengaruhi variabel AS.
- Ditambahkan **restriksi tanda dan nol** pada rotasi blok dengan prior seragam, sesuai algoritma Rubio-Ramirez dkk. dan Arias dkk.
- Parameter estimasi dan matriks input disesuaikan dengan data Indonesia.
- Tampilan grafik perbandingan Impulse Response Function

**Catatan lisensi (© 2020 American Economic Association):** skrip dan perangkat lunak turunan tunduk pada *Modified BSD License*. Basis data turunan dilisensikan di bawah *Creative Commons Attribution 4.0 International*. Lihat berkas [`LICENSE`](LICENSE).

## 7. Keterbatasan

- **Cakupan variabel domestik terbatas.** Model belum memuat nilai tukar rupiah, VIX, arus modal asing, cadangan devisa, atau harga komoditas. Saluran-saluran ini bisa penting bagi Indonesia.
- **Satu metode dan satu strategi identifikasi.** Hasil belum diuji dengan estimasi alternatif seperti TVP-VAR atau *Local Projection*.
- **Hanya Indonesia.** Belum ada perbandingan dengan negara berkembang lain, sehingga belum jelas apakah pola ini khas Indonesia.
- **Kontribusi FEVD bersifat sebagian.** Total 8,5–14,5% adalah kontribusi dua guncangan The Fed yang diidentifikasi, bukan seluruh pengaruh eksternal, dan sisanya tidak bisa langsung disebut berasal dari faktor domestik saja.
- **Ketidakpastian posterior.** Beberapa respons (terutama BI Rate, IPI, IHK pada guncangan moneter murni, serta yield awal pada guncangan informasi) memiliki interval kredibel yang mencakup nol.

## 8. Implikasi Kebijakan (ringkas)

Respons kebijakan domestik terhadap naiknya ekspektasi suku bunga The Fed perlu mempertimbangkan **sumber guncangannya**. Jika berasal dari guncangan moneter murni, tekanan pada yield dan IHSG cenderung meningkat. Jika disertai informasi positif tentang prospek ekonomi, dampaknya ke pasar saham dan output bisa berbeda. Rincian lengkap ada di Bab V skripsi.

## 9. Sitasi

Jika Anda memakai kode atau hasil ini, mohon sitasi:

```bibtex
@thesis{ismail2026bsvar,
  author = {Ismail, Syafri},
  title  = {Spillover Guncangan Moneter Murni dan Informasi Bank Sentral Amerika Serikat terhadap Perekonomian Indonesia},
  school = {Universitas Andalas},
  year   = {2026},
  type   = {Skripsi},
  url    = {http://scholar.unand.ac.id/id/eprint/532061}
}
```

Karya yang diadaptasi:

> Jarociński, M., & Karadi, P. (2020). Deconstructing Monetary Policy Surprises: The Role of Information Shocks. *American Economic Journal: Macroeconomics*, 12(2), 1–43.

## 10. Lisensi

Lihat [`LICENSE`](LICENSE) untuk ketentuan lisensi kode dan data turunan.
