# Wedding Invitation — Adi Muhtar & Nazila Kartika

Website undangan digital final berbasis proyek asli.

## Menjalankan
1. Ekstrak ZIP.
2. Buka `index.html` melalui browser modern.
3. Pertahankan folder `assets` tetap sejajar dengan `index.html`.

## Catatan revisi
- Struktur halaman, konten, 21 tema warna, musik, galeri, RSVP, countdown, navigasi, dan aset asli dipertahankan.
- REVISION 10: `assets/tile-pattern.jpg` diterapkan pada seluruh halaman, termasuk halaman penutup. Halaman ganjil memakai lapisan warna tema 20%; halaman genap memakai lapisan warna tema 90%.
- Foto selain cover memakai lazy loading dan decoding async.
- RSVP tetap berupa konfirmasi di sisi halaman; tidak ada klaim penyimpanan database.
- Sebagian font Google Fonts memerlukan koneksi internet; font fallback tetap disediakan.

Dibuat dari `Invitation-main-REV-GLASS-FINAL.zip`.


## Revisi 7 — Firebase RSVP & Doa

Form RSVP mengirim data dengan HTTP POST ke Realtime Database pada path:
`/weddingResponses`. Setiap entri menyimpan `name`, `attendance`, `wish`, `guest`,
`createdAt`, dan `source`. Data dapat diperiksa melalui Firebase Console → Realtime
Database → Data → `weddingResponses`.

Sebelum dipublikasikan, atur Rules pada Realtime Database. Contoh rules berikut
mengizinkan pengiriman dari pengunjung tanpa login, tetapi tidak mengizinkan
pengunjung membaca daftar RSVP dari browser publik (pemilik proyek tetap dapat
melihatnya melalui Firebase Console):

```json
{
  "rules": {
    "weddingResponses": {
      ".read": false,
      ".write": true,
      "$responseId": {
        ".validate": "newData.hasChildren(['name','attendance','createdAt']) && newData.child('name').isString() && newData.child('name').val().length <= 100 && newData.child('attendance').isString() && (newData.child('attendance').val() === 'Hadir' || newData.child('attendance').val() === 'Tidak hadir') && newData.child('wish').isString() && newData.child('wish').val().length <= 500"
      }
    }
  }
}
```

Catatan: aturan `.write: true` berarti endpoint publik dapat menerima kiriman tanpa
login. Pantau spam dan pertimbangkan App Check / autentikasi atau pembatasan tambahan
sebelum trafik besar. Jika Rules proyek saat ini berbeda, sesuaikan/deploy rules di
Firebase Console agar pengiriman dapat diterima.


## Revisi 10 — Sistem latar dan tema terpadu
- Seluruh halaman menggunakan `assets/tile-pattern.jpg` sebagai lapisan dasar yang sama.
- Halaman ganjil (1, 3, 5, 7, 9, 11) mendapat overlay warna tema pilihan sebesar 20%.
- Halaman genap (2, 4, 6, 8, 10) mendapat overlay warna tema pilihan sebesar 90%, sehingga tekstur tetap terlihat tipis.
- Warna teks, aksen, kartu, tombol, dan input diatur mengikuti kontras latar halaman.
- Border atas dan bawah seluruh halaman dibuat solid/opacity 100% dan mengikuti warna aksen tema yang dipilih.
- Pilihan warna tema disimpan dan dipakai kembali saat undangan dibuka lagi.
- CSS/JavaScript diperiksa sintaksnya; sistem overlay diuji pada browser Chromium menggunakan seluruh 11 halaman contoh.


REVISION 11: halaman ganjil memakai lapisan warna dinamis 80% mengikuti tema yang dipilih; halaman genap tetap 90%. Klasifikasi ganjil/genap dihitung berdasarkan urutan elemen halaman di dalam main.


REVISION 13 VISUAL UPDATE
- Odd pages (1, 3, 5, 7, 9, 11): selected theme overlay 80%.
- Even pages (2, 4, 6, 8, 10): selected theme overlay 30%.
- Tile pattern remains the common base layer on every page.
- All content/text panels use a consistent translucent glassmorphism surface with high-contrast light text; form fields remain light with dark text for legibility.
- Theme colors are CSS-variable driven and update with the theme picker.


## REVISION 16 — Border, font contrast, and public RSVP comments
- Top and bottom ornamental borders are rendered in a dedicated topmost `.page-border-art` layer with pointer events disabled, so they do not block buttons.
- Typography is assigned per glass panel: dark text on light glass and light text on dark glass; form controls remain dark-on-light.
- RSVP & Doa now displays saved Firebase entries in a scrollable guest comment feed.
- IMPORTANT: To display comments to all visitors, deploy the included `FIREBASE-DATABASE-RULES.json`. Its `.read: true` setting makes submitted names, attendance, and wishes publicly readable. Use this only if the couple agrees to publish guest messages.
