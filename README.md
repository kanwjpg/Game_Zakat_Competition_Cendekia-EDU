# 🌙 Zakat Quest — Petualangan Cahaya Berbagi

Game edukasi **zakat** berbasis web untuk pelajar **SMP & SMA**. Tujuh bab petualangan berisi
cerita bergambar, kuis berbatas waktu, mini-game interaktif, dan **kalkulator zakat** yang
benar-benar bisa dipakai menghitung zakat sungguhan.

Dibuat untuk **Kompetisi Cendekia-EDU**.

---

## ✨ Isi Permainan

### Tujuh bab perjalanan
| Bab | Judul | Materi |
|-----|-------|--------|
| 1 | 🕌 Gerbang Ilmu | Makna zakat, dasar hukum, beda zakat–infak–sedekah |
| 2 | 🪙 Pasar Barokah | Nisab, haul, zakat emas, perak, dan uang |
| 3 | 🌾 Sawah Hijau | Zakat pertanian (5% & 10%), peternakan, rikaz |
| 4 | 🏢 Kantor Amil | Zakat penghasilan/profesi dan perdagangan |
| 5 | 🤲 Kampung Asnaf | Delapan golongan penerima zakat |
| 6 | 🌙 Malam Takbir | Zakat fitrah: takaran dan waktu |
| 7 | 🏅 Ujian Akbar | Ujian campuran — waktu ketat, tanpa petunjuk |

Setiap bab terdiri atas **cerita → kuis → mini-game → hasil**.

### Tiga jenis mini-game
- **Sortir kartu** — seret (atau ketuk) kartu ke keranjang yang tepat. Mendukung *drag & drop*
  di desktop dan *ketuk-lalu-pilih* di ponsel.
- **Timbangan Nisab** — timbangan menukik mengikuti berat harta terhadap nisab; pemain memutuskan
  wajib zakat atau belum.
- **Hitung Cepat** — papan angka untuk menghitung zakat, bisa juga diketik dari papan ketik.

### Kalkulator zakat (7 jenis)
Penghasilan · Emas & Perak · Tabungan & Uang · Perdagangan · Pertanian · Fitrah · Rikaz.

Harga acuan **emas, perak, dan beras dapat diubah pengguna** dan tersimpan otomatis — angka bawaan
hanyalah contoh dan **harus diperbarui** agar hasilnya akurat.

### Progres & motivasi
Bintang per bab (maksimal 21), poin, tingkatan (Pemula → Ahli Zakat), **8 lencana**,
papan skor lokal, dan statistik akurasi. Semua tersimpan di `localStorage`.

### Aksesibilitas
- Pintasan papan ketik: `A–D` / `1–4` memilih jawaban, `H` petunjuk, `Enter` lanjut, `Esc` tutup.
- Sakelar **efek suara**, **animasi penuh**, **teks besar**, dan **kontras tinggi**.
- Menghormati `prefers-reduced-motion`.
- Efek suara disintesis lewat Web Audio API — tidak ada berkas audio yang perlu diunduh.

---

## 🚀 Menjalankan

Tidak ada proses *build*, tidak ada dependensi.

```bash
# cara tercepat
buka index.html di peramban
```

Atau lewat server lokal (disarankan agar penyimpanan progres berperilaku persis seperti saat daring):

```bash
python3 -m http.server 8000
# lalu buka http://localhost:8000
```

### Publikasi ke GitHub Pages
`Settings → Pages → Source: Deploy from a branch → main / (root)`.
Karena seluruh permainan berada dalam satu berkas, tidak ada langkah tambahan.

Permainan tetap berjalan **sepenuhnya luring**. Berkas font dimuat dari Google Fonts bila
tersedia; jika tidak, permainan otomatis memakai font sistem tanpa ada yang rusak.

---

## 🧱 Struktur Kode

Seluruh permainan berada di `index.html`, disusun berurutan agar mudah ditelusuri:

| Bagian | Isi |
|--------|-----|
| `<style>` | Token desain, latar langit beranimasi, komponen, tata letak responsif |
| 1–6 | Util, penyimpanan, audio, partikel FX, pembantu UI, *router* layar |
| 7 | `sceneSVG()` — latar cerita SVG yang digambar lewat kode |
| 8 | **`CHAPTERS`** — seluruh materi: cerita, soal, mini-game |
| 9–13 | Progres, peta, keadaan permainan, mesin cerita, mesin kuis |
| 14 | Mesin mini-game (`sort`, `scale`, `calc`) |
| 15–16 | Layar hasil, kalkulator zakat |
| 17–20 | Papan skor & lencana, pengaturan, latar paralaks, *boot* |

### Menambah bab baru
Cukup tambahkan satu objek ke larik `CHAPTERS` — peta, pembukaan kunci, bintang, dan papan skor
menyesuaikan sendiri:

```js
{
  id: 8, title: 'Bab Baru', topic: 'Topik', emoji: '📗', scene: 'market',
  blurb: 'Ringkasan singkat untuk kartu bab.',
  story: [{ c: 'ustadz', t: 'Dialog pembuka…' }],
  questions: [{
    q: 'Pertanyaan?',
    opts: ['A', 'B', 'C', 'D'],
    a: 0,                       // indeks jawaban benar (opsi diacak saat dimainkan)
    hint: 'Petunjuk singkat.',
    why: 'Penjelasan setelah dijawab.',
    dalil: 'Rujukan (opsional).'
  }],
  mini: { type: 'sort', /* atau 'scale' / 'calc' */ name: '…', desc: '…', /* … */ }
}
```

Lalu tambahkan satu koordinat pada `NODE_POS` untuk titiknya di peta.

---

## 📚 Dasar Materi

Ketentuan yang dipakai mengikuti fikih zakat arus utama di Indonesia:

- Nisab emas **85 gram**, perak **595 gram**, kadar **2,5%**, haul 1 tahun hijriah.
- Zakat penghasilan — **Fatwa MUI No. 3 Tahun 2003**; diperkuat UU No. 23 Tahun 2011.
- Pertanian — nisab **5 wasaq ≈ 653 kg** gabah; **10%** tanpa biaya pengairan, **5%** bila berbiaya.
- Ternak — unta 5 ekor, sapi 30 ekor, kambing 40 ekor.
- Rikaz — **20%**, tanpa nisab dan haul.
- Zakat fitrah — **1 sha‘ ≈ 2,5 kg** makanan pokok per jiwa.
- Delapan asnaf — **QS At-Taubah 60**.

> **Catatan.** Kalkulator ini alat bantu belajar. Harga emas, perak, dan beras berubah tiap hari —
> perbarui angka acuannya sebelum dipakai. Untuk kasus yang rumit, rujuk amil atau ustaz setempat.

---

## 🧭 Rencana Pengembangan

Fondasi berikut sudah siap dipakai untuk pengembangan lanjutan:

- **Mode guru** — satu berkas ekspor/impor progres kelas dari `localStorage`.
- **Bank soal lebih besar** — `questions` sudah berupa data murni, tinggal ditambah.
- **Mode turnamen** — papan skor sudah menyimpan nama, skor, bab, dan tanggal.
- **Sertifikat cetak** — layar rangkuman sudah ada, tinggal ditambah `window.print()` bergaya.
- **Ensiklopedia zakat** — istilah sudah tersebar di `why`/`dalil`, siap dikumpulkan jadi glosarium.

---

## 🖥 Dukungan Peramban

Chrome, Edge, Firefox, dan Safari versi mutakhir, di desktop maupun ponsel.
Tata letak diuji pada lebar 390 px hingga 1440 px.
