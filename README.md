
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>smart cash</title>
  <style>
    :root {
      --primary: #7b1fa2;
      --secondary: #4a148c;
      --light-bg: #f8f9fc;
      --card-bg: #ffffff;
      --text-dark: #1a1a1a;
      --text-muted: #555;
      --border: #e0e0e0;
    }

    body.dark-mode {
      --light-bg: #0d0d0d;
      --card-bg: #1a1a1a;
      --text-dark: #f5f5f5;
      --text-muted: #aaa;
      --border: #333;
    }

    * { margin:0; padding:0; box-sizing:border-box; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: var(--light-bg);
      color: var(--text-dark);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      transition: background 0.4s, color 0.4s;
    }

    .page { display: none; flex:1; flex-direction:column; padding:1.5rem; max-width:800px; margin:0 auto; width:100%; }
    .page.active { display:flex; }

    .back-arrow {
      position: absolute;
      top: 15px;
      left: 15px;
      font-size: 28px;
      color: var(--primary);
      cursor: pointer;
      z-index: 10;
    }

    #splash {
      position: fixed; inset: 0;
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      z-index: 2000; transition: opacity 0.8s ease;
    }
    #splash.hidden { opacity: 0; pointer-events: none; }

    #splash h1 {
      font-size: 1.6rem; font-weight: 400; margin-bottom: 0.6rem;
      color: white; text-transform: lowercase; letter-spacing: 1px;
    }
    .hand-money { width: 68px; height: auto; animation: pulse 2s infinite ease-in-out; }
    @keyframes pulse { 0%,100% { transform: scale(1); } 50% { transform: scale(1.08); } }

    .card {
      background: var(--card-bg); padding: 2rem; border-radius: 16px;
      width: 100%; box-shadow: 0 6px 24px rgba(0,0,0,0.08); border: 1px solid var(--border);
      transition: background 0.4s;
    }

    .notice {
      background: #7b1fa2; color: white; padding: 0.9rem; border-radius: 10px;
      font-size: 0.95rem; margin-bottom: 1.5rem; text-align: center;
    }

    h2, h3 { text-align:center; margin-bottom:1.2rem; color:var(--primary); }

    input, select {
      width: 100%; padding: 0.95rem; margin-bottom: 1rem;
      border: 1px solid var(--border); border-radius: 10px; font-size: 1rem;
      background: var(--card-bg); color: var(--text-dark);
    }
    input:focus, select:focus { outline:none; border-color:var(--primary); box-shadow:0 0 0 3px rgba(123,31,162,0.15); }

    input[type="tel"] {
      -moz-appearance: textfield;
    }
    input[type="tel"]::-webkit-inner-spin-button,
    input[type="tel"]::-webkit-outer-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }

    button {
      width:100%; padding:1.05rem; background:var(--primary); color:white;
      border:none; border-radius:12px; font-size:1.05rem; cursor:pointer;
      transition: background 0.3s;
    }
    button:hover { background:var(--secondary); }

    .copy-icon {
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      color: var(--primary);
      margin-left: 8px;
      user-select: none;
    }
    .copy-icon:active { color: #4a148c; transform: scale(0.95); }

    .spinner {
      border: 5px solid rgba(123,31,162,0.2);
      border-top: 5px solid var(--primary);
      border-radius: 50%;
      width: 40px;
      height: 40px;
      animation: spin 0.8s linear infinite;
      margin: 2rem auto;
    }
    @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

    .balance-card {
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      color:white; padding:2.2rem; border-radius:16px; text-align:center;
      margin-bottom:2rem; box-shadow:0 10px 30px rgba(123,31,162,0.25);
    }
    .amount { font-size: 2.2rem; font-weight:700; }

    .grid {
      display:grid; grid-template-columns:repeat(auto-fit, minmax(140px, 1fr));
      gap:1rem; margin-bottom:2rem;
    }
    .btn-action {
      background:var(--card-bg); border:1px solid var(--border);
      padding:1.3rem 0.8rem; border-radius:12px; text-align:center;
      cursor:pointer; font-weight:500; box-shadow:0 3px 10px rgba(0,0,0,0.05);
      display:flex; flex-direction:column; align-items:center; gap:6px;
    }
    .btn-action:hover { transform:translateY(-4px); border-color:var(--primary); }

    #logoutBtn { margin-top:auto; background:#d32f2f; color:white; border:none; }

    .details p {
      margin: 10px 0;
      font-size: 1.1rem;
      font-weight: 500;
    }

    .dark-toggle {
      font-size: 16px;
      cursor: pointer;
      color: var(--primary);
      opacity: 0.65;
      margin: 0.3rem auto 1.2rem;
      text-align: center;
      display: block;
      transition: opacity 0.3s;
    }
    .dark-toggle:hover { opacity: 0.9; }
  </style>
</head>
<body>

  <!-- Splash -->
  <div id="splash">
    <h1>welcome to Easy cash</h1>
    <img src="https://thumbs.dreamstime.com/b/fair-hand-holding-d-rendered-nigerian-naira-notes-isolated-white-background-233559633.jpg" alt="Hand holding money" class="hand-money"/>
  </div>

  <!-- Login -->
  <div id="login" class="page active">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card">
      <div class="notice">Enter your name and Gmail to login and access your dashboard.</div>
      <h2>Login</h2>
      <form id="loginForm">
        <input type="text" id="name" placeholder="Your Name" required>
        <input type="email" id="email" placeholder="yourname@gmail.com" required>
        <button type="submit">Login</button>
      </form>
    </div>
  </div>

  <!-- Notification -->
  <div id="notification" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card">
      <div class="notice">Your account has been credited. Click OK to view dashboard.</div>
      <h3 id="notifMsg"></h3>
      <button id="okBtn">OK</button>
    </div>
  </div>

  <!-- Dashboard -->
  <div id="dashboard" class="page">
    <div class="notice">Welcome! Tap any button below to perform actions.</div>
    <div id="welcomeHeader" style="text-align:center;font-size:1.5rem;font-weight:600;color:var(--primary);margin-bottom:0.2rem;"></div>
    <span class="dark-toggle" id="darkToggle">🌙</span>
    
    <div class="balance-card">
      <h3>Available Balance</h3>
      <div class="amount" id="balance">₦150,000</div>
    </div>

    <div class="grid">
      <div class="btn-action" id="withdrawBtn">💳 Withdraw</div>
      <div class="btn-action" id="helpBtn">❓ Help</div>
      <div class="btn-action" id="groupBtn">👥 Group</div>
      <div class="btn-action">📋 FAQ</div>
      <div class="btn-action" id="airtimeBtn">📞 Airtime</div>
      <div class="btn-action" id="dataBtn">📡 Data</div>
    </div>

    <button id="logoutBtn" class="btn-action">Log Out</button>
  </div>

  <!-- Help Page -->
  <div id="helpPage" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card">
      <h2>Support</h2>
      <div id="helpInstructions">
        Copy the Telegram username below,<br>
        open your Telegram app, paste it in the search bar,<br>
        and contact support for help.
      </div>
      <div id="usernameBox">
        <div id="username">@EASYMONIE010</div>
        <div id="copyContainer">
          <span class="copy-icon" onclick="copyTelegramUsername()">copy</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Withdrawal Page -->
  <div id="withdrawal" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="notice">Enter your bank details to withdraw exactly ₦150,000.</div>
    <div class="card">
      <h2>Withdraw Funds</h2>
      <form id="withdrawForm">
        <input type="text" id="accName" placeholder="Account Name" required>
        <select id="bank" required>
          <option value="">Select Bank</option>
          <option value="Access Bank Plc">Access Bank Plc</option>
          <option value="Zenith Bank Plc">Zenith Bank Plc</option>
          <option value="Guaranty Trust Bank Plc">Guaranty Trust Bank Plc</option>
          <option value="First Bank of Nigeria Limited">First Bank of Nigeria Limited</option>
          <option value="United Bank for Africa Plc">United Bank for Africa Plc</option>
          <option value="Fidelity Bank Plc">Fidelity Bank Plc</option>
          <option value="Ecobank Nigeria Plc">Ecobank Nigeria Plc</option>
          <option value="Wema Bank Plc">Wema Bank Plc</option>
          <option value="Sterling Bank Plc">Sterling Bank Plc</option>
          <option value="Stanbic IBTC Bank Plc">Stanbic IBTC Bank Plc</option>
          <option value="Union Bank of Nigeria Plc">Union Bank of Nigeria Plc</option>
          <option value="Polaris Bank Limited">Polaris Bank Limited</option>
          <option value="Keystone Bank Limited">Keystone Bank Limited</option>
          <option value="Heritage Bank Plc">Heritage Bank Plc</option>
          <option value="Unity Bank Plc">Unity Bank Plc</option>
          <option value="Jaiz Bank Plc">Jaiz Bank Plc</option>
          <option value="Citibank Nigeria Limited">Citibank Nigeria Limited</option>
          <option value="Standard Chartered Bank Limited">Standard Chartered Bank Limited</option>
          <option value="First City Monument Bank Limited (FCMB)">First City Monument Bank Limited (FCMB)</option>
          <option value="Titan Trust Bank Limited">Titan Trust Bank Limited</option>
          <option value="Globus Bank Limited">Globus Bank Limited</option>
          <option value="Premium Trust Bank">Premium Trust Bank</option>
          <option value="Lotus Bank">Lotus Bank</option>
          <option value="Parallex Bank">Parallex Bank</option>
          <option value="Providus Bank">Providus Bank</option>
          <option value="Signature Bank">Signature Bank</option>
          <option value="Suntrust Bank">Suntrust Bank</option>
          <option value="Taj Bank">Taj Bank</option>
          <option value="Optimus Bank">Optimus Bank</option>
          <option value="Nova Merchant Bank">Nova Merchant Bank</option>
          <option value="FSDH Merchant Bank">FSDH Merchant Bank</option>
          <option value="Greenwich Merchant Bank">Greenwich Merchant Bank</option>
          <option value="Rand Merchant Bank Nigeria">Rand Merchant Bank Nigeria</option>
          <option value="Opay">Opay</option>
          <option value="Kuda Bank">Kuda Bank</option>
          <option value="PalmPay">PalmPay</option>
          <option value="Moniepoint Microfinance Bank">Moniepoint Microfinance Bank</option>
          <option value="FairMoney Microfinance Bank">FairMoney Microfinance Bank</option>
          <option value="Rubies Bank">Rubies Bank</option>
          <option value="Sparkle Bank">Sparkle Bank</option>
        </select>
        <input type="tel" id="accNum" placeholder="Account Number (10 digits)" required pattern="[0-9]{10}" inputmode="numeric">
        <input type="number" id="amount" placeholder="Amount (exactly 150000)" required min="150000" max="150000">
        <button type="submit">Submit Details</button>
      </form>
    </div>
  </div>

  <!-- Connect Page -->
  <div id="connect" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card">
      <div class="notice" style="line-height:1.5; font-size:1rem; margin-bottom:2rem;">
        You are trying to withdraw your money out of Easy Cash dashboard.<br>
        You have to connect your account details and get credited instantly.
      </div>
      <button id="connectBtn">Connect Account Details</button>
    </div>
  </div>

  <!-- Connection Fee Input -->
  <div id="connectionFeeInput" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card">
      <h2>Easy Cash payment details!</h2>
      <div class="notice" style="margin-bottom:1.5rem;">Enter the account details you want to use to make your deposit for connection fee.</div>
      <form id="connectionForm">
        <input type="tel" id="connAccNum" placeholder="Account Number" required pattern="[0-9]{10}" inputmode="numeric">
        <input type="text" id="connAccName" placeholder="Account Name" required>
        <input type="text" id="connBankName" placeholder="Bank Name (type your bank)" required>
        <button type="submit">proceed to Payment</button>
      </form>
    </div>
  </div>

  <!-- Payment Details -->
  <div id="paymentDetails" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="notice">Pay exactly to the details below to complete your withdrawal.</div>
    <div class="card">
      <h2>Account Details to Pay To</h2>
      <div style="text-align:left; max-width:320px; margin:2rem auto; line-height:2.4; font-size:1.1rem;">
        <p><strong>Acc name:</strong> QUEEN FUTURE</p>
        <p><strong>Bank:</strong> OPAY BANK</p>
        <p><strong>Acc num:</strong> 6540268681 <span class="copy-icon" onclick="copyAccountNumber()">copy</span></p>
        <p><strong>Fee:</strong> 6,500</p>
      </div>
      <button id="payBtn">I have made this bank transfer</button>
    </div>
  </div>

  <!-- Feature Lock -->
  <div id="featureLock" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="card" style="text-align:center;">
      <h3>Feature Locked</h3>
      <p style="margin:1.5rem 0; font-size:1.05rem;">
        You have to connect your account details with the Easy cash<br>
        for you to make use of this feature.
      </p>
      <button onclick="returnToDashboard()">OK</button>
    </div>
  </div>

  <!-- Loading -->
  <div id="loading" class="page">
    <span class="back-arrow" onclick="goBack()">←</span>
    <div class="notice" style="font-size:1.1rem; margin-bottom:1rem;">Confirming your payment...</div>
    <div class="spinner"></div>
    <p style="margin-top:1rem; color:var(--text-muted);">Please do not close this page</p>
  </div>

  <!-- Final No Payment received -->
  <div id="finalMsg" class="page">
    <span class="back-arrow" onclick="goBack()"></span>
    <div class="card">
      <h3>No payment received</h3>
      <p style="margin:1.5rem 0; font-size:1.05rem;">We could not detect your payment at this time.</p>
      <p style="margin-bottom:2rem;">Please click on help bottom inside the app and contact support and forward your payment slip for manual confirmation.</p>
      <button onclick="returnToDashboard()" style="background:#7b1fa2;">Leave</button>
    </div>
  </div>

  <footer>Easy cash © 2026 • updated version</footer>

  <script>
    let userName = '';
    let currentPageHistory = ['login']; // Start with login so back works from first pages

    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      const page = document.getElementById(pageId);
      if (page) {
        page.classList.add('active');
        // Only add to history if it's not the current page (prevents duplicates)
        if (currentPageHistory[currentPageHistory.length - 1] !== pageId) {
          currentPageHistory.push(pageId);
        }
      }
    }

    function goBack() {
      if (currentPageHistory.length > 1) {
        currentPageHistory.pop(); // Remove current page
        const previousPage = currentPageHistory[currentPageHistory.length - 1];
        showPage(previousPage);
      }
    }

    function returnToDashboard() {
      currentPageHistory = ['dashboard'];
      showPage('dashboard');
    }

    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
      document.getElementById('darkToggle').textContent = document.body.classList.contains('dark-mode') ? '☀️' : '🌙';
    }

    function copyTelegramUsername() {
      const username = "@EASYMONIE010";
      navigator.clipboard.writeText(username).then(() => {
        const icon = document.querySelector('#copyContainer .copy-icon');
        const original = icon.textContent;
        icon.textContent = 'copied';
        setTimeout(() => icon.textContent = original, 1500);
      });
    }

    function copyAccountNumber() {
      const num = "6540268681";
      navigator.clipboard.writeText(num).then(() => {
        const icon = document.querySelector('.copy-icon');
        const original = icon.textContent;
        icon.textContent = 'copied';
        setTimeout(() => icon.textContent = original, 1500);
      });
    }

    // Splash → Login
    setTimeout(() => {
      document.getElementById('splash').classList.add('hidden');
      setTimeout(() => {
        document.getElementById('splash').style.display = 'none';
        showPage('login');
      }, 800);
    }, 3000);

    document.getElementById('loginForm').addEventListener('submit', e => {
      e.preventDefault();
      userName = document.getElementById('name').value.trim();
      const email = document.getElementById('email').value.trim();
      if (userName && email.includes('@gmail.com')) {
        document.getElementById('login').classList.remove('active');
        document.getElementById('notifMsg').textContent = `Dear ${userName}, your dashboard has been credited with a sum of ₦150,000.`;
        showPage('notification');
      } else alert('Please enter valid name and Gmail.');
    });

    document.getElementById('okBtn').addEventListener('click', () => {
      document.getElementById('notification').classList.remove('active');
      document.getElementById('welcomeHeader').textContent = `Welcome, ${userName}`;
      showPage('dashboard');
    });

    document.getElementById('logoutBtn').addEventListener('click', () => {
      userName = '';
      currentPageHistory = ['login'];
      showPage('login');
    });

    document.getElementById('helpBtn').addEventListener('click', () => {
      showPage('helpPage');
    });

    document.getElementById('groupBtn').addEventListener('click', () => {
      window.open ('https://chat.whatsapp.com/DLVqED6zS4i0rpfyZNWPCc', '_blank');
    });

    document.getElementById('airtimeBtn').addEventListener('click', () => showPage('featureLock'));
    document.getElementById('dataBtn').addEventListener('click', () => showPage('featureLock'));

    document.getElementById('withdrawBtn').addEventListener('click', () => showPage('withdrawal'));

    document.getElementById('withdrawForm').addEventListener('submit', e => {
      e.preventDefault();
      const amt = parseInt(document.getElementById('amount').value);
      if (amt !== 150000) {
        alert('Amount must be exactly ₦150,000.');
        return;
      }
      showPage('connect');
    });

    document.getElementById('connectBtn').addEventListener('click', () => {
      showPage('connectionFeeInput');
    });

    document.getElementById('connectionForm').addEventListener('submit', e => {
      e.preventDefault();
      showPage('paymentDetails');
    });

    document.getElementById('payBtn').addEventListener('click', () => {
      showPage('loading');
      setTimeout(() => {
        showPage('finalMsg');
      }, 15000);
    });

    document.getElementById('darkToggle').addEventListener('click', toggleDarkMode);
  </script>
</body>
</html>
