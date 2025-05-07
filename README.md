<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>KBC Smart Quiz App</title>
  <style>
    body {
      font-family: 'Verdana', sans-serif;
      background-color: #0c0032;
      color: #fff;
      margin: 0;
      padding: 0;
      text-align: center;
    }
    h1, h2 {
      color: gold;
    }
    #login-container, #signup-container, #quiz-container {
      max-width: 500px;
      margin: auto;
      padding: 20px;
      background: #1a1a40;
      border-radius: 10px;
    }
    input, select, button {
      width: 80%;
      margin: 10px 0;
      padding: 10px;
      font-size: 1em;
      border-radius: 5px;
      border: none;
    }
    button {
      background-color: #fdbb2d;
      cursor: pointer;
    }
    button:hover {
      background-color: #fcb045;
    }
    #question-box {
      margin-top: 20px;
    }
    #timer {
      font-size: 1.5em;
      color: cyan;
    }
    #options button {
      display: block;
      margin: 10px auto;
      width: 90%;
    }
    .correct { background-color: green !important; }
    .incorrect { background-color: red !important; }
    #super-sawaal {
      display: none;
      color: red;
      font-weight: bold;
    }
    #ladder {
      list-style: none;
      padding: 0;
      margin-top: 20px;
      background-color: #1c1c3c;
      border-radius: 10px;
    }
    #ladder li {
      padding: 5px;
      border-bottom: 1px solid #444;
    }
    #ladder li.active {
      background-color: gold;
      color: black;
    }
    #team {
      margin-top: 30px;
      font-size: 0.9em;
      color: #ccc;
    }
  </style>
</head>
<body>
  <div id="login-container">
    <h2>Login</h2>
    <input type="text" id="login-username" placeholder="Username" />
    <input type="password" id="login-password" placeholder="Password" />
    <button onclick="login()">Login</button>
    <p>Don't have an account? <a href="#" onclick="showSignup()">Sign up</a></p>
  </div>

  <div id="signup-container" style="display: none;">
    <h2>Sign Up</h2>
    <input type="text" id="signup-username" placeholder="Username" />
    <input type="password" id="signup-password" placeholder="Password" />
    <button onclick="signup()">Sign Up</button>
    <p>Already have an account? <a href="#" onclick="showLogin()">Login</a></p>
  </div>

  <div id="quiz-container" style="display: none;">
    <h1>KBC Smart Quiz</h1>
    <select id="category-selector" onchange="loadQuestions()">
      <option value="">Select Category</option>
      <option value="general">General Knowledge</option>
      <option value="math">Math</option>
      <option value="english">English</option>
      <option value="nepali">Nepali</option>
      <option value="chemistry">Chemistry</option>
      <option value="physics">Physics</option>
      <option value="computer">Computer</option>
    </select>

    <div id="question-box">
      <div id="timer">⏳ 20</div>
      <div id="question"></div>
      <div id="options"></div>
      <div id="super-sawaal">🔥 Super Sawaal!</div>
      <div id="host-message"></div>
      <div id="controls">
        <button onclick="use5050()">50:50</button>
        <button onclick="quitGame()">Quit Game</button>
      </div>
    </div>

    <div id="prize-ladder">
      <h3>Prize Ladder</h3>
      <ul id="ladder"></ul>
    </div>
    <div id="team">
      <p>Team Members: Promish Rawal, Sandesh GC, Subodh Karki, Saurav Thapa, Aaryan Chhetri</p>
    </div>
  </div>

  <script>
    const questions = {
      general: [
        {
          q: "What is the capital of Nepal?",
          options: ["Pokhara", "Kathmandu", "Biratnagar", "Lalitpur"],
          answer: 1,
          super: false
        },
        {
          q: "Which is the longest river in the world?",
          options: ["Amazon", "Ganga", "Nile", "Mississippi"],
          answer: 2,
          super: true
        }
      ]
    };

    let currentQ = 0, currentCategory = '', score = 0, timer, timeLeft = 20, used5050 = false;

    function showSignup() {
      document.getElementById('login-container').style.display = 'none';
      document.getElementById('signup-container').style.display = 'block';
    }
    function showLogin() {
      document.getElementById('signup-container').style.display = 'none';
      document.getElementById('login-container').style.display = 'block';
    }
    function login() {
      document.getElementById('login-container').style.display = 'none';
      document.getElementById('quiz-container').style.display = 'block';
    }
    function signup() {
      alert("Account created! Please login.");
      showLogin();
    }

    function loadQuestions() {
      currentCategory = document.getElementById('category-selector').value;
      if (!currentCategory) return;
      currentQ = 0; score = 0; used5050 = false;
      showQuestion();
    }

    function showQuestion() {
      const qObj = questions[currentCategory][currentQ];
      document.getElementById('question').textContent = qObj.q;
      const options = document.getElementById('options');
      options.innerHTML = '';
      qObj.options.forEach((opt, idx) => {
        const btn = document.createElement('button');
        btn.textContent = opt;
        btn.onclick = () => lockAnswer(idx);
        options.appendChild(btn);
      });
      document.getElementById('super-sawaal').style.display = qObj.super ? 'block' : 'none';
      document.getElementById('host-message').textContent = '';
      startTimer();
    }

    function startTimer() {
      clearInterval(timer);
      timeLeft = 20;
      document.getElementById('timer').textContent = `⏳ ${timeLeft}`;
      timer = setInterval(() => {
        timeLeft--;
        document.getElementById('timer').textContent = `⏳ ${timeLeft}`;
        if (timeLeft <= 0) {
          clearInterval(timer);
          lockAnswer(-1);
        }
      }, 1000);
    }

    function lockAnswer(index) {
      clearInterval(timer);
      const qObj = questions[currentCategory][currentQ];
      const options = document.getElementById('options').children;
      document.getElementById('host-message').textContent = 'Computer ji, lock kar diya jaye...';
      setTimeout(() => {
        for (let i = 0; i < options.length; i++) {
          if (i === qObj.answer) options[i].classList.add('correct');
          else if (i === index) options[i].classList.add('incorrect');
        }
        if (index === qObj.answer) score++;
        currentQ++;
        setTimeout(() => {
          if (currentQ < questions[currentCategory].length) showQuestion();
          else document.getElementById('host-message').textContent = `Level ${currentCategory} is complete!`;
        }, 2000);
      }, 2000);
    }

    function use5050() {
      if (used5050) return;
      used5050 = true;
      const qObj = questions[currentCategory][currentQ];
      let count = 0;
      for (let i = 0; i < 4; i++) {
        if (i !== qObj.answer && count < 2) {
          document.getElementById('options').children[i].style.display = 'none';
          count++;
        }
      }
    }

    function quitGame() {
      alert(`You quit with score: ${score}`);
      location.reload();
    }
  </script>
</body>
</html>
