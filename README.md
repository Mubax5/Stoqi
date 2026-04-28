# PRD — Stoqi | POS + Stok + QRIS + Laporan Keuangan 

Platform: SaaS Web App / PWA / Mobile-friendly Web  
Target perangkat: HP Android, tablet Android, laptop, browser modern  
Database: PostgreSQL untuk produksi, SQLite hanya untuk prototype lokal  
Tema UI: Emerald, teal, cyan, slate, amber — bersih, modern, nyaman dibaca  
Role: Super Admin SaaS, Owner, Manager, Kasir, Staff Gudang, Akuntan / Finance Viewer  
Status dokumen: Draft lengkap siap implementasi  
Versi: 1.0  
Tanggal: 28 April 2026

---

# Daftar Isi

1. Ringkasan Produk
2. Latar Belakang
3. Visi Produk
4. Tujuan Produk
5. Ruang Lingkup Sistem
6. Role Pengguna
7. Matriks Hak Akses
8. Gambaran Umum Alur Sistem
9. Fitur Utama Sistem
10. Use Case Utama
11. Diagram Use Case
12. Spesifikasi Use Case Detail
13. Kebutuhan Fungsional
14. Kebutuhan Modul
15. Kebutuhan Non-Fungsional
16. Aturan Bisnis
17. Validasi Data
18. Status dan Lifecycle Data
19. Struktur Database Umum
20. Detail Tabel Database
21. ERD
22. Relasi Database
23. Model Class / Domain Model
24. Diagram Class
25. Sequence Diagram
26. Diagram Status
27. Struktur Arsitektur Proyek
28. Desain Tampilan UI
29. Rekomendasi Layout Dashboard
30. Query Data Dashboard yang Disarankan
31. API Endpoint yang Disarankan
32. Event Tracking dan Analytics
33. Pembagian Tugas Tim
34. Skenario Demo Presentasi
35. Pricing dan Paket Produk
36. Roadmap Implementasi
37. Testing dan Quality Assurance
38. Risiko dan Solusi
39. Rekomendasi Implementasi Bertahap
40. Kesimpulan

---

# 1. Ringkasan Produk

## 1.1 Nama Produk

**KasirMitra POS — Aplikasi Kasir, Stok, QRIS, dan Laporan Keuangan Sederhana untuk UMKM Indonesia**

Alternatif nama produk:

1. KasirMitra.
2. TokoPilot.
3. WarungOS.
4. KasirGo.
5. MitraKasir.
6. TokoRapi.
7. DagangOS.

## 1.2 Jenis Produk

KasirMitra POS adalah aplikasi SaaS berbasis web/PWA untuk membantu UMKM mencatat penjualan, mengelola stok, merekap pembayaran QRIS, mengontrol kasir, dan membaca laporan keuangan sederhana.

Produk ini bukan software akuntansi berat. Fokusnya adalah operasional harian: transaksi cepat, stok otomatis, pembayaran rapi, dan owner tahu kondisi bisnis dari HP.

## 1.3 Tujuan Utama

1. Kasir mencatat transaksi dengan cepat.
2. Stok produk otomatis berkurang ketika transaksi berhasil.
3. Owner melihat omzet, transaksi, metode pembayaran, produk terlaris, stok menipis, laba kotor, dan pengeluaran.
4. QRIS bisa dicatat manual pada MVP dan diintegrasikan secara dinamis pada versi Pro.
5. Owner menerima laporan dashboard dan WhatsApp.
6. Manager dapat mengelola stok, supplier, produk, pegawai, dan outlet.
7. Sistem mendukung multi-role dan multi-outlet.

## 1.4 One-Line Pitch

> Aplikasi kasir UMKM yang otomatis catat penjualan, kurangi stok, rekap QRIS, dan kirim laporan untung harian ke WhatsApp owner.

## 1.5 Core Value Proposition

| Value              | Penjelasan                                                                                    |
| ------------------ | --------------------------------------------------------------------------------------------- |
| Transaksi cepat    | Kasir bisa menyelesaikan transaksi normal dalam beberapa klik.                                |
| Stok otomatis      | Setiap produk terjual langsung mengurangi stok.                                               |
| QRIS rapi          | Pembayaran QRIS tidak hanya masuk rekening, tetapi juga tercatat di laporan.                  |
| Laporan mudah      | Owner melihat omzet, laba kotor, payment mix, dan stok menipis tanpa istilah akuntansi berat. |
| Kontrol pegawai    | Refund, void, diskon, dan tutup shift tercatat jelas.                                         |
| WhatsApp report    | Owner menerima rekap harian tanpa membuka dashboard.                                          |
| Offline terbatas   | Transaksi tetap bisa dicatat saat internet bermasalah.                                        |
| Multi-outlet ready | Arsitektur sejak awal mendukung cabang.                                                       |

---

# 2. Latar Belakang

Banyak UMKM sudah menerima QRIS dan transaksi digital, tetapi pencatatan operasional masih manual. Transaksi sering dicatat di buku, Excel, atau hanya mengandalkan mutasi rekening. Akibatnya owner sulit mengetahui performa usaha secara real-time.

Masalah yang sering muncul:

- Penjualan tidak tercatat rapi.
- Stok sering selisih.
- QRIS masuk rekening, tetapi tidak tersambung ke transaksi kasir.
- Owner tidak tahu laba karena HPP dan pengeluaran tidak dihitung.
- Kasir bisa memberi diskon, refund, atau membatalkan transaksi tanpa audit jelas.
- Owner multi-outlet sulit memantau cabang dari jauh.

KasirMitra POS hadir untuk menghubungkan empat area utama:

**POS → Inventory → Payment → Report**

Alur besar:

**setup bisnis → input produk → transaksi kasir → pembayaran cash/QRIS → stok otomatis berkurang → tutup shift → laporan owner → keputusan restock/promo/evaluasi**.

---

# 3. Visi Produk

Menyediakan sistem kasir dan operasional harian paling mudah untuk UMKM Indonesia agar owner dapat mengambil keputusan berdasarkan data tanpa memahami software akuntansi kompleks.

Sistem harus:

- mudah digunakan oleh kasir non-teknis,
- cepat untuk transaksi harian,
- dapat dipakai dari HP atau tablet,
- memiliki dashboard owner yang jelas,
- mendukung stok dan payment tracking,
- memberi laporan dengan bahasa sederhana,
- dapat berkembang dari satu outlet menjadi multi-outlet,
- mendukung integrasi QRIS, WhatsApp, printer, dan export laporan.

## 3.1 Prinsip Produk

1. Cepat lebih penting daripada mewah.
2. Bahasa UMKM lebih penting daripada bahasa akuntansi.
3. Owner butuh insight, bukan tabel rumit.
4. Offline harus dipikirkan sejak awal.
5. Permission harus jelas.
6. MVP harus realistis.

---

# 4. Tujuan Produk

## 4.1 Tujuan Operasional

- Memudahkan kasir membuat transaksi.
- Memudahkan owner memantau omzet dari jarak jauh.
- Mengurangi stok otomatis setelah penjualan.
- Mencatat stok masuk/keluar.
- Mempermudah stock opname.
- Memisahkan metode pembayaran cash, QRIS, transfer, debit, dan e-wallet.
- Mencatat pengeluaran harian.
- Menghitung laba kotor dan estimasi laba bersih.
- Membuat laporan shift kasir.
- Memberikan alert stok menipis.
- Mengirim laporan harian via WhatsApp.
- Mendukung export laporan.
- Memberi audit trail pada tindakan sensitif.

## 4.2 Tujuan Bisnis

- Membangun subscription bulanan berulang.
- Menurunkan CAC melalui konten edukasi dan reseller.
- Meningkatkan retention karena produk dipakai harian.
- Membuka add-on revenue dari QRIS, WhatsApp, hardware, multi-outlet, dan setup data.

## 4.3 Tujuan Teknis

- Arsitektur modular.
- Database multi-tenant.
- Service layer jelas.
- Audit log.
- Idempotency untuk transaksi.
- Offline transaction queue.
- Webhook payment aman.
- Role-based access control.

---

# 5. Ruang Lingkup Sistem

## 5.1 Yang Termasuk dalam Sistem

1. Registrasi bisnis.
2. Login multi-role.
3. Dashboard berdasarkan role.
4. Manajemen outlet.
5. Manajemen pegawai/user.
6. Manajemen produk.
7. Manajemen kategori produk.
8. Manajemen varian produk sederhana.
9. Manajemen harga jual dan harga modal.
10. Manajemen stok per outlet.
11. Stok masuk.
12. Stok keluar.
13. Stock adjustment.
14. Stock opname.
15. Supplier sederhana.
16. Modul POS/kasir.
17. Keranjang transaksi.
18. Diskon item.
19. Diskon transaksi.
20. Pajak/service charge opsional.
21. Metode pembayaran cash.
22. Metode pembayaran QRIS manual.
23. Metode pembayaran transfer.
24. Metode pembayaran debit/e-wallet.
25. QRIS dinamis pada versi Pro.
26. Callback/webhook payment gateway.
27. Print struk.
28. Kirim struk via WhatsApp.
29. Refund/void transaksi.
30. Approval untuk refund/void/diskon besar.
31. Shift kasir.
32. Cash opening.
33. Cash closing.
34. Cash discrepancy.
35. Laporan omzet.
36. Laporan payment method.
37. Laporan produk terlaris.
38. Laporan stok menipis.
39. Laporan laba kotor.
40. Laporan pengeluaran.
41. Laporan laba bersih sederhana.
42. Export laporan CSV/XLSX/PDF.
43. WhatsApp daily report.
44. Audit log.
45. Role-based permission.
46. Offline transaction queue terbatas.
47. Sync saat online.
48. Multi-outlet dasar.
49. Transfer stok antar outlet.
50. Customer sederhana.
51. Loyalty sederhana pada roadmap.
52. API internal untuk frontend.
53. API webhook untuk payment gateway.
54. Admin panel SaaS untuk memantau tenant.

## 5.2 Yang Tidak Termasuk dalam MVP

1. Akuntansi lengkap seperti jurnal umum.
2. Neraca.
3. Buku besar.
4. Laporan arus kas formal.
5. Payroll.
6. BPJS.
7. PPh 21.
8. Integrasi marketplace.
9. Integrasi e-commerce.
10. AI agent penuh.
11. Forecasting stok otomatis kompleks.
12. Franchise management lanjutan.
13. Loyalty point kompleks.
14. CRM campaign besar.
15. Mobile native iOS/Android dari awal.
16. Integrasi mesin EDC bank secara langsung.
17. Inventory bahan baku detail untuk resep F&B kompleks.
18. Multi-gudang enterprise.
19. Approval workflow bertingkat seperti ERP.
20. Accounting compliance enterprise.

## 5.3 Yang Masuk Setelah MVP

1. QRIS dinamis.
2. WhatsApp report otomatis.
3. Loyalty sederhana.
4. Customer database.
5. Multi-outlet.
6. Transfer stok antar outlet.
7. Barcode label printing.
8. Supplier purchase order sederhana.
9. Promo bundling.
10. Product recipe sederhana untuk F&B.
11. Dashboard owner multi-cabang.
12. API external.
13. Reseller/admin partner dashboard.
14. Hardware bundle setup.

---

# 6. Role Pengguna

## 6.1 Super Admin SaaS

Role internal perusahaan KasirMitra.

### Tugas Utama

- Melihat daftar tenant.
- Mengatur subscription.
- Mengelola feature flag.
- Melihat error log.
- Mengelola template WhatsApp.
- Mengelola konfigurasi payment gateway.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

## 6.2 Owner

Pemilik bisnis UMKM.

### Tugas Utama

- Mengelola outlet.
- Mengelola pegawai.
- Mengelola produk.
- Melihat seluruh laporan.
- Approve refund/void/diskon besar.
- Menerima WhatsApp report.
- Mengelola subscription.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

## 6.3 Manager

Pengelola operasional outlet.

### Tugas Utama

- Mengelola produk dan stok.
- Melakukan stock opname.
- Melihat laporan operasional.
- Mengelola supplier.
- Menangani refund sesuai limit.
- Transfer stok.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

## 6.4 Kasir

Pengguna transaksi harian.

### Tugas Utama

- Membuka shift.
- Membuat transaksi.
- Memilih metode pembayaran.
- Mencetak struk.
- Mengirim struk WhatsApp.
- Menutup shift.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

## 6.5 Staff Gudang

Pengelola stok.

### Tugas Utama

- Melihat stok.
- Input stok masuk.
- Input stok keluar.
- Melakukan opname.
- Membuat permintaan restock.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

## 6.6 Akuntan / Finance Viewer

User laporan.

### Tugas Utama

- Melihat laporan penjualan.
- Melihat laporan pembayaran.
- Melihat pengeluaran.
- Export laporan.
- Melihat QRIS settlement.

### Batasan Umum

- Tidak boleh mengakses data di luar permission.
- Semua tindakan sensitif masuk audit log.
- Akses outlet dibatasi berdasarkan role dan assignment.

---

# 7. Matriks Hak Akses

| Fitur                | Super Admin SaaS | Owner |           Manager |          Kasir | Staff Gudang | Akuntan |
| -------------------- | ---------------: | ----: | ----------------: | -------------: | -----------: | ------: |
| Login                |               Ya |    Ya |                Ya |             Ya |           Ya |      Ya |
| Dashboard SaaS       |               Ya | Tidak |             Tidak |          Tidak |        Tidak |   Tidak |
| Dashboard bisnis     |     Support only |    Ya |                Ya |       Terbatas |     Terbatas |      Ya |
| Kelola bisnis/tenant |               Ya |    Ya |             Tidak |          Tidak |        Tidak |   Tidak |
| Kelola outlet        |     Support only |    Ya |          Terbatas |          Tidak |        Tidak |   Tidak |
| Kelola user          |     Support only |    Ya |          Terbatas |          Tidak |        Tidak |   Tidak |
| Kelola produk        |            Tidak |    Ya |                Ya |          Tidak |     Terbatas |   Tidak |
| Lihat produk         |     Support only |    Ya |                Ya |             Ya |           Ya |      Ya |
| Lihat harga modal    |            Tidak |    Ya | Ya jika diizinkan |          Tidak |     Terbatas |      Ya |
| Kelola stok          |            Tidak |    Ya |                Ya |          Tidak |           Ya |   Tidak |
| Stock opname         |            Tidak |    Ya |                Ya |          Tidak |           Ya |   Tidak |
| POS transaksi        |            Tidak |    Ya |                Ya |             Ya |        Tidak |   Tidak |
| Refund transaksi     |            Tidak |    Ya |      Sesuai limit | Jika diizinkan |        Tidak |   Tidak |
| Void transaksi       |            Tidak |    Ya |      Sesuai limit | Jika diizinkan |        Tidak |   Tidak |
| Diskon transaksi     |            Tidak |    Ya |      Sesuai limit |   Sesuai limit |        Tidak |   Tidak |
| Buka shift           |            Tidak |    Ya |                Ya |             Ya |        Tidak |   Tidak |
| Tutup shift          |            Tidak |    Ya |                Ya |             Ya |        Tidak |   Tidak |
| Input pengeluaran    |            Tidak |    Ya |                Ya | Jika diizinkan |        Tidak |      Ya |
| Lihat laporan omzet  |     Support only |    Ya |                Ya |  Shift sendiri |        Tidak |      Ya |
| Lihat laporan laba   |            Tidak |    Ya | Ya jika diizinkan |          Tidak |        Tidak |      Ya |
| Lihat audit log      |     Support only |    Ya |          Terbatas |          Tidak |        Tidak |   Tidak |
| Export laporan       |            Tidak |    Ya |                Ya |          Tidak |        Tidak |      Ya |
| Atur QRIS            |     Support only |    Ya |             Tidak |          Tidak |        Tidak |   Tidak |
| Atur subscription    |               Ya |    Ya |             Tidak |          Tidak |        Tidak |   Tidak |

---

# 8. Gambaran Umum Alur Sistem

## 8.1 Alur Onboarding Bisnis

1. Owner membuka halaman daftar.
2. Owner membuat akun.
3. Owner memasukkan nama bisnis.
4. Owner memilih jenis bisnis.
5. Sistem membuat tenant/business.
6. Owner membuat outlet pertama.
7. Owner menambahkan user kasir.
8. Owner menginput produk pertama.
9. Owner mengatur metode pembayaran.
10. Owner mengatur printer/struk.
11. Sistem menampilkan checklist onboarding.
12. Owner mulai transaksi.

---

## 8.2 Alur Transaksi POS

1. Kasir login.
2. Kasir memilih outlet.
3. Kasir membuka shift.
4. Kasir membuka layar POS.
5. Kasir memilih atau scan produk.
6. Sistem menambahkan produk ke cart.
7. Sistem menghitung subtotal.
8. Kasir menambahkan diskon jika perlu.
9. Sistem menghitung total akhir.
10. Kasir memilih metode pembayaran.
11. Sistem menyimpan transaksi.
12. Sistem menyimpan item transaksi.
13. Sistem menyimpan payment.
14. Sistem mengurangi stok.
15. Sistem membuat stock movement.
16. Sistem memperbarui laporan.
17. Kasir mencetak/mengirim struk.

---

## 8.3 Alur QRIS Manual

1. Kasir memilih pembayaran QRIS.
2. Sistem menampilkan instruksi pembayaran.
3. Customer scan QRIS statis toko.
4. Customer membayar.
5. Customer menunjukkan bukti pembayaran.
6. Kasir menekan tombol Tandai Sudah Dibayar.
7. Sistem menyimpan payment dengan status Paid.
8. Transaksi masuk laporan QRIS.

---

## 8.4 Alur QRIS Dinamis

1. Kasir memilih QRIS dinamis.
2. Sistem request QR dinamis ke payment gateway.
3. Payment gateway mengembalikan QR payload.
4. Sistem menampilkan QR.
5. Customer membayar.
6. Payment gateway mengirim webhook.
7. Backend memverifikasi signature.
8. Sistem mengubah status payment dan sale menjadi Paid.
9. POS menerima update real-time.

---

## 8.5 Alur Tutup Shift

1. Kasir membuka menu shift.
2. Kasir memilih Tutup Shift.
3. Sistem menampilkan total cash expected dan payment mix.
4. Kasir mengh
