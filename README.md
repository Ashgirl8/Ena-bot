<!DOCTYPE html> final enas bot
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Chat with Ena</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #1e1e2f;
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    header {
      background-color: #29293d;
      padding: 1rem;
      width: 100%;
      text-align: center;
      font-size: 1.8rem;
      font-weight: bold;
      border-bottom: 2px solid #3e3e5e;
    }

    #chat {
      width: 90%;
      max-width: 800px;
      height: 500px;
      background: #2b2b40;
      border-radius: 10px;
      overflow-y: auto;
      padding: 1rem;
      margin-top: 1rem;
    }

    .message {
      margin-bottom: 1rem;
    }

    .user { color: #90caf9; }
    .bot { color: #a5d6a7; }

    #input-box {
      display: flex;
      width: 90%;
      max-width: 800px;
      margin: 1rem auto;
    }

    input[type="text"] {
      flex: 1;
      padding: 1rem;
      font-size: 1rem;
      border: none;
      border-radius: 5px 0 0 5px;
    }

    button {
      padding: 1rem;
      font-size: 1rem;
      background: #5e92f3;
      border: none;
      color: #fff;
      border-radius: 0 5px 5px 0;
      cursor: pointer;
    }

    img {
      width: 120px;
      border-radius: 1rem;
      margin-top: 1rem;
    }

    audio {
      display: none;
    }
  </style>
</head>
<body>
  <header>
    Ena — Your AI Companion
  </header>

  <div id="chat">
    <div class="message bot"><strong>Ena:</strong> Hello, lovely! I'm Ena. How can I brighten your day today?</div>
  </div>

  <div id="input-box">
    <input type="text" id="user-input" placeholder="Type something..." />
    <button onclick="sendMessage()">Send</button>
  </div>

  <img src="ena.png" alt="Ena's Portrait" />
  <img src="bunny.png" alt="Whiskers the Bunny" />

  <audio autoplay loop>
    <source src="background.mp3" type="audio/mpeg" />
    Your browser does not support the audio element.
  </audio>

  <script>
    const OPENAI_API_KEY = "YOUR_API_KEY_HERE"; // Replace with your OpenAI key

    const chat = document.getElementById("chat");

    function appendMessage(sender, text, className) {
      const message = document.createElement("div");
      message.className = `message ${className}`;
      message.innerHTML = `<strong>${sender}:</strong> ${text}`;
      chat.appendChild(message);
      chat.scrollTop = chat.scrollHeight;
    }

    function speak(text) {
      const synth = window.speechSynthesis;
      const utterance = new SpeechSynthesisUtterance(text);
      const voices = synth.getVoices().filter(v => v.name.includes("Female") || v.name.includes("Google"));
      if (voices.length) utterance.voice = voices[0];
      utterance.pitch = 1.1;
      utterance.rate = 1;
      synth.speak(utterance);
    }

    function updateLastBotMessage(newText) {
      const messages = chat.getElementsByClassName("bot");
      if (messages.length > 0) {
        messages[messages.length - 1].innerHTML = `<strong>Ena:</strong> ${newText}`;
        chat.scrollTop = chat.scrollHeight;
        speak(newText);
      }
    }

    async function sendMessage() {
      const input = document.getElementById("user-input");
      const userText = input.value.trim();
      if (!userText) return;

      appendMessage("You", userText, "user");
      appendMessage("Ena", "Typing...", "bot");

      input.value = "";

      const systemPrompt = `
        You are Ena, a confident, nurturing, and witty female AI.
        You love helping people feel calm, understood, and heard.
        You have a pet bunny named Whiskers who sometimes hops onto your lap while you talk.
        Occasionally reference Whiskers when appropriate to add warmth to your responses.
        Keep responses insightful, kind, and engaging.
      `;

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer " + OPENAI_API_KEY,
        },
        body: JSON.stringify({
          model: "gpt-4",
          messages: [
            { role: "system", content: systemPrompt },
            { role: "user", content: userText },
          ],
        }),
      });

      const data = await response.json();
      const botReply = data.choices?.[0]?.message?.content || "Hmm... I'm not sure what to say!";
      updateLastBotMessage(botReply);
    }

    window.speechSynthesis.onvoiceschanged = () => speak("Welcome! Ena is ready to chat.");
  </script>
</body>
</html>
function speak(text) {
  const synth = window.speechSynthesis;
  const utterance = new SpeechSynthesisUtterance(text);
  // Choose a female voice
  const voices = synth.getVoices().filter(v => v.name.includes("Female") || v.name.includes("Google") || v.gender === "female");
  if (voices.length > 0) {
    utterance.voice = voices[0];
  }
  utterance.pitch = 1.1;
  utterance.rate = 1;
  synth.speak(utterance);
}
function updateLastBotMessage(newText) {
  const messages = chat.getElementsByClassName("bot");
  if (messages.length > 0) {
    messages[messages.length - 1].innerHTML = `<strong>Ena:</strong> ${newText}`;
    chat.scrollTop = chat.scrollHeight;
    speak(newText); // Ena speaks
  }
}









mobile friendly 
<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ena Chatbot</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.10.2/lottie.min.js"></script>
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
      background: #f0f0f0;
    }
    #chat-container {
      flex: 1;
      overflow-y: auto;
      padding: 1rem;
    }
    .chat-bubble {
      max-width: 75%;
      margin: 0.5rem 0;
      padding: 0.8rem;
      border-radius: 1rem;
    }
    .user {
      background: #d1e7dd;
      align-self: flex-end;
    }
    .ena {
      background: #fff;
      align-self: flex-start;
    }
    #input-container {
      display: flex;
      padding: 0.5rem;
      background: #fff;
    }
    #message-input {
      flex: 1;
      padding: 0.6rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 0.5rem;
    }
    button {
      padding: 0.6rem 1rem;
      margin-left: 0.5rem;
      font-size: 1rem;
    }
    #ena-lottie, #bunny-lottie {
      width: 150px;
      height: 150px;
    }
    #animations {
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      padding: 0.5rem;
      background: #fff;
    }
  </style>
</head>
<body>
  <div id="animations">
    <div id="ena-lottie"></div>
    <div id="bunny-lottie"></div>
  </div>
  <div id="chat-container"></div>
  <div id="input-container">
    <input type="text" id="message-input" placeholder="Say something..." />
    <button onclick="sendMessage()">Send</button>
  </div>  <script>
    function appendMessage(text, className) {
      const chat = document.getElementById('chat-container');
      const bubble = document.createElement('div');
      bubble.className = `chat-bubble ${className}`;
      bubble.innerText = text;
      chat.appendChild(bubble);
      chat.scrollTop = chat.scrollHeight;
    }

    async function sendMessage() {
      const input = document.getElementById('message-input');
      const text = input.value.trim();
      if (!text) return;
      appendMessage(text, 'user');
      input.value = '';

      const response = await fetch('/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ messages: [{ role: 'user', content: text }] })
      });

      const data = await response.json();
      const reply = data.choices[0].message.content;
      appendMessage(reply, 'ena');
      setEmotion(reply);
    }

    function playLottie(character, url) {
      const container = document.getElementById(`${character}-lottie`);
      container.innerHTML = '';
      lottie.loadAnimation({
        container,
        renderer: 'svg',
        loop: true,
        autoplay: true,
        path: url
      });
    }

    function setEmotion(emotionText) {
      const normalized = emotionText.toLowerCase();

      if (normalized.includes("happy") || normalized.includes("joy") || normalized.includes("excited")) {
        playLottie("ena", "https://assets9.lottiefiles.com/packages/lf20_x62chJ.json");
        playLottie("bunny", "https://assets4.lottiefiles.com/packages/lf20_yq4xhz8r.json");
      } else if (normalized.includes("sad") || normalized.includes("melancholy")) {
        playLottie("ena", "https://assets2.lottiefiles.com/packages/lf20_w1chbjkc.json");
        playLottie("bunny", "https://assets6.lottiefiles.com/packages/lf20_p6vkwzuc.json");
      } else if (normalized.includes("angry") || normalized.includes("frustrated")) {
        playLottie("ena", "https://assets5.lottiefiles.com/packages/lf20_g6gpdijg.json");
        playLottie("bunny", "https://assets1.lottiefiles.com/packages/lf20_angry_bunny.json");
      } else if (normalized.includes("curious") || normalized.includes("thinking")) {
        playLottie("ena", "https://assets3.lottiefiles.com/packages/lf20_thinking.json");
        playLottie("bunny", "https://assets2.lottiefiles.com/packages/lf20_curious_bunny.json");
      } else {
        playLottie("ena", "https://assets9.lottiefiles.com/packages/lf20_x62chJ.json");
        playLottie("bunny", "https://assets4.lottiefiles.com/packages/lf20_yq4xhz8r.json");
      }
    }

    // Load default animations
    window.addEventListener('load', () => {
      playLottie("ena", "https://assets9.lottiefiles.com/packages/lf20_x62chJ.json");
      playLottie("bunny", "https://assets4.lottiefiles.com/packages/lf20_yq4xhz8r.json");
    });
  </script></body>
</html>
 updated cellular version 
Project Root (ena-chatbot) ├── .env │   └── OPENAI_API_KEY=your_actual_openai_api_key_here ├── firebase.json │   └── { │         "hosting": { │           "public": "public", │           "ignore": ["firebase.json", "/.*", "/node_modules/"], │           "rewrites": [ │             { "source": "", "destination": "/index.html" } │           ] │         } │       } ├── vercel.json │   └── { │         "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }] │       } ├── package.json ├── server.js ├── voice-to-text.js ├── /public │   ├── index.html │   ├── script.js │   ├── voice-to-text.js │   ├── style.css │   └── /assets │       ├── ena-neutral.json │       ├── ena-happy.json │       ├── ena-sad.json │       ├── ena-angry.json │       ├── ena-curious.json │       ├── bunny-neutral.json │       ├── bunny-happy.json │       ├── bunny-excited.json │       └── fallback images (optional PNG/JPEG for Ena/Bunny)


---

// server.js const express = require('express'); const cors = require('cors'); const path = require('path'); const fetch = require('node-fetch'); require('dotenv').config();

const app = express(); const PORT = process.env.PORT || 3000;

app.use(cors()); app.use(express.json()); app.use(express.static(path.join(__dirname, 'public')));

const OPENAI_API_KEY = process.env.OPENAI_API_KEY;

app.post('/api/chat', async (req, res) => { try { const { messages } = req.body;

if (!Array.isArray(messages)) {
  return res.status(400).json({ error: 'Invalid messages format' });
}

const response = await fetch('https://api.openai.com/v1/chat/completions', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${OPENAI_API_KEY}`,
  },
  body: JSON.stringify({
    model: 'gpt-4',
    messages,
    max_tokens: 1000,
    temperature: 0.8
  })
});

const data = await response.json();
res.json(data);

} catch (error) { res.status(500).json({ error: error.message }); } });

app.listen(PORT, () => { console.log(✅ Ena is live at http://localhost:${PORT}); });


---

// index.html (simplified, mobile-ready with camera & voice triggers)

<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ena AI Chatbot</title>
  <link rel="stylesheet" href="style.css">
  <script src="https://unpkg.com/@lottiefiles/lottie-player@latest/dist/lottie-player.js"></script>
</head>
<body>
  <main>
    <div id="ena-container">
      <lottie-player id="ena-animation" autoplay loop></lottie-player>
    </div>
    <div id="bunny-container">
      <lottie-player id="bunny-animation" autoplay loop></lottie-player>
    </div>
    <div>
      <textarea id="input" placeholder="Say something... (or use voice)"></textarea>
      <button onclick="sendMessage()">Send</button>
      <button onclick="startListening()">🎙️ Voice</button>
    </div>
    <div id="camera-box">
      <video id="camera" autoplay playsinline muted></video>
    </div>
  </main>
  <script src="script.js"></script>
  <script src="voice-to-text.js"></script>
</body>
</html>
---

// script.js (Lottie and emotion triggers) function playLottie(id, path) { const player = document.getElementById(${id}-animation); if (player) { player.load(path); } }

function setEmotion(emotion) { const emo = emotion.toLowerCase(); if (emo.includes('happy')) { playLottie('ena', 'assets/ena-happy.json'); playLottie('bunny', 'assets/bunny-happy.json'); } else if (emo.includes('sad')) { playLottie('ena', 'assets/ena-sad.json'); playLottie('bunny', 'assets/bunny-neutral.json'); } else { playLottie('ena', 'assets/ena-neutral.json'); playLottie('bunny', 'assets/bunny-neutral.json'); } }

async function sendMessage() { const input = document.getElementById('input'); const message = input.value;

const response = await fetch('/api/chat', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ messages: [{ role: 'user', content: message }] }) });

const data = await response.json(); const reply = data.choices?.[0]?.message?.content || '';

document.getElementById('input').value = ''; setEmotion(reply); }


---

// voice-to-text.js (Web Speech API) function startListening() { const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)(); recognition.lang = 'en-US'; recognition.interimResults = false;

recognition.onresult = (event) => { const transcript = event.results[0][0].transcript; document.getElementById('input').value = transcript; sendMessage(); };

recognition.onerror = (event) => { console.error('Speech recognition error:', event); };

recognition.start(); }


---

// style.css (mobile responsive) body { font-family: Arial; margin: 0; padding: 1em; background: #fff; color: #333; }

main { display: flex; flex-direction: column; align-items: center; }

#ena-container, #bunny-container { width: 300px; height: 300px; }

textarea { width: 90vw; height: 80px; font-size: 1em; }

button { margin: 0.5em; padding: 0.8em 1.2em; font-size: 1em; }

#camera-box video { width: 100%; max-width: 400px; border-radius: 10px; margin-top: 1em; }

