B.T.G-Productions
This is the first app where u can make and collaborate beats and produce your own sound and much much more we hope everyone will get the best of our creativity and support so enjoy and we couldn't done this with out the help of our AI friend the Copilot. 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Audio Platform</title>
    <style>
        body { font-family: Arial, sans-serif; }
        form { max-width: 300px; margin: auto; }
        input, button { width: 100%; padding: 10px; margin: 5px 0; }
    </style>
    <script src="https://cdn.socket.io/4.0.0/socket.io.min.js"></script>
</head>
<body>
    <h1>Audio Platform</h1>

    <!-- Registration and Login Forms -->
    <h2>Register</h2>
    <form id="registerForm">
        <input type="text" id="username" placeholder="Username" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="password" id="password" placeholder="Password" required>
        <button type="submit">Register</button>
    </form>

    <h2>Login</h2>
    <form id="loginForm">
        <input type="email" id="loginEmail" placeholder="Email" required>
        <input type="password" id="loginPassword" placeholder="Password" required>
        <button type="submit">Login</button>
    </form>

    <!-- File Upload Form -->
    <h2>Upload Music</h2>
    <input type="file" id="fileInput">
    <button id="uploadButton">Upload</button>
    <p id="status"></p>

    <!-- Music Video Recording -->
    <h2>Create Your Music Video</h2>
    <video id="video" width="640" height="480" autoplay></video>
    <button id="startRecording">Start Recording</button>
    <button id="stopRecording">Stop Recording</button>
    <video id="playback" controls></video>

    <!-- Payment -->
    <h2>Make a Payment</h2>
    <button id="checkoutButton">Checkout</button>

    <!-- Chat System -->
    <h2>Chat</h2>
    <div id="chatBox" style="border:1px solid #ccc; height:200px; overflow-y:scroll;"></div>
    <input id="chatInput" placeholder="Type your message">
    <button id="sendChat">Send</button>

    <script>
        const socket = io('http://localhost:3000');

        document.getElementById('registerForm').addEventListener('submit', async (event) => {
            event.preventDefault();
            const username = document.getElementById('username').value;
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            const response = await fetch('/auth/register', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ username, email, password })
            });
            if (response.ok) alert('User registered successfully!');
        });

        document.getElementById('loginForm').addEventListener('submit', async (event) => {
            event.preventDefault();
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;
            const response = await fetch('/auth/login', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ email, password })
            });
            if (response.ok) {
                const { token } = await response.json();
                alert('Logged in successfully! Token: ' + token);
            }
        });

        document.getElementById('uploadButton').onclick = async () => {
            const file = document.getElementById('fileInput').files[0];
            const formData = new FormDataconst express = require('express');
const Stripe = require('stripe');
const stripe = Stripe(process.env.STRIPE_SECRET_KEY);
const router = express.Router();

router.post('/create-checkout-session', async (req, res) => {
    const session = await stripe.checkout.sessions.create({
        payment_method_types: ['card'],
        line_items: [{
            price_data: {
                currency: 'usd',
                product_data: {
                    name: 'Audio Platform Service',
                },
                unit_amount: 5000,
            },
            quantity: 1,
        }],
        mode: 'payment',
        success_url: 'https://your-website.com/success',
        cancel_url: 'https://your-website.com/cancel',
    });
    res.json({ id: session.id });
});

module.exports = router;
const express = require('express');
const router = express.Router();

// Chat route can be used for future enhancements
router.get('/', (req, res) => {
    res.send('Chat route');
});

module.exports = router;
const express = require('express');
const multer = require('multer');
const router = express.Router();

const storage = multer.memoryStorage();
const upload = multer({ storage });

router.post('/', upload.single('file'), (req, res) => {
    // Upload logic here (e.g., AWS S3)
    res.send('File uploaded successfully');
});

module.exports = router;
const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const User = require('../models/User');
const router = express.Router();

router.post('/register', async (req, res) => {
    const { username, email, password } = req.body;
    const hashedPassword = await bcrypt.hash(password, 10);
    const newUser = new User({ username, email, password: hashedPassword });
    await newUser.save();
    res.status(201).send("User registered successfully!");
});

router.post('/login', async (req, res) => {
    const { email, password } = req.body;
    const user = await User.findOne({ email });
    if (!user || !await bcrypt.compare(password, user.password)) {
        return res.status(400).send("Invalid credentials!");
    }
    const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET);
    res.json({ token });
});

module.exports = router;
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
    username: { type: String, required: true, unique: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
});

module.exports = mongoose.model('User', userSchema);
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const dotenv = require('dotenv');
const http = require('http');
const { Server } = require('socket.io');

dotenv.config();

const app = express();
const server = http.createServer(app);
const io = new Server(server, {
    cors: {
        origin: '*',
    }
});

mongoose.connect(process.env.MONGO_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
});

app.use(cors());
app.use(express.json());

app.use('/auth', require('./routes/auth'));
app.use('/upload', require('./routes/upload'));
app.use('/chat', require('./routes/chat'));
app.use('/payment', require('./routes/payment'));

io.on('connection', (socket) => {
    console.log('a user connected');
    socket.on('disconnect', () => {
        console.log('user disconnected');
    });
    socket.on('chat message', (msg) => {
        io.emit('chat message', msg);
    });
});

server.listen(3000, () => {
    console.log('Server running on port 3000');
});
MONGO_URI=mongodb://localhost:27017/audioPlatform
JWT_SECRET=your_jwt_secret
STRIPE_SECRET_KEY=your_stripe_secret_key
mkdir audioPlatform
cd audioPlatform
mkdir server
cd server
npm init -y
npm install express mongoose bcryptjs jsonwebtoken stripe dotenv multer socket.io cors
audioPlatform/
├── server/
│   ├── models/
│   │   └── User.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── chat.js
│   │   ├── payment.js
│   │   └── upload.js
│   ├── .env
│   ├── server.js
│   ├── package.json
│   └── ...other files
└── client/
    └── ...client code
