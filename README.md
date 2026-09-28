<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Attendance Calculator</title>

  <style>
    * {
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #667eea, #764ba2);
    }

    .calculator {
      width: 350px;
      padding: 30px;
      background: white;
      border-radius: 15px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    }

    h1 {
      text-align: center;
      margin-bottom: 25px;
      color: #333;
    }

    label {
      display: block;
      margin-top: 15px;
      margin-bottom: 5px;
      color: #555;
      font-weight: bold;
    }

    input {
      width: 100%;
      padding: 12px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    button {
      width: 100%;
      margin-top: 25px;
      padding: 13px;
      border: none;
      border-radius: 8px;
      background: #667eea;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }

    button:hover {
      background: #5568d9;
    }

    #result {
      margin-top: 20px;
      padding: 15px;
      border-radius: 8px;
      text-align: center;
      display: none;
      line-height: 1.6;
    }

    .good {
      background: #d4edda;
      color: #155724;
    }

    .bad {
      background: #f8d7da;
      color: #721c24;
    }
  </style>
</head>

<body>

  <div class="calculator">
    <h1>Attendance Calculator</h1>

    <label for="total">Total Classes</label>
    <input type="number" id="total" placeholder="e.g. 50" min="1">

    <label for="attended">Classes Attended</label>
    <input type="number" id="attended" placeholder="e.g. 40" min="0">

    <label for="required">Required Attendance (%)</label>
    <input type="number" id="required" value="75" min="1" max="100">

    <button onclick="calculateAttendance()">Calculate</button>

    <div id="result"></div>
  </div>

  <script>
    function calculateAttendance() {
      const total = Number(document.getElementById("total").value);
      const attended = Number(document.getElementById("attended").value);
      const required = Number(document.getElementById("required").value);
      const result = document.getElementById("result");

      if (total <= 0 || attended < 0 || required <= 0 || required > 100) {
        result.style.display = "block";
        result.className = "bad";
        result.innerHTML = "Please enter valid values.";
        return;
      }

      if (attended > total) {
        result.style.display = "block";
        result.className = "bad";
        result.innerHTML = "Attended classes cannot be greater than total classes.";
        return;
      }

      const percentage = (attended / total) * 100;

      result.style.display = "block";

      if (percentage >= required) {
        // Classes that can be missed while staying at required %
        const canMiss = Math.floor(
          (attended / (required / 100)) - total
        );

        result.className = "good";
        result.innerHTML = `
          <strong>Attendance: ${percentage.toFixed(2)}%</strong><br>
          🎉 You meet the ${required}% requirement.<br>
          You can miss approximately ${Math.max(0, canMiss)} more class(es).
        `;
      } else {
        // Minimum future classes needed to reach required %
        const needed = Math.ceil(
          (required * total / 100 - attended) / (1 - required / 100)
        );

        result.className = "bad";
        result.innerHTML = `
          <strong>Attendance: ${percentage.toFixed(2)}%</strong><br>
          ⚠️ You are below the required ${required}%.<br>
          Attend the next <strong>${needed}</strong> class(es)
          continuously to reach ${required}%.
        `;
      }
    }
  </script>

</body>
</html>
