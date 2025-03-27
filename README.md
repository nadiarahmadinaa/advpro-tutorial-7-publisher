# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Sesuai dengan konsep design pattern Observer, banyak subscriber dapat berlangganan ke suatu publisher. Namun, jenis dari subscriber ini bervariasi atau tidaknya tergantung environment yang kita bangun. Jika BambangShop ingin memperluas subscribernya dengan melayani berbagai macam subscriber class, maka penting untuk melakukan implementasi interface atau trait di Rust. Hal ini untuk menjamin berjalannya fungsionalitas notifikasi pada semua jenis class subscriber. Namun, jika BambangShop memiliki jenis subscriber yang monoton, implementasi single struct saja sudah cukup.
2. Dalam implementasi Vec atau list, ketika kita ingin mencari id atau url maka harus iterasi satu-persatu seluruh isi Vec. Dalam dataset yang kecil, ini tidak menjadi masalah besar. Namun dalam dataset yang besar seperti kasus dimana BambangShop memiliki subscriber banyak, maka akan memakan waktu yang sangat lama (O(n) time complexity) untuk menemukan id atau url ini. Dengan DashMap, kita bisa melakukan efisiensi waktu menjadi O(1) time complexity. Oleh karena itu, sangat disarankan untuk beralih ke DashMap, khususnya apabila kita ingin mempersiapkan untuk skalabilitas yang besar.
3. Sebaiknya kita tetap mengimplementasikan DashMap, karena DashMap di Rust sudah bersifat thread safe. Lain halnya dengan Singleton Pattern yang membuka aksesnya secara global dan tidak thread safe. Jika kita mengimplementasikan singleton ini dengan lock untuk mendapatkan keamanan yang serupa dengan DashMap, extra measure ini akan memperlambat program. Maka dari itu, singleton tidak efektif untuk digunakan.

#### Reflection Publisher-2
1. Dalam MVC biasa, business logic dan penyimpanan data memang berada satu package yang sama, yaitu model. Namun dalam project ini kita memisahkan Repository dan Service sebagai bentuk kepatuhan terhadap SRP. SRP adalah Single Responsibility Principle, salah satu bagian dari SOLID principle. Prinsip ini dilakukan untuk meningkatkan readability dan maintainability dari kode. Scalability dan testing juga dipermudah dengan mekanisme ini, karena memberi fleksibilitas lebih tanpa constraint yang terlalu berat.

2. Jika kita hanya menggunakan Model sebagai storage data dan business logic, maka ini akan menyulitkan development kedepannya. Sebagai contoh, jika ketiga model ini terikat dan berkomunikasi secara langsung, seperti Program mengirim notifikasi ke subscriber, maka terdapat resiko dimana suatu perubahan dapat merusak model lain juga. Maka dari itu, kita implementasikan service agar dependency antar model tidak begitu kuat sehingga berkemungkinan kecil untuk saling merusak.

3. Postman bermanfaat bagi saya untuk melakukan testing API. Terdapat banyak fitur postman seperti fitur yang membolehkan kita memasukkan parameter, mengubah HTTP Header, dan mengimplementasikan cookies. Dengan postman, testing API dipermudah karena kita tidak harus melakukannya by terminal seperti menggunakan cURL. Selain itu, kita bisa share API collection seperti yang dilakukan pada tutorial ini sehingga kolaborasi dengan tim dapat berjalan lebih mudah dan cepat.

#### Reflection Publisher-3
