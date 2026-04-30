# paint-mix-app
[paint.html](https://github.com/user-attachments/files/27230897/paint.html)
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>塗料配合計算</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #1f1f1f;
      color: white;
      margin: 0;
      padding: 20px;
    }

    .container {
      max-width: 500px;
      margin: auto;
      background: #2d2d2d;
      padding: 20px;
      border-radius: 16px;
    }

    h1 {
      text-align: center;
      margin-bottom: 20px;
    }

    .section {
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-bottom: 8px;
      font-size: 18px;
    }

    input, select {
      width: 100%;
      padding: 16px;
      font-size: 20px;
      border-radius: 12px;
      border: none;
      box-sizing: border-box;
    }

    .ratio-buttons {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
    }

    button {
      padding: 18px;
      font-size: 22px;
      border: none;
      border-radius: 12px;
      background: #ff9800;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      opacity: 0.9;
    }

    .result {
      background: #3a3a3a;
      padding: 20px;
      border-radius: 12px;
      font-size: 22px;
      line-height: 1.8;
    }

    .active {
      background: #4caf50;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>🎨 塗料配合計算</h1>

    <div class="section">
      <label>比率を選択</label>
      <div class="ratio-buttons">
        <button onclick="setRatio(4,1,this)">4:1</button>
        <button onclick="setRatio(5,1,this)">5:1</button>
        <button onclick="setRatio(3,1,this)">3:1</button>
      </div>
    </div>

    <div class="section">
      <label>計算方法</label>
      <select id="mode">
        <option value="total">合計量から計算</option>
        <option value="base">主剤量から計算</option>
      </select>
    </div>

    <div class="section">
      <label>量 (g)</label>
      <select id="amount">
        <option value="100">100g</option>
        <option value="200">200g</option>
        <option value="300">300g</option>
        <option value="400">400g</option>
        <option value="500" selected>500g</option>
        <option value="600">600g</option>
        <option value="700">700g</option>
        <option value="800">800g</option>
        <option value="900">900g</option>
        <option value="1000">1000g</option>
        <option value="2000">2kg</option>
        <option value="3000">3kg</option>
        <option value="5000">5kg</option>
        <option value="10000">10kg</option>
        <option value="15000">15kg</option>
        <option value="20000">20kg</option>
      </select>
    </div>

    <div class="section">
      <label>シンナー (%)</label>
      <select id="thinner">
        <option value="0" selected>0%</option>
        <option value="10">10%</option>
        <option value="20">20%</option>
        <option value="30">30%</option>
        <option value="40">40%</option>
      </select>
    </div>

    <button onclick="calculate()">計算する</button>

    <div class="section result" id="result">
      主剤: - g<br>
      硬化剤: - g<br>
      シンナー: - g
    </div>
  </div>

  <script>
    let mainRatio = 4;
    let hardenerRatio = 1;

    function setRatio(main, hardener, btn) {
      mainRatio = main;
      hardenerRatio = hardener;

      document.querySelectorAll('.ratio-buttons button').forEach(b => {
        b.classList.remove('active');
      });

      btn.classList.add('active');
    }

    function calculate() {
      const mode = document.getElementById('mode').value;
      const amount = parseFloat(document.getElementById('amount').value);
      const thinnerPercent = parseFloat(document.getElementById('thinner').value) || 0;

      if (!amount || amount <= 0) {
        alert('量を入力してください');
        return;
      }

      let base;
      let hardener;

      if (mode === 'total') {
        const totalParts = mainRatio + hardenerRatio;
        hardener = amount / totalParts;
        base = amount - hardener;
      } else {
        base = amount;
        hardener = base / mainRatio * hardenerRatio;
      }

      const thinner = (base + hardener) * (thinnerPercent / 100);

      document.getElementById('result').innerHTML = `
        主剤: ${base.toFixed(1)} g<br>
        硬化剤: ${hardener.toFixed(1)} g<br>
        シンナー: ${thinner.toFixed(1)} g
      `;
    }
  </script>
</body>
</html>
