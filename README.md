<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <style>
    :root {
      --bg-light: #f9f9f9;
      --bg-dark: #121212;
      --text-light: #000;
      --text-dark: #fff;
      --accent-color: #4caf50;
      --success-light: #e0ffe0;
      --success-dark: #265e26;
    }body {
  font-family: 'Segoe UI', sans-serif;
  margin: 0;
  padding: 0;
  background-color: var(--bg-light);
  color: var(--text-light);
  transition: all 0.3s;
}

body.dark {
  background-color: var(--bg-dark);
  color: var(--text-dark);
}

.menu {
  background-color: var(--accent-color);
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.section {
  padding: 20px;
}

.box {
  border: 1px solid #ccc;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 10px;
}

.success-message {
  background-color: var(--success-light);
  color: #006400;
  padding: 10px;
  margin-top: 10px;
  border-radius: 5px;
}

body.dark .success-message {
  background-color: var(--success-dark);
  color: #a5ffa5;
}

.toggle-mode {
  cursor: pointer;
  background: none;
  border: none;
  color: white;
  font-size: 18px;
}

input, button {
  padding: 5px;
  margin: 5px 0;
}

.calculator {
  margin-top: 20px;
}

.calculator input {
  width: 100%;
  padding: 8px;
  font-size: 18px;
  margin-bottom: 10px;
}

.calculator button {
  padding: 10px;
  margin: 2px;
  font-size: 16px;
}

  </style>
</head>
<body>
  <div class="menu">
    <span>menu نظام إدارة الصيدلية</span>
    <button class="toggle-mode" onclick="toggleDarkMode()">dark_mode</button>
  </div>  <div class="section">
    <h2>لوحة التحكم</h2>
    <div class="box"><h3>عدد الأدوية</h3><p>0</p></div>
    <div class="box"><h3>أدوية منتهية الصلاحية</h3><p>0</p></div>
    <div class="box"><h3>طلبات معلقة</h3><p>0</p></div>
    <div class="box"><h3>إجمالي المبيعات</h3><p>0</p></div>
  </div>  <div class="section">
    <h2>إدارة الأدوية</h2>
    <label>اسم الدواء:</label><br>
    <input type="text" id="medName"><br>
    <label>الكمية:</label><br>
    <input type="number" id="medQty"><br>
    <label>تاريخ الانتهاء:</label><br>
    <input type="date" id="medExp"><br>
    <button onclick="addMedicine()">إضافة/تحديث الدواء</button>
    <div id="successMsg" class="success-message" style="display: none;">تمت إضافة الدواء بنجاح!</div>
  </div>  <div class="section">
    <h2>إدارة الطلبات</h2>
    <!-- يمكن إضافة نموذج الطلب هنا -->
  </div>  <div class="section calculator">
    <h2>الآلة الحاسبة</h2>
    <input type="text" id="calcDisplay" readonly>
    <div>
      <button onclick="press('1')">1</button>
      <button onclick="press('2')">2</button>
      <button onclick="press('3')">3</button>
      <button onclick="press('+')">+</button><br>
      <button onclick="press('4')">4</button>
      <button onclick="press('5')">5</button>
      <button onclick="press('6')">6</button>
      <button onclick="press('-')">-</button><br>
      <button onclick="press('7')">7</button>
      <button onclick="press('8')">8</button>
      <button onclick="press('9')">9</button>
      <button onclick="press('*')">×</button><br>
      <button onclick="press('0')">0</button>
      <button onclick="press('.')">.</button>
      <button onclick="calculate()">=</button>
      <button onclick="press('/')">÷</button><br>
      <button onclick="clearCalc()">C</button>
    </div>
  </div>  <script>
    function toggleDarkMode() {
      document.body.classList.toggle('dark');
    }

    function addMedicine() {
      const name = document.getElementById('medName').value;
      const qty = document.getElementById('medQty').value;
      const exp = document.getElementById('medExp').value;
      if (name && qty && exp) {
        const msg = document.getElementById('successMsg');
        msg.style.display = 'block';
        setTimeout(() => msg.style.display = 'none', 3000);
      }
    }

    let expression = '';
    function press(val) {
      expression += val;
      document.getElementById('calcDisplay').value = expression;
    }

    function calculate() {
      try {
        const result = eval(expression);
        document.getElementById('calcDisplay').value = result;
        expression = '';
      } catch {
        document.getElementById('calcDisplay').value = 'خطأ';
        expression = '';
      }
    }

    function clearCalc() {
      expression = '';
      document.getElementById('calcDisplay').value = '';
    }
  </script></body>
</html>
