https://attendenace-qr.onrender.com/
📱 QR Attendance System
A lightweight, real-time QR-based attendance tracking system for classrooms.

https://img.shields.io/badge/QR-Attendance-blue
https://img.shields.io/badge/Node.js-18+-green
https://img.shields.io/badge/Express-4.x-lightgrey

✨ Features
👨‍🏫 Teacher/Admin Panel
Create attendance sessions with custom duration

Generate unique QR codes for each session

Real-time attendance monitoring

Export attendance data to CSV

Session management (start/stop)

👨‍🎓 Student Panel
Scan QR code to mark attendance

Simple name/roll number submission

Instant confirmation

Prevents duplicate entries

🏗️ Architecture
text
Frontend (Static HTML/CSS/JS) → GitHub Pages
Backend (Node.js/Express API) → Render/Railway
🚀 Quick Start
Prerequisites
Node.js 18+ installed

Git installed

1. Clone the Repository
bash
git clone https://github.com/yourusername/qr-attendance.git
cd qr-attendance
2. Install Dependencies
bash
npm install express
3. Run Locally
bash
node server.js
Server starts at: http://localhost:3000

🌐 Deployment
Frontend Deployment (GitHub Pages)
Push code to GitHub repository

Go to Settings → Pages

Set source to main branch and /public folder

Your site will be at: https://username.github.io/repo-name

Backend Deployment (Render)
Create account at render.com

Click "New Web Service"

Connect your GitHub repository

Configure:

Build Command: npm install

Start Command: node server.js

Environment: Node

Click "Create Web Service"

Update API Endpoint
After backend deployment, update the API URL in your frontend:

js
// In public/script.js
const API_BASE = 'https://your-backend.onrender.com/api';
📁 Project Structure
text
qr-attendance/
├── public/              # Frontend files (for GitHub Pages)
│   ├── index.html      # Teacher panel
│   ├── student.html    # Student panel
│   ├── style.css       # Styles
│   └── script.js       # Frontend logic
├── server.js           # Backend API server
├── package.json        # Dependencies
└── README.md          # This file
🔧 API Endpoints
Method	Endpoint	Description
POST	/api/sessions	Create new session
POST	/api/attendance	Mark student attendance
GET	/api/attendance	Get attendance list
GET	/api/attendance/csv	Download CSV
💡 Usage Guide
1. Teacher Creates Session
Enter class name and duration

Click "Start Session"

Share the generated QR code with students

2. Students Mark Attendance
Scan QR code with phone

Enter name and roll number

Click "Submit Attendance"

3. Teacher Monitors & Downloads
View real-time attendance list

Stop session when done

Download CSV for records

⚙️ Configuration Options
In the teacher panel, you can set:

Session Duration: 30-300 seconds

Poll Interval: How often to refresh attendance (5-30 seconds)

Class Name: Custom identifier for sessions

🔒 Security Considerations (For Production)
⚠️ Current setup is for demo only. For production use:

Add authentication for teacher/admin access

Use HTTPS for all communications

Implement rate limiting on API endpoints

Add input validation and sanitization

Use database instead of in-memory storage

Add session expiration with auto-cleanup

🐛 Troubleshooting
Issue	Solution
Server won't start	Check if port 3000 is free
QR code not generating	Ensure server is running
Attendance not saving	Check browser console for errors
CSV download empty	Verify session has active students
📚 Dependencies
Express.js - Web server framework

QRCode.js - QR code generation (frontend)

Browser's Geolocation API - Optional location tracking

🤝 Contributing
Fork the repository

Create a feature branch

Commit your changes

Push to the branch

Open a Pull Request

📄 License
MIT License - see LICENSE file

🙏 Acknowledgments
QR code generation by davidshimjs/qrcodejs

Icons from Font Awesome

UI inspiration from modern attendance systems

🔗 Live Demo
Frontend: https://yourusername.github.io/qr-attendance

Backend API: https://your-backend.onrender.com

📞 Support
For issues and questions:

Open a GitHub Issue

Check Wiki for documentation

Made with ❤️ for educators and students everywhere

Simplify attendance. Focus on teaching.
