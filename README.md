# Real-Time Chat Application

A full-stack real-time chat application built with Node.js, Express, MongoDB, Socket.IO, React, TypeScript, and TailwindCSS.

## 🚀 Features

### Backend
- ✅ Complete User CRUD operations
- ✅ JWT-based Authentication & Authorization
- ✅ Real-time messaging with Socket.IO
- ✅ Persistent chat history in MongoDB
- ✅ Analytics (total users and messages count)
- ✅ RESTful API architecture
- ✅ TypeScript for type safety
- ✅ Password hashing with bcrypt

### Frontend
- ✅ Real-time message updates
- ✅ User authentication (Login/Register)
- ✅ Live user join/leave notifications
- ✅ Chat statistics display
- ✅ Modern UI with TailwindCSS
- ✅ Responsive design
- ✅ TypeScript for type safety

## 🛠️ Tech Stack

### Backend
- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB with Mongoose
- **Real-time:** Socket.IO
- **Language:** TypeScript
- **Authentication:** JWT (jsonwebtoken)
- **Security:** bcryptjs for password hashing

### Frontend
- **Framework:** React 18
- **Language:** TypeScript
- **Styling:** TailwindCSS
- **Build Tool:** Vite
- **HTTP Client:** Axios
- **Real-time:** Socket.IO Client

## 📋 Prerequisites

Before you begin, ensure you have the following installed:
- **Node.js** (v18 or higher)
- **npm** or **yarn**
- **MongoDB** (local installation or MongoDB Atlas account)
- **Git**

## 📦 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/brokestocker1/task-manager.git
cd task-manager
```

### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create .env file from example
cp .env.example .env

# Update .env with your configuration
# Edit the .env file with your MongoDB URI and other settings
```

#### Backend Environment Variables (.env)

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/chat-app
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRE=7d
NODE_ENV=development
CORS_ORIGIN=http://localhost:5173
```

### 3. Frontend Setup

```bash
# Navigate to frontend directory (from root)
cd frontend

# Install dependencies
npm install

# Create .env file from example
cp .env.example .env

# Update .env with your configuration
```

#### Frontend Environment Variables (.env)

```env
VITE_API_URL=http://localhost:5000/api
VITE_SOCKET_URL=http://localhost:5000
```

## 🚀 Running the Application

### Start MongoDB

Make sure MongoDB is running on your system:

```bash
# If using local MongoDB
mongod

# Or if using MongoDB as a service
sudo systemctl start mongod
```

### Start Backend Server

```bash
# From the backend directory
cd backend
npm run dev
```

The backend server will start on `http://localhost:5000`

### Start Frontend Application

```bash
# From the frontend directory (open a new terminal)
cd frontend
npm run dev
```

The frontend application will start on `http://localhost:5173`

## 📚 API Documentation

### Authentication Endpoints

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user_id",
    "username": "johndoe",
    "email": "john@example.com",
    "token": "jwt_token"
  }
}
```

#### Login User
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "user_id",
    "username": "johndoe",
    "email": "john@example.com",
    "token": "jwt_token"
  }
}
```

### User Endpoints (Protected)

All user endpoints require authentication. Include the JWT token in the Authorization header:
```
Authorization: Bearer <token>
```

#### Get All Users
```http
GET /api/users
```

#### Get Single User
```http
GET /api/users/:id
```

#### Update User
```http
PUT /api/users/:id
Content-Type: application/json

{
  "username": "newusername",
  "email": "newemail@example.com"
}
```

#### Delete User
```http
DELETE /api/users/:id
```

### Analytics Endpoints (Protected)

#### Get Statistics
```http
GET /api/analytics/stats
```

**Response:**
```json
{
  "success": true,
  "data": {
    "totalUsers": 10,
    "totalMessages": 150
  }
}
```

#### Get Chat History
```http
GET /api/analytics/messages?limit=50
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": "message_id",
      "content": "Hello!",
      "username": "johndoe",
      "createdAt": "2025-01-14T12:00:00.000Z",
      "user": {
        "id": "user_id",
        "username": "johndoe",
        "email": "john@example.com"
      }
    }
  ]
}
```

## 🔌 Socket.IO Events

### Client to Server Events

#### Connect to Socket
```javascript
// Authentication required
socket = io('http://localhost:5000', {
  auth: { token: 'jwt_token' }
});
```

#### Send Message
```javascript
socket.emit('message:send', {
  content: 'Hello, World!'
});
```

### Server to Client Events

#### Receive Message
```javascript
socket.on('message:receive', (data) => {
  // data: { id, content, username, createdAt, user }
  console.log('New message:', data);
});
```

#### User Joined
```javascript
socket.on('user:joined', (data) => {
  // data: { username, message }
  console.log('User joined:', data);
});
```

#### User Left
```javascript
socket.on('user:left', (data) => {
  // data: { username, message }
  console.log('User left:', data);
});
```

#### Stats Update
```javascript
socket.on('stats:update', (data) => {
  // data: { totalMessages }
  console.log('Stats updated:', data);
});
```

## 📁 Project Structure

```
task-manager/
├── backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── db.ts                 # MongoDB connection
│   │   ├── models/
│   │   │   ├── User.ts               # User model
│   │   │   └── Message.ts            # Message model
│   │   ├── routes/
│   │   │   ├── auth.routes.ts        # Authentication routes
│   │   │   ├── user.routes.ts        # User CRUD routes
│   │   │   └── analytics.routes.ts   # Analytics routes
│   │   ├── middleware/
│   │   │   └── auth.middleware.ts    # JWT verification
│   │   ├── controllers/
│   │   │   ├── auth.controller.ts    # Auth logic
│   │   │   ├── user.controller.ts    # User CRUD logic
│   │   │   └── analytics.controller.ts # Analytics logic
│   │   ├── socket/
│   │   │   └── socket.handler.ts     # Socket.IO handlers
│   │   └── server.ts                 # Main server file
│   ├── package.json
│   ├── tsconfig.json
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Auth/
│   │   │   │   ├── Login.tsx         # Login component
│   │   │   │   └── Register.tsx      # Register component
│   │   │   └── Chat/
│   │   │       ├── ChatBox.tsx       # Main chat container
│   │   │       ├── MessageList.tsx   # Message display
│   │   │       ├── MessageInput.tsx  # Message input
│   │   │       └── UserStats.tsx     # Statistics display
│   │   ├── context/
│   │   │   └── AuthContext.tsx       # Auth state management
│   │   ├── services/
│   │   │   ├── api.ts                # API service
│   │   │   └── socket.ts             # Socket service
│   │   ├── types/
│   │   │   └── index.ts              # TypeScript types
│   │   ├── App.tsx                   # Main App component
│   │   ├── main.tsx                  # Entry point
│   │   └── index.css                 # Global styles
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.js
│   ├── vite.config.ts
│   └── .env.example
└── README.md
```

## 🔧 Build for Production

### Backend

```bash
cd backend
npm run build
npm start
```

### Frontend

```bash
cd frontend
npm run build
npm run preview
```

## 🐛 Troubleshooting

### MongoDB Connection Issues

**Problem:** Cannot connect to MongoDB

**Solutions:**
- Ensure MongoDB is running: `sudo systemctl status mongod`
- Check your `MONGODB_URI` in `.env`
- If using MongoDB Atlas, ensure your IP is whitelisted
- Verify network connectivity

### Port Already in Use

**Problem:** Port 5000 or 5173 is already in use

**Solutions:**
- Change the port in `.env` files
- Kill the process using the port:
  ```bash
  # Find process
  lsof -i :5000
  # Kill process
  kill -9 <PID>
  ```

### Socket Connection Failed

**Problem:** Cannot connect to Socket.IO server

**Solutions:**
- Ensure backend server is running
- Check `VITE_SOCKET_URL` in frontend `.env`
- Verify CORS settings in backend
- Check browser console for errors

### Authentication Issues

**Problem:** Token expired or invalid

**Solutions:**
- Clear browser localStorage
- Re-login to get a new token
- Check `JWT_SECRET` consistency in backend `.env`

### Dependencies Installation Failed

**Problem:** npm install fails

**Solutions:**
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and package-lock.json
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

## 📝 Notes for Palm Mind Technology

This project demonstrates:
- ✅ Complete CRUD operations with authentication
- ✅ Real-time communication using Socket.IO
- ✅ MongoDB integration with Mongoose
- ✅ TypeScript implementation on both frontend and backend
- ✅ Modern React development with hooks and context
- ✅ Professional code structure and organization
- ✅ Comprehensive error handling
- ✅ Security best practices (JWT, password hashing)
- ✅ Responsive UI with TailwindCSS
- ✅ Complete documentation

## 📄 License

This project was created as a hiring task for Palm Mind Technology.

## 👤 Author

**Gaurav**
- GitHub: [@brokestocker1](https://github.com/brokestocker1)

---

**Commitment:** Ready to commit for 1+ year with 2 months notice period as required.

For any questions or issues, please feel free to reach out!