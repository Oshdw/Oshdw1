<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>نظام إدارة الصيدلية</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <style>
    :root {
      --main-bg: #f4f6f8;
      --card-bg: #ffffff;
      --primary: #3a86ff;
      --text-color: #222;
      --button-bg: #3a86ff;
    }[data-theme="dark"] {
  --main-bg: #121212;
  --card-bg: #1e1e1e;
  --primary: #90caf9;
  --text-color: #ffffff;
  --button-bg: #2196f3;
}

* {
  box-sizing: border-box;
}
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: var(--main-bg);
  color: var(--text-color);
}
header {
  background: var(--primary);
  color: white;
  padding: 1rem;
  text-align: center;
}
button {
  padding: 0.6rem 1.2rem;
  background: var(--button-bg);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}
input, select {
  padding: 0.6rem;
  margin: 0.5rem 0;
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 6px;
}
main {
  padding: 1.5rem;
}
.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}
.card {
  background: var(--card-bg);
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  text-align: center;
}
.card h3 {
  color: var(--primary);
}
table {
  width: 100%;
  border-collapse: collapse;
  background: var(--card-bg);
  margin-top: 1rem;
}
th, td {
  padding: 0.75rem;
  border: 1px solid #ddd;
  text-align: center;
}
th {
  background: #e9f0fb;
}
.hidden {
  display: none;
}
.top-controls {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

  </style>
</head>
<body data-theme="light">
  <header>
    <h1>نظام إدارة الصيدلية</h1>
    <div class="top-controls">
      <button onclick="toggleTheme()">تبديل الوضع</button>
      <button onclick="logout()">تسجيل الخروج</button>
    </div>
  </header>
  <main>
    <section id="dashboard">
      <div class="cards">
        <div class="card"><h3>عدد الأدوية</h3><p>10</p></div>
        <div class="card"><h3>المبيعات</h3><p>1500 جنيه</p></div>
        <div class="card"><h3>الطلبيات</h3><p>5</p></div>
        <div class="card"><h3>الآلة الحاسبة</h3><button onclick="showCalculator()">افتح</button></div>
      </div>
      <div style="margin-top: 2rem; text-align:center">
        <button onclick="showSection('medicines')">الأدوية</button>
        <button onclick="showSection('sales')">المبيعات</button>
        <button onclick="showSection('orders')">الطلبيات</button>
      </div>
    </section><section id="medicines" class="hidden">
  <h2>إدارة الأدوية</h2>
  <input type="text" placeholder="بحث عن دواء...">
  <table>
    <thead><tr><th>الاسم</th><th>الكمية</th><th>انتهاء الصلاحية</th></tr></thead>
    <tbody><tr><td>باراسيتامول</td><td>20</td><td>2025-12-31</td></tr></tbody>
  </table>
  <button onclick="showSection('dashboard')">رجوع</button>
</section>

<section id="sales" class="hidden">
  <h2>المبيعات</h2>
  <input type="text" placeholder="اسم الدواء">
  <input type="number" placeholder="الكمية المباعة">
  <input type="number" placeholder="السعر">
  <button>تسجيل بيع</button>
  <button onclick="showSection('dashboard')">رجوع</button>
</section>

<section id="orders" class="hidden">
  <h2>الطلبيات</h2>
  <input type="text" placeholder="اسم المورد">
  <input type="text" placeholder="الدواء المطلوب">
  <input type="number" placeholder="الكمية">
  <button>إضافة طلبية</button>
  <button onclick="showSection('dashboard')">رجوع</button>
</section>

<section id="calculator" class="hidden">
  <h2>الآلة الحاسبة</h2>
  <input id="calcDisplay" type="text" readonly>
  <div>
    <button onclick="calc('1')">1</button>
    <button onclick="calc('2')">2</button>
    <button onclick="calc('3')">3</button>
    <button onclick="calc('+')">+</button><br>
    <button onclick="calc('4')">4</button>
    <button onclick="calc('5')">5</button>
    <button onclick="calc('6')">6</button>
    <button onclick="calc('-')">-</button><br>
    <button onclick="calc('7')">7</button>
    <button onclick="calc('8')">8</button>
    <button onclick="calc('9')">9</button>
    <button onclick="calc('*')">*</button><br>
    <button onclick="calc('0')">0</button>
    <button onclick="calc('/')">/</button>
    <button onclick="calc('C')">C</button>
    <button onclick="calc('=')">=</button>
  </div>
  <button onclick="showSection('dashboard')">رجوع</button>
</section>

  </main>  <script>
    function toggleTheme() {
      const body = document.body;
      const current = body.getAttribute('data-theme');
      body.setAttribute('data-theme', current === 'light' ? 'dark' : 'light');
    }

    function logout() {
      alert("تم تسجيل الخروج");
    }

    function showSection(id) {
      document.querySelectorAll('main section').forEach(sec => sec.classList.add('hidden'));
      document.getElementById(id).classList.remove('hidden');
    }

    function showCalculator() {
      showSection('calculator');
    }

    let calcInput = "";
    function calc(val) {
      const display = document.getElementById('calcDisplay');
      if (val === 'C') {
        calcInput = "";
      } else if (val === '=') {
        try {
          calcInput = eval(calcInput).toString();
        } catch {
          calcInput = "خطأ";
        }
      } else {
        calcInput += val;
      }
      display.value = calcInput;
    }
  </script></body>
</html>
