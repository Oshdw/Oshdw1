<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <title>نظام إدارة الصيدلية - احترافي</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    /* Reset */
    * {
      box-sizing: border-box;
    }
    body, html {
      margin: 0; padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f4f6f8;
      color: #222;
      transition: background-color 0.3s, color 0.3s;
      height: 100vh;
      overflow: hidden;
    }

    /* الوضع الليلي */
    body.dark {
      background-color: #121212;
      color: #e0e0e0;
    }

    a {
      text-decoration: none;
      color: inherit;
    }

    /* الحاوية الرئيسية */
    #container {
      display: flex;
      height: 100vh;
      overflow: hidden;
    }

    /* الستارة الجانبية */
    #sidebar {
      width: 250px;
      background: #3a86ff;
      color: white;
      display: flex;
      flex-direction: column;
      padding: 1rem 0;
      transition: width 0.3s;
      overflow-y: auto;
    }

    #sidebar.collapsed {
      width: 70px;
    }

    #sidebar h2 {
      text-align: center;
      margin: 0 0 1rem;
      font-weight: 700;
      letter-spacing: 2px;
    }

    #sidebar.collapsed h2 {
      display: none;
    }

    #sidebar nav {
      flex-grow: 1;
    }

    #sidebar ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    #sidebar ul li {
      padding: 1rem 1.5rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 12px;
      font-size: 1.1rem;
      border-left: 4px solid transparent;
      transition: background 0.2s, border-color 0.3s;
      white-space: nowrap;
    }

    #sidebar ul li:hover, #sidebar ul li.active {
      background: rgba(255,255,255,0.2);
      border-left-color: #ffbe0b;
    }

    #sidebar.collapsed ul li {
      justify-content: center;
      font-size: 1.5rem;
    }

    /* أيقونات (استخدمت رموز Unicode بسيطة) */
    #sidebar ul li span.icon {
      font-size: 1.5rem;
      width: 25px;
      text-align: center;
    }

    /* زر تصغير / توسيع الستارة */
    #toggleSidebar {
      background: transparent;
      border: none;
      color: white;
      font-size: 1.8rem;
      cursor: pointer;
      padding: 0.5rem;
      margin: 0 1rem 1rem 1rem;
      align-self: flex-end;
      transition: transform 0.3s;
    }

    #toggleSidebar.collapsed {
      transform: rotate(180deg);
    }

    /* المحتوى الرئيسي */
    #mainContent {
      flex-grow: 1;
      background-color: white;
      overflow-y: auto;
      padding: 1.5rem 2rem;
      transition: background-color 0.3s, color 0.3s;
    }

    body.dark #mainContent {
      background-color: #1e1e1e;
    }

    /* العناوين */
    h1, h2 {
      margin-top: 0;
    }

    /* الأزرار */
    button {
      cursor: pointer;
      background-color: #3a86ff;
      border: none;
      border-radius: 6px;
      color: white;
      padding: 0.6rem 1.3rem;
      font-size: 1rem;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #265dff;
    }
    button:disabled {
      background-color: #999;
      cursor: not-allowed;
    }

    /* نمط الحقول */
    input[type="text"], input[type="number"], input[type="date"], input[type="password"] {
      padding: 0.5rem;
      font-size: 1rem;
      border-radius: 5px;
      border: 1px solid #ccc;
      width: 100%;
      margin-bottom: 1rem;
      transition: border-color 0.3s;
    }
    input:focus {
      border-color: #3a86ff;
      outline: none;
    }

    /* الجداول */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 0.8rem;
      text-align: center;
      transition: background-color 0.3s, color 0.3s;
    }
    th {
      background-color: #e9f0fb;
    }
    body.dark th {
      background-color: #333;
      color: #ddd;
    }
    body.dark td {
      background-color: #222;
      color: #ccc;
    }

    /* أزرار العمليات داخل الجدول */
    .table-btn {
      margin: 0 3px;
      padding: 0.3rem 0.7rem;
      font-size: 0.9rem;
      border-radius: 5px;
      border: none;
      color: white;
      transition: background-color 0.3s;
    }
    .edit-btn { background-color: #ffbe0b; color: #333; }
    .edit-btn:hover { background-color: #e0a800; }
    .delete-btn { background-color: #ff595e; }
    .delete-btn:hover { background-color: #e04143; }

    /* صفحات مخفية */
    .page {
      display: none;
    }
    .page.active {
      display: block;
    }

    /* التنبيهات */
    .alert {
      background-color: #ff595e;
      color: white;
      padding: 0.5rem 1rem;
      border-radius: 6px;
      margin-bottom: 1rem;
      font-weight: bold;
      animation: pulse 1.5s infinite alternate;
    }
    @keyframes pulse {
      0% { opacity: 1; }
      100% { opacity: 0.6; }
    }

    /* الآلة الحاسبة */
    #calculator {
      max-width: 350px;
      margin: 0 auto;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 6px 15px rgba(0,0,0,0.1);
      background: #3a86ff;
      color: white;
    }
    #calcDisplay {
      background: rgba(0,0,0,0.15);
      font-size: 2rem;
      padding: 1rem;
      text-align: right;
      letter-spacing: 1px;
      user-select: none;
      height: 50px;
      line-height: 50px;
      overflow-x: auto;
      white-space: nowrap;
    }
    #calcButtons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.3rem;
      padding: 1rem;
      background: #265dff;
    }
    #calcButtons button {
      background: #1f4dbf;
      border-radius: 6px;
      border: none;
      font-size: 1.3rem;
      color: white;
      transition: background-color 0.3s;
    }
    #calcButtons button:hover {
      background: #144a9c;
    }
    #calcButtons button.operator {
      background: #ffbe0b;
      color: #333;
    }
    #calcButtons button.operator:hover {
      background: #e0a800;
    }
    #calcButtons button.zero {
      grid-column: span 2;
    }

    /* الرسائل */
    #messageBox {
      position: fixed;
      bottom: 1rem;
      right: 1rem;
      background: #3a86ff;
      color: white;
      padding: 1rem 1.5rem;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.4s;
      z-index: 1000;
      user-select: none;
    }
    #messageBox.show {
      opacity: 1;
      pointer-events: auto;
    }

    /* شريط التبديل للوضع الليلي */
    #darkModeToggle {
      background: transparent;
      border: 2px solid white;
      border-radius: 20px;
      color: white;
      padding: 0.3rem 1rem;
      margin: 1rem auto 0 auto;
      font-size: 0.9rem;
      cursor: pointer;
      user-select: none;
      transition: background-color 0.3s, color 0.3s;
    }
    #darkModeToggle:hover {
      background-color: white;
      color: #3a86ff;
    }
    #sidebar.collapsed #darkModeToggle {
      margin: 1rem auto;
    }

    /* تسجيل خروج */
    #logoutBtn {
      background: transparent;
      border: 2px solid white;
      margin: 1rem auto 0 auto;
      width: 80%;
      padding: 0.5rem 0;
      font-size: 1rem;
      border-radius: 6px;
      color: white;
      cursor: pointer;
      transition: background-color 0.3s;
      user-select: none;
    }
    #logoutBtn:hover {
      background-color: white;
      color: #3a86ff;
    }

  </style>
</head>
<body>

<div id="container">

  <!-- الستارة الجانبية -->
  <aside id="sidebar">
    <h2>الصيدلية</h2>
    <button id="toggleSidebar" title="تصغير / توسيع القائمة">&#9776;</button>
    <nav>
      <ul>
        <li class="active" data-page="dashboard"><span class="icon">🏠</span><span class="text">لوحة التحكم</span></li>
        <li data-page="medicines"><span class="icon">💊</span><span class="text">إدارة الأدوية</span></li>
        <li data-page="sales"><span class="icon">🛒</span><span class="text">المبيعات</span></li>
        <li data-page="orders"><span class="icon">📦</span><span class="text">الطلبيات</span></li>
        <li data-page="calculator"><span class="icon">🧮</span><span class="text">آلة الحاسبة</span></li>
        <li data-page="settings"><span class="icon">⚙️</span><span class="text">الإعدادات</span></li>
      </ul>
    </nav>
    <button id="darkModeToggle">وضع ليلي</button>
    <button id="logoutBtn">تسجيل خروج</button>
  </aside>

  <!-- المحتوى الرئيسي -->
  <main id="mainContent">
    <!-- لوحة التحكم -->
    <section id="dashboard" class="page active">
      <h1>لوحة التحكم</h1>
      <div style="display:flex;gap:1rem;flex-wrap: wrap;">
        <div style="background:#3a86ff;color:white;padding:1rem;border-radius:10px;flex:1; min-width: 150px; text-align:center;">
          <h3>عدد الأدوية</h3>
          <p id="medicineCount">0</p>
        </div>
        <div style="background:#ff595e;color:white;padding:1rem;border-radius:10px;flex:1; min-width: 150px; text-align:center;">
          <h3>أدوية منتهية</h3>
          <p id="expiredCount">0</p>
        </div>
        <div style="background:#ffbe0b;color:#333;padding:1rem;border-radius:10px;flex:1; min-width: 150px; text-align:center;">
          <h3>تنبيهات نقص الكمية</h3>
          <p id="alertsCount">0</p>
        </div>
        <div style="background:#3a86ff;color:white;padding:1rem;border-radius:10px;flex:1; min-width: 150px; text-align:center;">
          <h3>إجمالي الكمية</h3>
          <p id="totalQuantity">0</p>
        </div>
      </div>
    </section>

    <!-- إدارة الأدوية -->
    <section id="medicines" class="page">
      <h1>إدارة الأدوية</h1>
      <input type="text" id="searchMedicineInput" placeholder="ابحث باسم الدواء..." />
      <button id="addMedicineBtn">+ إضافة دواء</button>
      <table>
        <thead>
          <tr>
            <th>الاسم</th>
            <th>الكمية</th>
            <th>تاريخ الانتهاء</th>
            <th>الخيارات</th>
          </tr>
        </thead>
        <tbody id="medicinesTableBody"></tbody>
      </table>
    </section>

    <!-- المبيعات -->
    <section id="sales" class="page">
      <h1>المبيعات</h1>
      <button id="addSaleBtn">+ إضافة عملية بيع</button>
      <table>
        <thead>
          <tr>
            <th>اسم الدواء</th>
            <th>الكمية المباعة</th>
            <th>التاريخ</th>
            <th>الخيارات</th>
          </tr>
        </thead>
        <tbody id="salesTableBody"></tbody>
      </table>
    </section>

    <!-- الطلبيات -->
    <section id="orders" class="page">
      <h1>الطلبيات</h1>
      <button id="
