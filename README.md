# Android-Rest-Messenger

# Native Android RESTful Chat Service

I built this native Android messaging app to demonstrate a complete mobile client-server architecture. It handles asynchronous REST API consumption, real-time JSON payload synchronization, and Android background lifecycle management.

## System Overview
This project is a concurrent, multi-room chat application. The mobile client is built natively in Java for Android, and it talks to a custom backend web service powered by Jersey (JAX-RS) and a Netty HTTP Server. My main focus here was keeping the architecture decoupled ensuring the Android UI thread stays completely fluid while background services handle the heavy lifting of network polling, data serialization, and state synchronization.

## Tech Stack & Architecture
* **Frontend Client:** Native Android (Java), Android SDK, Android Architecture Components
* **Backend Service:** Java, Jersey (RESTful Web Services), Netty (NIO client-server framework)
* **Data Exchange:** REST Architecture, strictly-typed JSON payloads
* **Development Tools:** Android Studio, Postman (for API contract validation)

## Core Technical Features
* **Asynchronous REST Integration:** The client makes non-blocking HTTP requests to push and fetch message payloads. The raw JSON responses are mapped directly into strongly typed Java POJOs to keep the data structures predictable and prevent runtime crashes.
* **Multi-Room Concurrency:** The app supports dynamic message routing to isolated chat rooms (like 'default', 'Ferrari', or 'BYD'), keeping state strictly separated between different chat contexts.
* **Service Lifecycle & Notifications:** I used Android's native Service components to run registration and message-syncing tasks in the background. Once the server handshake is successful, it dispatches system-level push notifications to the Android tray.
* **Geospatial Metadata:** The app hooks into the device's location services to append precise Latitude and Longitude coordinates to outgoing message payloads.

## Network & Data Flow (JSON Contract)
To keep the client-server communication reliable, the system relies on a strict JSON data contract. Here is a sample of the payload structure handled by the network layer:

```json
{
  "messageId": "8f2a1b94-uuid-4c8d",
  "chatroom": "default",
  "sender": "Sayed",
  "message": "Hello! I would like to buy a Ferrari.",
  "timestamp": "2026-09-24T18:30:00Z",
  "latitude": 40.744999,
  "longitude": -74.023937
}
```
## Local Setup & installation
If you want to run this locally, follow these steps:

1) Clone the repository to your local machine:
```git clone https://github.com/yourusername/android-rest-messenger.git```
2) Start the backend service first. Run the packaged JAR file from your terminal to initialize the Netty server on localhost:8080:
java -jar ChatServer-REST.jar
3) Open the Android project folder in Android Studio.
4) Sync the Gradle project and deploy the app to an Android Emulator (API 30+ recommended).
5) Register a username, join a chat room, and start sending messages.

## How to Contribute

If you'd like to contribute to this project, whether it's optimizing the network polling, adding new UI features, or refactoring the backend, feel free to submit a pull request.

Fork the repository using the "Fork" button at the top right of this page.

Clone your fork locally:
```git clone https://github.com/yourusername/android-rest-messenger.git```

Create a new branch for your feature or bugfix:
```git checkout -b feature/your-feature-name```

Commit your changes with descriptive messages:
```git commit -m "Add feature X"```

Push your branch to your forked repository:
```git push origin feature/your-feature-name```

Open a Pull Request from your fork to the main branch of this repository. I will review it as soon as possible!

## Architectural Takeaways
The biggest engineering challenge of this project was strict thread management. By offloading REST consumption and JSON deserialization to background workers, the application completely avoids NetworkOnMainThreadException crashes and UI lag. Building this solidified my habit of treating the API and validating it via Postman or browser inspection—as the source of truth before ever touching the client interface.

