k<!DOCTYPE html> final enas bot
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

updated mobile (no bunny,pixi controls)
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Ena — Live2D 4D (Mic + Camera)</title>
  <link rel="stylesheet" href="/style.css" />
  <style>
    /* small additions for camera & mic controls */
    #controls-top { position: absolute; top: 12px; right: 12px; z-index: 999; display:flex; gap:8px; }
    .ctrl-btn { background:#b77a2b;color:#111;border:none;padding:8px 10px;border-radius:8px;font-weight:700; }
    #camera-video { display:none; width:160px; height:120px; position: absolute; top: 60px; right: 12px; border-radius:8px; z-index:998; box-shadow:0 6px 20px rgba(0,0,0,0.6); }
    #emotion-badge { position:absolute; top:12px; left:12px; background:rgba(0,0,0,0.5); color:#fff; padding:6px 10px; border-radius:8px; z-index:999; font-weight:700; }
  </style>
  <!-- face-api (used for expression detection) -->
  <script defer src="https://cdn.jsdelivr.net/npm/face-api.js@0.22.2/dist/face-api.min.js"></script>
</head>
<body>
  <div id="stage-root">
    <div id="live2d-wrap"></div>
  </div>

  <div id="controls-top">
    <button id="micBtn" class="ctrl-btn">🎤 Mic</button>
    <button id="camBtn" class="ctrl-btn">📷 Camera</button>
    <button id="musicBtn" class="ctrl-btn">🔊 Music</button>
  </div>

  <div id="emotion-badge">Emotion: neutral</div>

  <!-- tiny camera preview (hidden by default) -->
  <video id="camera-video" autoplay muted playsinline></video>

  <!-- background music -->
  <audio id="bgMusic" src="/assets/bg-music.mp3" loop preload="auto"></audio>

  <!-- PIXI + pixi-live2d-display -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/6.5.8/browser/pixi.min.js"></script>
  <script src="https://unpkg.com/pixi-live2d-display@^4.0.0/dist/pixi-live2d-display.min.js"></script>

  <!-- core live2d + 4D behaviour -->
  <script src="/live2d-4
/* live2d-4d.js
   Pixi + pixi-live2d-display runtime with:
   - Mic audio analyser -> ParamMouthOpenY (lip sync)
   - Camera -> face-api expression -> emotion triggers
   - Idle loop: blink, breath, arm sway
   - Expression/motion API using model.internalModel.parameter setters
*/

(async function () {
  // -----------------------
  // Config - adjust IDs to match your model
  // -----------------------
  const MODEL_JSON = '/models/ena/ena.model3.json'; // path to your exported model
  const PARAMS = {
    eyeL: 'ParamEyeLOpen',
    eyeR: 'ParamEyeROpen',
    mouth: 'ParamMouthOpenY',
    breath: 'ParamBreath',
    arm: 'ParamArmSway',
    angleX: 'ParamAngle
/* live2d-4d.js
   Pixi + pixi-live2d-display runtime with:
   - Mic audio analyser -> ParamMouthOpenY (lip sync)
   - Camera -> face-api expression -> emotion triggers
   - Idle loop: blink, breath, arm sway
   - Expression/motion API using model.internalModel.parameter setters
*/

(async function () {
  // -----------------------
  // Config - adjust IDs to match your model
  // -----------------------
  const MODEL_JSON = '/models/ena/ena.model3.json'; // path to your exported model
  const PARAMS = {
    eyeL: 'ParamEyeLOpen',
    eyeR: 'ParamEyeROpen',
    mouth: 'ParamMouthOpenY',
    breath: 'ParamBreath',
    arm: 'ParamArmSway',
    angleX: 'ParamAngleX',
    angleY: 'ParamAngleY'
  };

  // -----------------------
  // PIXI App + Live2D model load
  // -----------------------
  const app = new PIXI.Application({
    view: document.createElement('canvas'), // create temporary; we'll append to wrap
    autoStart: true,
    backgroundAlpha: 0,
    resizeTo: window
  });

  const wrap = document.getElementById('live2d-wrap');
  wrap.style.width = '100%';
  wrap.style.height = '100vh';
  wrap.style.position = 'relative';
  // append pixi view
  wrap.appendChild(app.view);

  // ensure proper retina scaling
  app.renderer.resolution = window.devicePixelRatio || 1;

  // load model
  let model = null;
  try {
    model = await PIXI.live2d.Live2DModel.from(MODEL_JSON);
    model.anchor.set(0.5, 1.0);
    app.stage.addChild(model);
    centerModel();
  } catch (e) {
    console.error('Failed to load Live2D model. Check MODEL_JSON path and exported files.', e);
  }

  function centerModel() {
    if (!model) return;
    const w = app.renderer.width;
    const h = app.renderer.height;
    model.x = w / 2;
    model.y = h * 0.95;
    // scale to fit
    const desiredWidth = Math.min(w * 0.9, 520);
    const scale = desiredWidth / (model.width || 480);
    model.scale.set(scale);
  }
  window.addEventListener('resize', centerModel);

  // Helper: safe set parameter by id (works for most pixi-live2d-display runtime)
  function setParamById(id, value) {
    try {
      if (!model || !model.internalModel) return false;
      const params = model.internalModel.parameters;
      if (Array.isArray(params)) {
        const p = params.find((x) => x.id === id);
        if (p) {
          p.value = value;
          return true;
        }
      }
      // fallback: runtime may expose setValueById
      if (model.internalModel.parameters.setValueById) {
        model.internalModel.parameters.setValueById(id, value);
        return true;
      }
    } catch (err) {
      console.warn('setParamById error', err);
    }
    return false;
  }

  // -----------------------
  // Idle behaviours: blink, breathe, arm sway
  // -----------------------
  (function startIdle() {
    if (!model) return;

    // Blink
    let lastBlink = Date.now();
    function blinkTick() {
      const now = Date.now();
      if (now - lastBlink > 3000 + Math.random() * 5000) {
        lastBlink = now;
        setParamById(PARAMS.eyeL, 0.0);
        setParamById(PARAMS.eyeR, 0.0);
        setTimeout(() => {
          setParamById(PARAMS.eyeL, 1.0);
          setParamById(PARAMS.eyeR, 1.0);
        }, 90 + Math.random() * 180);
      }
      requestAnimationFrame(blinkTick);
    }
    blinkTick();

    // Breathing
    let t = 0;
    function breathTick() {
      t += 0.02;
      const v = 0.02 + Math.sin(t) * 0.02;
      setParamById(PARAMS.breath, v);
      setTimeout(breathTick, 100);
    }
    breathTick();

    // Arm sway
    let at = 0;
    function armTick() {
      at += 0.012;
      const v = Math.sin(at) * 0.06;
      setParamById(PARAMS.arm, v);
      setTimeout(armTick, 60);
    }
    armTick();
  })();

  // -----------------------
  // Microphone analyser -> lip sync
  // -----------------------
  let micAnalyser = null;
  async function startMicAnalyser() {
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      const ctx = new (window.AudioContext || window.webkitAudioContext)();
      const src = ctx.createMediaStreamSource(stream);
      const analyser = ctx.createAnalyser();
      analyser.fftSize = 512;
      src.connect(analyser);
      const data = new Uint8Array(analyser.frequencyBinCount);
      micAnalyser = { analyser, data };

      function tick() {
        analyser.getByteFrequencyData(data);
        let sum = 0;
        for (let i = 0; i < data.length; i++) sum += data[i];
        const avg = sum / data.length;
        // tune mapping: avg roughly 0..255 -> normalized mouth 0..1
        const normalized = Math.min(1, Math.max(0, (avg - 5) / 60));
        setParamById(PARAMS.mouth, normalized);
        requestAnimationFrame(tick);
      }
      tick();
      return true;
    } catch (err) {
      console.warn('Mic analyser init failed', err);
      return false;
    }
  }

  // Start mic when user presses Mic button
  const micBtn = document.getElementById('micBtn');
  micBtn.addEventListener('click', async () => {
    micBtn.disabled = true;
    const ok = await startMicAnalyser();
    if (!ok) alert('Microphone permission denied or not supported.');
    micBtn.disabled = false;
  });

  // -----------------------
  // Camera -> face-api expression detection -> emotion mapping
  // -----------------------
  const camBtn = document.getElementById('camBtn');
  const cameraVideo = document.getElementById('camera-video');
  const emotionBadge = document.getElementById('emotion-badge');

  let camStream = null;
  let cameraRunning = false;

  // load face-api models from /models/face-api/
  async function loadFaceApiModels() {
    try {
      await faceapi.nets.tinyFaceDetector.loadFromUri('/models/face-api/');
      await faceapi.nets.faceExpressionNet.loadFromUri('/models/face-api/');
      console.log('face-api models loaded');
    } catch (err) {
      console.warn('face-api load failed', err);
    }
  }
  loadFaceApiModels();

  async function startCamera() {
    try {
      camStream = await navigator.mediaDevices.getUserMedia({ video: { facingMode: 'user' }, audio: false });
      cameraVideo.srcObject = camStream;
      cameraVideo.style.display = 'block';
      cameraRunning = true;
      detectExpressionsLoop();
    } catch (err) {
      cameraVideo.style.display = 'none';
      cameraRunning = false;
      console.warn('Camera permission failed', err);
    }
  }

  function stopCamera() {
    if (camStream) {
      camStream.getTracks().forEach((t) => t.stop());
    }
    cameraVideo.style.display = 'none';
    cameraRunning = false;
  }

  camBtn.addEventListener('click', async () => {
    camBtn.disabled = true;
    if (!cameraRunning) {
      await startCamera();
    } else {
      stopCamera();
    }
    camBtn.disabled = false;
  });

  async function detectExpressionsLoop() {
    if (!cameraRunning) return;
    if (cameraVideo.readyState < 2) {
      setTimeout(detectExpressionsLoop, 500);
      return;
    }
    try {
      const result = await faceapi
        .detectSingleFace(cameraVideo, new faceapi.TinyFaceDetectorOptions())
        .withFaceExpressions();
      if (result && result.expressions) {
        const expressions = result.expressions;
        // pick highest prob
        const sorted = Object.entries(expressions).sort((a, b) => b[1] - a[1]);
        const dominant = sorted[0][0]; // e.g., happy, sad, angry, surprised, neutral
        const prob = sorted[0][1];
        // Map to Ena emotions using thresholds
        let mapped = 'neutral';
        if (dominant === 'happy' && prob > 0.5) mapped = 'happy';
        else if (dominant === 'sad' && prob > 0.5) mapped = 'sad';
        else if (dominant === 'angry' && prob > 0.45) mapped = 'angry';
        else if (dominant === 'surprised' && prob > 0.45) mapped = 'surprised';
        else if (dominant === 'neutral' && prob > 0.6) mapped = 'neutral';

        // trigger animation + badge
        handleEmotion(mapped);
      }
    } catch (err) {
      // console.warn('detectExpressions error', err);
    }
    setTimeout(detectExpressionsLoop, 450);
  }

  // -----------------------
  // Emotion handling -> motions + lottie overlay (if present) + badge
  // -----------------------
  function handleEmotion(emotion) {
    emotionBadge.textContent = 'Emotion: ' + emotion;
    // Play expression (if exported) or play motion
    try {
      // first try expression mapping
      if (model && model.setExpression) {
        model.setExpression && model.setExpression(emotion);
      }
      // then try motion groups (if available)
      if (model && model.internalModel && model.internalModel.motionManager) {
        // try to start a motion with group name same as emotion
        try {
          model.internalModel.motionManager.startRandomMotion(emotion, 0);
        } catch (e) {
          // fallback: model.motion if exposed by runtime
          try { model.motion && model.motion(emotion, 0, 1); } catch(e){}
        }
      }
    } catch (err) {
      console.warn('handleEmotion error', err);
    }
    // optional: play small lottie accent
    try { window.playLottieEmotion && window.playLottieEmotion(emotion); } catch(e){}
  }

  // -----------------------
  // Voice tone -> head angle subtle mapping (optional)
  // Use mic analyser amplitude to slightly move head angle parameters for expressiveness
  // -----------------------
  function startHeadAngleMapping() {
    if (!micAnalyser) return;
    const { analyser, data } = micAnalyser;
    (function tick() {
      try {
        analyser.getByteFrequencyData(data);
        let sum = 0;
        for (let i = 0; i < data.length; i++) sum += data[i];
        const avg = sum / data.length;
        // map avg (0..~200) to angleX (-15..15 deg) normalized -1..1
        const normalized = (avg - 10) / 60; // tuned factor
        const angleX = Math.max(-1, Math.min(1, normalized));
        setParamById(PARAMS.angleX, angleX * 15); // if model expects degrees or normalized, tune accordingly
      } catch (err) {}
      requestAnimationFrame(tick);
    })();
  }

  // start head mapping once mic analyser available
  // we monitor micAnalyser variable and start mapping once non-null
  (function waitForMic() {
    if (micAnalyser) startHeadAngleMapping();
    else setTimeout(waitForMic, 500);
  })();

  // -----------------------
  // Background music toggle
  // -----------------------
  const musicBtn = document.getElementById('musicBtn');
  const bgMusic = document.getElementById('bgMusic');
  musicBtn.addEventListener('click', async () => {
    try {
      if (bgMusic.paused) {
        // ensure audio context
        try { const ctx = new (window.AudioContext || window.webkitAudioContext)(); if (ctx.state === 'suspended') await ctx.resume(); } catch(e){}
        await bgMusic.play().catch(()=>{});
        musicBtn.textContent = '🔇 Music';
      } else {
        bgMusic.pause();
        musicBtn.textContent = '🔊 Music';
      }
    } catch (err) { console.warn('Music error', err); }
  });

  // -----------------------
  // Keyboard testing triggers (optional)
  // -----------------------
  document.addEventListener('keydown', (e) => {
    if (!model) return;
    switch (e.key) {
      case '1': handleEmotion('happy'); break;
      case '2': handleEmotion('sad'); break;
      case '3': handleEmotion('angry'); break;
      case '4': handleEmotion('surprised'); break;
      case '0': handleEmotion('neutral'); break;
    }
  });

  // Expose API for debugging
  window._EnaLive2D = {
    model,
    setParam: setParamById,
    handleEmotion,
    startMicAnalyser: async () => {
      const ok = await startMicAnalyser();
      if (ok) startHeadAngleMapping();
      return ok;
    },
    startCamera,
    stopCamera
  };

  console.log('Live2D 4D runtime initialized');

})();
updated mobile (Lottie files)

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ena: Emotionally Expressive AI</title>
  <link rel="stylesheet" href="/styles.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.10.2/lottie.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.5.0"></script>
  <script src="https://cdn.jsdelivr.net/npm/face-api.js"></script>
</head>
<body>
  <div id="container">
    <!-- Background -->
    <div id="background">
      <img src="/assets/background-grassy-dandelions.jpg" alt="Background" />
    </div>

    <!-- Ena: 2D Lottie -->
    <div id="ena-container">
      <div id="ena-lottie"></div>
      <canvas id="live2d" width="400" height="600"></canvas>
    </div>

    <!-- Bunny -->
    <div id="bunny-container">
      <div id="bunny-lottie"></div>
    </div>

    <!-- Voice Input -->
    <div id="voice-input">
      <button onclick="startVoice()">🎙 Speak</button>
      <p id="voice-output">Say something to Ena…</p>
    </div>

    <!-- Chat Log -->
    <div id="chat-log"></div>

    <!-- Camera Emotion Detection -->
    <video id="camera" autoplay muted playsinline width="0" height="0"></video>

    <!-- Script Inclusions -->
    <script src="/scripts/lottie-controller.js"></script>
    <script src="/scripts/voice.js"></script>
    <script src="/scripts/emotion-detection.js"></script>
    <script src="/scripts/live2d-loader.js"></script>
    <script src="/scripts/main.js"></script>
  </div>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ena AI - Emotionally Expressive Chatbot</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.7.4/lottie.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/facemesh"></script>
  <script defer src="/script.js"></script>
  <link rel="stylesheet" href="/style.css">
</head>
<body>
  <div id="app">
    <video id="camera-feed" autoplay muted playsinline></video>
    <div id="ena-container">
      <div id="ena-lottie"></div>
    </div>
    <div id="chat-container">
      <textarea id="user-input" placeholder="Talk to Ena..."></textarea>
      <button onclick="sendMessage()">Send</button>
      <button onclick="startVoiceRecognition()">🎤 Speak</button>
    </div>
    <div id="messages"></div>
  </div>

  <script>
    let enaPlayer;

    function playLottie(path) {
      if (enaPlayer) enaPlayer.destroy();
      enaPlayer = lottie.loadAnimation({
        container: document.getElementById("ena-lottie"),
        renderer: "svg",
        loop: true,
        autoplay: true,
        path
      });
    }

    function setEmotion(emotionText) {
      const emo = emotionText.toLowerCase();
      if (emo.includes("happy") || emo.includes("joy")) {
        playLottie("/assets/ena-happy.json");
      } else if (emo.includes("sad")) {
        playLottie("/assets/ena-sad.json");
      } else if (emo.includes("angry")) {
        playLottie("/assets/ena-angry.json");
      } else if (emo.includes("curious") || emo.includes("thinking")) {
        playLottie("/assets/ena-curious.json");
      } else {
        playLottie("/assets/ena-neutral.json");
      }
    }

    function sendMessage() {
      const input = document.getElementById("user-input").value;
      fetch("/api/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message: input })
      })
        .then(res => res.json())
        .then(data => {
          setEmotion(data.emotion || "neutral");
          speak(data.response);
          document.getElementById("messages").innerHTML += `<p><strong>You:</strong> ${input}</p><p><strong>Ena:</strong> ${data.response}</p>`;
        });
    }

    function speak(text) {
      const utterance = new SpeechSynthesisUtterance(text);
      speechSynthesis.speak(utterance);
    }

    function startVoiceRecognition() {
      const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
      recognition.lang = 'en-US';
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        document.getElementById("user-input").value = transcript;
        sendMessage();
      };
    }

    async function startCamera() {
      const stream = await navigator.mediaDevices.getUserMedia({ video: true });
      document.getElementById("camera-feed").srcObject = stream;
    }

    window.onload = () => {
      playLottie("/assets/ena-neutral.json");
      startCamera();
    }
  </script>
</body>
</html>

<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ena - Expressive AI</title>
  <link rel="stylesheet" href="/style.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/lottie-web/5.12.2/lottie.min.js"></script>
</head>
<body>
  <div id="ena-container">
    <div id="ena-animation"></div>
    <canvas id="video-canvas" style="display: none"></canvas>
    <div id="ena-chat">
      <textarea id="user-input" placeholder="Talk to Ena..."></textarea>
      <button onclick="sendMessage()">Send</button>
    </div>
    <div id="voice-controls">
      <button onclick="startVoiceInput()">🎤 Speak</button>
    </div>
  </div>  <script>
    let enaAnimation;
    let currentEmotion = "neutral";

    const emotionMap = {
      neutral: '/assets/ena-neutral.json',
      happy: '/assets/ena-happy.json',
      sad: '/assets/ena-sad.json',
      angry: '/assets/ena-angry.json',
      curious: '/assets/ena-curious.json'
    };

    function loadEnaAnimation(emotion) {
      if (enaAnimation) enaAnimation.destroy();
      enaAnimation = lottie.loadAnimation({
        container: document.getElementById('ena-animation'),
        renderer: 'svg',
        loop: true,
        autoplay: true,
        path: emotionMap[emotion] || emotionMap['neutral']
      });
    }

    function updateEmotionFromText(text) {
      const lowered = text.toLowerCase();
      if (lowered.includes("happy") || lowered.includes("joy")) return "happy";
      if (lowered.includes("sad") || lowered.includes("upset")) return "sad";
      if (lowered.includes("angry") || lowered.includes("mad")) return "angry";
      if (lowered.includes("curious") || lowered.includes("think")) return "curious";
      return "neutral";
    }

    function sendMessage() {
      const input = document.getElementById('user-input').value;
      if (!input.trim()) return;

      fetch('/api/message', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ message: input })
      })
        .then(res => res.json())
        .then(data => {
          const emotion = updateEmotionFromText(data.reply);
          currentEmotion = emotion;
          loadEnaAnimation(emotion);
          speakText(data.reply);
        });
    }

    function speakText(text) {
      const synth = window.speechSynthesis;
      const utter = new SpeechSynthesisUtterance(text);
      synth.cancel();
      synth.speak(utter);
    }

    function startVoiceInput() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      const recognition = new SpeechRecognition();
      recognition.lang = 'en-US';
      recognition.onresult = function(event) {
        document.getElementById('user-input').value = event.results[0][0].transcript;
        sendMessage();
      };
      recognition.start();
    }

    loadEnaAnimation(currentEmotion);
  </script></body>
</html>
// server.js import express from "express"; import cors from "cors"; import dotenv from "dotenv"; import bodyParser from "body-parser"; import { Configuration, OpenAIApi } from "openai";

// Load environment variables from .env file dotenv.config();

const app = express(); const port = process.env.PORT || 3000;

app.use(cors()); app.use(bodyParser.json()); app.use(express.static("public"));

// OpenAI configuration const configuration = new Configuration({ apiKey: process.env.OPENAI_API_KEY, });

const openai = new OpenAIApi(configuration);

// Route to handle user messages and return emotional response app.post("/api/message", async (req, res) => { try { const userMessage = req.body.message;

const completion = await openai.createChatCompletion({
  model: "gpt-4",
  messages: [
    {
      role: "system",
      content:
        "You are Ena, a humanlike assistant who expresses emotions. Always respond with an emotion label before your message like this: [happy], [sad], [curious], [angry], [surprised].",
    },
    { role: "user", content: userMessage },
  ],
});

const response = completion.data.choices[0].message.content;
res.json({ response });

} catch (error) { console.error("Error from OpenAI:", error.message); res.status(500).json({ error: "Failed to generate response" }); } });

// Start server app.listen(port, () => { console.log(Ena server running on http://localhost:${port}); });

// server.js import express from 'express'; import bodyParser from 'body-parser'; import cors from 'cors'; import fetch from 'node-fetch'; import dotenv from 'dotenv'; dotenv.config();

const app = express(); const port = process.env.PORT || 3000;

app.use(cors()); app.use(bodyParser.json()); app.use(express.static('public'));

app.post('/chat', async (req, res) => { const userMessage = req.body.message; if (!userMessage) return res.status(400).json({ error: 'No message provided' });

try { const response = await fetch('https://api.openai.com/v1/chat/completions', { method: 'POST', headers: { 'Authorization': Bearer ${process.env.OPENAI_API_KEY}, 'Content-Type': 'application/json' }, body: JSON.stringify({ model: 'gpt-4', messages: [ { role: 'system', content: 'You are Ena, an expressive, steampunk, post-apocalyptic AI who replies as humanlike and emotionally expressive as possible.' }, { role: 'user', content: userMessage } ] }) });

const data = await response.json();
const reply = data.choices[0].message.content;
res.json({ reply });

} catch (err) { console.error(err); res.status(500).json({ error: 'Failed to get response from OpenAI' }); } });

app.listen(port, () => { console.log(Ena backend running on http://localhost:${port}); });


// server.js (Backend) import express from 'express'; import bodyParser from 'body-parser'; import cors from 'cors'; import fetch from 'node-fetch'; import dotenv from 'dotenv'; dotenv.config();

const app = express(); const port = process.env.PORT || 3000;

app.use(cors()); app.use(bodyParser.json()); app.use(express.static('public'));

app.post('/api/message', async (req, res) => { const userMessage = req.body.message; try { const response = await fetch('https://api.openai.com/v1/chat/completions', { method: 'POST', headers: { 'Authorization': Bearer ${process.env.OPENAI_API_KEY}, 'Content-Type': 'application/json' }, body: JSON.stringify({ model: 'gpt-4o', messages: [ { role: 'system', content: 'You are Ena, an emotionally expressive assistant.' }, { role: 'user', content: userMessage } ] }) }); const data = await response.json(); res.json({ reply: data.choices[0].message.content }); } catch (error) { console.error('Error communicating with OpenAI:', error); res.status(500).send('Error generating response.'); } });

app.listen(port, () => console.log(Server is running on http://localhost:${port}));

// public/index.html (Frontend)

<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ena - Emotionally Expressive Assistant</title>
  <script src="https://unpkg.com/@lottiefiles/lottie-player@latest/dist/lottie-player.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/blazeface"></script>
  <script defer src="https://unpkg.com/face-api.js"></script>
  <style>
    body { background: url('/assets/background-grass-dandelions.jpg') no-repeat center center fixed; background-size: cover; font-family: sans-serif; margin: 0; }
    #ena-container { position: relative; width: 100%; max-width: 420px; margin: 40px auto; }
    #ena-animation { width: 100%; height: auto; }
    #chat { display: flex; flex-direction: column; align-items: center; }
    textarea { width: 90%; height: 80px; margin-top: 10px; }
    button { padding: 10px 20px; margin: 10px; font-size: 16px; }
  </style>
</head>
<body>
  <div id="ena-container">
    <canvas id="ena-3d-canvas"></canvas>
  </div>
  <div id="chat">
    <textarea id="user-input" placeholder="Talk to Ena..."></textarea>
    <button onclick="sendMessage()">Send</button>
    <div id="response"></div>
  </div>
  <script type="module">
    import * as LIVE2DCUBISMCORE from "https://cdn.jsdelivr.net/npm/@cubism/live2dcubismcore@latest";
    import * as LIVE2DVIEWER from "https://cdn.jsdelivr.net/npm/live2d-widget@4.0.0-beta.2";LIVE2DVIEWER.Live2DModel.from("/models/ena/ena.model3.json", {
  canvas: document.getElementById("ena-3d-canvas"),
  width: 400,
  height: 600,
  idleMotionGroup: "Idle",
  motionGroups: {
    Happy: ["happy"],
    Sad: ["sad"],
    Angry: ["angry"],
    Surprised: ["surprised"]
  },
  eyeBlinkIds: ["ParamEyeLOpen", "ParamEyeROpen"],
  lipSyncIds: ["ParamMouthOpenY"]
});

async function sendMessage() {
  const input = document.getElementById("user-input").value;
  const responseBox = document.getElementById("response");
  const res = await fetch("/api/message", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ message: input })
  });
  const data = await res.json();
  responseBox.innerText = data.reply;

  if (data.reply.toLowerCase().includes("happy")) {
    LIVE2DVIEWER.motion("Happy");
  } else if (data.reply.toLowerCase().includes("sad")) {
    LIVE2DVIEWER.motion("Sad");
  } else if (data.reply.toLowerCase().includes("angry")) {
    LIVE2DVIEWER.motion("Angry");
  } else if (data.reply.toLowerCase().includes("surprised")) {
    LIVE2DVIEWER.motion("Surprised");
  } else {
    LIVE2DVIEWER.idle();
  }
}

  </script>
</body>
</html>// .env OPENAI_API_KEY=your_actual_openai_api_key_here

// Folder Structure

project-root/

server.js

.env

public/

index.html

assets/

background-grass-dandelions.jpg


models/

ena/

ena.model3.json

motion/

happy.motion3.json

sad.motion3.json

angry.motion3.json

surprised.motion3.json


textures/

texture_00.png


physics.json

expressions.json


<!-- index.html: Ena's 3D Frontend with Cubism & Lottie Integration --><!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ena - Post Apocalyptic Steampunk AI</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: url('./assets/bg-grassy-dandelions.jpg') no-repeat center center fixed;
      background-size: cover;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: flex-end;
      height: 100vh;
    }
    #ena-container {
      position: relative;
      width: 100%;
      height: 90%;
    }
    canvas {
      width: 100% !important;
      height: 100% !important;
    }
    #voice-btn {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      padding: 10px 20px;
      background-color: #333;
      color: white;
      border: none;
      border-radius: 5px;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div id="ena-container">
    <canvas id="live2d"></canvas>
    <button id="voice-btn">Talk to Ena 🎙️</button>
  </div>
  <script src="https://unpkg.com/pixi.js"></script>
  <script src="https://unpkg.com/@cubism/live2dcubismcore"></script>
  <script src="https://unpkg.com/@cubism/live2d-framework"></script>
  <script>
    import * as PIXI from "pixi.js";
    import * as Live2DCubismFramework from "@cubism/live2d-framework";let app = new PIXI.Application({ view: document.getElementById("live2d"), autoStart: true });

async function loadModel() {
  const model = await PIXI.live2d.Live2DModel.from("./model/ena.model3.json");
  model.scale.set(0.5);
  model.x = app.renderer.width / 2 - model.width / 2;
  model.y = app.renderer.height - model.height;
  app.stage.addChild(model);
  return model;
}

function triggerEmotion(model, emotion) {
  const motionGroup = {
    happy: "motion_happy",
    sad: "motion_sad",
    angry: "motion_angry",
    surprised: "motion_surprised",
  }[emotion] || "motion_neutral";

  model.motion(motionGroup).play();
}

loadModel().then((model) => {
  document.getElementById("voice-btn").addEventListener("click", () => {
    const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.lang = "en-US";
    recognition.onresult = (event) => {
      const input = event.results[0][0].transcript;
      fetch("/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ message: input })
      })
        .then(res => res.json())
        .then(data => {
          if (data.emotion) triggerEmotion(model, data.emotion);
        });
    };
    recognition.start();
  });
});

  </script>
</body>
</html>
const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognition.continuous = true;
recognition.onresult = function(event) {
  const transcript = event.results[event.results.length - 1][0].transcript;
  enaMouth.move(transcript);  // Lip animation controller
  setEmotion(transcript);     // Trigger emotion animation
};
recognition.start();

function setEmotion(emotionText) {
  const emotion = emotionText.toLowerCase();

  if (emotion.includes("happy") || emotion.includes("joy") || emotion.includes("excited")) {
    playLottie("ena", "assets/ena-happy.json");
    switchCubismMotion("happy");
  } else if (emotion.includes("sad") || emotion.includes("melancholy")) {
    playLottie("ena", "assets/ena-sad.json");
    switchCubismMotion("sad");
  } else if (emotion.includes("angry") || emotion.includes("frustrated")) {
    playLottie("ena", "assets/ena-angry.json");
    switchCubismMotion("angry");
  } else if (emotion.includes("curious") || emotion.includes("thinking")) {
    playLottie("ena", "assets/ena-curious.json");
    switchCubismMotion("curious");
  } else {
    playLottie("ena", "assets/ena-neutral.json");
    switchCubismMotion("neutral");
  }
}
<style>
  .sound-button {
    position: absolute;
    top: 10px;
    right: 10px;
    padding: 10px;
    z-index: 1000;
    background-color: rgba(0,0,0,0.6);
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 14px;
  }
</style>

<script>
  const music = document.getElementById('background-music');
  const toggleBtn = document.getElementById('toggle-music');
  let musicEnabled = false;

  toggleBtn.addEventListener('click', () => {
    if (musicEnabled) {
      music.pause();
      toggleBtn.innerText = '🔇 Music Off';
    } else {
      music.play().catch(() => {}); // Silent catch for mobile
      toggleBtn.innerText = '🔊 Music On';
    }
    musicEnabled = !musicEnabled;
  });

  // Mobile workaround: play on first interaction
  window.addEventListener('touchstart', () => {
    if (!musicEnabled) {
      music.play().catch(() => {});
    }
  }, { once: true });
</script>
<!-- 🔊 Background Music -->
<audio id="background-music" src="/assets/bg-music.mp3" loop preload="auto"></audio>

<!-- 🔊 Sound Control -->
<button id="toggle-music" class="sound-button">🔊 Music On</button>

<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width,