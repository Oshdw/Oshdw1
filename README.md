<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية - 7 نجوم</title>
  <style>
    :root {
      --main-color: #3a86ff;
      --bg-light: #f4f6f8;
      --bg-dark: #121212;
      --text-light: #fff;
      --text-dark: #222;
      --card-bg-light: #fff;
      --card-bg-dark: #1e1e1e;
      --shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }
    * {
      box-sizing: border-box;
      font-family: 'Segoe UI', sans-serif;
    }
    body {
      margin: 0;
      background: var(--bg-light);
      color: var(--text-dark);
      transition: all 0.3s ease-in-out;
    }
    body.dark {
      background: var(--bg-dark);
      color: var(--text-light);
    }
    header {
      background: var(--main-color);
      color: white;
      padding: 1rem;
      text-align: center;
      position: relative;
    }
    .toggle-mode {
      position: absolute;
      top: 1rem;
      right: 1rem;
      background: transparent;
      border: 2px solid white;
      color: white;
      padding: 0.3rem 0.6rem;
      border-radius: 6px;
      cursor: pointer;
    }
    main {
      padding: 2rem;
    }
    .hidden {
      display: none;
    }
    .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1rem;
    }
    .card {
      background: var(--card-bg-light);
      padding: 1rem;
      border-radius: 12px;
      box-shadow: var(--shadow);
      text-align: center;
      transition: background 0.3s;
    }
    body.dark .card {
      background: var(--card-bg-dark);
    }
    button {
      background: var(--main-color);
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      margin: 0.5rem;
      border-radius: 6px;
      cursor: pointer;
    }
    input, select {
      padding: 0.5rem;
      margin: 0.5rem 0;
      border: 1px solid #ccc;
      border-radius: 6px;
      width: 100%;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background: var(--card-bg-light);
    }
    body.dark table {
      background: var(--card-bg-dark);
    }
    th, td {
      padding: 0.75rem;
      border: 1px solid #ddd;
      text-align: center;
    }
    th {
      background: #e9f0fb;
    }
    body.dark th {
      background: #2a2a2a;
    }
  </style>
</head>
<body>
  <header>
    <h1>نظام إدارة الصيدلية - 7 نجوم</h1>
    <button class="toggle-mode" onclick="toggleMode()">🌓</button>
  </header>
  <main>
    <div class="cards">
      <div class="card">
        <h3>الأدوية</h3>
        <p><button onclick="showSection('medicines')">عرض</button></p>
      </div>
      <div class="card">
        <h3>المبيعات</h3>
        <p><button onclick="showSection('sales')">عرض</button></p>
      </div>
      <div class="card">
        <h3>الطلبيات</h3>
        <p><button onclick="showSection('orders')">عرض</button></p>
      </div>
      <div class="card">
        <h3>آلة حاسبة</h3>
        <p><button onclick="showSection('calculator')">فتح</button></p>
      </div>
    </div><section id="medicines" class="hidden">
  <h2>إدارة الأدوية</h2>
  <input type="text" id="medSearch" placeholder="ابحث باسم الدواء..." oninput="renderMedicines()">
  <button onclick="addMedicine()">+ إضافة دواء</button>
  <table>
    <thead><tr><th>الاسم</th><th>الكمية</th><th>الانتهاء</th><th>إجراءات</th></tr></thead>
    <tbody id="medTable"></tbody>
  </table>
</section>

<section id="sales" class="hidden">
  <h2>المبيعات</h2>
  <p>سجل بيع دواء:</p>
  <input type="text" id="saleName" placeholder="اسم الدواء">
  <input type="number" id="saleQty" placeholder="الكمية">
  <button onclick="recordSale()">تسجيل</button>
  <ul id="salesLog"></ul>
</section>

<section id="orders" class="hidden">
  <h2>الطلبيات</h2>
  <input type="text" id="orderName" placeholder="اسم الدواء">
  <input type="number" id="orderQty" placeholder="الكمية المطلوبة">
  <button onclick="addOrder()">إضافة</button>
  <ul id="orderList"></ul>
</section>

<section id="calculator" class="hidden">
  <h2>آلة حاسبة</h2>
  <input type="text" id="calcDisplay" readonly>
  <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:0.5rem;">
    "123+456-*/0.=C".split('').forEach(c => document.write(`<button onclick="calcInput('${c}')">${c}</button>`));
  </div>
</section>

  </main>  <script>
    let medicines = JSON.parse(localStorage.getItem('meds')) || [];
    let orders = JSON.parse(localStorage.getItem('orders')) || [];
    let sales = JSON.parse(localStorage.getItem('sales')) || [];

    function showSection(id) {
      document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
      document.getElementById(id).classList.remove('hidden');
      if (id === 'medicines') renderMedicines();
      if (id === 'orders') renderOrders();
      if (id === 'sales') renderSales();
    }

    function addMedicine() {
      const name = prompt('اسم الدواء:');
      const qty = +prompt('الكمية:');
      const expiry = prompt('تاريخ الانتهاء (YYYY-MM-DD):');
      if (name && qty && expiry) {
        medicines.push({ name, qty, expiry });
        localStorage.setItem('meds', JSON.stringify(medicines));
        renderMedicines();
      }
    }

    function renderMedicines() {
      const table = document.getElementById('medTable');
      const search = document.getElementById('medSearch').value;
      table.innerHTML = '';
      medicines.filter(m => m.name.includes(search)).forEach((m, i) => {
        table.innerHTML += `<tr>
          <td>${m.name}</td><td>${m.qty}</td><td>${m.expiry}</td>
          <td><button onclick="deleteMed(${i})">حذف</button></td>
        </tr>`;
      });
    }

    function deleteMed(i) {
      if (confirm('تأكيد الحذف؟')) {
        medicines.splice(i, 1);
        localStorage.setItem('meds', JSON.stringify(medicines));
        renderMedicines();
      }
    }

    function addOrder() {
      const name = document.getElementById('orderName').value;
      const qty = document.getElementById('orderQty').value;
      if (name && qty) {
        orders.push({ name, qty });
        localStorage.setItem('orders', JSON.stringify(orders));
        renderOrders();
      }
    }

    function renderOrders() {
      const list = document.getElementById('orderList');
      list.innerHTML = '';
      orders.forEach((o, i) => {
        list.innerHTML += `<li>${o.name} - ${o.qty} <button onclick="deleteOrder(${i})">حذف</button></li>`;
      });
    }

    function deleteOrder(i) {
      orders.splice(i, 1);
      localStorage.setItem('orders', JSON.stringify(orders));
      renderOrders();
    }

    function recordSale() {
      const name = document.getElementById('saleName').value;
      const qty = +document.getElementById('saleQty').value;
      if (name && qty) {
        sales.push({ name, qty });
        localStorage.setItem('sales', JSON.stringify(sales));
        renderSales();
      }
    }

    function renderSales() {
      const list = document.getElementById('salesLog');
      list.innerHTML = '';
      sales.forEach(s => {
        list.innerHTML += `<li>${s.name} - ${s.qty}</li>`;
      });
    }

    let calc = '';
    function calcInput(c) {
      const d = document.getElementById('calcDisplay');
      if (c === 'C') calc = '';
      else if (c === '=') {
        try { calc = eval(calc).toString(); } catch { calc = 'خطأ'; }
      } else calc += c;
      d.value = calc;
    }

    function toggleMode() {
      document.body.classList.toggle('dark');
    }
  </script></body>
</html>
