# Student-portal
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Justice Academy Voting Portal</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>🗳 Justice Academy Student Voting Portal</h1>
  </header>

  <section id="vote">
    <h2>Cast Your Vote</h2>
    <form id="voteForm">
      <label for="name">Full Name:</label>
      <input type="text" id="name" required>

      <label for="age">Age:</label>
      <input type="number" id="age" min="10" max="100" required>

      <label for="index">Index Number:</label>
      <input type="text" id="index" required>

      <label for="student">Select Candidate:</label>
      <select id="student" required>
        <option value="">--Choose Candidate--</option>
        <option value="Chidera">Chidera</option>
        <option value="Chikachi">Chikachi</option>
        <option value="Charis">Charis</option>
        <option value="Lesta">Lesta</option>
        <option value="Jonas">Jonas</option>
        <option value="Benard">Benard</option>
        <option value="Chisom">Chisom</option>
        <option value="Beastmark">Beastmark</option>
        <option value="Joshua">Joshua</option>
        <option value="Peter">Peter</option>
      </select>

      <!-- Simple CAPTCHA -->
      <label for="captcha">Type the number shown: <span id="captchaQuestion"></span></label>
      <input type="text" id="captcha" required>

      <button type="submit">Submit Vote</button>
    </form>
    <p id="voteMessage"></p>
  </section>

  <section id="results">
    <h2>Live Results</h2>
    <ul id="resultsList"></ul>
  </section>

  <footer>
    <p>&copy; 2026 Justice Academy Voting Portal</p>
  </footer>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: 'Segoe UI', Arial, sans-serif;
  margin: 0;
  background: #f4f4f4;
  color: #333;
}

header {
  background: #2c3e50;
  color: #fff;
  padding: 1.5rem;
  text-align: center;
}

section {
  padding: 2rem;
  margin: 1rem;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}

form {
  display: flex;
  flex-direction: column;
  max-width: 400px;
}

form label {
  margin-top: 10px;
}

form input, form select {
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

form button {
  margin-top: 15px;
  padding: 10px;
  background: #2c3e50;
  color: #fff;
  border: none;
  cursor: pointer;
}

form button:hover {
  background: #34495e;
}

#resultsList {
  list-style: none;
  padding: 0;
}

#resultsList li {
  margin: 5px 0;
  padding: 8px;
  background: #ecf0f1;
  border-radius: 5px;
}
// Candidate list
const candidates = [
  "Chidera","Chikachi","Charis","Lesta","Jonas",
  "Benard","Chisom","Beastmark","Joshua","Peter"
];
let votes = {};
candidates.forEach(name => votes[name] = 0);

// CAPTCHA setup
let captchaAnswer;
function generateCaptcha() {
  const num1 = Math.floor(Math.random() * 10);
  const num2 = Math.floor(Math.random() * 10);
  captchaAnswer = num1 + num2;
  document.getElementById("captchaQuestion").textContent = `${num1} + ${num2}`;
}
generateCaptcha();

// Handle voting
document.getElementById("voteForm").addEventListener("submit", function(event) {
  event.preventDefault();

  const name = document.getElementById("name").value.trim();
  const age = parseInt(document.getElementById("age").value);
  const index = document.getElementById("index").value.trim();
  const student = document.getElementById("student").value;
  const captcha = parseInt(document.getElementById("captcha").value);

  if (!name || !index || !student || isNaN(age)) {
    document.getElementById("voteMessage").textContent = "Please fill all fields.";
    return;
  }

  if (captcha !== captchaAnswer) {
    document.getElementById("voteMessage").textContent = "CAPTCHA failed. Try again.";
    generateCaptcha();
    return;
  }

  votes[student]++;
  document.getElementById("voteMessage").textContent =
    `Thank you ${name} (Index: ${index}, Age: ${age}) for voting for ${student}.`;

  updateResults();
  generateCaptcha(); // refresh captcha
});

// Update results
function updateResults() {
  const resultsList = document.getElementById("resultsList");
  resultsList.innerHTML = "";
  for (const [name, count] of Object.entries(votes)) {
    const li = document.createElement("li");
    li.textContent = `${name}: ${count} votes`;
    resultsList.appendChild(li);
  }
}

// Initialize results
updateResults();
