import os
from zipfile import ZipFile

# Recreate the HTML content since previous state was reset
google_drive_html = """<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Currency Converter</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      background-image: url('https://images.unsplash.com/photo-1605902711622-cfb43c4437d3?auto=format&fit=crop&w=1350&q=80');
      background-size: cover;
      background-repeat: no-repeat;
      background-position: center;
      color: #fff;
    }
    .box {
      background: rgba(0, 0, 0, 0.8);
      padding: 30px;
      display: inline-block;
      border-radius: 15px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.5);
      margin-top: 50px;
    }
    input, select, button {
      margin: 10px;
      padding: 12px;
      width: 220px;
      font-size: 16px;
      border-radius: 8px;
      border: none;
    }
    input, select {
      background: #fff;
      color: #000;
    }
    button {
      background-color: #28a745;
      color: white;
      cursor: pointer;
    }
    button:hover {
      background-color: #218838;
    }
    h2 {
      font-size: 28px;
      margin-bottom: 20px;
      color: #ffd700;
    }
    #result {
      color: #00ffcc;
    }
    footer {
      margin-top: 30px;
      font-size: 14px;
      color: #fff;
    }
  </style>
</head>
<body>
  <h2>💸 Online Currency Converter 💱</h2>
  <div class="box">
    <input type="number" id="amount" placeholder="Enter amount" />
    <br />
    <select id="from"></select>
    <select id="to"></select>
    <br />
    <button onclick="convertCurrency()">Convert</button>
    <h3 id="result">Output: </h3>
  </div>

  <footer>
    Made by <strong>Muhammad Qasim Qureshi</strong> — Currency Exchange Themed Project 🌍💰
  </footer>

  <script>
    const currencies = ['USD','EUR','GBP','PKR','INR','JPY','AUD','CAD','CNY','AED'];

    // Populate dropdowns
    const from = document.getElementById("from");
    const to = document.getElementById("to");
    currencies.forEach(curr => {
      from.innerHTML += `<option value="${curr}">${curr}</option>`;
      to.innerHTML += `<option value="${curr}">${curr}</option>`;
    });
    from.value = "USD";
    to.value = "PKR";

    async function convertCurrency() {
      const amt = document.getElementById("amount").value;
      const fromCur = from.value;
      const toCur = to.value;

      const res = await fetch(`https://api.exchangerate-api.com/v4/latest/${fromCur}`);
      const data = await res.json();
      const rate = data.rates[toCur];
      const result = amt * rate;

      document.getElementById("result").innerText = `Output: ${result.toFixed(2)} ${toCur}`;
    }
  </script>
</body>
</html>
"""

# Create folder and write HTML to file
folder_path = "/mnt/data/Currency_Converter_For_GDrive"
os.makedirs(folder_path, exist_ok=True)
html_file_path = os.path.join(folder_path, "index.html")
with open(html_file_path, "w") as file:
    file.write(google_drive_html)

# Zip the folder for download
zip_path = "/mnt/data/Currency_Converter_For_GoogleDrive.zip"
with ZipFile(zip_path, "w") as zipf:
    zipf.write(html_file_path, arcname="index.html")

zip_path
