**Commit 1 Reflection notes**  
Setelah membaca dokumentasi, saya memahami bahwa fungsi handle_connection() berguna untuk membaca dan menampilkan HTTP request yang dikirim browser ke sever. Pada fungsi, Buffreader digunakan untuk membungkus TcpStream agar bisa dibaca baris per baris, lalu .lines() mengubah stream menjadi iterator per baris, take_while() berhenti saat menemukan baris kosong (tanda akhir HTTP header), dan .collect() mengumpulkan semua baris ke dalam vec String.

**Commit 2 Reflection notes**  
fungsi handle_connection() di commit kedua ini diupdate  agar bisa mengirim respons HTML kembali ke browser. 
fs::read_to_string("hello.html") membaca isi file HTML menjadi String. Lalu Respons yang dikirim harus mengikuti format HTTP, yaitu:
- Status line: HTTP/1.1 200 OK
- Header Content-Length berisi panjang isi konten
- Baris kosong (\r\n\r\n) sebagai pemisah header dan body
- Diikuti isi HTML sebagai body
Tanpa Content-Length, browser tidak tahu seberapa banyak data 
yang harus dibaca, sehingga halaman bisa tidak tampil dengan benar.  

Berikut hasil ketika di browser:  
![Commit 2 screen capture](/assets/images/commit2.png)

**Commit 3 Reflection notes**  
Pada milestone 3, server diperbarui agar bisa membedakan request yang masuk dan memberikan respons yang sesuai, dimana Baris pertama HTTP request (request_line) dibaca untuk mengetahui path yang diminta. Jika GET / HTTP/1.1 maka server mengembalikan hello.html dengan status 200 OK, selain itu mengembalikan 404.html dengan status 404 NOT FOUND. Untuk Refactoring dilakukan dengan memisahkan penentuan status_line dan filename ke dalam satu blok if/else, sehingga kode untuk membangun dan mengirim response cukup ditulis sekali.

**Commit 4 Reflection notes**  
Pada milestone 4, kita menambahkan route /sleep yang mensimulasikan proses lambat dengan thread::sleep(Duration::from_secs(10)). Sehingga, Ketika dua tab browser dibuka bersamaan, satu mengakses /sleep dan 
satu mengakses /, tab kedua harus menunggu tab pertama selesai terlebih dahulu baru bisa mendapat respons. Hal ini terjadi karena server masih berjalan di single-thread, sehingga hanya bisa menangani satu request dalam satu waktu. Selama thread utama 
sedang sleep menangani /sleep, request lain yang masuk harus mengantri. Dari simulasi ini, kita dapat melihat seberapa terbatasnya single-threaded di dunia nyata ketika ada banyak user yang mengakses secara bersamaan.

