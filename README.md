<!DOCTYPE html><html lang="ar" dir="rtl"><head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <style>
    :root {
      --primary-color: #4caf50;
      --secondary-color: #f44336;
      --bg-light: #f4f4f4;
      --bg-dark: #1e1e1e;
      --text-light: #fff;
      --text-dark: #333;
    }* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
}

body {
  background-color: var(--bg-light);
  color: var(--text-dark);
  transition: 0.3s;
}

body.dark {
  background-color: var(--bg-dark);
  color: var(--text-light);
}

header {
  background: var(--primary-color);
  color: white;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.sidebar {
  position: fixed;
  right: 0;
  top: 0;
  height: 100%;
  width: 250px;
  background: var(--primary-color);
  color: white;
  padding: 1rem;
  transform: translateX(100%);
  transition: 0.3s;
}

.sidebar.active {
  transform: translateX(0);
}

.sidebar h3 {
  margin-bottom: 1rem;
}

.toggle-sidebar {
  cursor: pointer;
  font-size: 1.2rem;
  background: transparent;
  border: none;
  color: white;
}

.container {
  padding: 2rem;
}

.section {
  margin-bottom: 2rem;
  padding: 1rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

body.dark .section {
  background: #333;
}

.section h2 {
  margin-bottom: 1rem;
}

.card {
  background: var(--primary-color);
  color: white;
  padding: 1rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.calculator {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}

.calculator input {
  grid-column: span 4;
  height: 40px;
  font-size: 1.2rem;
  text-align: right;
}

.calculator button {
  padding: 1rem;
  font-size: 1rem;
  cursor: pointer;
}

.notification {
  display: none;
  padding: 1rem;
  margin-top: 1rem;
  background: #4caf50;
  color: white;
  border-radius: 8px;
}

footer {
  margin-top: 4rem;
  text-align: center;
  font-weight: bold;
}

  </style>
</head><body>
  <header>
    <h1>نظام إدارة الصيدلية</h1>
    <div>
      <button class="toggle-sidebar">☰ القائمة</button>
      <button onclick="toggleMode()">🌓 الوضع</button>
    </div>
  </header>  <aside class="sidebar" id="sidebar">
    <h3>القائمة</h3>
    <ul>
      <li><a href="#dashboard">لوحة التحكم</a></li>
      <li><a href="#medicines">إدارة الأدوية</a></li>
      <li><a href="#orders">إدارة الطلبات</a></li>
      <li><a href="#calculator">الآلة الحاسبة</a></li>
    </ul>
  </aside>  <main class="container">
    <section class="section" id="dashboard">
      <h2>لوحة التحكم</h2>
      <div class="card">عدد الأدوية: <span id="medCount">0</span></div>
      <div class="card">منتهية الصلاحية: <span id="expiredCount">0</span></div>
      <div class="card">طلبات معلقة: <span id="pendingOrders">0</span></div>
      <div class="card">إجمالي المبيعات: $<span id="totalSales">0.00</span></div>
    </section><section class="section" id="medicines">
  <h2>إدارة الأدوية</h2>
  <input type="text" id="medName" placeholder="اسم الدواء">
  <input type="number" id="medQty" placeholder="الكمية">
  <input type="date" id="medExp" placeholder="تاريخ الانتهاء">
  <button onclick="addMedicine()">إضافة/تحديث الدواء</button>
  <div class="notification" id="medNotification">تمت إضافة الدواء بنجاح!</div>
  <ul id="medList"></ul>
</section>

<section class="section" id="orders">
  <h2>إدارة الطلبات</h2>
  <p>يتم تطوير هذه الميزة لاحقاً...</p>
</section>

<section class="section" id="calculator">
  <h2>الآلة الحاسبة</h2>
  <div class="calculator">
    <input type="text" id="calcDisplay" readonly>
    <button onclick="press('1')">1</button>
    <button onclick="press('2')">2</button>
    <button onclick="press('3')">3</button>
    <button onclick="press('+')">+</button>
    <button onclick="press('4')">4</button>
    <button onclick="press('5')">5</button>
    <button onclick="press('6')">6</button>
    <button onclick="press('-')">-</button>
    <button onclick="press('7')">7</button>
    <button onclick="press('8')">8</button>
    <button onclick="press('9')">9</button>
    <button onclick="press('*')">×</button>
    <button onclick="press('0')">0</button>
    <button onclick="press('.')">.</button>
    <button onclick="calculate()">=</button>
    <button onclick="press('/')">÷</button>
    <button onclick="clearCalc()">C</button>
  </div>
</section>

<footer>
  تصميم: عمر بابكر (Omer Babiker - Omer Bk)
</footer>

  </main>  <script>
    const sidebar = document.getElementById("sidebar");
    document.querySelector(".toggle-sidebar").onclick = () => {
      sidebar.classList.toggle("active");
    };

    function toggleMode() {
      document.body.classList.toggle("dark");
    }

    let medicines = [];

    function addMedicine() {
      const name = document.getElementById("medName").value;
      const qty = parseInt(document.getElementById("medQty").value);
      const exp = document.getElementById("medExp").value;

      if (!name || !qty || !exp) return alert("يرجى إدخال جميع البيانات");

      medicines.push({ name, qty, exp });
      document.getElementById("medNotification").style.display = 'block';
      setTimeout(() => document.getElementById("medNotification").style.display = 'none', 3000);
      updateMedList();
    }

    function updateMedList() {
      const list = document.getElementById("medList");
      list.innerHTML = "";
      let expired = 0;
      let total = 0;

      medicines.forEach((med, i) => {
        const li = document.createElement("li");
        li.textContent = `${med.name} - ${med.qty} - ${med.exp}`;
        list.appendChild(li);

        total += med.qty;
        if (new Date(med.exp) < new Date()) expired++;
      });

      document.getElementById("medCount").textContent = total;
      document.getElementById("expiredCount").textContent = expired;
    }

    let calcDisplay = document.getElementById("calcDisplay");

    function press(val) {
      calcDisplay.value += val;
    }

    function clearCalc() {
      calcDisplay.value = "";
    }

    function calculate() {
      try {
        calcDisplay.value = eval(calcDisplay.value);
      } catch {
        calcDisplay.value = "خطأ";
      }
    }
  </script></body></html>
