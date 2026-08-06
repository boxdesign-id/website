# Menghubungkan Box Design ke Firebase (agar Upload/Edit/Hapus Tersimpan Permanen)

File `index.html` sudah disiapkan untuk terhubung ke Firestore (database gratis dari Firebase).
Tanpa langkah ini, situs tetap tampil normal dengan data contoh, tapi perubahan dari tombol
Upload/Ubah/Hapus tidak akan tersimpan permanen.

## 1. Buat Proyek Firebase
1. Buka https://console.firebase.google.com
2. Klik **Add project**, beri nama (mis. `box-design`), lanjutkan sampai selesai (Google Analytics boleh dimatikan).

## 2. Aktifkan Firestore Database
1. Di sidebar kiri, buka **Build → Firestore Database**.
2. Klik **Create database**.
3. Pilih lokasi server terdekat (mis. `asia-southeast2`), lalu pilih **Start in production mode**.

## 3. Atur Security Rules
1. Masih di Firestore, buka tab **Rules**.
2. Ganti isinya dengan isi file `firestore.rules` yang saya sertakan, lalu klik **Publish**.
3. **Catatan keamanan:** rules ini mengizinkan siapa saja membaca & menulis koleksi `items`
   (karena kunci PIN di website hanya kunci tampilan, bukan autentikasi sungguhan). Ini cukup
   untuk penggunaan pribadi/demo. Kalau nanti butuh keamanan lebih ketat (misalnya hanya Anda
   yang bisa menulis), langkah selanjutnya adalah menambahkan **Firebase Authentication** —
   beri tahu saya kapan pun Anda siap, saya bisa bantu tambahkan.

## 4. Ambil Konfigurasi Web App
1. Di **Project settings** (ikon gerigi di sidebar) → tab **General**.
2. Scroll ke **Your apps**, klik ikon **</>** (Web) untuk mendaftarkan app baru. Beri nama bebas, tidak perlu centang Firebase Hosting.
3. Firebase akan menampilkan blok kode berisi `firebaseConfig` seperti ini:
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "box-design-xxxxx.firebaseapp.com",
     projectId: "box-design-xxxxx",
     storageBucket: "box-design-xxxxx.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
4. Salin nilai-nilai tersebut.

## 5. Tempel ke `index.html`
1. Buka `index.html` (di GitHub, klik ikon pensil untuk edit).
2. Cari bagian ini (sekitar baris ke-700-an, tepat di bawah komentar FIREBASE):
   ```js
   const firebaseConfig = {
     apiKey: "ISI_API_KEY_ANDA",
     authDomain: "ISI_PROJECT_ID.firebaseapp.com",
     projectId: "ISI_PROJECT_ID",
     storageBucket: "ISI_PROJECT_ID.appspot.com",
     messagingSenderId: "ISI_SENDER_ID",
     appId: "ISI_APP_ID"
   };
   ```
3. Ganti setiap nilai `ISI_...` dengan nilai asli dari langkah 4.
4. Commit perubahan.

## 6. Selesai
Buka website Anda (via GitHub Pages atau langsung buka file-nya). Katalog contoh akan otomatis
tersalin ke Firestore saat pertama kali dibuka. Setelah itu, semua Upload/Ubah/Hapus lewat
Mode Pemilik akan tersimpan permanen dan terlihat oleh semua pengunjung.

## Ringkasan Batasan
- PIN Mode Pemilik (`ADMIN_PIN` di dalam `index.html`) hanya kunci tampilan di sisi browser —
  ganti nilainya sesuai keinginan Anda, tapi jangan anggap ini keamanan tingkat produksi.
- Firestore gratis (Spark plan) sudah lebih dari cukup untuk skala katalog seperti ini
  (puluhan–ratusan item foto terkompresi).
- Foto disimpan langsung sebagai data terkompresi di dalam dokumen Firestore (bukan file
  terpisah), jadi tidak perlu mengaktifkan Firebase Storage maupun paket berbayar.
