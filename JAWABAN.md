**Latihan 1**

Eksperimen 1:
Buka /test/view (apa yang muncul?) = Muncul teks besar "Ini dari @Controller"
Buka /test/text (apa yang muncul?) = Muncul teks langsung di browser "Ini dari @Controller + @ResponseBody"

Kesimpulan:
@Controller tanpa @ResponseBody -> return nama template
@Controller dengan @ResponseBody -> return data langsung


Eksperimen 2:
Apakah berhasil? Tidak
HTTP Status Code: 500
Error Message:
This application has no explicit mapping for /error, so you are seeing this as a fallback.

Sun Mar 01 10:55:03 GMT+07:00 2026
There was an unexpected error (type=Internal Server Error, status=500).

Kesimpulan:
Jika Controller return nama view yang tidak ada, Spring akan mengembalikan error 500
(Internal Server Error)

Karena template/view yang direturn tidak ditemukan oleh template engine.


Eksperimen 3:
/greet = Selamat Pagi, Mahasiswa! (dan tombol Home)
/greet?name=Budi = Selamat Pagi, Budi! (dan tombol Home)
/greet?name=Budi&waktu=Siang = Selamat Siang, Budi! (dan tombol Home)
/greet/Ani = Halo, Ani! (dan tombol Home)
/greet/Ani/detail = Halo, Ani! (dan tombol Home)
/greet/Ani/detail?lang=EN = Hello, Ani! (dan tombol Home)

Pertanyaan:
1. URL mana yang pakai @RequestParam? =
- /greet
- /greet?name=Budi
- /greet?name=Budi&waktu=Siang

2. URL mana yang pakai @PathVariable? =
- /greet/Ani

3. URL mana yang pakai keduanya? =
- /greet/Ani/detail?lang=EN


Pertanyaan Refleksi
1. Apa perbedaan antara @Controller dan @RestController? Dalam kasus apa kamu pakai masing-masing?
@Controller = return dianggap sebagai nama view (HTML/template)
Dipakai untuk aplikasi web yang menampilkan halaman

@RestController = return dianggap sebagai data langsung (JSON/text)
Dipakai untuk membuat REST API

2. Perhatikan bahwa template product/list.html dipakai oleh 3 endpoint berbeda (list all, filter by category, search).
Apa keuntungannya membuat template yang reusable seperti ini?
- Mengurangi duplikasi kode
- Mudah maintenance
- Lebih rapi
- Efisiensi dan Scalable

3. Kenapa Controller inject ProductService (bukan langsung akses data di ArrayList)?
Apa yang terjadi kalau Controller langsung manage data?
Controller inject Service supaya bersih, scalable, dan maintainable
Kalau langsung kelola data -> aplikasi cepat berantakan saat membesar.

4. Apa perbedaan model.addAttribute("products", products) dengan return products langsung seperti di @RestController?
model.addAttribute =
- Tampilan HTML
- Dipakai template
- Web MVC
return products =
- Kirim data (JSON)
- Langsung response
- REST API

5. Jika kamu buka http://localhost:8080/products/abc (ID bukan angka), apa yang terjadi? Kenapa?
Akan muncul error 400 (Bad Request) Karena:
@PathVariable Long id (Spring mau angka), tapi "abc" adalah String.
Sehingga Spring mencoba mengonversi "abc" menjadi Long, tetapi konversi gagal sehingga
Spring mengembalikan 400 Bad Request.

6. Apa keuntungan pakai @RequestMapping("/products") di level class dibanding menulis full path di setiap @GetMapping?
Keuntungan memakai @RequestMapping("/products") di level class:
- Mengurangi Duplikasi
- Lebih mudah maintenance
- Struktur URL lebih rapi
- Mengurangi risiko error

7. Dalam lab ini, kata "Model" muncul dalam beberapa konteks berbeda. Sebutkan minimal 2 arti yang berbeda dan jelaskan perbedaannya.
1. Model sebagai parameter
- Untuk kirim data ke View
- Bagian dari alur MVC
- Sifatnya sementara
2. Model sebagai class data
- Representasi data
- Bagian dari domain
- Struktur data aplikasi


**Latihan 2**

Eksperimen 1:
Apakah error? Ya
Error message:
This application has no explicit mapping for /error, so you are seeing this as a fallback.

Sun Mar 01 12:27:32 GMT+07:00 2026
There was an unexpected error (type=Internal Server Error, status=500).

Kesimpulan:
Jika nama fragment salah, Thymeleaf akan melempar error karena fragment tidak ditemukan.

Eksperimen 2:
Menghapus th = CSS bekerja? Ya
Path yang salah =
- Halaman error? Tidak
- CSS diterapkan? Tidak

Kesimpulan: 
th:href="@{}" lebih baik karena secara otomatis menyesuaikan context path aplikasi
Jika file CSS tidak ada, halaman akan tetap tampil tapi tanpa styling CSS.