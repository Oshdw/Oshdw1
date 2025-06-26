<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>نظام إدارة الصيدلية - متكامل</title>
<style>
  /* عام */
  body, html {
    margin: 0; padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f5f9ff;
    color: #003366;
    height: 100vh;
    overflow: hidden;
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark {
    background-color: #001f4d;
    color: #cce0ff;
  }
  #container {
    display: flex;
    height: 100vh;
  }
  /* القائمة الجانبية */
  #sidebar {
    background-color: #004080;
    width: 260px;
    display: flex;
    flex-direction: column;
    padding: 1rem 0;
    color: white;
    transition: width 0.3s;
  }
  #sidebar.collapsed {
    width: 70px;
  }
  #sidebar h2 {
    margin: 0 0 1.5rem 0;
    text-align: center;
    font-weight: 700;
    letter-spacing: 3px;
  }
  #sidebar.collapsed h2 {
    display: none;
  }
  #sidebar ul {
    list-style: none;
    padding: 0;
    margin: 0;
    flex-grow: 1;
  }
  #sidebar ul li {
    cursor: pointer;
    padding: 1rem 1.5rem;
    display: flex;
    align-items: center;
    gap: 14px;
    font-size: 1.1rem;
    border-left: 4px solid transparent;
    transition: background-color 0.3s, border-color 0.3s;
    white-space: nowrap;
  }
  #sidebar ul li.active, #sidebar ul li:hover {
    background-color: #3366cc;
    border-left-color: #ffcc00;
  }
  #sidebar.collapsed ul li {
    justify-content: center;
    font-size: 1.6rem;
  }
  #sidebar ul li span.icon {
    width: 28px;
    text-align: center;
  }
  #toggleSidebar {
    background: none;
    border: none;
    color: white;
    font-size: 2rem;
    cursor: pointer;
    padding: 0.5rem 1rem;
    margin: 0 1rem 1rem 1rem;
    align-self: flex-end;
    transition: transform 0.3s;
  }
  #toggleSidebar.collapsed {
    transform: rotate(180deg);
  }
  #darkModeToggle {
    background-color: transparent;
    border: 2px solid white;
    color: white;
    border-radius: 20px;
    margin: 1rem auto 0 auto;
    padding: 0.3rem 1.2rem;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.3s, color 0.3s;
  }
  #darkModeToggle:hover {
    background-color: white;
    color: #004080;
  }
  #sidebar.collapsed #darkModeToggle {
    margin: 1rem auto;
  }
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
    color: #004080;
  }
  /* المحتوى الرئيسي */
  #mainContent {
    flex-grow: 1;
    background-color: white;
    overflow-y: auto;
    padding: 2rem 3rem;
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark #mainContent {
    background-color: #003366;
  }
  h1, h2 {
    margin-top: 0;
  }
  .page {
    display: none;
  }
  .page.active {
    display: block;
  }
  /* جداول */
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
  }
  table th, table td {
    border: 1px solid #ccc;
    padding: 0.5rem 0.7rem;
    text-align: center;
  }
  body.dark table th, body.dark table td {
    border-color: #66a3ff;
  }
  /* نماذج */
  form {
    margin-top: 1rem;
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }
  form label {
    flex-basis: 100%;
    font-weight: 600;
  }
  form input, form select, form button {
    padding: 0.5rem;
    font-size: 1rem;
  }
  form input, form select {
    flex-grow: 1;
    border: 1px solid #004080;
    border-radius: 4px;
  }
  form button {
    background-color: #004080;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    min-width: 120px;
    transition: background-color 0.3s;
  }
  form button:hover {
    background-color: #3366cc;
  }
  /* أزرار في الجدول */
  .btn-edit, .btn-delete {
    cursor: pointer;
    padding: 4px 8px;
    border: none;
    border-radius: 3px;
    color: white;
    font-weight: 600;
  }
  .btn-edit {
    background-color: #3399ff;
    margin-right: 4px;
  }
  .btn-edit:hover {
    background-color: #66b2ff;
  }
  .btn-delete {
    background-color: #ff4d4d;
  }
  .btn-delete:hover {
    background-color: #ff6666;
  }
  /* رسالة */
  #message {
    margin-top: 1rem;
    padding: 0.7rem;
    background-color: #dff0d8;
    color: #3c763d;
    border-radius: 4px;
    display: none;
  }
  body.dark #message {
    background-color: #4b7033;
    color: #cdebb5;
  }
  /* الآلة الحاسبة */
  #calculator form {
    max-width: 400px;
  }
  #calculator label, #calculator input {
    width: 100%;
  }
  #calculator button {
    margin-top: 1rem;
    width: 100%;
  }
</style>
</head>
<body>
<div id="container">
  <aside id="sidebar">
    <h2>صيدليتك</h2>
    <nav>
      <ul>
        <li class="active" data-page="dashboard"><span class="icon">🏠</span> <span class="text">الرئيسية</span></li>
        <li data-page="medicines"><span class="icon">💊</span> <span class="text">إدارة الأدوية</span></li>
        <li data-page="sales"><span class="icon">🛒</span> <span class="text">المبيعات</span></li>
        <li data-page="orders"><span class="icon">📦</span> <span class="text">الطلبيات</span></li>
        <li data-page="calculator"><span class="icon">🧮</span> <span class="text">الآلة الحاسبة</span></li>
      </ul>
    </nav>
    <button id="darkModeToggle">تفعيل الوضع الداكن</button>
    <button id="logoutBtn">تسجيل خروج</button>
    <button id="toggleSidebar" title="طي القائمة">⮜</button>
  </aside>
  <main id="mainContent">
    <!-- الرئيسية -->
    <section id="dashboard" class="page active">
      <h1>مرحبا بك في نظام إدارة الصيدلية</h1>
      <p>هذا النظام يمكنك من إدارة الأدوية، المبيعات، الطلبيات والعمليات الحسابية بشكل احترافي وسهل.</p>
    </section>
    <!-- إدارة الأدوية -->
    <section id="medicines" class="page">
      <h1>إدارة الأدوية</h1>
      <form id="medicineForm">
        <input type="hidden" id="medicineId" />
        <label for="medicineName">اسم الدواء:</label>
        <input type="text" id="medicineName" required placeholder="أدخل اسم الدواء" />
        <label for="medicineQuantity">الكمية:</label>
        <input type="number" id="medicineQuantity" min="0" required placeholder="كمية الدواء" />
        <label for="medicinePrice">سعر الوحدة:</label>
        <input type="number" id="medicinePrice" min="0" step="0.01" required placeholder="سعر الوحدة" />
        <button type="submit" id="addMedicineBtn">إضافة دواء</button>
        <button type="button" id="cancelEditBtn" style="display:none;">إلغاء التعديل</button>
      </form>
      <table id="medicinesTable" aria-label="جدول الأدوية">
        <thead>
          <tr>
            <th>اسم الدواء</th>
            <th>الكمية</th>
            <th>سعر الوحدة</th>
            <th>الإجمالي</th>
            <th>إجراءات</th>
          </tr>
        </thead>
        <tbody>
          <!-- بيانات الأدوية تظهر هنا -->
        </tbody>
      </table>
    </section>
    <!-- المبيعات -->
    <section id="sales" class="page">
      <h1>تسجيل المبيعات</h1>
      <form id="salesForm">
        <label for="saleMedicineSelect">اختر دواء:</label>
        <select id="saleMedicineSelect" required>
          <!-- خيارات الأدوية -->
        </select>
        <label for="saleQuantity">الكمية المباعة:</label>
        <input type="number" id="saleQuantity" min="1" required placeholder="كمية البيع" />
        <button type="submit">تسجيل البيع</button>
      </form>
      <table id="salesTable" aria-label="جدول المبيعات">
        <thead>
          <tr>
            <th>اسم الدواء</th>
            <th>الكمية المباعة</th>
            <th>السعر لكل وحدة</th>
            <th>الإجمالي</th>
          </tr>
        </thead>
        <tbody>
          <!-- بيانات المبيعات تظهر هنا -->
        </tbody>
      </table>
    </section>
    <!-- الطلبيات -->
    <section id="orders" class="page">
      <h1>إدارة الطلبيات</h1>
      <form id="ordersForm">
        <label for="orderMedicineName">اسم الدواء:</label>
        <input type="text" id="orderMedicineName" required placeholder="اسم الدواء المطلوب" />
        <label for="orderQuantity">الكمية المطلوبة:</label>
        <input type="number" id="orderQuantity" min="1" required placeholder="الكمية" />
        <button type="submit">إضافة طلبية</button>
      </form>
      <table id="ordersTable" aria-label="جدول الطلبيات">
        <thead>
          <tr>
            <th>اسم الدواء</th>
            <th>الكمية المطلوبة</th>
            <th>حذف</th>
          </tr>
        </thead>
        <tbody>
          <!-- بيانات الطلبيات تظهر هنا -->
        </tbody>
      </table>
    </section>
    <!-- الآلة الحاسبة -->
    <section id="calculator" class="page">
      <h1>آلة حاسبة تكلفة البيع</h1>
      <form id="calculatorForm">
        <label for="calcPrice">سعر الوحدة:</label>
        <input type="number" id="calcPrice" min="0" step="0.01" required placeholder="أدخل سعر الوحدة" />
        <label for="calcQuantity">الكمية:</label>
        <input type="number" id="calcQuantity" min="1" required placeholder="أدخل الكمية" />
        <button type="submit">احسب التكلفة</button>
      </form>
      <p id="calcResult" style="margin-top: 1rem; font-weight: 600;"></p>
    </section>
  </main>
</div>

<script>
  // البيانات المبدئية
  let medicines = [];
  let sales = [];
  let orders = [];

  // اختيار العناصر
  const sidebarItems = document.querySelectorAll('#sidebar nav ul li');
  const pages = document.querySelectorAll('.page');
  const sidebar = document.getElementById('sidebar');
  const toggleSidebarBtn = document.getElementById('toggleSidebar');
  const darkModeToggle = document.getElementById('darkModeToggle');
  const logoutBtn = document.getElementById('logoutBtn');

  // التنقل بين الصفحات
  sidebarItems.forEach(item => {
    item.addEventListener('click', () => {
      sidebarItems.forEach(i => i.classList.remove('active'));
      pages.forEach(p => p.classList.remove('active'));
      item.classList.add('active');
      const pageId = item.getAttribute('data-page');
      document.getElementById(pageId).classList.add('active');

      if (pageId === 'medicines') renderMedicinesTable();
      if (pageId === 'sales') {
        populateSalesMedicineSelect();
        renderSalesTable();
      }
      if (pageId === 'orders') renderOrdersTable();
    });
  });

  // طي القائمة الجانبية
  toggleSidebarBtn.addEventListener('click', () => {
    sidebar.classList.toggle('collapsed');
    toggleSidebarBtn.classList.toggle('collapsed');
  });

  // الوضع الداكن
  darkModeToggle.addEventListener('click', () => {
    document.body.classList.toggle('dark');
    darkModeToggle.textContent = document.body.classList.contains('dark') ? 'تعطيل الوضع الداكن' : 'تفعيل الوضع الداكن';
  });

  // تسجيل خروج (محاكاة)
  logoutBtn.addEventListener('click', () => {
    alert('تم تسجيل الخروج (محاكاة)');
  });

  // إدارة الأدوية
  const medicineForm = document.getElementById('medicineForm');
  const medicineIdInput = document.getElementById('medicineId');
  const medicineNameInput = document.getElementById('medicineName');
  const medicineQuantityInput = document.getElementById('medicineQuantity');
  const medicinePriceInput = document.getElementById('medicinePrice');
  const addMedicineBtn = document.getElementById('addMedicineBtn');
  const cancelEditBtn = document
