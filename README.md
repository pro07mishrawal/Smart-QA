<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Smart KBC Quiz App</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <!-- Login/Signup UI -->
  <div id="auth-screen">
    <h2>KBC Smart Quiz Login</h2>
    <input type="text" id="username" placeholder="Enter your name" />
    <button onclick="login()">Start Quiz</button>
  </div>

  <!-- Quiz Screen -->
  <div id="quiz-screen" style="display:none;">
    <div class="header">
      <h1>Kaun Banega Crorepati</h1>
      <div id="user-info"></div>
      <select id="category-selector" onchange="changeCategory(this.value)">
        <option value="General Knowledge">General Knowledge</option>
        <option value="Math">Math</option>
        <option value="English">English</option>
        <option value="Nepali">Nepali</option>
        <option value="Chemistry">Chemistry</option>
        <option value="Physics">Physics</option>
        <option value="Computer">Computer</option>
      </select>
    </div>

    <div id="quiz-box">
      <div id="question-number"></div>
      <div id="question-text"></div>
      <div id="options"></div>
      <div id="host-message"></div>
      <div id="super-sawaal">🔥 Super Sawaal!</div>
      <div id="timer">20</div>
    </div>

    <div id="prize-board"></div>

    <div id="summary" style="display:none;">
      <h2>Quiz Completed!</h2>
      <p id="summary-text"></p>
      <button onclick="restartQuiz()">Restart</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
