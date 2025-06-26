<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pharmacy Management System</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --primary: #1976d2;
      --background-light: #ffffff;
      --background-dark: #121212;
      --text-light: #ffffff;
      --text-dark: #000000;
    }* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
}

body {
  display: flex;
  background-color: var(--background-light);
  color: var(--text-dark);
  transition: background-color 0.3s, color 0.3s;
}

.sidebar {
  width: 250px;
  background-color: var(--primary);
  color: white;
  height: 100vh;
  padding: 1rem;
  position: fixed;
  left: 0;
  top: 0;
  overflow-y: auto;
  transition: transform 0.3s ease-in-out;
}

.sidebar.hidden {
  transform: translateX(-100%);
}

.sidebar h2 {
  text-align: center;
  margin-bottom: 2rem;
}

.sidebar button {
  background: none;
  border: none;
  color: white;
  font-size: 1.1rem;
  width: 100%;
  text-align: left;
  margin: 0.5rem 0;
  cursor: pointer;
}

.main {
  margin-left: 250px;
  padding: 2rem;
  flex-grow: 1;
  transition: margin-left 0.3s;
}

.main.full {
  margin-left: 0;
}

.topbar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.toggle-sidebar {
  font-size: 2rem;
  cursor: pointer;
}

.toggle-theme {
  cursor: pointer;
}

.card {
  background-color: #f0f0f0;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

canvas {
  background-color: white;
  padding: 1rem;
  border-radius: 8px;
}

.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  margin-top: 2rem;
}

.calculator input {
  grid-column: span 4;
  height: 40px;
  font-size: 1.2rem;
  text-align: right;
}

.calculator button {
  padding: 1rem;
  font-size: 1.2rem;
  background-color: var(--primary);
  color: white;
  border: none;
  border-radius: 5px;
}

body.dark {
  background-color: var(--background-dark);
  color: var(--text-light);
}

body.dark .card,
body.dark canvas {
  background-color: #333;
  color: white;
}

  </style>
</head>
<body>
  <div class="sidebar" id="sidebar">
    <h2>Pharmacy</h2>
    <button onclick="navigate('dashboard')">لوحة التحكم</button>
    <button onclick="navigate('medicines')">إدارة الأدوية</button>
    <button onclick="navigate('orders')">إدارة الطلبات</button>
    <button onclick="navigate('calculator')">الآلة الحاسبة</button>
    <button onclick="navigate('statistics')">الإحصائيات</button>
  </div>
  <div class="main" id="main">
    <div class="topbar">
      <span class="material-icons toggle-sidebar" onclick="toggleSidebar()">menu</span>
      <span class="material-icons toggle-theme" onclick="toggleTheme()">dark_mode</span>
    </div><div id="dashboard" class="page">
  <div class="card">عدد الأدوية: <span id="med-count">0</span></div>
  <div class="card">أدوية منتهية الصلاحية: <span id="expired">0</span></div>
  <div class="card">طلبات معلقة: <span id="pending">0</span></div>
  <div class="card">إجمالي المبيعات: <span id="sales">0</span> ج</div>
</div>

<div id="medicines" class="page" style="display:none">
  <div class="card">
    <h3>إضافة/تحديث الدواء</h3>
    <input placeholder="اسم الدواء" id="medName" />
    <input placeholder="الكمية" id="medQty" />
    <input placeholder="تاريخ الانتهاء" id="medExp" />
    <button onclick="addMedicine()">إضافة</button>
    <p id="add-msg"></p>
  </div>
</div>

<div id="orders" class="page" style="display:none">
  <div class="card">لا توجد طلبات حالياً</div>
</div>

<div id="calculator" class="page" style="display:none">
  <div class="card">
    <h3>الآلة الحاسبة</h3>
    <div class="calculator">
      <input type="text" id="calc-display" readonly>
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

<div id="statistics" class="page" style="display:none">
  <div class="card">
    <h3>رسم بياني للمبيعات</h3>
    <canvas id="salesChart" width="400" height="200"></canvas>
  </div>
</div>

  </div>  <script>
    let darkMode = false;

    function toggleSidebar() {
      document.getElementById('sidebar').classList.toggle('hidden');
      document.getElementById('main').classList.toggle('full');
    }

    function toggleTheme() {
      darkMode = !darkMode;
      document.body.classList.toggle('dark', darkMode);
    }

    function navigate(pageId) {
      const pages = document.querySelectorAll('.page');
      pages.forEach(p => p.style.display = 'none');
      document.getElementById(pageId).style.display = 'block';
    }

    function addMedicine() {
      const name = document.getElementById('medName').value;
      const qty = document.getElementById('medQty').value;
      const exp = document.getElementById('medExp').value;
      if(name && qty && exp) {
        document.getElementById('add-msg').textContent = 'تمت الإضافة بنجاح';
        document.getElementById('add-msg').style.color = darkMode ? 'lightgreen' : 'green';
      } else {
        document.getElementById('add-msg').textContent = 'يرجى ملء جميع الحقول';
        document.getElementById('add-msg').style.color = 'red';
      }
    }

    let calcValue = '';
    function calc(val) {
      calcValue += val;
      document.getElementById('calc-display').value = calcValue;
    }

    function calculate() {
      try {
        calcValue = eval(calcValue).toString();
        document.getElementById('calc-display').value = calcValue;
      } catch (e) {
        document.getElementById('calc-display').value = 'خطأ';
      }
    }

    function clearCalc() {
      calcValue = '';
      document.getElementById('calc-display').value = '';
    }

    const ctx = document.getElementById('salesChart').getContext('2d');
    const salesChart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: ['يناير', 'فبراير', 'مارس', 'أبريل'],
        datasets: [{
          label: 'المبيعات (ج)',
          data: [5000, 7000, 4000, 9000],
          backgroundColor: 'rgba(25, 118, 210, 0.7)',
        }]
      },
      options: {
        responsive: true,
        plugins: {
          legend: { position: 'top' },
          title: { display: true, text: 'مبيعات الأشهر الأخيرة' }
        }
      }
    });
  </script></body>
</html>
