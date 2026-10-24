# Praktikum Pemrograman Berorientasi Objek

Repository praktikum **Pemrograman Berorientasi Objek (PBO)** menggunakan **Java** dengan studi kasus **Laundry Apps**. Proyek memuat latihan class dan object, getter/setter, interface DAO, akses database melalui JDBC, serta antarmuka desktop menggunakan Swing.

| Identitas | Keterangan |
| --- | --- |
| Nama | Habibi Putra Rizqullah |
| NIM | 2411531001 |
| Kelas | B |
| Mata Kuliah | Pemrograman Berorientasi Objek |
| Repository | [PrakPBO_B_2411531001](https://github.com/habibiputrar/PrakPBO_B_2411531001) |

## Materi dan Implementasi

| Bagian | Isi |
| --- | --- |
| Class dan object | Pemodelan pengguna, pelanggan, layanan, dan pesanan laundry. |
| Getter dan setter | Pengisian serta pembacaan atribut pada class model. |
| Interface DAO | Kontrak operasi penyimpanan, pembacaan, perubahan, dan penghapusan data. |
| Repository JDBC | Implementasi query CRUD pengguna dan pelanggan pada MySQL. |
| Form login | Login latihan melalui `User.login()`, kemudian membuka menu utama. |
| Menu utama | Tampilan menu pesanan, layanan, pelanggan, pengguna, laporan, dan profil. |
| Form pengguna | Input nama, username, dan password; tombol Save, Update, Delete; serta pemilihan data melalui tabel. |
| Table model | Penyajian daftar pengguna pada `JTable` melalui `AbstractTableModel`. |
| Latihan konsol | Contoh penggunaan object pelanggan, layanan, dan pesanan pada package `model` dan `Tugas`. |

## Struktur Proyek

```text
.
├── src/
│   ├── confg/
│   │   ├── Database.java       # Koneksi JDBC ke MySQL
│   │   └── TestKoneksi.java    # Program pemeriksaan koneksi
│   ├── DAO/                   # Interface DAO dan repository
│   ├── model/                 # User, Costumer, Service, dan Order
│   ├── Table/
│   │   └── TableUser.java      # Model tabel pengguna
│   ├── ui/
│   │   ├── LoginFrame.java
│   │   ├── MainFrame.java
│   │   └── UserFrame.java
│   └── Tugas/                 # Latihan object berbasis konsol
├── Laporan Pratikum/          # Laporan PDF pekan 1–3
├── .classpath                 # Konfigurasi build path Eclipse
├── .project                   # Identitas proyek Eclipse
├── .gitignore
└── README.md
```

Penamaan `confg`, `Costumer`, dan `Laporan Pratikum` mengikuti nama yang digunakan pada repository.

## Teknologi dan Kebutuhan

- **JDK 17**, sesuai konfigurasi pada `.classpath`.
- **Java Swing dan AWT** untuk antarmuka desktop.
- **JDBC dan MySQL** untuk operasi database.
- **MySQL Connector/J**, dengan referensi `mysql-connector-j-9.0.0.jar` pada konfigurasi proyek.
- **Eclipse** atau editor Java lain; proyek juga dapat dikompilasi melalui terminal.

File JAR Connector/J belum disertakan. Tambahkan driver ke classpath mengikuti [panduan resmi MySQL Connector/J](https://dev.mysql.com/doc/connector-j/en/connector-j-binary-installation.html).

## Menjalankan Proyek

### 1. Unduh Repository

```bash
git clone https://github.com/habibiputrar/PrakPBO_B_2411531001.git
cd PrakPBO_B_2411531001
```

### 2. Buka melalui Eclipse

1. Import folder repository sebagai proyek Java yang sudah ada.
2. Gunakan JDK 17 pada konfigurasi proyek.
3. Periksa **Java Build Path** dan sesuaikan referensi JAR MySQL Connector/J dengan lokasi file pada komputer sendiri.
4. Jalankan class yang memiliki method `main` sesuai tabel entry point di bawah.

Konfigurasi `.classpath` saat ini menggunakan path absolut JAR pada komputer pengembang, sehingga lokasi tersebut perlu disesuaikan saat proyek dibuka di komputer lain.

### 3. Siapkan Database untuk Fitur JDBC

Aktifkan server MySQL dan siapkan database `laundry_apps`. Pengaturan koneksi berada pada [src/confg/Database.java](src/confg/Database.java); sesuaikan host, nama database, username, dan password dengan lingkungan lokal.

Repository belum menyertakan dump SQL. Struktur tabel yang dibutuhkan oleh query repository adalah:

| Tabel | Kolom yang Digunakan | Kebutuhan dari Kode |
| --- | --- | --- |
| `user` | `id`, `name`, `username`, `password` | `UserRepo.save()` tidak mengirim nilai ID, sehingga database perlu menghasilkan ID secara otomatis. |
| `costumer` | `id`, `nama`, `alamat`, `nomor_hp` | `CostumerRepo.save()` mengirim seluruh kolom, termasuk ID pelanggan. |

Siapkan tipe data dan key tabel sesuai query pada [UserRepo.java](src/DAO/UserRepo.java) serta [CostumerRepo.java](src/DAO/CostumerRepo.java). Setelah driver dan database siap, jalankan `confg.TestKoneksi` sebelum membuka form pengguna.

### 4. Kompilasi melalui Terminal

Perintah berikut dijalankan dari root repository:

```bash
javac -encoding UTF-8 -d bin -sourcepath src src/ui/LoginFrame.java src/ui/UserFrame.java src/confg/TestKoneksi.java src/model/Order.java
```

Compiler menemukan class pendukung melalui `-sourcepath src` dan menyimpan hasilnya ke folder `bin`.

Contoh menjalankan latihan object pesanan tanpa database:

```bash
java -cp bin model.Order
```

Untuk mencoba form login:

```bash
java -cp bin ui.LoginFrame
```

Untuk menjalankan fitur database, sertakan JAR Connector/J pada classpath. Contoh berikut untuk Windows; ganti path JAR dengan lokasi file yang sebenarnya:

```powershell
java -cp "bin;C:/path/mysql-connector-j-9.0.0.jar" confg.TestKoneksi
java -cp "bin;C:/path/mysql-connector-j-9.0.0.jar" ui.UserFrame
```

Pada Linux atau macOS, pemisah classpath adalah `:`. Program Swing memerlukan lingkungan desktop.

## Entry Point

| Class | Kegunaan | Membutuhkan Database |
| --- | --- | --- |
| [ui.LoginFrame](src/ui/LoginFrame.java) | Membuka form login latihan | Tidak |
| [ui.MainFrame](src/ui/MainFrame.java) | Membuka tampilan menu utama secara langsung | Tidak |
| [ui.UserFrame](src/ui/UserFrame.java) | Menampilkan dan mengelola data pengguna | Ya |
| [confg.TestKoneksi](src/confg/TestKoneksi.java) | Memeriksa koneksi JDBC ke MySQL | Ya |
| [model.Order](src/model/Order.java) | Menjalankan contoh object pesanan di konsol | Tidak |

## Cakupan Implementasi

Login menggunakan akun latihan yang didefinisikan pada method `login()` di [model/User.java](src/model/User.java), terpisah dari tabel pengguna di database.

Menu pada `MainFrame` belum dihubungkan ke seluruh form, sehingga form CRUD pengguna dijalankan melalui `ui.UserFrame`. Tombol Cancel pada form pengguna dan aksi Keluar pada menu utama belum memiliki implementasi. `ServiceDAO` sudah tersedia, sedangkan `ServiceRepo` masih berupa kerangka class.

Repository ini mendokumentasikan tahap latihan PBO dan pengembangan aplikasi laundry yang tersedia pada source code.

## Laporan Praktikum

| Pekan | Laporan |
| --- | --- |
| 1 | [Laporan Praktikum PBO Pekan 1](Laporan%20Pratikum/LAPORAN%20PRATIKUM%20PBO%20PEKAN%201.pdf) |
| 2 | [Laporan Praktikum PBO Pekan 2](Laporan%20Pratikum/LAPORAN%20PRATIKUM%20PBO%20PEKAN%202.pdf) |
| 3 | [Laporan Praktikum PBO Pekan 3](Laporan%20Pratikum/LAPORAN%20PRATIKUM%20PBO%20PEKAN%203.pdf) |
