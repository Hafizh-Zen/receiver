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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Why do we use RwLock<> instead of Mutex<>?
In this tutorial, we use RwLock<> to manage access to the Vec of notifications because it allows multiple threads to read the data at the same time, as long as no one is writing to it. This is a big advantage in situations where reads happen more frequently than writes—like in this case—because it avoids unnecessary blocking and improves performance.

On the other hand, Mutex<> only allows one thread at a time, even for reading. That means if several threads just want to read data, they’d still have to take turns, which can slow things down in read-heavy scenarios.

2. Why doesn’t Rust allow direct mutation of static variables like Java does?
Rust takes a stricter approach to memory safety compared to languages like Java. It enforces rules around ownership and borrowing to prevent issues like data races. Allowing direct modification of static variables—especially in multi-threaded environments—would break these guarantees and could lead to unpredictable behavior.

Instead, Rust requires the use of safe wrappers like RwLock<> or Mutex<> to manage shared mutable state. This ensures that all access is properly synchronized, making your program more stable and less prone to hard-to-find bugs.

#### Reflection Subscriber-2

1. Did you explore anything beyond the tutorial steps, like src/lib.rs?
Yeah, I checked out src/lib.rs to see how the app is set up behind the scenes. I noticed it uses the AppConfig struct along with dotenvy and Figment to handle environment variables. It’s a pretty neat setup because it keeps the config flexible—you don’t have to hardcode anything, and it’s easy to switch between environments.

2. How does the Observer pattern help with multiple subscribers? And what about running more than one main app instance?
The Observer pattern really helps here—it makes adding new subscribers simple. Each receiver just registers itself, and it’ll automatically get notifications. You don’t need to change the core logic, which keeps things clean and scalable.

Running multiple main instances (the publishers) is a bit trickier, though. You’d need to make sure they’re in sync so that all subscribers still get the right updates. But as long as each receiver is connected to the correct publisher, it’s doable.

3. Did you write your own tests or update the Postman collection? Was it useful?
Yeah, I wrote some tests for NotificationRepository and NotificationService, just to make sure everything worked like it should and to catch any weird edge cases. I also cleaned up the Postman collection by adding clearer descriptions and example requests. It definitely helped, especially when working with others—it made testing and debugging way smoother.