Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

- RPC unary adalah yang paling sederhana di mana klien mengirimkan satu permintaan ke server dan mendapatkan satu respons balik. 

- RPC streaming server adalah di mana klien mengirimkan satu permintaan ke server dan mendapatkan stream atau banyak data pada beberapa respons. Klien membaca dari stream yang dikembalikan sampai tidak ada lagi data yang dikirim. Ini cocok untuk skenario di mana server perlu mengirim banyak data (seperti file besar atau video streaming) ke klien.

- RPC bi-directional streaming adalah dimana klien dan server mengirim serangkaian pesan menggunakan read-write. Kedua _stream_ beroperasi secara independen, sehingga klien dan server dapat membaca dan menulis dalam urutan apa pun yang mereka suka. Ini cocok untuk skenario di mana klien dan server perlu mengirim banyak data satu sama lain secara independen seperti pada aplikasi obrolan.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Dalam penerapan gRPC di Rust, beberapa aspek keamanan yang krusial mencakup autentikasi, izin, dan enkripsi data. Autentikasi dan izin diperlukan untuk setiap komponen data dalam gRPC guna menjamin keamanannya. Cara autentikasi seperti penggunaan SSL/TLS atau model token perlu terintegrasi dan divalidasi secara cermat, sedangkan logika izin harus memastikan penerapan kebijakan kontrol akses yang efisien. Selain itu, penerapan enkripsi pada setiap data yang dikirim baik oleh server maupun klien menjadi kebutuhan penting untuk menjaga kerahasiaan data. Enkripsi data, yang diterapkan melalui SSL/TLS, bertujuan untuk melindungi saluran komunikasi, dilengkapi dengan validasi input yang kuat untuk mencegah kerentanan yang umum terjadi. 

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Mengelola concurrency, penanganan kesalahan, urutan pesan, alokasi sumber daya komputasi, keamanan, skalabilitas, dan latensi adalah faktor-faktor penting yang perlu diperhatikan saat menerapkan aliran data dua arah di gRPC. Memastikan komunikasi yang efisien antara klien dan server, menjaga integritas pesan, menangani masalah concurrency seperti race condition, dan mengelola sumber daya dengan efisien sambil tetap memperhatikan keamanan dan skalabilitas merupakan hal-hal yang sangat vital.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Menggunakan tokio_stream::wrappers::ReceiverStream memiliki keunggulan seperti pengaliran asinkron Namun, penggunaan ini juga membawa kompleksitas tambahan dalam kode, terutama bagi yang belum terbiasa dengan pemrograman asinkron Rust, dan memerlukan penanganan kesalahan yang lebih teliti untuk memastikan komunikasi yang benar. Selain itu, mengelola asinkronisasi dapat menjadi tantangan tersendiri yang berpotensi menyebabkan terminasi prematur.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Untuk menjamin kemudahan pengembangan dan pemeliharaan layanan gRPC yang efisien dalam Rust, disarankan untuk menggunakan library yang sudah ada guna mengurangi duplikasi kode dan memudahkan pemeliharaan. Selain itu, mengadopsi desain modular dengan mengatur kode ke dalam modul terpisah yang jelas dapat membantu meningkatkan readability dan maintenance.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Agar dapat menangani logika pemrosesan pembayaran yang lebih kompleks, peningkatan pada implementasi MyPaymentService dapat dilakukan dengan mengubah metode process_payment menjadi server streaming. Hal ini memungkinkan pengiriman respons secara bertahap kepada klien, sehingga memfasilitasi pengiriman data yang lebih kompleks dengan lebih efisien.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Penggunaan gRPC sebagai protokol komunikasi memberikan dampak signifikan pada arsitektur dan desain sistem terdistribusi. Kemampuan gRPC yang merupakan language agnostic menjadi salah satu fitur utama. gRPC memungkinkan komunikasi berjalan seolah-olah terjadi secara native yang sebenarnya kode tersebut ada pada machine lain. gRPC mendukung arsitektur moderen salah satunya adalah arsitektur microservice. 

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

HTTP/2 memiliki keunggulan dibandingkan dengan HTTP/1.1 dan HTTP/1.1 dengan WebSocker untuk REST API. Salah satu fiturnya adalah multiplexing untuk menangani permintaan secara bersamaan. HTTP/2 memungkinkan untuk satu request ke server digunakan untuk mendapatkan banyak data sekaligus yang memungkinkan untuk mempercepat permintaan. Namun, implementasi dan pemeliharaan HTTP/2 dapat menjadi kompleks. Terdapat tantangan terkait kompatibilitas browser pula.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

REST API dikenal karena sifatnya yang sederhana, memanfaatkan metode HTTP standar. Namun, meskipun kelebihan tersebut, REST API tidak mendukung komunikasi real-time, sering kali bergantung pada teknik seperti polling atau long-polling yang dapat menimbulkan kompleksitas dan latensi. Sebaliknya, gRPC menyediakan fitur streaming dua arah yang memungkinkan klien dan server untuk saling mengirim data secara independen. Hal ini membuatnya lebih cocok untuk skenario yang membutuhkan respons real-time dan berpotensi meningkatkan kelincahan sistem. Meskipun mungkin lebih kompleks, kemampuan streaming dua arah gRPC dapat menghasilkan kinerja yang lebih baik dan memberikan manfaat komunikasi real-time dalam kasus penggunaan tertentu dibandingkan dengan REST API.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

gRPC dengan Protocol Buffer menawarkan kompatibilitas dan mendukung perangkat-perangkat yang lebih baik berkat pendekatannya yang berbasis pada skema. Di sisi lain, penggunaan JSON dalam muatan REST API memberikan fleksibilitas dan kemudahan dibaca di berbagai bahasa dan platform, meskipun dengan kemungkinan biaya tambahan. Pemilihan antara keduanya bergantung pada kebutuhan spesifik, dengan gRPC lebih menguntungkan untuk skenario yang membutuhkan jenis data yang jelas, kinerja yang tinggi, dan menjamin bahwa client support komunikasi dengan gRPC yang mana belum disupport browser sekarang. Sedangkan JSON dalam REST API sesuai untuk situasi yang mengutamakan fleksibilitas dan kemudahan dibaca.