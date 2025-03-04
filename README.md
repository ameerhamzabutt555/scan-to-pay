# Scan to Pay Project with Docker, WebSocket, and Redis Integration

This is a NestJS project that demonstrates how to integrate **Docker**, **WebSocket**, and **Redis**. The project includes a real-time notification system using WebSocket and Redis for caching or pub/sub functionality.

---

## **Features**
1. **NestJS**: A progressive Node.js framework for building efficient and scalable server-side applications.
2. **Docker**: Containerization for easy deployment and development.
3. **WebSocket**: Real-time communication for notifications.
4. **Redis**: Integrated for caching or pub/sub messaging.

---

## **Prerequisites**
Before you begin, ensure you have the following installed on your machine:
- [Node.js](https://nodejs.org/) (v18 or later)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Redis](https://redis.io/) (optional, if running locally without Docker)

---

## **Getting Started**

### **1. Clone the Repository**
Clone the repository to your local machine:
```bash
git clone https://github.com/your-username/your-repo-name.git
cd scan-to-pay
```

---

### **2. Install Dependencies**
Install the required dependencies using npm:
```bash
npm install
```

---

### **3. Set Up Environment Variables**
Create a `.env` file in the root directory and add the following environment variables:
```plaintext
# Application Port
PORT=3000

# Redis Configuration
REDIS_HOST=redis
REDIS_PORT=6379
```

---

### **4. Build and Run with Docker**
The project includes a `Dockerfile` for easy setup.

#### **Build the Docker Image**
Build the Docker image for the NestJS application:
```bash
docker-compose build
```

#### **Run the Application**
Start the application and Redis using Docker Compose:
```bash
docker-compose up
```

#### **Access the Application**
- The NestJS application will be running at `http://localhost:3000`.
- Redis will be running on `redis://localhost:6379`.

---

### **5. Run Without Docker**
If you prefer to run the application locally without Docker:

#### **Start Redis**
Ensure Redis is installed and running on your machine:
```bash
redis-server
```

#### **Start the NestJS Application**
Run the application in development mode:
```bash
npm run start:dev
```

---

## **Project Structure**
```
src/
├── auth/               # Authentication module
├── booking/            # Booking module
├── notification/       # Notification module (WebSocket)
├── redis/              # Redis integration
├── app.module.ts       # Root application module
├── main.ts             # Application entry point
```

---

## **WebSocket Implementation**
The project includes a WebSocket gateway for real-time notifications. Here's how it works:

1. **Join a Room**:
   - Clients can join a room using the `joinRoom` event:
     ```json
     {
       "event": "joinRoom",
       "data": "USER_ID"
     }
     ```

2. **Send Notifications**:
   - Notifications are sent to specific users using the `sendNotification` method:
     ```typescript
     this.notificationGateway.sendNotification(userId, message);
     ```

3. **Receive Notifications**:
   - Clients listen for the `newNotification` event:
     ```json
     {
       "event": "newNotification",
       "data": {
         "message": "Booking created successfully. Booking ID: 123"
       }
     }
     ```

---

## **Redis Integration**
Redis is used for caching or pub/sub messaging. The project includes a Redis module with the following features:

1. **Redis Service**:
   - The `RedisService` provides methods for interacting with Redis:
     ```typescript
     async set(key: string, value: string): Promise<void>;
     async get(key: string): Promise<string | null>;
     async publish(channel: string, message: string): Promise<void>;
     async subscribe(channel: string, callback: (message: string) => void): Promise<void>;
     ```

2. **Pub/Sub Example**:
   - Publish a message:
     ```typescript
     await this.redisService.publish('channel', 'Hello, Redis!');
     ```
   - Subscribe to a channel:
     ```typescript
     await this.redisService.subscribe('channel', (message) => {
       console.log('Received message:', message);
     });
     ```

---

## **Testing**
### **WebSocket Testing**
Use tools like **Postman** (v10+), **wscat**, or **WebSocket King** to test WebSocket events.

#### Example: Using Postman
1. Connect to `ws://localhost:3000`.
2. Send the `joinRoom` event with a `userId`.
3. Trigger a notification from the backend.
4. Receive the `newNotification` event in Postman.

---

## **Contributing**
Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeatureName`).
3. Commit your changes (`git commit -m 'Add some feature'`).
4. Push to the branch (`git push origin feature/YourFeatureName`).
5. Open a pull request.

---

## **License**
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## **Support**
If you encounter any issues or have questions, feel free to open an issue on GitHub or contact the maintainers.

---

Enjoy building with NestJS, Docker, WebSocket, and Redis! 🚀
