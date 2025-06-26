<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <style>
    :root {
      --primary-color: #0077cc;
      --bg-color: #ffffff;
      --text-color: #000000;
      --success-color: #28a745;
    }[data-theme="dark"] {
  --bg-color: #1a1a1a;
  --text-color: #ffffff;
}

body {
  font-family: 'Cairo', sans-serif;
  background-color: var(--bg-color);
  color: var(--text-color);
  margin: 0;
  padding: 0;
  transition: all 0.3s ease;
}

header {
  background-color: var(--primary-color);
  color: white;
  padding: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 {
  margin: 0;
}

.container {
  padding: 20px;
}

.section {
  margin-bottom: 40px;
}

.section h2 {
  border-bottom: 2px solid var(--primary-color);
  padding-bottom: 10px;
}

input, button {
  padding: 10px;
  margin: 5px 0;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  background-color: var(--primary-color);
  color: white;
  border: none;
  cursor: pointer;
}

.success-message {
  color: var(--success-color);
  font-weight: bold;
  margin-top: 10px;
}

.sidebar {
  position: fixed;
  right: 0;
  top: 0;
  height: 100%;
  background-color: var(--primary-color);
  color: white;
  width: 250px;
  transform: translateX(100%);
  transition: transform 0.3s ease;
  padding: 20px;
}

.sidebar.active {
  transform: translateX(0);
}

.sidebar button {
  background: none;
  border: none;
  color: white;
  text-align: right;
  display: block;
  width: 100%;
  margin: 10px 0;
  font-size: 16px;
}

.toggle-sidebar {
  background-color: #333;
  color: white;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 4px;
}

.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  max-width: 300px;
}

.calculator input[type="text"] {
  grid-column: span 4;
  padding: 10px;
  font-size: 18px;
  text-align: left;
}

.calculator button {
  padding: 15px;
  font-size: 18px;
}

  </style>
</head>
<body>  <header>
    <h1>نظام إدارة الصيدلية</h1>
    <div>
      <button class="toggle-sidebar" onclick="toggleSidebar()">القائمة</button>
      <button onclick="toggleTheme()">تغيير الوضع</button>
    </div>
  </header>  <aside class="sidebar" id="sidebar">
    <button onclick="navigateTo('dashboard')">لوحة التحكم</button>
    <button onclick="navigateTo('meds')">إدارة الأدوية</button>
    <button onclick="navigateTo('orders')">إدارة الطلبات</button>
    <button onclick="navigateTo('calculator')">الآلة الحاسبة</button>
  </aside>  <div class="container">
    <section class="section" id="dashboard">
      <h2>لوحة التحكم</h2>
      <p>عدد الأدوية: <strong>0</strong></p>
      <p>أدوية منتهية الصلاحية: <strong>0</strong></p>
      <p>طلبات معلقة: <strong>0</strong></p>
      <p>إجمالي المبيعات: <strong>0</strong></p>
    </section><section class="section" id="meds">
  <h2>إدارة الأدوية</h2>
  <label>اسم الدواء:</label>
  <input type="text" id="drug-name">
  <label>الكمية:</label>
  <input type="number" id="quantity">
  <label>تاريخ الانتهاء:</label>
  <input type="date" id="expiry-date">
  <br>
  <button onclick="addDrug()">إضافة/تحديث الدواء</button>
  <div id="message" class="success-message"></div>
</section>

<section class="section" id="orders">
  <h2>إدارة الطلبات</h2>
  <p>الطلبات ستُعرض هنا مستقبلاً.</p>
</section>

<section class="section" id="calculator">
  <h2>الآلة الحاسبة</h2>
  <div class="calculator">
    <input type="text" id="calc-display" readonly>
    <button onclick="appendToCalc('1')">1</button>
    <button onclick="appendToCalc('2')">2</button>
    <button onclick="appendToCalc('3')">3</button>
    <button onclick="appendToCalc('+')">+</button>
    <button onclick="appendToCalc('4')">4</button>
    <button onclick="appendToCalc('5')">5</button>
    <button onclick="appendToCalc('6')">6</button>
    <button onclick="appendToCalc('-')">-</button>
    <button onclick="appendToCalc('7')">7</button>
    <button onclick="appendToCalc('8')">8</button>
    <button onclick="appendToCalc('9')">9</button>
    <button onclick="appendToCalc('*')">×</button>
    <button onclick="appendToCalc('0')">0</button>
    <button onclick="appendToCalc('.')">.</button>
    <button onclick="calculateResult()">=</button>
    <button onclick="appendToCalc('/')">÷</button>
    <button onclick="clearCalc()">C</button>
  </div>
</section>

  </div>  <script>
    function toggleSidebar() {
      const sidebar = document.getElementById("sidebar");
      sidebar.classList.toggle("active");
    }

    function toggleTheme() {
      const html = document.documentElement;
      if (html.getAttribute("data-theme") === "dark") {
        html.removeAttribute("data-theme");
      } else {
        html.setAttribute("data-theme", "dark");
      }
    }

    function navigateTo(sectionId) {
      document.querySelectorAll(".section").forEach(section => {
        section.style.display = "none";
      });
      document.getElementById(sectionId).style.display = "block";
    }

    function addDrug() {
      const name = document.getElementById("drug-name").value;
      const quantity = document.getElementById("quantity").value;
      const expiry = document.getElementById("expiry-date").value;
      const msg = document.getElementById("message");

      if (name && quantity && expiry) {
        msg.style.color = "var(--success-color)";
        msg.textContent = "تمت إضافة الدواء بنجاح!";
      } else {
        msg.style.color = "red";
        msg.textContent = "يرجى ملء جميع الحقول.";
      }
    }

    let calcExpression = "";

    function appendToCalc(value) {
      calcExpression
