<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', sans-serif;
      background: var(--bg-color);
      color: var(--text-color);
      transition: background 0.3s, color 0.3s;
    }
    :root {
      --bg-color: #f4f6f8;
      --text-color: #222;
      --card-bg: #fff;
      --accent-color: #3a86ff;
    }
    body.dark {
      --bg-color: #121212;
      --text-color: #f0f0f0;
      --card-bg: #1f1f1f;
    }
    header {
      background: var(--accent-color);
      color: white;
      padding: 1rem;
      text-align: center;
    }
    .container {
      display: flex;
    }
    .sidebar {
      width: 250px;
      background: var(--card-bg);
      min-height: 100vh;
      padding: 1rem;
      transition: transform 0.3s;
    }
    .sidebar.hide { transform: translateX(-100%); }
    .sidebar button {
      display: block;
      width: 100%;
      padding: 0.75rem;
      margin: 0.5rem 0;
      background: var(--accent-color);
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    .main {
      flex-grow: 1;
      padding: 2rem;
    }
    .card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 1rem;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      margin-bottom: 1rem;
    }
    .card h3 { color: var(--accent-color); }
    .hidden { display: none; }
    .theme-toggle {
      position: fixed; bottom: 20px; left: 20px;
      background: var(--accent-color);
      border: none;
      color: white;
      padding: 0.5rem 1rem;
      border-radius: 20px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <header>
    <h1>نظام إدارة الصيدلية</h1>
  </header>
  <div class="container">
    <nav class="sidebar" id="sidebar">
      <button onclick="showPage('dashboard')"><i class="fas fa-chart-line"></i> لوحة التحكم</button>
      <button onclick="showPage('medicines')"><i class="fas fa-pills"></i> إدارة الأدوية</button>
      <button onclick="showPage('orders')"><i class="fas fa-box"></i> إدارة الطلبات</button>
      <button onclick="showPage('sales')"><i class="fas fa-file-invoice-dollar"></i> تقارير المبيعات</button>
      <button onclick="showPage('expenses')"><i class="fas fa-receipt"></i> سجل المنصرفات</button>
      <button onclick="showPage('calculator')"><i class="fas fa-calculator"></i> الآلة الحاسبة</button>
      <button onclick="toggleSidebar()"><i class="fas fa-bars"></i> إخفاء القائمة</button>
    </nav>
    <main class="main">
      <section id="dashboard" class="page">
        <div class="card"><h3>عدد الأدوية:</h3><p id="medCount">0</p></div>
        <div class="card"><h3>عدد الطلبات:</h3><p id="orderCount">0</p></div>
        <div class="card"><h3>الإجمالي:</h3><p id="totalSum">0</p></div>
      </section>
      <section id="medicines" class="page hidden">
        <div class="card">
          <h3>إدارة الأدوية</h3>
          <button onclick="addMedicine()">إضافة دواء</button>
          <ul id="medicineList"></ul>
        </div>
      </section>
      <section id="orders" class="page hidden">
        <div class="card">
          <h3>إدارة الطلبات</h3>
          <button onclick="addOrder()">إضافة طلب</button>
          <ul id="orderList"></ul>
        </div>
      </section>
      <section id="sales" class="page hidden">
        <div class="card">
          <h3>تقارير المبيعات</h3>
          <p>قيد التطوير...</p>
        </div>
      </section>
      <section id="expenses" class="page hidden">
        <div class="card">
          <h3>سجل المنصرفات</h3>
          <button onclick="addExpense()">إضافة صرف</button>
          <ul id="expenseList"></ul>
        </div>
      </section>
      <section id="calculator" class="page hidden">
        <div class="card">
          <h3>الآلة الحاسبة</h3>
          <input type="text" id="calcDisplay" disabled style="width:100%; padding:1rem; font-size:1.2rem; text-align:right;">
          <div style="display:grid; grid-template-columns: repeat(4, 1fr); gap: 0.5rem; margin-top:1rem;">
            <button onclick="calcInput('7')">7</button>
            <button onclick="calcInput('8')">8</button>
            <button onclick="calcInput('9')">9</button>
            <button onclick="calcInput('/')">÷</button>
            <button onclick="calcInput('4')">4</button>
            <button onclick="calcInput('5')">5</button>
            <button onclick="calcInput('6')">6</button>
            <button onclick="calcInput('*')">×</button>
            <button onclick="calcInput('1')">1</button>
            <button onclick="calcInput('2')">2</button>
            <button onclick="calcInput('3')">3</button>
            <button onclick="calcInput('-')">−</button>
            <button onclick="calcInput('0')">0</button>
            <button onclick="calcInput('.')">.</button>
            <button onclick="calcClear()">C</button>
            <button onclick="calcInput('+')">+</button>
            <button style="grid-column: span 4" onclick="calcEquals()">=</button>
          </div>
        </div>
      </section>
    </main>
  </div>
  <button class="theme-toggle" onclick="toggleTheme()">الوضع الليلي</button>
  <script>
    const pages = document.querySelectorAll('.page');
    function showPage(id) {
      pages.forEach(p => p.classList.add('hidden'));
      document.getElementById(id).classList.remove('hidden');
    }
    function toggleSidebar() {
      document.getElementById('sidebar').classList.toggle('hide');
    }
    function toggleTheme() {
      document.body.classList.toggle('dark');
    }const medicineList = document.getElementById('medicineList');
const orderList = document.getElementById('orderList');
const expenseList = document.getElementById('expenseList');

function addMedicine() {
  const name = prompt('اسم الدواء:');
  if (name) medicineList.innerHTML += `<li>${name}</li>`;
}
function addOrder() {
  const desc = prompt('وصف الطلب:');
  if (desc) orderList.innerHTML += `<li>${desc}</li>`;
}
function addExpense() {
  const item = prompt('بند الصرف:');
  if (item) expenseList.innerHTML += `<li>${item}</li>`;
}

let calcDisplay = document.getElementById('calcDisplay');
function calcInput(val) {
  calcDisplay.value += val;
}
function calcClear() {
  calcDisplay.value = '';
}
function calcEquals() {
  try {
    calcDisplay.value = eval(calcDisplay.value);
  } catch {
    calcDisplay.value = 'خطأ';
  }
}

  </script>
</body>
</html>
