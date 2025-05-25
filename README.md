# Ray250527
Index file attempt
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI Gig Matcher</title>
  <!-- Manifest for PWA -->
  <link rel="manifest" href="data:application/manifest+json,{
    &quot;name&quot;: &quot;AI Gig Matcher&quot;,
    &quot;short_name&quot;: &quot;GigMatch&quot;,
    &quot;start_url&quot;: &quot;.&quot;,
    &quot;display&quot;: &quot;standalone&quot;,
    &quot;background_color&quot;: &quot;#ffffff&quot;,
    &quot;theme_color&quot;: &quot;#333333&quot;
  }" />
  <!-- PWA Meta -->
  <meta name="theme-color" content="#333333" />
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      padding: 20px;
      background: #f0f4f8;
      color: #333;
    }
    #app {
      max-width: 600px;
      margin: auto;
      background: white;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,0,0,0.1);
      padding: 25px;
    }
    h1 {
      text-align: center;
      color: #2a2a2a;
    }
    .question {
      margin-bottom: 20px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: 600;
    }
    select, button {
      width: 100%;
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background-color: #007BFF;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    #result {
      margin-top: 25px;
      font-size: 18px;
      font-weight: bold;
      background: #e9ffe8;
      border-left: 5px solid #4caf50;
      padding: 15px;
      border-radius: 5px;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div id="app">
    <h1>Find Your AI Gig</h1>
    <form id="quizForm">
      <div class="question">
        <label>1. What do you enjoy most?</label>
        <select name="q1" required>
          <option value="">Choose...</option>
          <option value="writing">Writing</option>
          <option value="design">Designing</option>
          <option value="talking">Talking with people</option>
          <option value="coding">Coding</option>
        </select>
      </div>
      <div class="question">
        <label>2. Preferred work style?</label>
        <select name="q2" required>
          <option value="">Choose...</option>
          <option value="solo">Solo & focused</option>
          <option value="team">Team collaboration</option>
          <option value="client">Client interaction</option>
        </select>
      </div>
      <div class="question">
        <label>3. How technical are you?</label>
        <select name="q3" required>
          <option value="">Choose...</option>
          <option value="beginner">Beginner</option>
          <option value="intermediate">Intermediate</option>
          <option value="expert">Expert</option>
        </select>
      </div>
      <button type="submit">Get My Gig!</button>
    </form>
    <div id="result" class="hidden"></div>
  </div>
  <script>
    document.getElementById("quizForm").addEventListener("submit", function(e) {
      e.preventDefault();
      const q1 = e.target.q1.value;
      const q2 = e.target.q2.value;
      const q3 = e.target.q3.value;
      let result = "";
      if (q1 === "writing" && q3 !== "expert") {
        result = "Try being an AI Copywriter using tools like Jasper or ChatGPT.";
      } else if (q1 === "design" && q3 !== "beginner") {
        result = "AI Image Creator — use tools like Midjourney or DALL·E.";
      } else if (q1 === "talking") {
        result = "AI Chatbot Assistant — help clients automate support with tools like ChatGPT or Tidio.";
      } else if (q1 === "coding") {
        result = "AI App Developer — build tools powered by GPT, Claude, or Hugging Face APIs.";
      } else {
        result = "Explore gigs like AI transcription, data labeling, or prompt engineering!";
      }
      document.getElementById("result").innerText = result;
      document.getElementById("result").classList.remove("hidden");
      localStorage.setItem("aiGigResult", result);
    });
    // Register Service Worker
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', () => {
        navigator.serviceWorker.register('sw.js').catch(err => console.log('SW registration failed', err));
      });
    }
  </script>
  <!-- Inlined Service Worker Script (via Blob) -->
  <script>
    if ('serviceWorker' in navigator) {
      const swCode = `
        self.addEventListener('install', function(e) {
          e.waitUntil(
            caches.open('gig-matcher').then(cache => {
              return cache.addAll([
                '/',
                '/index.html'
              ]);
            })
          );
        });
        self.addEventListener('fetch', function(e) {
          e.respondWith(
            caches.match(e.request).then(response => {
              return response || fetch(e.request);
            })
          );
        });
      `;
      const blob = new Blob([swCode], { type: 'application/javascript' });
      const swURL = URL.createObjectURL(blob);
      navigator.serviceWorker.register(swURL);
    }
  </script>
</body>
</html>


