<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Quiz Comics - Devine le personnage</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Bangers&display=swap');

  body {
    font-family: 'Bangers', cursive;
    background: #111;
    color: #fff;
    text-align: center;
    margin: 0;
    padding: 20px;
    transition: background 0.6s ease;
  }

  h1 {
    color: #ffeb3b;
    text-shadow: 3px 3px #f00;
    font-size: 40px;
    margin-bottom: 30px;
  }

  .quiz-box {
    background: #222;
    border: 4px solid yellow;
    border-radius: 15px;
    box-shadow: 0 0 20px red;
    padding: 25px;
    max-width: 400px;
    margin: auto;
  }

  .emoji {
    font-size: 45px;
    margin-bottom: 20px;
  }

  input {
    padding: 10px;
    width: 80%;
    border-radius: 6px;
    border: none;
    font-size: 18px;
    text-align: center;
  }

  button {
    background: #ff4444;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 18px;
    cursor: pointer;
    margin-top: 10px;
    transition: 0.2s;
  }
  button:hover { background: #ff6666; }

  .timer {
    font-size: 22px;
    margin: 10px 0;
    color: #ffeb3b;
  }

  .score {
    font-size: 20px;
    margin-top: 10px;
    color: #00ff99;
  }

  .end-screen {
    display: none;
    padding: 20px;
  }

  .end-screen h2 {
    color: #ffeb3b;
    font-size: 32px;
  }
</style>
</head>
<body>

<h1>💥 QUIZ COMICS 💥</h1>

<div class="quiz-box" id="quiz">
  <div class="emoji" id="emoji">🕷️🕸️📸</div>
  <div class="timer" id="timer">⏱️ Temps : 10s</div>
  <input type="text" id="userAnswer" placeholder="Tape ta réponse ici">
  <br>
  <button onclick="checkAnswer()">Valider</button>
  <div class="score" id="score">Score : 0</div>
</div>

<div class="end-screen" id="end">
  <h2>🏆 Fin du quiz ! 🏆</h2>
  <p id="finalMessage"></p>
  <button onclick="restart()">🔁 Rejouer</button>
</div>

<script>
const questions = [
  { emojis: "🕷️🕸️📸", answer: "Spider-Man", color:"#d32f2f" },
  { emojis: "🛡️🇺🇸⭐", answer: "Captain America", color:"#1976d2" },
  { emojis: "🃏🤡💣", answer: "Joker", color:"#43a047" },
  { emojis: "🦇🌃💔", answer: "Batman", color:"#000000" },
  { emojis: "⚡🟥👊", answer: "Flash", color:"#ffeb3b" },
  { emojis: "👑🐆🌍", answer: "Black Panther", color:"#4a148c" }
];

// Mélange aléatoire
questions.sort(() => Math.random() - 0.5);

let current = 0;
let score = 0;
let timeLeft = 10;
let timerInterval;

function startTimer() {
  clearInterval(timerInterval);
  timeLeft = 10;
  document.getElementById("timer").innerText = `⏱️ Temps : ${timeLeft}s`;
  timerInterval = setInterval(() => {
    timeLeft--;
    document.getElementById("timer").innerText = `⏱️ Temps : ${timeLeft}s`;
    if(timeLeft <= 0) {
      clearInterval(timerInterval);
      nextQuestion(false);
    }
  }, 1000);
}

function updateQuestion() {
  const q = questions[current];
  document.getElementById("emoji").innerText = q.emojis;
  document.body.style.background = q.color;
  document.getElementById("userAnswer").value = "";
  document.getElementById("userAnswer").focus();
  startTimer();
}

function checkAnswer() {
  const input = document.getElementById("userAnswer").value.trim().toLowerCase();
  const correct = questions[current].answer.toLowerCase();
  clearInterval(timerInterval);
  if(input === correct) {
    score++;
  }
  nextQuestion();
}

function nextQuestion(auto=false) {
  current++;
  if(current >= questions.length) {
    endQuiz();
    return;
  }
  document.getElementById("score").innerText = "Score : " + score;
  updateQuestion();
}

function endQuiz() {
  document.getElementById("quiz").style.display = "none";
  document.getElementById("end").style.display = "block";
  const msg = score === questions.length 
    ? "🔥 Parfait ! Tu es un vrai super-héros !" 
    : score >= questions.length / 2 
      ? "💪 Pas mal ! Tu as de vrais pouvoirs !" 
      : "😅 Tu ferais mieux d’appeler Alfred pour t’aider...";
  document.getElementById("finalMessage").innerText = `Ton score final : ${score}/${questions.length}\n${msg}`;
}

function restart() {
  score = 0;
  current = 0;
  questions.sort(() => Math.random() - 0.5);
  document.getElementById("end").style.display = "none";
  document.getElementById("quiz").style.display = "block";
  document.getElementById("score").innerText = "Score : 0";
  updateQuestion();
}

// Démarrage initial
updateQuestion();
</script>

</body>
</html>
