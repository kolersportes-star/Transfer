
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Secure Transfer</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, sans-serif; }
  body {
    min-height: 100vh;
    background: linear-gradient(135deg, #0a2540 0%, #1a3a5c 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    padding: 20px;
  }
  .card {
    background: #fff;
    color: #222;
    border-radius: 12px;
    padding: 40px 50px;
    max-width: 520px;
    width: 100%;
    box-shadow: 0 20px 60px rgba(0,0,0,0.4);
    text-align: center;
  }
  .logo {
    font-size: 14px;
    letter-spacing: 3px;
    color: #0a2540;
    font-weight: 700;
    margin-bottom: 8px;
  }
  .subtitle {
    font-size: 12px;
    color: #888;
    margin-bottom: 30px;
    letter-spacing: 1px;
  }
  .transfer-info {
    background: #f5f7fa;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 30px;
    text-align: left;
  }
  .row {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid #e4e8ec;
    font-size: 14px;
  }
  .row:last-child { border-bottom: none; }
  .row .label { color: #666; }
  .row .value { font-weight: 600; color: #0a2540; }
  .amount { font-size: 28px; color: #0a2540; font-weight: 700; margin: 20px 0; }
  .spinner {
    width: 70px;
    height: 70px;
    border: 6px solid #e4e8ec;
    border-top: 6px solid #0a7c3a;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 20px auto;
  }
  @keyframes spin { to { transform: rotate(360deg); } }
  .status {
    font-size: 15px;
    color: #555;
    margin-top: 15px;
    min-height: 22px;
  }
  .progress-bar {
    width: 100%;
    height: 6px;
    background: #e4e8ec;
    border-radius: 3px;
    overflow: hidden;
    margin-top: 20px;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #0a7c3a, #14a04c);
    width: 0%;
    transition: width 0.3s ease;
  }
  .success {
    display: none;
    animation: fadeIn 0.5s ease;
  }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
  .checkmark {
    width: 90px;
    height: 90px;
    border-radius: 50%;
    background: #0a7c3a;
    margin: 20px auto;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: pop 0.4s ease;
  }
  @keyframes pop { 0% { transform: scale(0); } 70% { transform: scale(1.1); } 100% { transform: scale(1); } }
  .checkmark svg { width: 50px; height: 50px; stroke: white; stroke-width: 5; fill: none; }
  .success h2 { color: #0a7c3a; margin: 15px 0 10px; font-size: 24px; }
  .success p { color: #666; font-size: 14px; }
  .ref {
    margin-top: 20px;
    font-size: 12px;
    color: #888;
    font-family: 'Courier New', monospace;
  }
</style>
</head>
<body>
  <div class="card">
    <div id="loading">
      <div class="logo">SECURE TRANSFER</div>
      <div class="subtitle">SEPA INTERBANK PROTOCOL</div>

      <div class="transfer-info">
        <div class="row"><span class="label">From</span><span class="value">Reclaim Experts</span></div>
        <div class="row"><span class="label">To</span><span class="value">La Banque Postale</span></div>
        <div class="row"><span class="label">Currency</span><span class="value">EUR</span></div>
        <div class="row"><span class="label">Type</span><span class="value">SEPA Credit Transfer</span></div>
      </div>

      <div class="amount">€30,000.00</div>

      <div class="spinner"></div>
      <div class="status" id="status">Initializing secure connection...</div>
      <div class="progress-bar"><div class="progress-fill" id="progress"></div></div>
    </div>

    <div class="success" id="success">
      <div class="logo">SECURE TRANSFER</div>
      <div class="checkmark">
        <svg viewBox="0 0 52 52"><polyline points="14,27 22,35 38,18"/></svg>
      </div>
      <h2>Transfer Successful</h2>
      <p>€30,000.00 has been transferred from<br><strong>Reclaim Experts</strong> to <strong>La Banque Postale</strong></p>
      <div class="transfer-info" style="margin-top:25px;">
        <div class="row"><span class="label">Status</span><span class="value" style="color:#0a7c3a;">COMPLETED</span></div>
        <div class="row"><span class="label">Date</span><span class="value" id="date"></span></div>
        <div class="row"><span class="label">Reference</span><span class="value" id="ref"></span></div>
      </div>
    </div>
  </div>

<script>
  const steps = [
    "Initializing secure connection...",
    "Authenticating sender account...",
    "Verifying recipient IBAN...",
    "Contacting La Banque Postale...",
    "Processing SEPA transaction...",
    "Confirming funds transfer...",
    "Finalizing operation..."
  ];
  const statusEl = document.getElementById('status');
  const progressEl = document.getElementById('progress');
  let i = 0;
  const total = 10000;
  const interval = total / steps.length;

  const t = setInterval(() => {
    if (i < steps.length) {
      statusEl.textContent = steps[i];
      progressEl.style.width = ((i + 1) / steps.length * 100) + '%';
      i++;
    }
  }, interval);

  setTimeout(() => {
    clearInterval(t);
    document.getElementById('loading').style.display = 'none';
    document.getElementById('success').style.display = 'block';
    const now = new Date();
    document.getElementById('date').textContent = now.toLocaleString('en-GB');
    document.getElementById('ref').textContent = 'TRX' + Date.now().toString().slice(-10);
  }, 10000);
</script>
</body>
</html>
