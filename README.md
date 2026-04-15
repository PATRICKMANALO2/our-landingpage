[Landing page.html](https://github.com/user-attachments/files/26764358/Landing.page.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Smart Staycation Finder</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: white;
      text-align: center;
    }

    header { padding: 20px; }
    .container { padding: 40px 20px; }

    .glass {
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(10px);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
    }

    .video-section { margin: 30px auto; max-width: 800px; }
    video { width: 100%; border-radius: 10px; }

    .cards {
      display: flex;
      gap: 20px;
      justify-content: center;
      flex-wrap: wrap;
      margin-top: 40px;
    }

    .card {
      width: 250px;
      border-radius: 15px;
      overflow: hidden;
      background: rgba(255,255,255,0.1);
      backdrop-filter: blur(10px);
      transition: 0.3s;
    }

    .card img {
      width: 100%;
      height: 150px;
      object-fit: cover;
      transition: transform 0.4s ease;
    }

    .card:hover img { transform: scale(1.1); }

    .card:hover {
      box-shadow: 0 10px 25px rgba(0,0,0,0.3);
      transform: translateY(-5px);
    }

    .active {
      transform: scale(1.15) !important;
      box-shadow: 0 15px 35px rgba(0,0,0,0.5) !important;
      border: 2px solid #00c6ff;
    }

    .card h3 { margin: 10px 0; }

    input, select {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      border: none;
    }

    button {
      padding: 12px 20px;
      background-color: #00c6ff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .book-now button {
      font-size: 18px;
      padding: 15px 30px;
      background-color: #ff5e62;
    }

    .chatbot {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 260px;
      background: white;
      color: black;
      border-radius: 10px;
      display: none;
      overflow: hidden;
    }

    .chat-header {
      background: #2a5298;
      color: white;
      padding: 10px;
      cursor: pointer;
      font-weight: bold;
    }

    .chat-body {
      padding: 10px;
      height: 150px;
      overflow-y: auto;
      text-align: left;
      font-size: 14px;
    }

    .chat-footer { display: flex; border-top: 1px solid #ccc; }
    .chat-input { flex: 1; padding: 10px; border: none; outline: none; }
    .chat-btn { width: 70px; background: #00c6ff; color: white; border: none; cursor: pointer; }
    .chat-btn:hover { background: #009ad6; }

    .chat-toggle {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #00c6ff;
      color: white;
      padding: 10px;
      border-radius: 50%;
      cursor: pointer;
    }
  </style>
</head>
<body>

  <header>
    <h1>Smart Staycation Finder</h1>
    <p>Find your perfect stay using our smart recommendations</p>
  </header>

  <div class="container">

    <div class="video-section glass">
      <h2>Discover Your Perfect Stay</h2>
      <video autoplay muted loop playsinline controls>
        <source src="Promo vid.mp4" type="video/mp4">
      </video>
    </div>

    <div class="cards" id="cards">
      <div class="card" id="standard">
        <img src="Standard.jpg">
        <h3>Standard</h3>
        <p>₱1,500 / night</p>
      </div>

      <div class="card" id="deluxe">
        <img src="Deluxe.jpg">
        <h3>Deluxe</h3>
        <p>₱3,500 / night</p>
      </div>

      <div class="card" id="luxury">
        <img src="Luxury.jpg">
        <h3>Luxury</h3>
        <p>₱7,000 / night</p>
      </div>
    </div>

    <div class="glass" style="max-width:500px; margin:40px auto;">
      <h2>Find Your Perfect Stay</h2>

      <input type="number" id="budget" placeholder="Enter your budget (PHP)" min="1500">
      <div id="budgetError" style="color:#ffb3b3; font-size:14px; display:none;">Minimum amount is ₱1500</div>
      <input type="number" id="guests" placeholder="Number of guests">

      <select id="type">
        <option value="">Choose a Place</option>
        <option value="makati">Makati</option>
        <option value="pasig">Pasig</option>
        <option value="antipolo">Antipolo</option>
      </select>

      <br>
      <button onclick="recommendRoom()">Get Recommendation</button>
    </div>

    <div class="book-now">
      <button onclick="bookNow()">Book Now</button>
    </div>

  </div>

  <div class="chat-toggle" onclick="toggleChat()">💬</div>

  <div class="chatbot" id="chatbot">
    <div class="chat-header" onclick="toggleChat()">AI Assistant</div>
    <div class="chat-body" id="chatBody">
      <p><b>AI:</b> Hi! I can help you find a staycation 😊</p>
      <p><b>AI:</b> Try asking: "price" or "recommend"</p>
    </div>
    <div class="chat-footer">
      <input class="chat-input" id="chatInput" placeholder="Ask something...">
      <button class="chat-btn" onclick="sendMessage()">Send</button>
    </div>
  </div>

  <script>
    function clearActive() {
      document.getElementById("standard").classList.remove("active");
      document.getElementById("deluxe").classList.remove("active");
      document.getElementById("luxury").classList.remove("active");
    }

    function recommendRoom() {
      var budget = parseInt(document.getElementById("budget").value);

      if (budget < 1500) {
        document.getElementById("budgetError").style.display = "block";
        return;
      }

      document.getElementById("budgetError").style.display = "none";
      clearActive();

      if (budget < 3500) {
        document.getElementById("standard").classList.add("active");
      } else if (budget < 7000) {
        document.getElementById("deluxe").classList.add("active");
      } else {
        document.getElementById("luxury").classList.add("active");
      }

      document.getElementById("cards").scrollIntoView({ behavior: "smooth" });
    }

    function bookNow() {
      alert("Redirecting to booking page...");
    }

    function toggleChat() {
      var chat = document.getElementById("chatbot");
      chat.style.display = chat.style.display === "block" ? "none" : "block";
    }

    function sendMessage() {
      var inputEl = document.getElementById("chatInput");
      if (!inputEl.value.trim()) return;

      var input = inputEl.value;
      var body = document.getElementById("chatBody");
      var place = document.getElementById("type").value || "your selected location";

      body.innerHTML += "<p><b>You:</b> " + input + "</p>";

      var response = "";
      var number = parseInt(input.replace(/[^0-9]/g, ""));

      if (!isNaN(number)) {
        document.getElementById("budget").value = number;

        if (number < 1500) {
          document.getElementById("budgetError").style.display = "block";
          response = "Minimum amount is ₱1500";
        } else if (number < 3500) {
          document.getElementById("budgetError").style.display = "none";
          response = "We recommend a Standard(₱1,500/night) in " + place + ".";
          recommendRoom();
        } else if (number < 7000) {
          document.getElementById("budgetError").style.display = "none";
          response = "We recommend a Deluxe(₱3,500/night) in " + place + ".";
          recommendRoom();
        } else {
          document.getElementById("budgetError").style.display = "none";
          response = "We recommend a Luxury(₱7,000/night) in " + place + ".";
          recommendRoom();
        }
      } else if (input.toLowerCase().includes("price")) {
        response = "Price start at ₱1,500.";
      } else if (input.toLowerCase().includes("recommend")) {
        response = "Sure! What is your budget?";
      } else {
        response = "I can help you find a place! Try typing your budget.";
      }

      body.innerHTML += "<p><b>AI:</b> " + response + "</p>";
      inputEl.value = "";
      body.scrollTop = body.scrollHeight;
    }
  </script>

</body>
</html>
