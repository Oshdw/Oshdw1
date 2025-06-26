<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pharmacy Dashboard</title>
  <style>
    :root {
      --bg-light: #f5f5f5;
      --bg-dark: #1e1e2f;
      --text-light: #ffffff;
      --text-dark: #1e1e2f;
      --primary: #4caf50;
      --accent: #2196f3;
      --danger: #f44336;
      --card-bg-light: #ffffff;
      --card-bg-dark: #2b2b3c;
    }body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background-color: var(--bg-light);
  color: var(--text-dark);
  transition: all 0.3s ease;
}

body.dark-mode {
  background-color: var(--bg-dark);
  color: var(--text-light);
}

body.dark-mode .card {
  background-color: var(--card-bg-dark);
}

.navbar {
  background-color: var(--primary);
  color: #fff;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.navbar h2 {
  margin: 0;
}

.nav-links {
  display: flex;
  gap: 1rem;
}

.page {
  display: none;
  padding: 1rem;
}

.page.active {
  display: block;
}

.card {
  background-color: var(--card-bg-light);
  padding: 1rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 1rem;
}

button {
  padding: 0.5rem 1rem;
  background-color: var(--accent);
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

input {
  padding: 0.5rem;
  margin: 0.2rem 0;
  width: 100%;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

table th, table td {
  border: 1px solid #ccc;
  padding: 0.5rem;
  text-align: center;
}

.dark-mode table th, .dark-mode table td {
  border: 1px solid #444;
  background-color: #333;
  color: #fff;
}

.search-box {
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .nav-links {
    flex-direction: column;
  }

  .navbar {
    text-align: center;
  }
}

  </style>
</head>
<body>
  <div class="navbar">
    <h2>Pharmacy</h2>
    <div class="nav-links">
      <button onclick="showPage('dashboard')">لوحة التحكم</button>
      <button onclick="showPage('medicines')">إدارة الأدوية</button>
      <button onclick="showPage('orders')">إدارة الطلبات</button>
      <button onclick="showPage('calculator')">الآلة الحاسبة</button>
      <button onclick="showPage('statistics')">الإحصائيات</button>
      <button onclick="toggleDarkMode()">الوضع الليلي</button>
    </div>
  </div>  <div id="dashboard" class="page active">
    <div class="card">عدد الأدوية: <span id="med-count">0</span></div>
    <div class="card">أدوية منتهية الصلاحية: <span id="expired">0</span></div>
    <div class="card">طلبات معلقة: <span id="pending">0</span></div>
    <div class="card">إجمالي المبيعات: <span id="sales">0</span> ج</div>
  </div>  <div id="medicines" class="page">
    <div class="card">
      <h3>إضافة/تحديث الدواء</h3>
      <input placeholder="اسم الدواء" id="medName" />
      <input placeholder="الكمية" id="medQty" type="number" />
      <input placeholder="تاريخ الانتهاء" id="medExp" type="date" />
      <button onclick="addMedicine()">إضافة</button>
      <p id="add-msg"></p>
    </div><div class="card">
  <input class="search-box" id="searchBox" placeholder="ابحث عن دواء..." oninput="filterMedicines()" />
  <table id="medTable">
    <thead>
      <tr><th>الاسم</th><th>الكمية</th><th>الصلاحية</th></tr>
    </thead>
    <tbody></tbody>
  </table>
</div>

  </div>  <div id="orders" class="page">
    <div class="card">لا توجد طلبات حالياً</div>
  </div>  <div id="calculator" class="page">
    <div class="card">
      <h3>الآلة الحاسبة</h3>
      <div class="calculator">
        <input type="text" id="calc-display" readonly>
        <div>
          <button onclick="calc('1')">1</button>
          <button onclick="calc('2')">2</button>
          <button onclick="calc('3')">3</button>
          <button onclick="calc('+')">+</button>
          <button onclick="calc('4')">4</button>
          <button onclick="calc('5')">5</button>
          <button onclick="calc('6')">6</button>
          <button onclick="calc('-')">-</button>
          <button onclick="calc('7')">7</button>
          <button onclick="calc('8')">8</button>
          <button onclick="calc('9')">9</button>
          <button onclick="calc('*')">×</button>
          <button onclick="calc('0')">0</button>
          <button onclick="calc('.')">.</button>
          <button onclick="calculate()">=</button>
          <button onclick="clearCalc()">C</button>
        </div>
      </div>
    </div>
  </div>  <div id="statistics" class="page">
    <div class="card">
      <h3>رسم بياني للمبيعات</h3>
      <canvas id="salesChart" width="400" height="200"></canvas>
    </div>
  </div>  <script>
    const medicines = [];

    function showPage(id) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }

    function addMedicine() {
      const name = document.getElementById('medName').value;
      const qty = document.getElementById('medQty').value;
      const exp = document.getElementById('medExp').value;
      if (!name || !qty || !exp) return alert('يرجى ملء كل الحقول');
      medicines.push({ name, qty, exp });
      updateMedTable();
      document.getElementById('add-msg').innerText = 'تمت الإضافة بنجاح';
      document.getElementById('med-count').innerText = medicines.length;
    }

    function updateMedTable() {
      const tbody = document.querySelector('#medTable tbody');
      tbody.innerHTML = '';
      medicines.forEach(med => {
        const row = `<tr><td>${med.name}</td><td>${med.qty}</td><td>${med.exp}</td></tr>`;
        tbody.innerHTML += row;
      });
    }

    function filterMedicines() {
      const query = document.getElementById('searchBox').value.toLowerCase();
      const rows = document.querySelectorAll('#medTable tbody tr');
      rows.forEach(row => {
        const text = row.innerText.toLowerCase();
        row.style.display = text.includes(query) ? '' : 'none';
      });
    }

    function calc(value) {
      document.getElementById('calc-display').value += value;
    }

    function calculate() {
      const display = document.getElementById('calc-display');
      display.value = eval(display.value);
    }

    function clearCalc() {
      document.getElementById('calc-display').value = '';
    }
  </script>  <footer style="text-align:center; padding:1rem; color:gray">عمر بابكر (Omer Babiker - Omer Bk)</footer>
</body>
</html>
