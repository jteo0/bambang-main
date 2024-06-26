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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Karena di Bambangshop hanya ada satu tipe subscriber (sama semua), Bambangshop tidak perlu menggunakan interface/trait. Interface/trait hanya diperlukan jika ada beberapa tipe subscriber yang bisa dibedakan menurut behavior/traitnya.

2. Jika id pada Product dan url pada Subscriber dianggap pasti unik, maka DashMap lebih baik untuk digunakan dibandingkan Vec. Dengan adanya id dan url yang unik, DashMap dapat menggunakannya sebagai key, sehingga bisa menjalankan lookup, insertion, dan deletion dengan lebih cepat dibandingkan Vec (kompleksitas rata O(1) dibandingkan O(n)). Vec juga bisa digunakan, tapi kalau id dan url unik, lebih efisien untuk menggunakan DataMap.

3. DashMap digunakan daripada HashMap karena DashMap bersifat thread-safe, dimana dapat diakses dan dimodifikasi oleh banyak thread secara bersamaan tanpa menyebabkan data race. Singleton Pattern merupakan design pattern yang memastikan adanya hanya satu instance dari suatu class. Karena Singleton Pattern tidak membuat data SUBSCRIBERS lebih thread-safe (tetap bisa diakses dan dimodifikasi dengan cara dimana menyebabkan data race), DashMap tetap harus digunakan.

#### Reflection Publisher-2
1. Menurut prinsip <i>Seperation of Concerns</i>, suatu program dibuat dengan cara setiap bagian memiliki satu functionalitas/<i>concern</i>. Ini diimplementasi karena meningkatkan modularity, mantainability, dan scalability program. Walaupun Model di MVC menangani data storage dan business logic, ini tidak mengukuti prinsip seperation of concerns. Daripada menggunakan Model seperti pada MVC, Service digunakan untuk menganani business logic dan Repository untuk menangani data storage. Model digunakan untuk mendefinisikan suatu class.

2. Jika hanya menggunakan Model, ini berarti bahwa semua fungsionalitas yang ditaruh di Repository dan Service akan dimasukkan ke Model. Ini menyebabkan Model menjadi lebih kompleks dan sulit untuk dipelihara karena setiap functionalitas berada di satu tempat dan error di satu tempat bersifat susah untuk ditemukan dan bisa mengefek bagian yang besar.

3. Postman menolong saya untuk menguji coba program yang telah dibuat. Fitur yang menurut saya paling berguna adalah fitur untuk memasukkan request dan melihat isinya dan outputnya dengan mudah.

#### Reflection Publisher-3
1. Bambangshop menggunakan varian Push Model, dimana ketika melakukan create/delete/public Product, semua subscriber yang telah subscribe pada product_type itu akan menerima notifikasi.

2. Keuntungan dari penerapan Pull Method pada Bambangshop adalah informasi subscriber tidak ter-expose ke publisher. Kekurangannya adalah Pull Model akan memakan banyak resource karena harus mengecek status publisher secara seterusan untuk adanya perubahan.

3. Jika Bambangshop tidak menggunakan multi-threading dalam proses notifikasi, prosesnya akan berjalan dengan lebih lambat karena notifikasi akan dikirimkan kepada semua subscriber secara satu persatu.
