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

### Gaya visual
Desain **datar (flat) bernuansa Islami**: warna blok, sudut membulat, tanpa gradien
maupun bayangan tebal. Palet dikunci pada tiga warna — teal, pasir, dan krem — dengan
merah hanya untuk keadaan salah.

Unsur Islaminya dibangun dari motif, bukan tempelan:
- **Anyaman bintang delapan (girih)** pada latar, bergeser sangat lambat dan mulus.
- **Bulan sabit & bintang** di sudut langit, serta **lentera (fanoos)** yang naik perlahan.
- **Ornamen arabesque** sebagai pemisah pada layar judul, kartu hasil, dan rangkuman.
- **Motif bintang delapan** samar di sudut setiap panel.
- **Transisi antar layar berbentuk irisan bintang delapan.**
- Nada efek suara memakai tangga nada **maqam Hijaz** agar terdengar bernuansa Timur Tengah.

### Tingkat animasi
Animasi bisa diatur pemain lewat **Pengaturan → Animasi**:

| Tingkat | Isi |
|---------|-----|
| **Penuh** (bawaan) | Latar bergerak, transisi bintang, mesin ketik cerita, bintang penghargaan, ilustrasi beranimasi |
| **Ringan** | Hanya gerak yang membantu membaca keadaan: unsur muncul bergiliran, penekanan jawaban, bilah waktu |
| **Mati** | Diam sepenuhnya |

Pilihan tersimpan otomatis, dan `prefers-reduced-motion` pada sistem tetap dihormati.

### Aksesibilitas
- Pintasan papan ketik: `A–D` / `1–4` memilih jawaban, `H` petunjuk, `Enter` lanjut, `Esc` tutup.
- Sakelar **efek suara**, **teks besar**, **kontras tinggi**, dan tiga tingkat **animasi**.
- Menghormati `prefers-reduced-motion` bawaan sistem.
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
| `<style>` | Token desain datar, komponen, tata letak responsif |
| 1–6 | Util, penyimpanan, audio (maqam Hijaz), sistem gerak, pembantu UI, *router* layar |
| 7 | `sceneSVG()` — tujuh latar cerita SVG datar beranimasi, digambar lewat kode |
| 8 | **`CHAPTERS`** — seluruh materi: cerita, soal, mini-game |
| 9–13 | Progres, peta, keadaan permainan, mesin cerita, mesin kuis |
| 14 | Mesin mini-game (`sort`, `scale`, `calc`) |
| 15–16 | Layar hasil, kalkulator zakat |
| 17–19 | Papan skor & lencana, pengaturan, *boot* |

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

## 🎨 Mengubah Warna

Seluruh warna berada pada satu blok `:root` di bagian atas `<style>`. Mengganti tiga token
berikut sudah cukup untuk mengubah keseluruhan tampilan:

```css
--teal:#1e9184;   /* warna latar utama  */
--sand:#e8a75f;   /* aksen & tombol     */
--cream:#fff7ec;  /* bidang terang      */
```

Ilustrasi latar cerita memakai palet yang sama lewat objek `PAL` di bagian 7.

---

## 🖥 Dukungan Peramban

Chrome, Edge, Firefox, dan Safari versi mutakhir, di desktop maupun ponsel.
Tata letak diuji pada lebar 390 px hingga 1440 px.
