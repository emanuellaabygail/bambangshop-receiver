# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1
>In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Pada kasus ini, kita menggunakan RwLock<> untuk menyinkronkan penggunaan Vec dari Notifications karena memungkinkan akses baca secara paralel oleh beberapa thread, sementara akses tulis tetap eksklusif. Ini penting karena sebagian besar waktu kita hanya membaca notifikasi, sehingga menggunakan RwLock<> meningkatkan performa dengan memungkinkan banyak pembacaan bersamaan. Jika kita menggunakan Mutex<>, baik operasi baca maupun tulis akan saling mengunci (blocking) satu sama lain. Artinya, bahkan saat hanya membaca data, thread lain tetap harus menunggu. Hal ini mengurangi efisiensi karena pembacaan notifikasi menjadi lambat jika ada banyak thread yang mengakses. Dengan demikian, RwLock<> lebih tepat digunakan dalam kasus ini karena mengoptimalkan akses baca yang lebih sering terjadi.

>In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Berbeda dengan Java yang memungkinkan kita memodifikasi variabel static melalui fungsi static, Rust tidak mengizinkan hal itu secara langsung karena Rust mengutamakan safety dalam pengelolaan data bersama (shared state). Variabel static di Rust bersifat immutable secara default, dan jika kita ingin membuatnya dapat diubah, kita harus secara eksplisit menangani synchronization agar tetap thread-safe. Rust tidak mengizinkan perubahan langsung karena tanpa pengamanan, data static yang dapat diubah akan rentan terhadap kondisi balapan (race conditions) ketika diakses oleh banyak thread. Oleh karena itu, kita perlu menggunakan synchronization primitives seperti Mutex<>, RwLock<>, atau menggunakan pustaka seperti lazy_static untuk menginisialisasi variabel secara aman. Pendekatan ini memastikan bahwa modifikasi data static tetap aman dan tidak menyebabkan masalah pada multi-threading.

#### Reflection Subscriber-2
>Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

I have to be honest, I haven't lol sorry dont have enough time im already going crazy :]

>Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Observer pattern mempermudah penambahan subscriber karena setiap subscriber hanya perlu mengimplementasikan antarmuka (interface) yang ditentukan, tanpa memodifikasi kode subject (Main app). Dengan begitu, kita dapat menambahkan banyak Receiver secara dinamis tanpa mengubah struktur aplikasi secara keseluruhan. Namun, jika kita ingin menambahkan lebih dari satu instance dari Main app itu sendiri, tantangannya lebih besar. Hal ini karena Main app biasanya bertindak sebagai pusat pengelola notifikasi, dan menambahkan lebih banyak instance berarti kita harus memastikan koordinasi dan konsistensi data antar Main app tersebut. Hal ini memerlukan arsitektur yang lebih kompleks, seperti distributed systems atau sinkronisasi data secara global, sehingga tidak semudah menambah subscriber yang bersifat lebih modular.

>Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Again, I am very sorry but I haven't got any time to try T_T