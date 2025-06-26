<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Calcoo | Math Master</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://unpkg.com/@lottiefiles/lottie-player@latest/dist/lottie-player.js"></script>
  <style>
    /* (CSS remains unchanged for brevity) */
  </style>
</head>
<body>
  <!-- (HTML UI remains the same) -->
  <script>
    let splashAnimations = [
      "https://lottie.host/26e8e339-5d92-4885-b30e-5f5e37312bd8/MwczDv.json",
      "https://lottie.host/3432f8e1-2ff9-4d8f-8cfd-9600aa011c6e/Nmt1Z3.json"
    ];
    let correctAnim = "https://lottie.host/c80d4b3e-37cb-4261-a0f4-bccc6a56dbb6/YkGGVo.json";
    let wrongAnim = "https://lottie.host/9ce7c81e-2b41-4fa6-b6bc-34a9f40cfdd9/LCqYjs.json";
    let alarmSound = new Audio("https://cdn.pixabay.com/download/audio/2022/03/15/audio_23faac1138.mp3?filename=alarm.mp3");
    let currentOperation = '+';
    let questions = [], currentIndex = 0, correctCount = 0, attempts = 0;
    let timer, timeLeft = 30;
    let sessionOver = false;

    window.onload = () => {
      const anim = splashAnimations[Math.floor(Math.random() * splashAnimations.length)];
      document.getElementById('splashAnim').setAttribute('src', anim);
      setTimeout(() => {
        document.getElementById('splash').classList.add('hidden');
        document.getElementById('app').classList.remove('hidden');
      }, 3000);

      document.addEventListener('keydown', (e) => {
        if (sessionOver) return;
        if (e.key === 'Enter') submitAnswer();
        if (e.key === ' ') {
          e.preventDefault();
          nextQuestion();
        }
      });
    };

    function playSound(id) {
      const sound = document.getElementById(id);
      if (sound) sound.play();
    }

    function showFeedback(type) {
      const player = document.getElementById('feedbackAnim');
      player.setAttribute('src', type === 'correct' ? correctAnim : wrongAnim);
      player.classList.remove('hidden');
      setTimeout(() => player.classList.add('hidden'), 1500);
    }

    function playLogoAnim() {
      const anim = splashAnimations[Math.floor(Math.random() * splashAnimations.length)];
      const logo = document.getElementById('feedbackAnim');
      logo.setAttribute('src', anim);
      logo.classList.remove('hidden');
      setTimeout(() => logo.classList.add('hidden'), 2000);
    }

    function setOperation(op) {
      currentOperation = op;
      renderQuestion();
    }

    function startSession(level) {
      questions = [];
      sessionOver = false;
      for (let i = 0; i < 50; i++) {
        let a = Math.floor(Math.random() * 100);
        let b = Math.floor(Math.random() * 100);
        if (currentOperation === '/' && b === 0) b = 1;
        questions.push({ a, b });
      }
      currentIndex = 0;
      correctCount = 0;
      attempts = 0;
      renderQuestion();
    }

    function renderQuestion() {
      if (currentIndex >= questions.length) return;
      const q = questions[currentIndex];
      document.getElementById('question').innerText = `${q.a} ${currentOperation} ${q.b} = ?`;
      document.getElementById('answerInput').value = '';
      document.getElementById('result').innerText = '';
      timeLeft = 30;
      startTimer();
    }

    function getCorrectAnswer(q) {
      switch (currentOperation) {
        case '+': return q.a + q.b;
        case '-': return q.a - q.b;
        case '*': return q.a * q.b;
        case '/': return +(q.a / q.b).toFixed(2);
      }
    }

    function submitAnswer() {
      clearInterval(timer);
      const q = questions[currentIndex];
      const input = parseFloat(document.getElementById('answerInput').value);
      const correct = getCorrectAnswer(q);
      attempts++;
      if (input === correct) {
        correctCount++;
        document.getElementById('result').innerText = '✅ Correct!';
        showFeedback('correct');
        playSound('correctSound');
      } else {
        document.getElementById('result').innerText = `❌ Wrong. Ans: ${correct}`;
        showFeedback('wrong');
        playSound('wrongSound');
      }
    }

    function nextQuestion() {
      clearInterval(timer);
      currentIndex++;
      if (currentIndex < questions.length) {
        renderQuestion();
      } else {
        endSession();
      }
    }

    function startTimer() {
      clearInterval(timer);
      document.getElementById('timer').innerText = `⏱ Time Left: 00:${timeLeft}`;
      timer = setInterval(() => {
        timeLeft--;
        document.getElementById('timer').innerText = `⏱ Time Left: 00:${timeLeft < 10 ? '0' + timeLeft : timeLeft}`;
        if (timeLeft <= 0) {
          clearInterval(timer);
          alarmSound.play();
          document.getElementById('result').innerText = '⏰ Time up! Game Over';
          endSession();
        }
      }, 1000);
    }

    function endSession() {
      sessionOver = true;
      document.getElementById('app').style.display = 'none';
      document.getElementById('reportCard').style.display = 'block';
      document.getElementById('summaryText').innerText = `Correct: ${correctCount}, Attempts: ${attempts}`;
    }
  </script>
</body>
</html>

