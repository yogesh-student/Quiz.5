<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>5 Question Quiz</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f4f4f4; }
    h2, h3 { color: #333; }
    .question { margin-bottom: 20px; }
    .options label { display: block; }
    #result, #previous { margin-top: 30px; font-weight: bold; font-size: 18px; }
    button { padding: 10px 15px; font-size: 16px; cursor: pointer; }
  </style>
</head>
<body>
  <h2>Quick Quiz</h2>

  <div id="previous"></div>

  <form id="quizForm">
    <div class="question">
      <p>Question 1:</p>
      <div class="options">
        <label><input type="radio" name="q1" value="A"> A.</label>
        <label><input type="radio" name="q1" value="B"> B.</label>
        <label><input type="radio" name="q1" value="C"> C.</label>
        <label><input type="radio" name="q1" value="D"> D.</label>
      </div>
    </div>

    <div class="question">
      <p>Question 2:</p>
      <div class="options">
        <label><input type="radio" name="q2" value="A"> A.</label>
        <label><input type="radio" name="q2" value="B"> B.</label>
        <label><input type="radio" name="q2" value="C"> C.</label>
        <label><input type="radio" name="q2" value="D"> D.</label>
      </div>
    </div>

    <div class="question">
      <p>Question 3:</p>
      <div class="options">
        <label><input type="radio" name="q3" value="A"> A.</label>
        <label><input type="radio" name="q3" value="B"> B.</label>
        <label><input type="radio" name="q3" value="C"> C.</label>
        <label><input type="radio" name="q3" value="D"> D.</label>
      </div>
    </div>

    <div class="question">
      <p>Question 4:</p>
      <div class="options">
        <label><input type="radio" name="q4" value="A"> A.</label>
        <label><input type="radio" name="q4" value="B"> B.</label>
        <label><input type="radio" name="q4" value="C"> C.</label>
        <label><input type="radio" name="q4" value="D"> D.</label>
      </div>
    </div>

    <div class="question">
      <p>Question 5:</p>
      <div class="options">
        <label><input type="radio" name="q5" value="A"> A.</label>
        <label><input type="radio" name="q5" value="B"> B.</label>
        <label><input type="radio" name="q5" value="C"> C.</label>
        <label><input type="radio" name="q5" value="D"> D.</label>
      </div>
    </div>

    <button type="button" onclick="calculateScore()">Submit Quiz</button>
  </form>

  <div id="result"></div>

  <script>
    const answerKey = {
      q1: 'B',
      q2: 'A',
      q3: 'C',
      q4: 'D',
      q5: 'B'
    };

    function loadPreviousScore() {
      const prev = localStorage.getItem("quizResults");
      if (prev) {
        const data = JSON.parse(prev);
        document.getElementById("previous").innerHTML =
          `<h3>Previous Attempt:</h3>
          Score: ${data.score} <br>
          Correct: ${data.correct} | Incorrect: ${data.incorrect} | Skipped: ${data.skipped}`;
      } else {
        document.getElementById("previous").innerHTML = "<h3>No Previous Attempt Found</h3>";
      }
    }

    function calculateScore() {
      let score = 0;
      let correct = 0;
      let incorrect = 0;
      let skipped = 0;
      let resultText = "";

      for (let i = 1; i <= 5; i++) {
        const q = "q" + i;
        const selected = document.querySelector(`input[name="${q}"]:checked`);
        if (!selected) {
          score -= 3;
          skipped++;
          resultText += `Question ${i}: Skipped (-3)<br>`;
        } else if (selected.value === answerKey[q]) {
          score += 7;
          correct++;
          resultText += `Question ${i}: Correct (+7)<br>`;
        } else {
          score -= 9;
          incorrect++;
          resultText += `Question ${i}: Incorrect (-9)<br>`;
        }
      }

      resultText += `<br><strong>Total Score: ${score}</strong><br>`;
      resultText += `Correct: ${correct} | Incorrect: ${incorrect} | Skipped: ${skipped}`;
      document.getElementById("result").innerHTML = resultText;

      // Save current result to localStorage
      const currentResult = { score, correct, incorrect, skipped };
      localStorage.setItem("quizResults", JSON.stringify(currentResult));
    }

    window.onload = loadPreviousScore;
  </script>
</body>
</html>
