<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>USA Bank Prank Generator</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f9f9f9;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 800px;
      background: white;
      margin: auto;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1, h2 {
      text-align: center;
      color: #2c3e50;
    }
    select, input, button {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      font-size: 1em;
    }
    button {
      background-color: #1c3faa;
      color: white;
      border: none;
      cursor: pointer;
      border-radius: 6px;
    }
    .receipt {
      background: #fff;
      padding: 30px;
      border: 1px solid #ccc;
      margin-top: 30px;
      font-size: 1rem;
      font-family: monospace;
      line-height: 1.6;
      box-shadow: 0 0 6px rgba(0,0,0,0.05);
    }
    .receipt-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px dashed #ccc;
      padding-bottom: 10px;
      margin-bottom: 10px;
    }
    .receipt-logo {
      height: 40px;
    }
    .receipt h3 {
      text-align: center;
      margin-bottom: 20px;
    }
    .disclaimer {
      text-align: center;
      font-size: 0.8em;
      color: red;
      margin-top: 20px;
    }
    .hidden {
      display: none;
    }
  </style>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
</head>
<body>
  <div class="container">
    <h1>🏦 USA BANK PRANK RECEIPT GENERATOR</h1>

    <!-- Bank Selection -->
    <div id="bankSelection">
      <h2>Select a Bank</h2>
      <select id="bankList">
        <option value="">-- Choose a Bank --</option>
        <option value="Chase">Chase Bank</option>
        <option value="Bank of America">Bank of America</option>
        <option value="Wells Fargo">Wells Fargo</option>
        <option value="Citi">Citi Bank</option>
        <option value="Capital One">Capital One</option>
      </select>
      <button onclick="nextStep('accountList')">Next</button>
    </div>

    <!-- Account Selection -->
    <div id="accountList" class="hidden">
      <h2>Select a Bank Account</h2>
      <select id="accountSelect">
        <option value="1234567890">****1234 - Checking</option>
        <option value="0987654321">****5678 - Savings</option>
        <option value="1122334455">****2233 - Business</option>
      </select>
      <button onclick="nextStep('receiptForm')">Next</button>
    </div>

    <!-- Receipt Form -->
    <div id="receiptForm" class="hidden">
      <h2>Enter Transaction Details</h2>
      <input type="text" id="sender" placeholder="Sender Name" required />
      <input type="text" id="receiver" placeholder="Receiver Name" required />
      <input type="number" id="amount" placeholder="Amount (USD)" required />
      <input type="date" id="date" required />
      <input type="time" id="time" required />
      <button onclick="generateReceipt()">Generate Receipt</button>
    </div>

    <!-- Generated Receipt -->
    <div id="receiptOutput" class="receipt hidden"></div>
    <button id="downloadBtn" class="hidden" onclick="downloadPDF()">Download Receipt as PDF</button>
    <div class="disclaimer">This is a fake receipt for entertainment purposes only. Not affiliated with any real bank.</div>
  </div>

  <script>
    const bankLogos = {
      'Chase': 'https://upload.wikimedia.org/wikipedia/commons/5/59/Chase_Bank_logo_2007.svg',
      'Bank of America': 'https://upload.wikimedia.org/wikipedia/commons/e/e3/Bank_of_America_logo.svg',
      'Wells Fargo': 'https://upload.wikimedia.org/wikipedia/commons/1/16/Wells_Fargo_Logo_%282019%29.svg',
      'Citi': 'https://upload.wikimedia.org/wikipedia/commons/3/3c/Citi.svg',
      'Capital One': 'https://upload.wikimedia.org/wikipedia/commons/b/bf/Capital_One_logo.svg'
    };

    function nextStep(id) {
      document.getElementById('bankSelection').classList.add('hidden');
      document.getElementById('accountList').classList.add('hidden');
      document.getElementById('receiptForm').classList.add('hidden');

      document.getElementById(id).classList.remove('hidden');
    }

    function generateReceipt() {
      const sender = document.getElementById('sender').value;
      const receiver = document.getElementById('receiver').value;
      const amount = document.getElementById('amount').value;
      const date = document.getElementById('date').value;
      const time = document.getElementById('time').value;
      const bank = document.getElementById('bankList').value;
      const txnId = 'TXN-' + Math.random().toString(36).substring(2, 10).toUpperCase();

      const logoURL = bankLogos[bank] || '';

      const receipt = `
        <div class="receipt-header">
          <img src="${logoURL}" alt="${bank} Logo" class="receipt-logo" />
          <div>
            <strong>${bank}</strong><br />
            Transaction Receipt
          </div>
        </div>
        <p><strong>Sender:</strong> ${sender}</p>
        <p><strong>Receiver:</strong> ${receiver}</p>
        <p><strong>Amount:</strong> $${parseFloat(amount).toFixed(2)}</p>
        <p><strong>Date:</strong> ${date}</p>
        <p><strong>Time:</strong> ${time}</p>
        <p><strong>Transaction ID:</strong> ${txnId}</p>
        <p style="margin-top: 20px; font-size: 0.9em; color: #666;">This is not a real transaction. Generated for fun.</p>
      `;

      const output = document.getElementById('receiptOutput');
      output.innerHTML = receipt;
      output.classList.remove('hidden');

      document.getElementById('downloadBtn').classList.remove('hidden');

      nextStep('receiptOutput');
    }

    function downloadPDF() {
      const element = document.getElementById('receiptOutput');
      const opt = {
        margin: 0.5,
        filename: 'prank_receipt.pdf',
        image: { type: 'jpeg', quality: 0.98 },
        html2canvas: { scale: 2 },
        jsPDF: { unit: 'in', format: 'letter', orientation: 'portrait' }
      };
      html2pdf().set(opt).from(element).save();
    }
  </script>
</body>
</html>
