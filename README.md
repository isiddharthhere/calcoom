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
