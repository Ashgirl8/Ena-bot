<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Chat with Ena</title>
  <style>
    body {
      background: #0f0f0f;
      color: #f2f2f2;
      font-family: "Segoe UI", sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }
    h1 {
      color: #ffc0cb;
      font-size: 2rem;
    }
    #chat {
      width: 100%;
      max-width: 600px;
      height: 400px;
      overflow-y: auto;
      background: #1e1e1e;
      border-radius: 10px;
      padding: 1rem;
      margin-bottom: 1rem;
      box-shadow: 0 0 15px rgba(255, 192, 203, 0.2);
    }
    .message {
      margin-bottom: 1rem;
    }
    .user { color: #66ccff; }
    .bot { color: #ffd1dc; }
    input[type="text"] {
      width: 100%;
      max-width: 600px;
      padding: 0.7rem;
      font-size: 1rem;
      border: none;
      border-radius: 8px;
      outline: none;
      background: #2a2a2a;
      color: #fff;
    }
  </style>
</head>
<body>
  <h1>Chat with Ena</h1>
  <div id="chat"></div>
  <input id="input" type="text" placeholder="Say something..." autocomplete="off"/>

  <!-- Background Music -->
  <audio autoplay loop>
    <source src="https://www.bensound.com/bensound-music/bensound-sunny.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <script>
    const input = document.getElementById("input");
    const chat = document.getElementById("chat");

    const apiKey = "YOUR_OPENAI_API_KEY"; // Replace with your actual key

    const systemPrompt = {
      role: "system",
      content: `You are Ena, a nurturing, witty, and intelligent female AI.
      You speak with warmth, offer comfort when needed, and bring a touch of clever humor.
      You're insightful, friendly, and emotionally in-tune, like a modern-day oracle with charm and empathy.`
    };

    const messages = [systemPrompt];

    async function getResponse(userInput) {
      messages.push({ role: "user", content: userInput });

      const response = await fetch("https://api.openai.com/v1/chat/completions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${apiKey}`
        },
        body: JSON.stringify({
          model: "gpt-4",
          messages: messages
        })
      });

      const data = await response.json();
      const reply = data.choices[0].message.content;
      messages.push({ role: "assistant", content: reply });

      return reply;
    }

    input.addEventListener("keydown", async (e) => {
      if (e.key === "Enter" && input.value.trim()) {
        const userText = input.value.trim();
        appendMessage("You", userText, "user");
        input.value = "";

        appendMessage("Ena", "Typing...", "bot");
        const botReply = await getResponse(userText);
        updateLastBotMessage(botReply);
      }
    });

    function appendMessage(sender, text, cls) {
      const msg = document.createElement("div");
      msg.className = `message ${cls}`;
      msg.innerHTML = `<strong>${sender}:</strong> ${text}`;
      chat.appendChild(msg);
      chat.scrollTop = chat.scrollHeight;
    }

    function updateLastBotMessage(newText) {
      const messages = chat.getElementsByClassName("bot");
      if (messages.length > 0) {
        messages[messages.length - 1].innerHTML = `<strong>Ena:</strong> ${newText}`;
        chat.scrollTop = chat.scrollHeight;
      }
    }
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
# Ena-bot
Chat bot named ena
