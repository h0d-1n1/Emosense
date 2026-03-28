
**EmoSense — AI Mental Health Companion**
*Project Brief*

**What it is**
EmoSense is a web application designed for college students in India to monitor and improve their mental well-being using AI-powered emotion detection, voice chat therapy, and mood tracking analytics.

---

**Tech Stack**
- **Frontend:** React + Vite
- **Styling:** Inline styles (no CSS framework)
- **Auth + Database:** Firebase (Authentication + Firestore)
- **Hosting:** Firebase Hosting
- **Emotion Detection:** Hugging Face API (`trpakov/vit-face-expression`)
- **Speech to Text:** Sarvam AI (`saarika:v2.5`)
- **Text to Speech:** Sarvam AI (`bulbul:v1`)
- **AI Brain:** Groq API (`llama-3.1-8b-instant`)
- **Charts:** Recharts
- **Routing:** React Router DOM
- **Notifications:** React Hot Toast

---

**Environment Variables**
```
VITE_FIREBASE_API_KEY
VITE_FIREBASE_AUTH_DOMAIN
VITE_FIREBASE_PROJECT_ID
VITE_FIREBASE_STORAGE_BUCKET
VITE_FIREBASE_MESSAGING_SENDER_ID
VITE_FIREBASE_APP_ID
VITE_HF_API_KEY
VITE_SARVAM_API_KEY
VITE_GROQ_API_KEY
```

---

**File Structure**
```
emosense/
├── src/
│   ├── context/
│   │   └── AuthContext.jsx
│   ├── components/
│   │   └── ProtectedRoute.jsx
│   ├── pages/
│   │   ├── Login.jsx
│   │   ├── Signup.jsx
│   │   ├── Onboarding.jsx
│   │   ├── Dashboard.jsx
│   │   └── Chat.jsx
│   ├── firebase.js
│   └── App.jsx
├── .env
└── package.json
```

---

**Features**

**Auth**
- Email + password signup with email verification
- Google Sign-In
- Protected routes — unauthenticated users redirected to login

**Onboarding**
- Step 1: Name, age, gender
- Step 2: 5 PHQ-9 style mental health questions (mood, stress, sleep, support, anxiety)
- Data saved to Firestore under `users/{uid}`
- Sets `onboardingComplete: true` on finish

**Dashboard**
- Personalized greeting with user's name and current date
- **Emotion Scan:** Opens webcam → captures photo → sends to Hugging Face → returns detected emotion with confidence scores shown as bars
- Detected emotion + optional note saved to Firestore under `moodLogs/{uid}/logs/{logId}`
- **Analytics tab:** Recharts line chart (mood trend last 7 days) + pie chart (emotion distribution)
- **Logs tab:** Full mood history table with date, emotion, confidence, note
- Sidebar navigation

**AI Voice Chat**
- AI companion called "Emo"
- User can type or speak (mic button)
- Voice input → Sarvam STT transcribes → sent to Groq (Llama 3.1) → response → Sarvam TTS speaks it back
- Supports 11 Indian languages: English, Hindi, Bengali, Telugu, Tamil, Kannada, Malayalam, Marathi, Gujarati, Odia, Punjabi
- Language selector dropdown affects both STT and TTS
- Full chat history shown on screen
- Speaking indicator with stop button

**Emo's System Prompt**
> "You are Emo, a compassionate AI mental health companion for college students in India. You speak warmly, like a supportive friend who also has knowledge of psychology. You respond in the same language the user speaks in. You never diagnose. You always encourage professional help for serious concerns. Keep responses concise and conversational (2-4 sentences max for voice)."

---

**Firestore Structure**
```
users/{uid}
  name, age, gender, email
  onboardingComplete: boolean
  answers: object (onboarding question responses)
  createdAt: timestamp

moodLogs/{uid}/logs/{logId}
  emotion: string
  scores: array (all emotion confidences)
  note: string
  timestamp: ISO string
  date: string (en-IN format)
```

---

**Deployment**
- Live on Firebase Hosting
- To redeploy: `npm run build` → `firebase deploy`
- GitHub repo: `github.com/h0d-1n1/emosense`
