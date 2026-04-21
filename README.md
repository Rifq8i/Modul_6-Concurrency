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