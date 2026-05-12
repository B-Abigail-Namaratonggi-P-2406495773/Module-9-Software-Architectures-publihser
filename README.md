### Reflection

**a. How much data your publisher program will send to the message broker in one run?**

Berdasarkan implementasi kode pada file `main.rs` milik *publisher*, program akan mempublish atau mengirimkan tepat **5 buah pesan (*events*)** ke dalam *message broker* dalam satu kali eksekusi (*run*). Kelima data tersebut merupakan *event* dari *struct* `UserCreatedEventMessage` yang di-publish secara berurutan, masing-masing membawa data `user_id` (1 hingga 5) beserta `user_name` nya.

**b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?**

Kesamaan URL ini secara fundamental berarti bahwa baik *publisher* maupun *subscriber* melakukan koneksi ke ***instance message broker* (RabbitMQ) yang sama**.

Dalam arsitektur *event-driven*, agar komunikasi asinkronus dapat terjadi, pengirim pesan dan pendengar pesan harus berada di server yang sama. Jika URL-nya berbeda, berarti mereka terhubung ke server yang berbeda, sehingga pesan dari *publisher* tidak akan pernah ditangkap oleh *subscriber*. Dengan memakai URL `amqp://guest:guest@localhost:5672` di kedua sisi, kita memastikan bahwa *publisher* menaruh *event* ke antrean di lokal mesin port 5672, dan *subscriber* juga mendengarkan antrean dari tempat yang sama tersebut.

### Bukti Eksekusi RabbitMQ (Message Broker)

Berikut adalah *screenshot* dari *dashboard* RabbitMQ Management yang sedang berjalan secara lokal (via Docker) dan diakses melalui `http://localhost:15672`:

![img.png](img1.png)

### Hasil Eksekusi Publisher-Subscriber

![img.png](img.png)

**Apa yang terjadi pada konsol tersebut?**
Pada eksperimen ini, arsitektur *event-driven* berhasil dijalankan menggunakan **Publisher** dan **Subscriber** yang beroperasi secara bersamaan.

1. Konsol **Subscriber** dijalankan lebih dulu dan berada dalam mode *listening*, bertindak sebagai *consumer* yang *standby* menunggu pesan masuk dari *message broker* AMQP.
2. Konsol **Publisher** kemudian dieksekusi, di mana program ini berhasil mengirimkan (*dispatch*) 5 *event* (pesan) secara berurutan ke *message broker*.
3. Seperti yang terlihat pada *screenshot*, sesaat setelah Publisher mengirimkan rentetan *event* tersebut, Subscriber langsung menangkap dan memproses pesan-pesannya secara *real-time*.
