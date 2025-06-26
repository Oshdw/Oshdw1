<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>نظام إدارة الصيدلية</title>
  <style>
    /* Reset */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    body {
      background-color: #f4f6f8;
      color: #222;
      transition: background-color 0.3s, color 0.3s;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    /* Dark mode */
    body.dark {
      background-color: #121212;
      color: #eee;
    }
    /* Container */
    .app {
      display: flex;
      min-height: 100vh;
    }

    /* Sidebar */
    .sidebar {
      width: 250px;
      background-color: #0077cc;
      color: white;
      transition: transform 0.3s ease;
      padding: 20px;
      flex-shrink: 0;
      display: flex;
      flex-direction: column;
    }
    .sidebar.closed {
      transform: translateX(100%);
    }
    .sidebar h2 {
      margin-bottom: 30px;
      font-weight: 700;
      font-size: 1.5rem;
      text-align: center;
      user-select: none;
    }
    .sidebar nav a {
      color: white;
      padding: 10px 15px;
      margin-bottom: 10px;
      text-decoration: none;
      border-radius: 5px;
      display: block;
      transition: background-color 0.2s;
      font-weight: 600;
      user-select: none;
    }
    .sidebar nav a:hover,
    .sidebar nav a.active {
      background-color: #005fa3;
    }
    /* Toggle Sidebar Button */
    #sidebarToggle {
      background-color: #0077cc;
      color: white;
      border: none;
      padding: 10px 15px;
      cursor: pointer;
      font-size: 1.1rem;
      position: fixed;
      top: 10px;
      right: 10px;
      border-radius: 5px;
      z-index: 1000;
      user-select: none;
    }
    /* Main content */
    main {
      flex-grow: 1;
      padding: 20px 30px;
      overflow-y: auto;
      transition: margin-right 0.3s ease;
    }
    /* Adjust main margin when sidebar closed */
    .sidebar.closed ~ main {
      margin-right: 0;
    }

    /* Header */
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 25px;
    }
    header h1 {
      font-weight: 700;
      font-size: 2rem;
      user-select: none;
    }
    /* Dark mode toggle */
    #darkModeToggle {
      background-color: #0077cc;
      border: none;
      color: white;
      padding: 8px 14px;
      border-radius: 5px;
      cursor: pointer;
      user-select: none;
      font-weight: 600;
      transition: background-color 0.3s;
    }
    #darkModeToggle:hover {
      background-color: #005fa3;
    }

    /* Dashboard cards */
    .dashboard {
      display: grid;
      grid-template-columns: repeat(auto-fit,minmax(180px,1fr));
      gap: 20px;
      margin-bottom: 40px;
    }
    .card {
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
      text-align: center;
      user-select: none;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark .card {
      background-color: #1e1e1e;
      color: #eee;
      box-shadow: 0 3px 10px rgba(255,255,255,0.1);
    }
    .card h3 {
      font-size: 1.1rem;
      margin-bottom: 10px;
      font-weight: 600;
    }
    .card p {
      font-size: 2rem;
      font-weight: 700;
      color: #0077cc;
    }
    body.dark .card p {
      color: #66aaff;
    }

    /* Form styles */
    form {
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      max-width: 500px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
      user-select: none;
      transition: background-color 0.3s, color 0.3s;
      margin-bottom: 30px;
    }
    body.dark form {
      background-color: #1e1e1e;
      color: #eee;
      box-shadow: 0 3px 10px rgba(255,255,255,0.1);
    }
    form label {
      display: block;
      margin-bottom: 8px;
      font-weight: 600;
    }
    form input[type="text"],
    form input[type="number"],
    form input[type="date"] {
      width: 100%;
      padding: 8px 10px;
      margin-bottom: 15px;
      border-radius: 5px;
      border: 1px solid #ccc;
      font-size: 1rem;
      transition: border-color 0.3s;
    }
    body.dark form input[type="text"],
    body.dark form input[type="number"],
    body.dark form input[type="date"] {
      background-color: #333;
      border-color: #555;
      color: #eee;
    }
    form input[type="text"]:focus,
    form input[type="number"]:focus,
    form input[type="date"]:focus {
      outline: none;
      border-color: #0077cc;
    }
    /* Button inside form */
    form button {
      background-color: #0077cc;
      color: white;
      border: none;
      padding: 12px 20px;
      font-size: 1rem;
      border-radius: 6px;
      cursor: pointer;
      font-weight: 700;
      user-select: none;
      transition: background-color 0.3s;
    }
    form button:hover {
      background-color: #005fa3;
    }
    /* Message */
    #message {
      margin-top: 12px;
      font-weight: 700;
      user-select: none;
    }
    #message.success {
      color: green;
    }
    #message.error {
      color: #cc0000;
    }

    /* Table */
    table {
      width: 100%;
      border-collapse: collapse;
      user-select: none;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 12px 15px;
      text-align: center;
    }
    th {
      background-color: #0077cc;
      color: white;
      font-weight: 700;
    }
    body.dark th {
      background-color: #005fa3;
    }
    tr:nth-child(even) {
      background-color: #f9f9f9;
    }
    body.dark tr:nth-child(even) {
      background-color: #2a2a2a;
    }
    tr:hover {
      background-color: #e1e1e1;
    }
    body.dark tr:hover {
      background-color: #3a3a3a;
    }
    /* Options buttons */
    .option-btn {
      background-color: #0077cc;
      border: none;
      color: white;
      padding: 6px 12px;
      border-radius: 5px;
      cursor: pointer;
      margin: 0 3px;
      user-select: none;
      transition: background-color 0.3s;
    }
    .option-btn:hover {
      background-color: #005fa3;
    }

    /* Calculator styles */
    .calculator {
      max-width: 360px;
      background-color: white;
      border-radius: 10px;
      box-shadow: 0 3px 10px rgba(0,0,0,0.1);
      padding: 15px;
      user-select: none;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark .calculator {
      background-color: #1e1e1e;
      color: #eee;
      box-shadow: 0 3px 10px rgba(255,255,255,0.1);
    }
    #calc-screen {
      width: 100%;
      height: 50px;
      font-size: 1.5rem;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 8px;
      padding: 8px 15px;
      text-align: right;
      user-select: text;
      background-color: #f9f9f9;
      color: #222;
      transition: background-color 0.3s, color 0.3s;
    }
    body.dark #calc-screen {
      background-color: #333;
      color: #eee;
      border-color: #555;
    }
    .calc-buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }
    .calc-buttons button {
      padding: 15px 0;
      font-size: 1.2rem;
      border-radius: 8px;
      border: none;
      background-color: #0077cc;
      color: white;
      cursor: pointer;
      user-select: none;
      transition: background-color 0.3s;
    }
    .calc-buttons button:hover {
      background-color: #005fa3;
    }

    /* Responsive */
    @media (max-width: 768px) {
      .app {
        flex-direction: column;
      }
      .sidebar {
        width: 100%;
        position: fixed;
        top: 0;
        right: 0;
        height: 100vh;
        z-index: 999;
      }
      .sidebar.closed {
        transform: translateX(100%);
      }
      main {
        margin: 0;
        padding-top: 70px;
      }
      #sidebarToggle {
        top: 15px;
        right: 15px;
      }
    }
  </style>
</head>
<body>

<button id="sidebarToggle" aria-label="Toggle Menu">☰ القائمة</button>

<div class="app">
  <aside class="sidebar closed" id="sidebar">
    <h2>نظام إدارة الصيدلية</h2>
    <nav>
      <a href="#" class="active" data-section="dashboard">لوحة التحكم</a>
      <a href="#" data-section="drugs">إدارة الأدوية</a>
      <a href="#" data-section="orders">إدارة الطلبات</a>
      <a href="#" data-section="calculator">الآلة الحاسبة</a>
    </nav>
  </aside>

  <main>
    <header>
      <h1 id="sectionTitle">لوحة التحكم</h1>
      <button id="darkModeToggle">وضع داكن</button>
    </header>

    <!-- لوحة التحكم -->
    <section id="dashboard" class="section active">
      <div class="dashboard">
        <div class="card">
          <h3>عدد الأدوية</h3>
          <p id="drugsCount">0</p>
        </div>
        <div class="card">
          <h3>أدوية منتهية الصلاحية</h3>
          <p id="expiredCount">0</p>
        </div>
        <div class="card">
          <h3>طلبات معلقة</h3>
          <p id="pendingOrdersCount">0</p>
        </div>
        <div class="card">
          <h3>إجمالي المبيعات</h3>
          <p id="totalSales">0</p>
        </div>
      </div>
    </section>

    <!-- إدارة الأدوية -->
    <section id="drugs" class="section" style="display:none;">
      <form id="drugForm">
        <label for="drugName">اسم الدواء:</label>
        <input type="text" id="drugName" required />

        <label for="drugQuantity">الكمية:</label>
        <input type="number" id="drugQuantity" min="1" required />

        <label for="drugExpiry">تاريخ الانتهاء:</label>
        <input type="date" id="drugExpiry" required />

        <button type="submit">إضافة / تحديث الدواء</button>
        <div id="formMessage"></div>
      </form>

      <table id="drugsTable">
        <thead>
          <tr>
            <th>الاسم</th>
            <th>الكمية</th>
            <th>تاريخ الانتهاء</th>
            <th>خيارات</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>

    <!-- إدارة الطلبات -->
    <section id="orders" class="section" style="display:none;">
      <p>قيد التطوير...</p>
    </section>

    <!-- الآلة الحاسبة -->
    <section id="calculator" class="section" style="display:none;">
      <div class="calculator">
        <input type="text" id="calc-screen" readonly aria-label="شاشة الآلة الحاسبة" />
        <div class="calc-buttons">
          <button data-value="7">7</button>
          <button data-value="8">8</button>
          <button data-value="9">9</button>
          <button data-value="/">÷</button>
          <button data-value="4">4</button>
          <button data-value="5">5</button>
          <button data-value="6">6</button>
          <button data-value="*">×</button>
          <button data-value="1">1</button>
          <button data-value="2">2</button>
          <button data-value="3">3</button>
          <button data-value="-">-</button>
          <button data-value="0">0</button>
          <button data-value=".">.</button>
          <button id="calc-equal">=</button>
          <button data-value="+">+</button>
          <button id="calc-clear" style="grid-column: span 4; background-color:#cc0000;">C</button>
        </div>
      </div>
    </section>
  </main>
</div>

<script>
  // عناصر الواجهة
  const sidebar = document.getElementById('sidebar');
  const sidebarToggle = document.getElementById('sidebarToggle');
  const darkModeToggle = document.getElementById('darkModeToggle');
  const body = document.body;
  const navLinks = sidebar.querySelectorAll('nav a');
  const sections = document.querySelectorAll('.section');
  const sectionTitle = document.getElementById('sectionTitle');

  // بيانات وهمية للإحصائيات والأدوية والطلبات
  let drugsList = [];

  // إظهار وإخفاء الستارة الجانبية
  sidebarToggle.addEventListener('click', () => {
    sidebar.classList.toggle('closed');
  });

  // تبديل الوضع الليلي
  darkModeToggle.addEventListener('click', () => {
    body.classList.toggle('dark');
    if(body.classList.contains('dark')) {
      darkModeToggle.textContent = 'الوضع الفاتح';
      localStorage.setItem('darkMode', 'enabled');
    } else {
      darkModeToggle.textContent = 'وضع داكن';
      localStorage.setItem('darkMode', 'disabled');
    }
  });

  // تفعيل الوضع الليلي إذا محفوظ في التخزين المحلي
  if(localStorage.getItem('darkMode') === 'enabled') {
    body.classList.add('dark');
    darkModeToggle.textContent = 'الوضع الفاتح';
  }

  // التنقل بين الأقسام
  navLinks.forEach(link => {
    link.addEventListener('click', e => {
      e.preventDefault();
      const target = link.getAttribute('data-section');

      // إزالة تفعيل من جميع الروابط
      navLinks.forEach(l => l.classList.remove('active'));
      link.classList.add('active');

      // إخفاء كل الأقسام
      sections.forEach(sec => {
        sec.style.display = 'none';
        sec.classList.remove('active');
      });

      // إظهار القسم المختار
      const targetSection = document.getElementById(target);
      if(targetSection) {
        targetSection.style.display = 'block';
        targetSection.classList.add('active');
        sectionTitle.textContent = link.textContent;
      }

      // إغلاق الشريط الجانبي على الشاشات الصغيرة تلقائيًا
      if(window.innerWidth <= 768){
        sidebar.classList.add('closed');
      }
    });
  });

  // تحديث الإحصائيات في لوحة التحكم
  function updateDashboard() {
    const drugsCount = drugsList.length;
    const expiredCount = drugsList.filter(d => new Date(d.expiry) < new Date()).length;
    const pendingOrdersCount = 3; // بيانات وهمية
    const totalSales = 12000; // بيانات وهمية

    document.getElementById('drugsCount').textContent = drugsCount;
    document.getElementById('expiredCount').textContent = expiredCount;
    document.getElementById('pendingOrdersCount').textContent = pendingOrdersCount;
    document.getElementById('totalSales').textContent = totalSales.toLocaleString();
  }

  // تحديث جدول الأدوية
  function updateDrugsTable
