# Jawaban Pertanyaan - Praktikum 3 Pemrograman Web (CSS Dasar)

| | |
|---|---|
| **Nama** | Rafa Muhammad Fauzan |
| **NIM** | 312510277 |
| **Kelas** | I251C |
| **Program Studi** | Teknik Informatika |
| **Mata Kuliah** | Pemrograman Web |

**📄 Halaman:** [README (Laporan Praktikum)](README.md) | [JAWABAN (Jawaban Pertanyaan)](JAWABAN.md)

---

## 1. Eksperimen CSS

Eksperimen dilakukan dengan mengubah dan menambah properti serta nilai pada kode CSS, mengacu pada CSS Cheat Sheet, misalnya mengganti nilai `color`, `font-family`, `padding`, dan `background` pada selector yang sudah dibuat, lalu mengamati hasilnya setelah halaman di-refresh di browser. Dokumentasinya ada di README.md bagian [Eksperimen CSS](README.md#eksperimen-css).

## 2. Apa perbedaan pendeklarasian CSS elemen `h1 {...}` dengan `#intro h1 {...}`?

| | `h1 { ... }` | `#intro h1 { ... }` |
|---|---|---|
| **Jenis selector** | Selector elemen. | Selector turunan (*descendant selector*), gabungan ID dan elemen. |
| **Elemen yang terpengaruh** | **Semua** tag `<h1>` di seluruh halaman. | **Hanya** tag `<h1>` yang berada **di dalam** elemen dengan `id="intro"`. |
| **Spesifisitas** | Rendah (hanya 1 elemen). | Lebih tinggi, karena mengandung ID selector. |

Pada praktikum ini, ada dua `<h1>`: satu di dalam `<header>` (judul halaman) dan satu lagi di dalam `<div id="intro">` (teks "Hello World"). Deklarasi `h1 { color: #0F189F; text-align: center; ... }` dari CSS internal awalnya berlaku untuk **keduanya**. Tapi karena `#intro h1 { text-align: left; color: #fff; }` lebih spesifik (mengandung ID), aturan ini **menimpa** `color` dan `text-align` khusus untuk `<h1>` di dalam `#intro`, sementara `<h1>` di `<header>` tetap memakai gaya dari selector `h1` biasa.

Jadi `h1 {...}` menyasar berdasarkan **jenis tag**, sedangkan `#intro h1 {...}` menyasar berdasarkan **lokasi tag** (hanya yang ada di dalam elemen tertentu) dan punya prioritas lebih tinggi saat terjadi bentrok aturan.

## 3. Jika ada CSS internal, lalu ditambahkan CSS eksternal dan inline CSS pada elemen yang sama, deklarasi manakah yang ditampilkan browser?

CSS disebut *cascading* karena aturan yang bentrok diselesaikan berdasarkan **urutan prioritas**, dari yang paling lemah ke paling kuat:

1. **CSS internal/eksternal** (prioritas setara, yang menang adalah yang ditulis atau dimuat **terakhir** dalam dokumen)
2. **Inline CSS** (selalu menang atas internal dan eksternal, karena ditulis langsung di tag)

**Contoh dari praktikum:**

```html
<style>
    h1 { color: #0F189F; text-align: center; }
</style>
```

```css
/* style_eksternal.css */
#intro h1 { color: #fff; text-align: left; }
```

```html
<p style="text-align: center; color: #ccd8e4;">...</p>
```

- Untuk `<h1>` di dalam `#intro`: CSS eksternal (`#intro h1`) lebih **spesifik** daripada CSS internal (`h1`), jadi warna putih dan rata kiri dari eksternal yang dipakai, bukan biru dan rata tengah dari internal. (Ini sebenarnya soal spesifisitas selector, lihat jawaban No. 2, bukan murni soal internal vs eksternal.)
- Untuk `<p>`: karena memakai **inline CSS** (`style="text-align: center; color: #ccd8e4;"`), gaya inilah yang pasti dipakai browser, walaupun ada aturan CSS internal atau eksternal lain yang menyasar tag `<p>`. Inline selalu menang.

Kesimpulannya: **inline CSS memiliki prioritas tertinggi**, dan jika tidak ada inline, browser membandingkan spesifisitas selector antara aturan internal dan eksternal; yang lebih spesifik atau ditulis belakangan yang dipakai.

## 4. Jika satu elemen punya ID dan class, dan masing-masing selector punya deklarasi CSS, deklarasi manakah yang ditampilkan browser?

Contoh: `<p id="paragraf-1" class="text-paragraf">`

**ID selector memiliki spesifisitas lebih tinggi daripada class selector.** Jika keduanya mengatur property CSS yang sama, nilai dari **ID selector** yang akan dipakai browser, berapa pun urutan penulisannya dalam berkas CSS.

```css
#paragraf-1 {
    color: blue;
}
.text-paragraf {
    color: red;
}
```

Meskipun `.text-paragraf` ditulis setelah `#paragraf-1`, warna yang tampil tetap **biru**, karena ID selector lebih spesifik daripada class selector.

**Urutan spesifisitas CSS dari yang terendah ke tertinggi:**

1. Selector elemen (`p`)
2. Selector class (`.text-paragraf`)
3. Selector ID (`#paragraf-1`)
4. Inline CSS (`style="..."`)
5. `!important` (mengabaikan urutan spesifisitas di atas, sebaiknya dihindari kecuali benar-benar diperlukan)

Contoh dari praktikum: tombol `<a class="button btn-primary">` memakai dua class sekaligus. Keduanya sama-sama class, jadi yang menang bukan soal ID vs class, melainkan **urutan deklarasi**: `.btn-primary { background: #E42A42; }` ditulis setelah `.button { background: #bebcbd; }` di `style_eksternal.css`, sehingga warna latar akhirnya merah (`#E42A42`), menimpa abu-abu dari `.button`.
