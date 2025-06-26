<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>نظام إدارة الصيدلية الديناميكي</title>
<style>
  /* --- أساسي --- */
  * {
    box-sizing: border-box;
  }
  body {
    margin: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: #f0f4ff;
    color: #222;
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark {
    background: #121b2f;
    color: #eee;
  }
  header {
    background-color: #1976d2;
    color: white;
    padding: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  header h1 {
    margin: 0;
    font-weight: 700;
  }
  #toggleDarkMode {
    cursor: pointer;
    background: transparent;
    border: none;
    color: white;
    font-size: 1.25rem;
  }

  /* --- الشريط الجانبي --- */
  #sidebar {
    position: fixed;
    top: 60px;
    right: 0;
    width: 220px;
    height: calc(100% - 60px);
    background-color: #2196f3;
    color: white;
    display: flex;
    flex-direction: column;
    padding-top: 1rem;
    transition: transform 0.3s ease;
  }
  #sidebar.dark {
    background-color: #0d47a1;
  }
  #sidebar.hide {
    transform: translateX(100%);
  }
  #sidebar button {
    background: none;
    border: none;
    color: white;
    padding: 1rem;
    text-align: right;
    font-size: 1rem;
    cursor: pointer;
    border-left: 4px solid transparent;
    transition: background-color 0.2s, border-left-color 0.2s;
  }
  #sidebar button.active,
  #sidebar button:hover {
    background-color: rgba(255,255,255,0.2);
    border-left-color: #fff;
  }

  /* --- المحتوى الرئيسي --- */
  main {
    margin-right: 220px;
    padding: 1.5rem;
    min-height: calc(100vh - 60px);
    transition: margin-right 0.3s ease;
  }
  #sidebar.hide + main {
    margin-right: 0;
  }

  /* --- أزرار --- */
  button.btn {
    background-color: #1976d2;
    color: white;
    border: none;
    border-radius: 6px;
    padding: 0.5rem 1rem;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  button.btn:hover {
    background-color: #1565c0;
  }
  button.btn.danger {
    background-color: #d32f2f;
  }
  button.btn.danger:hover {
    background-color: #b71c1c;
  }

  /* --- جداول --- */
  table {
    width: 100%;
    border-collapse: collapse;
    background-color: white;
    color: #222;
    border-radius: 8px;
    overflow: hidden;
  }
  body.dark table {
    background-color: #1e2a47;
    color: #ddd;
  }
  th, td {
    padding: 0.75rem;
    border-bottom: 1px solid #ddd;
    text-align: center;
  }
  body.dark th, body.dark td {
    border-color: #444;
  }
  th {
    background-color: #1976d2;
    color: white;
  }
  body.dark th {
    background-color: #0d47a1;
  }

  /* --- النماذج --- */
  form > div {
    margin-bottom: 1rem;
  }
  input[type="text"], input[type="number"], input[type="date"] {
    width: 100%;
    padding: 0.5rem;
    border-radius: 5px;
    border: 1px solid #ccc;
    font-size: 1rem;
  }
  body.dark input[type="text"], body.dark input[type="number"], body.dark input[type="date"] {
    background-color: #324567;
    border-color: #555;
    color: #eee;
  }

  /* --- الآلة الحاسبة --- */
  #calculator {
    max-width: 320px;
    margin: auto;
  }
  #calcDisplay {
    width: 100%;
    height: 50px;
    font-size: 1.5rem;
    text-align: right;
    padding: 0.5rem;
    margin-bottom: 0.5rem;
    border-radius: 6px;
    border: 1px solid #ccc;
  }
  #calcButtons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.5rem;
  }
  #calcButtons button {
    padding: 1rem;
    font-size: 1.2rem;
    border-radius: 6px;
    border: none;
    background-color: #1976d2;
    color: white;
    cursor: pointer;
  }
  #calcButtons button.operator {
    background-color: #f57c00;
  }
  #calcButtons button:active {
    background-color: #115293;
  }
  #calcButtons button.operator:active {
    background-color: #ef6c00;
  }

  /* --- رسائل التنبيه --- */
  #message {
    margin-bottom: 1rem;
    padding: 0.75rem 1rem;
    border-radius: 6px;
    display: none;
  }
  #message.success {
    background-color: #4caf50;
    color: white;
  }
  #message.error {
    background-color: #d32f2f;
    color: white;
  }
</style>
</head>
<body>

<header>
  <h1>نظام إدارة الصيدلية الديناميكي</h1>
  <button id="toggleDarkMode" title="تبديل الوضع الليلي">🌙</button>
</header>

<div id="sidebar" class="dark">
  <button data-page="dashboard" class="active">🏠 لوحة التحكم</button>
  <button data-page="medicines">💊 إدارة الأدوية</button>
  <button data-page="orders">📦 إدارة الطلبات</button>
  <button data-page="sales">💰 تقارير المبيعات</button>
  <button data-page="expenses">🧾 سجل المنصرفات</button>
  <button data-page="statistics">📊 الإحصائيات</button>
  <button data-page="calculator">🧮 الآلة الحاسبة</button>
</div>

<main>

  <div id="message"></div>

  <section id="dashboard" class="page">
    <h2>لوحة التحكم</h2>
    <div style="display: flex; gap: 1rem; flex-wrap: wrap;">
      <div style="background: #1976d2; color:white; padding: 1rem; border-radius: 8px; flex: 1 1 150px; text-align: center;">
        <h3>عدد الأدوية</h3>
        <p id="statMedicines">0</p>
      </div>
      <div style="background: #d32f2f; color:white; padding: 1rem; border-radius: 8px; flex: 1 1 150px; text-align: center;">
        <h3>أدوية منتهية الصلاحية</h3>
        <p id="statExpired">0</p>
      </div>
      <div style="background: #f57c00; color:white; padding: 1rem; border-radius: 8px; flex: 1 1 150px; text-align: center;">
        <h3>طلبات معلقة</h3>
        <p id="statPendingOrders">0</p>
      </div>
      <div style="background: #388e3c; color:white; padding: 1rem; border-radius: 8px; flex: 1 1 150px; text-align: center;">
        <h3>إجمالي المبيعات</h3>
        <p id="statTotalSales">0</p>
      </div>
    </div>
  </section>

  <section id="medicines" class="page" style="display:none;">
    <h2>إدارة الأدوية</h2>
    <form id="medicineForm">
      <div>
        <label>اسم الدواء:</label>
        <input type="text" id="medName" required />
      </div>
      <div>
        <label>الكمية:</label>
        <input type="number" id="medQuantity" min="0" required />
      </div>
      <div>
        <label>تاريخ الانتهاء:</label>
        <input type="date" id="medExpiry" required />
      </div>
      <button type="submit" class="btn">إضافة/تحديث الدواء</button>
    </form>
    <hr />
    <input type="text" id="medSearch" placeholder="ابحث باسم الدواء..." style="width:100%; margin-bottom:1rem; padding:0.5rem; font-size:1rem;" />
    <table>
      <thead>
        <tr>
          <th>الاسم</th>
          <th>الكمية</th>
          <th>تاريخ الانتهاء</th>
          <th>خيارات</th>
        </tr>
      </thead>
      <tbody id="medTableBody"></tbody>
    </table>
  </section>

  <section id="orders" class="page" style="display:none;">
    <h2>إدارة الطلبات</h2>
    <form id="orderForm">
      <div>
        <label>اسم الدواء:</label>
        <input type="text" id="orderMedName" required />
      </div>
      <div>
        <label>الكمية:</label>
        <input type="number" id="orderQuantity" min="1" required />
      </div>
      <button type="submit" class="btn">إضافة طلب</button>
    </form>
    <hr />
    <table>
      <thead>
        <tr>
          <th>رقم الطلب</th>
          <th>اسم الدواء</th>
          <th>الكمية</th>
          <th>الحالة</th>
          <th>خيارات</th>
        </tr>
      </thead>
      <tbody id="orderTableBody"></tbody>
    </table>
  </section>

  <section id="sales" class="page" style="display:none;">
    <h2>تقارير المبيعات</h2>
    <form id="saleForm">
      <div>
        <label>اسم الدواء:</label>
        <input type="text" id="saleMedName" required />
      </div>
      <div>
        <label>الكمية:</label>
        <input type="number" id="saleQuantity" min="1" required />
      </div>
      <div>
        <label>السعر لكل وحدة:</label>
        <input type="number" id="salePrice" min="0" step="0.01" required />
      </div>
      <button type="submit" class="btn">تسجيل بيع</button>
    </form>
    <hr />
    <table>
      <thead>
        <tr>
          <th>رقم البيع</th>
          <th>اسم الدواء</th>
          <th>الكمية</th>
          <th>السعر لكل وحدة</th>
          <th>الإجمالي</th>
          <th>التاريخ</th>
          <th>خيارات</th>
        </tr>
      </thead>
      <tbody id="salesTableBody"></tbody>
    </table>
  </section>

  <section id="expenses" class="page" style="display:none;">
    <h2>سجل المنصرفات</h2>
    <form id="expenseForm">
      <div>
        <label>الوصف:</label>
        <input type="text" id="expenseDesc" required />
      </div>
      <div>
        <label>المبلغ:</label>
        <input type="number" id="expenseAmount"
