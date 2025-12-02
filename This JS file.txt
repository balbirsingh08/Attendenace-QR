// server.js
const express = require('express');
const path = require('path');

const app = express();
app.use(express.json());
app.use(express.static(path.join(__dirname, 'public')));

// In-memory storage (for demo only)
const sessions = new Map();     // sessionId -> { className, createdAt, durationMs }
const attendance = new Map();   // sessionId -> [ { time, sessionId, name, roll } ]

function genSessionId() {
  return 'S' + Math.random().toString(36).substring(2, 9).toUpperCase();
}

// Create a new session (called by admin)
app.post('/api/sessions', (req, res) => {
  const { className, durationSeconds } = req.body || {};
  const cls = (className || 'Unnamed Class').toString();
  const durSec = parseInt(durationSeconds, 10);
  const durationMs = Number.isFinite(durSec) && durSec > 0 ? durSec * 1000 : 180000;

  const id = genSessionId();
  sessions.set(id, {
    id,
    className: cls,
    createdAt: Date.now(),
    durationMs
  });
  if (!attendance.has(id)) attendance.set(id, []);
  res.json({ sessionId: id, className: cls });
});

// List attendance for a session (called by admin)
app.get('/api/attendance', (req, res) => {
  const { sessionId } = req.query;
  if (!sessionId || !sessions.has(sessionId)) {
    return res.status(400).json({ error: 'Invalid or missing sessionId' });
  }
  res.json(attendance.get(sessionId) || []);
});

// Add attendance (called by student)
app.post('/api/attendance', (req, res) => {
  const { sessionId, name, roll } = req.body || {};
  if (!sessionId || !sessions.has(sessionId)) {
    return res.status(400).json({ error: 'Invalid or missing sessionId' });
  }
  const trimmedName = (name || '').trim();
  const trimmedRoll = (roll || '').trim();
  if (!trimmedName) {
    return res.status(400).json({ error: 'Name is required' });
  }

  const list = attendance.get(sessionId) || [];
  const exists = list.find(
    r => r.name.toLowerCase() === trimmedName.toLowerCase()
  );
  if (exists) {
    return res.status(200).json({ ok: true, message: 'Already marked present' });
  }

  const record = {
    time: new Date().toISOString(),
    sessionId,
    name: trimmedName,
    roll: trimmedRoll
  };
  list.push(record);
  attendance.set(sessionId, list);
  res.json({ ok: true, message: 'Attendance recorded', record });
});

// CSV download for a session (called by admin)
app.get('/api/attendance/csv', (req, res) => {
  const { sessionId } = req.query;
  if (!sessionId || !sessions.has(sessionId)) {
    return res.status(400).send('Invalid or missing sessionId');
  }
  const list = attendance.get(sessionId) || [];
  let csv = 'Time,Session,Name,Roll\n';
  for (const r of list) {
    csv += `"${r.time}","${r.sessionId}","${r.name}","${r.roll || ''}"\n`;
  }
  res.setHeader('Content-Type', 'text/csv');
  res.setHeader('Content-Disposition', `attachment; filename="attendance_${sessionId}.csv"`);
  res.send(csv);
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
