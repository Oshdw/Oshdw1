<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    :root {
      --primary: #1976d2;
      --primary-dark: #1565c0;
      --danger: #d32f2f;
      --warning: #ffa000;
      --success: #388e3c;
      --info: #0288d1;
      --background-light: #ffffff;
      --background-dark: #121212;
      --card-light: #f5f5f5;
      --card-dark: #1e1e1e;
      --text-light: #ffffff;
      --text-dark: #212121;
      --text-gray: #757575;
      --border-light: #e0e0e0;
      --border-dark: #424242;
      --sidebar-width: 280px;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Tajawal', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      display: flex;
      background-color: var(--background-light);
      color: var(--text-dark);
      transition: background-color 0.3s, color 0.3s;
      min-height: 100vh;
    }

    body.dark {
      background-color: var(--background-dark);
      color: var(--text-light);
    }

    .sidebar {
      width: var(--sidebar-width);
      background-color: var(--primary);
      color: white;
      height: 100vh;
      padding: 1rem;
      position: fixed;
      right: -280px;
      top: 0;
      overflow-y: auto;
      transition: right 0.3s ease-in-out;
      z-index: 1000;
    }

    .sidebar.visible {
      right: 0;
    }

    .sidebar h2 {
      text-align: center;
      margin: 1rem 0;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
    }

    .sidebar h2 img {
      width: 30px;
      height: 30px;
      border-radius: 50%;
    }

    .sidebar-menu {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
    }

    .sidebar button {
      background: none;
      border: none;
      color: white;
      font-size: 1rem;
      width: 100%;
      text-align: right;
      padding: 0.8rem 1rem;
      margin: 0.25rem 0;
      cursor: pointer;
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      transition: all 0.2s;
    }

    .sidebar button:hover {
      background-color: rgba(255, 255, 255, 0.15);
      transform: translateX(-5px);
    }

    .sidebar button.active {
      background-color: rgba(255, 255, 255, 0.2);
      font-weight: bold;
    }

    .sidebar button .material-icons {
      margin-left: 10px;
    }

    .main {
      margin-right: 0;
      padding: 1.5rem;
      flex-grow: 1;
      transition: margin-right 0.3s;
      width: 100%;
      min-height: 100vh;
    }

    .main.full {
      margin-right: 0;
    }

    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--border-light);
      position: sticky;
      top: 0;
      background-color: var(--background-light);
      z-index: 100;
      padding-top: 1rem;
    }

    body.dark .topbar {
      border-bottom-color: var(--border-dark);
      background-color: var(--background-dark);
    }

    .topbar h2 {
      font-size: 1.8rem;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 10px;
    }

    body.dark .topbar h2 {
      color: var(--primary-dark);
    }

    .topbar-actions {
      display: flex;
      gap: 0.8rem;
      align-items: center;
    }

    .topbar-action-btn {
      cursor: pointer;
      padding: 0.6rem;
      border-radius: 50%;
      transition: background-color 0.2s;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: transparent;
      border: none;
      color: var(--text-dark);
    }

    body.dark .topbar-action-btn {
      color: var(--text-light);
    }

    .topbar-action-btn:hover {
      background-color: rgba(0, 0, 0, 0.08);
    }

    body.dark .topbar-action-btn:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .page {
      display: none;
      animation: fadeIn 0.3s ease-in-out;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .page.active {
      display: block;
    }

    .card {
      background-color: var(--card-light);
      padding: 1.5rem;
      margin-bottom: 1.5rem;
      border-radius: 10px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
    }

    body.dark .card {
      background-color: var(--card-dark);
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.3);
    }

    .card h3 {
      margin-bottom: 1.2rem;
      color: var(--primary);
      padding-bottom: 0.5rem;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark .card h3 {
      border-bottom-color: var(--border-dark);
    }

    .form-group {
      margin-bottom: 1.2rem;
    }

    .form-group label {
      display: block;
      margin-bottom: 0.6rem;
      font-weight: 500;
      color: var(--primary);
    }

    body.dark .form-group label {
      color: var(--primary-dark);
    }

    .form-control {
      width: 100%;
      padding: 0.8rem;
      border: 1px solid var(--border-light);
      border-radius: 6px;
      font-size: 1rem;
      transition: all 0.3s;
      background-color: var(--background-light);
      color: var(--text-dark);
    }

    body.dark .form-control {
      background-color: var(--card-dark);
      border-color: var(--border-dark);
      color: var(--text-light);
    }

    .form-control:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 3px rgba(25, 118, 210, 0.2);
    }

    .btn {
      background-color: var(--primary);
      color: white;
      border: none;
      padding: 0.8rem 1.5rem;
      border-radius: 6px;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 500;
      transition: all 0.2s;
      display: inline-flex;
      align-items: center;
      justify-content: center;
      gap: 0.5rem;
    }

    .btn:hover {
      background-color: var(--primary-dark);
    }

    .btn-danger {
      background-color: var(--danger);
    }

    .btn-danger:hover {
      background-color: #c62828;
    }

    .btn-success {
      background-color: var(--success);
    }

    .btn-success:hover {
      background-color: #2e7d32;
    }

    .btn-warning {
      background-color: var(--warning);
    }

    .btn-warning:hover {
      background-color: #ff8f00;
    }

    .btn-info {
      background-color: var(--info);
    }

    .btn-info:hover {
      background-color: #0277bd;
    }

    .btn-sm {
      padding: 0.5rem 1rem;
      font-size: 0.9rem;
    }

    .table-responsive {
      overflow-x: auto;
      margin-top: 1.5rem;
    }

    table {
      width: 100%;
      border-collapse: collapse;
    }

    th, td {
      padding: 1rem;
      text-align: right;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark th, 
    body.dark td {
      border-bottom-color: var(--border-dark);
      color: var(--text-light);
    }

    body.dark td {
      color: var(--text-gray);
    }

    th {
      background-color: var(--primary);
      color: white;
      font-weight: 500;
    }

    .actions-cell {
      display: flex;
      gap: 0.5rem;
      justify-content: flex-start;
    }

    .alert {
      padding: 1rem;
      border-radius: 6px;
      margin-bottom: 1.5rem;
      display: flex;
      align-items: center;
      gap: 0.8rem;
    }

    .alert-success {
      background-color: rgba(56, 142, 60, 0.15);
      color: var(--success);
      border: 1px solid var(--success);
    }

    .alert-error {
      background-color: rgba(211, 47, 47, 0.15);
      color: var(--danger);
      border: 1px solid var(--danger);
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }

    .stat-card {
      background-color: var(--card-light);
      padding: 1.5rem;
      border-radius: 10px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
    }

    body.dark .stat-card {
      background-color: var(--card-dark);
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.3);
    }

    .stat-card h4 {
      margin-bottom: 0.5rem;
      color: var(--primary);
    }

    .stat-card .value {
      font-size: 2rem;
      font-weight: bold;
      margin-bottom: 0.5rem;
    }

    .stat-card .icon {
      font-size: 2.5rem;
      margin-bottom: 1rem;
      color: var(--primary);
    }

    .chart-container {
      position: relative;
      height: 300px;
      margin-bottom: 1.5rem;
    }

    .form-row {
      display: flex;
      gap: 1rem;
      margin-bottom: 1.2rem;
    }

    .form-row .form-group {
      flex: 1;
    }

    .search-container {
      display: flex;
      gap: 1rem;
      margin-bottom: 1.5rem;
    }

    .search-container .form-control {
      flex: 1;
    }

    .calculator-container {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.5rem;
      margin-top: 1.5rem;
    }

    .calculator-display {
      grid-column: span 4;
      background-color: var(--card-light);
      padding: 1rem;
      text-align: left;
      font-size: 1.5rem;
      border-radius: 6px;
      border: 1px solid var(--border-light);
      min-height: 60px;
      word-break: break-all;
    }

    body.dark .calculator-display {
      background-color: var(--card-dark);
      border-color: var(--border-dark);
    }

    .calculator-btn {
      padding: 1rem;
      font-size: 1.2rem;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      background-color: var(--primary);
      color: white;
      transition: background-color 0.2s;
    }

    .calculator-btn:hover {
      background-color: var(--primary-dark);
    }

    .calculator-btn.operator {
      background-color: var(--warning);
    }

    .calculator-btn.operator:hover {
      background-color: #ff8f00;
    }

    .calculator-btn.equals {
      background-color: var(--success);
      grid-column: span 2;
    }

    .calculator-btn.equals:hover {
      background-color: #2e7d32;
    }

    .calculator-btn.clear {
      background-color: var(--danger);
    }

    .calculator-btn.clear:hover {
      background-color: #c62828;
    }

    .signature {
      text-align: center;
      margin-top: 2rem;
      padding: 1rem;
      border-top: 1px solid var(--border-light);
      color: var(--text-gray);
      font-size: 0.9rem;
    }

    body.dark .signature {
      border-top-color: var(--border-dark);
    }

    .logo {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
    }

    @media (max-width: 768px) {
      .sidebar {
        width: 85%;
        right: -85%;
      }
      
      .sidebar.visible {
        right: 0;
      }

      .main {
        padding: 1rem;
      }

      .form-row {
        flex-direction: column;
        gap: 0;
      }

      .grid {
        grid-template-columns: 1fr;
      }
      
      .calculator-container {
        grid-template-columns: repeat(4, 1fr);
      }
    }

    @media (max-width: 480px) {
      .calculator-btn {
        padding: 0.8rem;
        font-size: 1rem;
      }
    }
  </style>
</head>
<body>
  <div class="sidebar" id="sidebar">
    <h2>
      <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MTIgNTEyIj48cGF0aCBmaWxsPSJ3aGl0ZSIgZD0iTTQ0OCAwSDY0QzI4LjcgMCAwIDI4LjcgMCA2NHYzODRjMCAzNS4zIDI4LjcgNjQgNjQgNjRoMzg0YzM1LjMgMCA2NC0yOC43IDY0LTY0VjY0YzAtMzUuMy0yOC43LTY0LTY0LTY0ek0yNTYgMTYwYzM1LjMgMCA2NCAyOC43IDY0IDY0cy0yOC43IDY0LTY0IDY0LTY0LTI4LjctNjQtNjQgMjguNy02NCA2NC02NHptODAgMTkySDE3NmMtOC44IDAtMTYtNy4yLTE2LTE2cy03LjItMTYtMTYtMTZoLTE2Yy04LjggMC0xNiA3LjItMTYgMTZ2OTZjMCA4LjggNy4yIDE2IDE2IDE2aDM2MGM4LjggMCAxNi03LjIgMTYtMTZ2LTk2YzAtOC44LTcuMi0xNi0xNi0xNnptLTk2LTk2Yy0xNy43IDAtMzIgMTQuMy0zMiAzMnMxNC4zIDMyIDMyIDMyIDMyLTE0LjMgMzItMzItMTQuMy0zMi0zMi0zMnoiLz48L3N2Zz4=" alt="Logo" class="logo">
      نظام إدارة الصيدلية
    </h2>
    <div class="sidebar-menu">
      <button id="dashboardBtn" class="active">
        <span class="material-icons">dashboard</span>
        لوحة التحكم
      </button>
      <button id="medicinesBtn">
        <span class="material-icons">medication</span>
        إدارة الأدوية
      </button>
      <button id="suppliersBtn">
        <span class="material-icons">local_shipping</span>
        الموردون
      </button>
      <button id="customersBtn">
        <span class="material-icons">people</span>
        العملاء
      </button>
      <button id="ordersBtn">
        <span class="material-icons">receipt</span>
        الفواتير
      </button>
      <button id="prescriptionsBtn">
        <span class="material-icons">description</span>
        الوصفات الطبية
      </button>
      <button id="calculatorBtn">
        <span class="material-icons">calculate</span>
        الآلة الحاسبة
      </button>
      <button id="statisticsBtn">
        <span class="material-icons">bar_chart</span>
        الإحصائيات
      </button>
      <button id="settingsBtn">
        <span class="material-icons">settings</span>
        الإعدادات
      </button>
    </div>
  </div>
  
  <div class="main" id="main">
    <div class="topbar">
      <h2 id="page-title">
        <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA1MTIgNTEyIj48cGF0aCBmaWxsPSIjMTk3NmQyIiBkPSJNNDQ4IDBINjRDMjguNyAwIDAgMjguNyAwIDY0djM4NGMwIDM1LjMgMjguNyA2NCA2NCA2NGgzODRjMzUuMyAwIDY0LTI4LjcgNjQtNjRWNjRjMC0zNS4zLTI4LjctNjQtNjQtNjR6TTI1NiAxNjBjMzUuMyAwIDY0IDI4LjcgNjQgNjRzLTI4LjcgNjQtNjQgNjQtNjQtMjguNy02NC02NCAyOC43LTY0IDY0LTY0em04MCAxOTJIMTc2Yy04LjggMC0xNi03LjItMTYtMTZzLTcuMi0xNi0xNi0xNmgtMTZjLTguOCAwLTE2IDcuMi0xNiAxNnY5NmMwIDguOCA3LjIgMTYgMTYgMTZoMzYwYzguOCAwIDE2LTcuMiAxNi0xNnYtOTZjMC04LjgtNy4yLTE2LTE2LTE2em0tOTYtOTZjLTE3LjcgMC0zMiAxNC4zLTMyIDMyczE0LjMgMzIgMzIgMzIgMzItMTQuMyAzMi0zMi0xNC4zLTMyLTMyLTMyeiIvPjwvc3ZnPg==" alt="Logo" class="logo">
        لوحة التحكم
      </h2>
      <div class="topbar-actions">
        <button class="topbar-action-btn" id="toggleSidebar">
          <span class="material-icons">menu</span>
        </button>
        <button class="topbar-action-btn" id="toggleTheme">
          <span class="material-icons">dark_mode</span>
        </button>
      </div>
    </div>

    <div id="alertContainer"></div>

    <div id="dashboard" class="page active">
      <div class="card">
        <h3>مرحبا بك في نظام إدارة الصيدلية</h3>
        <p>يمكنك استخدام القائمة الجانبية للوصول إلى مختلف أجزاء النظام</p>
      </div>

      <div class="grid">
        <div class="stat-card">
          <span class="material-icons icon">medication</span>
          <h4>إجمالي الأدوية</h4>
          <div class="value" id="totalMedicines">0</div>
        </div>
        <div class="stat-card">
          <span class="material-icons icon">people</span>
          <h4>إجمالي العملاء</h4>
          <div class="value" id="totalCustomers">0</div>
        </div>
        <div class="stat-card">
          <span class="material-icons icon">receipt</span>
          <h4>إجمالي الفواتير</h4>
          <div class="value" id="totalOrders">0</div>
        </div>
        <div class="stat-card">
          <span class="material-icons icon">attach_money</span>
          <h4>إجمالي المبيعات</h4>
          <div class="value" id="totalSales">0 ج.م</div>
        </div>
      </div>

      <div class="card">
        <h3>أحدث الفواتير</h3>
        <div class="table-responsive">
          <table id="recentOrdersTable">
            <thead>
              <tr>
                <th>رقم الفاتورة</th>
                <th>العميل</th>
                <th>التاريخ</th>
                <th>المبلغ</th>
                <th>الحالة</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="medicines" class="page">
      <div class="card">
        <h3>إضافة دواء جديد</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="medName">اسم الدواء</label>
            <input type="text" id="medName" class="form-control" placeholder="أدخل اسم الدواء">
          </div>
          <div class="form-group">
            <label for="medScientificName">الاسم العلمي</label>
            <input type="text" id="medScientificName" class="form-control" placeholder="أدخل الاسم العلمي">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="medPrice">سعر البيع</label>
            <input type="number" id="medPrice" class="form-control" placeholder="أدخل سعر البيع">
          </div>
          <div class="form-group">
            <label for="medPurchasePrice">سعر الشراء</label>
            <input type="number" id="medPurchasePrice" class="form-control" placeholder="أدخل سعر الشراء">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="medQuantity">الكمية المتاحة</label>
            <input type="number" id="medQuantity" class="form-control" placeholder="أدخل الكمية المتاحة">
          </div>
          <div class="form-group">
            <label for="medExpiryDate">تاريخ الانتهاء</label>
            <input type="date" id="medExpiryDate" class="form-control">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="medCategory">الفئة</label>
            <select id="medCategory" class="form-control">
              <option value="">اختر الفئة</option>
              <option value="مضادات حيوية">مضادات حيوية</option>
              <option value="مسكنات">مسكنات</option>
              <option value="مضادات التهاب">مضادات التهاب</option>
              <option value="فيتامينات">فيتامينات</option>
              <option value="أدوية مزمنة">أدوية مزمنة</option>
              <option value="أخرى">أخرى</option>
            </select>
          </div>
          <div class="form-group">
            <label for="medSupplier">المورد</label>
            <select id="medSupplier" class="form-control">
              <option value="">اختر المورد</option>
              <!-- سيتم ملؤها بالبيانات -->
            </select>
          </div>
        </div>
        
        <div class="form-group">
          <label for="medDescription">الوصف</label>
          <textarea id="medDescription" class="form-control" rows="3" placeholder="أدخل وصف الدواء"></textarea>
        </div>
        
        <button id="addMedicineBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          إضافة دواء
        </button>
      </div>

      <div class="card">
        <h3>قائمة الأدوية</h3>
        <div class="search-container">
          <input type="text" id="searchMedicine" class="form-control" placeholder="ابحث عن دواء...">
          <button class="btn btn-info" id="searchMedicineBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        <div class="table-responsive">
          <table id="medicinesTable">
            <thead>
              <tr>
                <th>اسم الدواء</th>
                <th>الاسم العلمي</th>
                <th>السعر</th>
                <th>الكمية</th>
                <th>الفئة</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="suppliers" class="page">
      <div class="card">
        <h3>إضافة مورد جديد</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="supplierName">اسم المورد</label>
            <input type="text" id="supplierName" class="form-control" placeholder="أدخل اسم المورد">
          </div>
          <div class="form-group">
            <label for="supplierPhone">رقم الهاتف</label>
            <input type="tel" id="supplierPhone" class="form-control" placeholder="أدخل رقم الهاتف">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="supplierEmail">البريد الإلكتروني</label>
            <input type="email" id="supplierEmail" class="form-control" placeholder="أدخل البريد الإلكتروني">
          </div>
          <div class="form-group">
            <label for="supplierCompany">اسم الشركة</label>
            <input type="text" id="supplierCompany" class="form-control" placeholder="أدخل اسم الشركة">
          </div>
        </div>
        
        <div class="form-group">
          <label for="supplierAddress">العنوان</label>
          <textarea id="supplierAddress" class="form-control" rows="2" placeholder="أدخل عنوان المورد"></textarea>
        </div>
        
        <div class="form-group">
          <label for="supplierNotes">ملاحظات</label>
          <textarea id="supplierNotes" class="form-control" rows="2" placeholder="أدخل أي ملاحظات"></textarea>
        </div>
        
        <button id="addSupplierBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          إضافة مورد
        </button>
      </div>

      <div class="card">
        <h3>قائمة الموردين</h3>
        <div class="search-container">
          <input type="text" id="searchSupplier" class="form-control" placeholder="ابحث عن مورد...">
          <button class="btn btn-info" id="searchSupplierBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        <div class="table-responsive">
          <table id="suppliersTable">
            <thead>
              <tr>
                <th>اسم المورد</th>
                <th>الهاتف</th>
                <th>الشركة</th>
                <th>البريد الإلكتروني</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="customers" class="page">
      <div class="card">
        <h3>إضافة عميل جديد</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="customerName">اسم العميل</label>
            <input type="text" id="customerName" class="form-control" placeholder="أدخل اسم العميل">
          </div>
          <div class="form-group">
            <label for="customerPhone">رقم الهاتف</label>
            <input type="tel" id="customerPhone" class="form-control" placeholder="أدخل رقم الهاتف">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="customerAge">العمر</label>
            <input type="number" id="customerAge" class="form-control" placeholder="أدخل عمر العميل">
          </div>
          <div class="form-group">
            <label for="customerGender">الجنس</label>
            <select id="customerGender" class="form-control">
              <option value="ذكر">ذكر</option>
              <option value="أنثى">أنثى</option>
            </select>
          </div>
        </div>
        
        <div class="form-group">
          <label for="customerAddress">العنوان</label>
          <textarea id="customerAddress" class="form-control" rows="2" placeholder="أدخل عنوان العميل"></textarea>
        </div>
        
        <div class="form-group">
          <label for="customerMedicalHistory">التاريخ المرضي</label>
          <textarea id="customerMedicalHistory" class="form-control" rows="3" placeholder="أدخل التاريخ المرضي للعميل"></textarea>
        </div>
        
        <button id="addCustomerBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          إضافة عميل
        </button>
      </div>

      <div class="card">
        <h3>قائمة العملاء</h3>
        <div class="search-container">
          <input type="text" id="searchCustomer" class="form-control" placeholder="ابحث عن عميل...">
          <button class="btn btn-info" id="searchCustomerBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        <div class="table-responsive">
          <table id="customersTable">
            <thead>
              <tr>
                <th>اسم العميل</th>
                <th>الهاتف</th>
                <th>العمر</th>
                <th>الجنس</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="orders" class="page">
      <div class="card">
        <h3>إنشاء فاتورة جديدة</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="orderCustomer">العميل</label>
            <select id="orderCustomer" class="form-control">
              <option value="">اختر العميل</option>
              <!-- سيتم ملؤها بالبيانات -->
            </select>
          </div>
          <div class="form-group">
            <label for="orderDate">تاريخ الفاتورة</label>
            <input type="date" id="orderDate" class="form-control" value="">
          </div>
        </div>
        
        <div class="form-group">
          <label for="orderNotes">ملاحظات</label>
          <textarea id="orderNotes" class="form-control" rows="2" placeholder="أدخل أي ملاحظات"></textarea>
        </div>
        
        <h4>أضف أدوية للفاتورة</h4>
        <div class="form-row">
          <div class="form-group" style="flex: 2;">
            <label for="orderMedicine">الدواء</label>
            <select id="orderMedicine" class="form-control">
              <option value="">اختر الدواء</option>
              <!-- سيتم ملؤها بالبيانات -->
            </select>
          </div>
          <div class="form-group">
            <label for="orderQuantity">الكمية</label>
            <input type="number" id="orderQuantity" class="form-control" value="1" min="1">
          </div>
          <div class="form-group" style="align-self: flex-end;">
            <button id="addToOrderBtn" class="btn btn-info">
              <span class="material-icons">add</span>
              إضافة
            </button>
          </div>
        </div>
        
        <div class="table-responsive">
          <table id="orderItemsTable">
            <thead>
              <tr>
                <th>الدواء</th>
                <th>السعر</th>
                <th>الكمية</th>
                <th>المجموع</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" style="text-align: left; font-weight: bold;">الإجمالي:</td>
                <td id="orderTotal" style="font-weight: bold;">0 ج.م</td>
                <td></td>
              </tr>
            </tfoot>
          </table>
        </div>
        
        <button id="saveOrderBtn" class="btn btn-success" style="margin-top: 1.5rem;">
          <span class="material-icons">save</span>
          حفظ الفاتورة
        </button>
      </div>

      <div class="card">
        <h3>سجل الفواتير</h3>
        <div class="search-container">
          <input type="text" id="searchOrder" class="form-control" placeholder="ابحث عن فاتورة...">
          <button class="btn btn-info" id="searchOrderBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        <div class="table-responsive">
          <table id="ordersTable">
            <thead>
              <tr>
                <th>رقم الفاتورة</th>
                <th>العميل</th>
                <th>التاريخ</th>
                <th>المبلغ</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="prescriptions" class="page">
      <div class="card">
        <h3>إضافة وصفة طبية</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="prescriptionCustomer">العميل</label>
            <select id="prescriptionCustomer" class="form-control">
              <option value="">اختر العميل</option>
              <!-- سيتم ملؤها بالبيانات -->
            </select>
          </div>
          <div class="form-group">
            <label for="prescriptionDate">تاريخ الوصفة</label>
            <input type="date" id="prescriptionDate" class="form-control" value="">
          </div>
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="prescriptionDoctor">اسم الطبيب</label>
            <input type="text" id="prescriptionDoctor" class="form-control" placeholder="أدخل اسم الطبيب">
          </div>
          <div class="form-group">
            <label for="prescriptionDiagnosis">التشخيص</label>
            <input type="text" id="prescriptionDiagnosis" class="form-control" placeholder="أدخل التشخيص">
          </div>
        </div>
        
        <div class="form-group">
          <label for="prescriptionNotes">ملاحظات</label>
          <textarea id="prescriptionNotes" class="form-control" rows="2" placeholder="أدخل أي ملاحظات"></textarea>
        </div>
        
        <h4>أدوية الوصفة</h4>
        <div class="form-row">
          <div class="form-group" style="flex: 2;">
            <label for="prescriptionMedicine">الدواء</label>
            <select id="prescriptionMedicine" class="form-control">
              <option value="">اختر الدواء</option>
              <!-- سيتم ملؤها بالبيانات -->
            </select>
          </div>
          <div class="form-group">
            <label for="prescriptionDosage">الجرعة</label>
            <input type="text" id="prescriptionDosage" class="form-control" placeholder="جرعة الدواء">
          </div>
          <div class="form-group">
            <label for="prescriptionDuration">المدة</label>
            <input type="text" id="prescriptionDuration" class="form-control" placeholder="مدة الاستخدام">
          </div>
          <div class="form-group" style="align-self: flex-end;">
            <button id="addToPrescriptionBtn" class="btn btn-info">
              <span class="material-icons">add</span>
              إضافة
            </button>
          </div>
        </div>
        
        <div class="table-responsive">
          <table id="prescriptionItemsTable">
            <thead>
              <tr>
                <th>الدواء</th>
                <th>الجرعة</th>
                <th>المدة</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
        
        <button id="savePrescriptionBtn" class="btn btn-success" style="margin-top: 1.5rem;">
          <span class="material-icons">save</span>
          حفظ الوصفة
        </button>
      </div>

      <div class="card">
        <h3>سجل الوصفات الطبية</h3>
        <div class="search-container">
          <input type="text" id="searchPrescription" class="form-control" placeholder="ابحث عن وصفة طبية...">
          <button class="btn btn-info" id="searchPrescriptionBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        <div class="table-responsive">
          <table id="prescriptionsTable">
            <thead>
              <tr>
                <th>رقم الوصفة</th>
                <th>العميل</th>
                <th>الطبيب</th>
                <th>التاريخ</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody>
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <div id="calculator" class="page">
      <div class="card">
        <h3>آلة حاسبة</h3>
        <div class="calculator-display" id="calculatorDisplay">0</div>
        <div class="calculator-container">
          <button class="calculator-btn clear" onclick="clearCalculator()">C</button>
          <button class="calculator-btn operator" onclick="appendToCalculator('/')">/</button>
          <button class="calculator-btn operator" onclick="appendToCalculator('*')">×</button>
          <button class="calculator-btn operator" onclick="appendToCalculator('-')">-</button>
          
          <button class="calculator-btn" onclick="appendToCalculator('7')">7</button>
          <button class="calculator-btn" onclick="appendToCalculator('8')">8</button>
          <button class="calculator-btn" onclick="appendToCalculator('9')">9</button>
          <button class="calculator-btn operator" onclick="appendToCalculator('+')">+</button>
          
          <button class="calculator-btn" onclick="appendToCalculator('4')">4</button>
          <button class="calculator-btn" onclick="appendToCalculator('5')">5</button>
          <button class="calculator-btn" onclick="appendToCalculator('6')">6</button>
          <button class="calculator-btn" onclick="appendToCalculator('.')">.</button>
          
          <button class="calculator-btn" onclick="appendToCalculator('1')">1</button>
          <button class="calculator-btn" onclick="appendToCalculator('2')">2</button>
          <button class="calculator-btn" onclick="appendToCalculator('3')">3</button>
          <button class="calculator-btn equals" onclick="calculateResult()">=</button>
          
          <button class="calculator-btn" onclick="appendToCalculator('0')">0</button>
          <button class="calculator-btn" onclick="backspaceCalculator()">←</button>
        </div>
      </div>
    </div>

    <div id="statistics" class="page">
      <div class="card">
        <h3>إحصائيات المبيعات</h3>
        <div class="form-row">
          <div class="form-group">
            <label for="statsFromDate">من تاريخ</label>
            <input type="date" id="statsFromDate" class="form-control">
          </div>
          <div class="form-group">
            <label for="statsToDate">إلى تاريخ</label>
            <input type="date" id="statsToDate" class="form-control">
          </div>
          <div class="form-group" style="align-self: flex-end;">
            <button id="generateStatsBtn" class="btn btn-primary">
              <span class="material-icons">refresh</span>
              توليد الإحصائيات
            </button>
          </div>
        </div>
        
        <div class="grid">
          <div class="stat-card">
            <span class="material-icons icon">attach_money</span>
            <h4>إجمالي المبيعات</h4>
            <div class="value" id="statsTotalSales">0 ج.م</div>
          </div>
          <div class="stat-card">
            <span class="material-icons icon">receipt</span>
            <h4>عدد الفواتير</h4>
            <div class="value" id="statsTotalOrders">0</div>
          </div>
          <div class="stat-card">
            <span class="material-icons icon">medication</span>
            <h4>أكثر الأدوية مبيعاً</h4>
            <div class="value" id="statsTopMedicine">-</div>
          </div>
          <div class="stat-card">
            <span class="material-icons icon">person</span>
            <h4>أفضل العملاء</h4>
            <div class="value" id="statsTopCustomer">-</div>
          </div>
        </div>
        
        <div class="chart-container">
          <canvas id="salesChart"></canvas>
        </div>
        
        <div class="chart-container">
          <canvas id="medicinesChart"></canvas>
        </div>
      </div>
    </div>

    <div id="settings" class="page">
      <div class="card">
        <h3>إعدادات النظام</h3>
        <div class="form-group">
          <label for="pharmacyName">اسم الصيدلية</label>
          <input type="text" id="pharmacyName" class="form-control" placeholder="أدخل اسم الصيدلية">
        </div>
        
        <div class="form-row">
          <div class="form-group">
            <label for="pharmacyPhone">هاتف الصيدلية</label>
            <input type="tel" id="pharmacyPhone" class="form-control" placeholder="أدخل هاتف الصيدلية">
          </div>
          <div class="form-group">
            <label for="pharmacyEmail">بريد الصيدلية</label>
            <input type="email" id="pharmacyEmail" class="form-control" placeholder="أدخل بريد الصيدلية">
          </div>
        </div>
        
        <div class="form-group">
          <label for="pharmacyAddress">عنوان الصيدلية</label>
          <textarea id="pharmacyAddress" class="form-control" rows="2" placeholder="أدخل عنوان الصيدلية"></textarea>
        </div>
        
        <div class="form-group">
          <label for="pharmacyLogo">شعار الصيدلية</label>
          <input type="file" id="pharmacyLogo" class="form-control">
        </div>
        
        <h4>إعدادات الفواتير</h4>
        <div class="form-row">
          <div class="form-group">
            <label for="invoicePrefix">بادئة رقم الفاتورة</label>
            <input type="text" id="invoicePrefix" class="form-control" placeholder="مثال: INV-">
          </div>
          <div class="form-group">
            <label for="invoiceStartingNumber">رقم بداية الفاتورة</label>
            <input type="number" id="invoiceStartingNumber" class="form-control" placeholder="مثال: 1001">
          </div>
        </div>
        
        <div class="form-group">
          <label for="invoiceFooter">تذييل الفاتورة</label>
          <textarea id="invoiceFooter" class="form-control" rows="3" placeholder="أدخل نص تذييل الفاتورة"></textarea>
        </div>
        
        <button id="saveSettingsBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          حفظ الإعدادات
        </button>
      </div>
      
      <div class="card">
        <h3>نسخ احتياطي واستعادة</h3>
        <div class="form-row">
          <div class="form-group">
            <button id="backupBtn" class="btn btn-info">
              <span class="material-icons">backup</span>
              إنشاء نسخة احتياطية
            </button>
          </div>
          <div class="form-group">
            <label for="restoreFile">استعادة النسخة الاحتياطية</label>
            <input type="file" id="restoreFile" class="form-control">
          </div>
        </div>
        <button id="restoreBtn" class="btn btn-warning">
          <span class="material-icons">restore</span>
          استعادة البيانات
        </button>
      </div>
    </div>

    <div class="signature">
      <p>نظام إدارة الصيدلية &copy; <span id="currentYear"></span></p>
      <p>تمت البرمجة بواسطة عمر بابكر - Omer Bk</p>
    </div>
  </div>

  <script>
    // بيانات التطبيق
    let medicines = [];
    let suppliers = [];
    let customers = [];
    let orders = [];
    let prescriptions = [];
    let settings = {};
    let currentOrderItems = [];
    let currentPrescriptionItems = [];
    let calculatorValue = '0';
    let calculatorPreviousValue = '0';
    let calculatorOperation = null;
    
    // تهيئة التطبيق
    document.addEventListener('DOMContentLoaded', function() {
      // تعيين السنة الحالية
      document.getElementById('currentYear').textContent = new Date().getFullYear();
      
      // تعيين تاريخ اليوم لحقول التاريخ
      const today = new Date().toISOString().split('T')[0];
      document.getElementById('orderDate').value = today;
      document.getElementById('prescriptionDate').value = today;
      document.getElementById('statsFromDate').value = today;
      document.getElementById('statsToDate').value = today;
      
      // تعيين معالج الأحداث للقائمة الجانبية
      document.getElementById('toggleSidebar').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('visible');
      });

      // تعيين معالج الأحداث لتبديل الوضع الليلي
      document.getElementById('toggleTheme').addEventListener('click', function() {
        document.body.classList.toggle('dark');
      });

      // تعيين معالج الأحداث لأزرار التنقل
      const navButtons = {
        dashboard: document.getElementById('dashboardBtn'),
        medicines: document.getElementById('medicinesBtn'),
        suppliers: document.getElementById('suppliersBtn'),
        customers: document.getElementById('customersBtn'),
        orders: document.getElementById('ordersBtn'),
        prescriptions: document.getElementById('prescriptionsBtn'),
        calculator: document.getElementById('calculatorBtn'),
        statistics: document.getElementById('statisticsBtn'),
        settings: document.getElementById('settingsBtn')
      };

      const pages = {
        dashboard: document.getElementById('dashboard'),
        medicines: document.getElementById('medicines'),
        suppliers: document.getElementById('suppliers'),
        customers: document.getElementById('customers'),
        orders: document.getElementById('orders'),
        prescriptions: document.getElementById('prescriptions'),
        calculator: document.getElementById('calculator'),
        statistics: document.getElementById('statistics'),
        settings: document.getElementById('settings')
      };

      Object.keys(navButtons).forEach(key => {
        navButtons[key].addEventListener('click', function() {
          // إزالة النشاط من جميع الأزرار
          Object.values(navButtons).forEach(btn => {
            btn.classList.remove('active');
          });
          
          // إضافة النشاط للزر المحدد
          this.classList.add('active');
          
          // إخفاء جميع الصفحات
          Object.values(pages).forEach(page => {
            page.classList.remove('active');
          });
          
          // إظهار الصفحة المحددة
          pages[key].classList.add('active');
          
          // تحديث عنوان الصفحة
          document.getElementById('page-title').textContent = this.textContent.trim();
          
          // إخفاء القائمة الجانبية بعد اختيار الصفحة (للهواتف)
          if (window.innerWidth <= 768) {
            document.getElementById('sidebar').classList.remove('visible');
          }
          
          // إذا كانت صفحة الإحصائيات، قم بتحديث الرسوم البيانية
          if (key === 'statistics') {
            updateCharts();
          }
        });
      });

      // معالج حدث إضافة دواء جديد
      document.getElementById('addMedicineBtn').addEventListener('click', addMedicine);
      
      // معالج حدث إضافة مورد جديد
      document.getElementById('addSupplierBtn').addEventListener('click', addSupplier);
      
      // معالج حدث إضافة عميل جديد
      document.getElementById('addCustomerBtn').addEventListener('click', addCustomer);
      
      // معالج حدث إضافة عنصر للفاتورة
      document.getElementById('addToOrderBtn').addEventListener('click', addToOrder);
      
      // معالج حدث حفظ الفاتورة
      document.getElementById('saveOrderBtn').addEventListener('click', saveOrder);
      
      // معالج حدث إضافة عنصر للوصفة
      document.getElementById('addToPrescriptionBtn').addEventListener('click', addToPrescription);
      
      // معالج حدث حفظ الوصفة
      document.getElementById('savePrescriptionBtn').addEventListener('click', savePrescription);
      
      // معالج حدث حفظ الإعدادات
      document.getElementById('saveSettingsBtn').addEventListener('click', saveSettings);
      
      // معالج حدث إنشاء نسخة احتياطية
      document.getElementById('backupBtn').addEventListener('click', createBackup);
      
      // معالج حدث استعادة النسخة الاحتياطية
      document.getElementById('restoreBtn').addEventListener('click', restoreBackup);
      
      // معالج حدث توليد الإحصائيات
      document.getElementById('generateStatsBtn').addEventListener('click', generateStatistics);
      
      // معالج أحداث البحث
      document.getElementById('searchMedicineBtn').addEventListener('click', searchMedicines);
      document.getElementById('searchSupplierBtn').addEventListener('click', searchSuppliers);
      document.getElementById('searchCustomerBtn').addEventListener('click', searchCustomers);
      document.getElementById('searchOrderBtn').addEventListener('click', searchOrders);
      document.getElementById('searchPrescriptionBtn').addEventListener('click', searchPrescriptions);

      // تحميل البيانات الأولية
      loadSampleData();
      
      // تحديث الإحصائيات على لوحة التحكم
      updateDashboardStats();
      
      // تحديث شاشة الآلة الحاسبة
      updateCalculatorDisplay();
    });

    // ========== دوال الأعمال ==========
    
    function addMedicine() {
      const name = document.getElementById('medName').value;
      const scientificName = document.getElementById('medScientificName').value;
      const price = document.getElementById('medPrice').value;
      const purchasePrice = document.getElementById('medPurchasePrice').value;
      const quantity = document.getElementById('medQuantity').value;
      const expiryDate = document.getElementById('medExpiryDate').value;
      const category = document.getElementById('medCategory').value;
      const supplier = document.getElementById('medSupplier').value;
      const description = document.getElementById('medDescription').value;
      
      if (!name || !price || !quantity || !category) {
        showAlert('error', 'الرجاء إدخال جميع البيانات المطلوبة');
        return;
      }
      
      const newMedicine = {
        id: Date.now(),
        name: name,
        scientificName: scientificName,
        price: parseFloat(price),
        purchasePrice: parseFloat(purchasePrice) || 0,
        quantity: parseInt(quantity),
        expiryDate: expiryDate,
        category: category,
        supplier: supplier,
        description: description
      };
      
      medicines.push(newMedicine);
      updateMedicinesTable();
      updateSelectOptions();
      
      // إظهار رسالة نجاح
      showAlert('success', 'تم إضافة الدواء بنجاح');
      
      // مسح الحقول
      document.getElementById('medName').value = '';
      document.getElementById('medScientificName').value = '';
      document.getElementById('medPrice').value = '';
      document.getElementById('medPurchasePrice').value = '';
      document.getElementById('medQuantity').value = '';
      document.getElementById('medExpiryDate').value = '';
      document.getElementById('medCategory').value = '';
      document.getElementById('medSupplier').value = '';
      document.getElementById('medDescription').value = '';
      
      // تحديث الإحصائيات
      updateDashboardStats();
    }
    
    function addSupplier() {
      const name = document.getElementById('supplierName').value;
      const phone = document.getElementById('supplierPhone').value;
      const email = document.getElementById('supplierEmail').value;
      const company = document.getElementById('supplierCompany').value;
      const address = document.getElementById('supplierAddress').value;
      const notes = document.getElementById('supplierNotes').value;
      
      if (!name || !phone) {
        showAlert('error', 'الرجاء إدخال اسم المورد ورقم الهاتف');
        return;
      }
      
      const newSupplier = {
        id: Date.now(),
        name: name,
        phone: phone,
        email: email,
        company: company,
        address: address,
        notes: notes
      };
      
      suppliers.push(newSupplier);
      updateSuppliersTable();
      updateSelectOptions();
      
      showAlert('success', 'تم إضافة المورد بنجاح');
      
      // مسح الحقول
      document.getElementById('supplierName').value = '';
      document.getElementById('supplierPhone').value = '';
      document.getElementById('supplierEmail').value = '';
      document.getElementById('supplierCompany').value = '';
      document.getElementById('supplierAddress').value = '';
      document.getElementById('supplierNotes').value = '';
      
      // تحديث الإحصائيات
      updateDashboardStats();
    }
    
    function addCustomer() {
      const name = document.getElementById('customerName').value;
      const phone = document.getElementById('customerPhone').value;
      const age = document.getElementById('customerAge').value;
      const gender = document.getElementById('customerGender').value;
      const address = document.getElementById('customerAddress').value;
      const medicalHistory = document.getElementById('customerMedicalHistory').value;
      
      if (!name || !phone) {
        showAlert('error', 'الرجاء إدخال اسم العميل ورقم الهاتف');
        return;
      }
      
      const newCustomer = {
        id: Date.now(),
        name: name,
        phone: phone,
        age: age,
        gender: gender,
        address: address,
        medicalHistory: medicalHistory
      };
      
      customers.push(newCustomer);
      updateCustomersTable();
      updateSelectOptions();
      
      showAlert('success', 'تم إضافة العميل بنجاح');
      
      // مسح الحقول
      document.getElementById('customerName').value = '';
      document.getElementById('customerPhone').value = '';
      document.getElementById('customerAge').value = '';
      document.getElementById('customerGender').value = 'ذكر';
      document.getElementById('customerAddress').value = '';
      document.getElementById('customerMedicalHistory').value = '';
      
      // تحديث الإحصائيات
      updateDashboardStats();
    }
    
    function addToOrder() {
      const medicineId = parseInt(document.getElementById('orderMedicine').value);
      const quantity = parseInt(document.getElementById('orderQuantity').value);
      
      if (!medicineId || !quantity || quantity < 1) {
        showAlert('error', 'الرجاء اختيار دواء وكمية صحيحة');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showAlert('error', 'الدواء المحدد غير موجود');
        return;
      }
      
      if (medicine.quantity < quantity) {
        showAlert('error', `الكمية المتاحة غير كافية (المتاح: ${medicine.quantity})`);
        return;
      }
      
      // التحقق من وجود الدواء مسبقاً في الفاتورة
      const existingItem = currentOrderItems.find(item => item.medicineId === medicineId);
      if (existingItem) {
        existingItem.quantity += quantity;
      } else {
        currentOrderItems.push({
          medicineId: medicineId,
          name: medicine.name,
          price: medicine.price,
          quantity: quantity
        });
      }
      
      updateOrderItemsTable();
      
      // مسح حقل الكمية
      document.getElementById('orderQuantity').value = '1';
    }
    
    function saveOrder() {
      const customerId = parseInt(document.getElementById('orderCustomer').value);
      const date = document.getElementById('orderDate').value;
      const notes = document.getElementById('orderNotes').value;
      
      if (!customerId) {
        showAlert('error', 'الرجاء اختيار عميل');
        return;
      }
      
      if (currentOrderItems.length === 0) {
        showAlert('error', 'الرجاء إضافة أدوية للفاتورة');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showAlert('error', 'العميل المحدد غير موجود');
        return;
      }
      
      // حساب الإجمالي
      const total = currentOrderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
      
      const newOrder = {
        id: Date.now(),
        customerId: customerId,
        customerName: customer.name,
        date: date,
        items: [...currentOrderItems],
        total: total,
        notes: notes
      };
      
      orders.push(newOrder);
      
      // تحديث كمية الأدوية في المخزون
      currentOrderItems.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          medicine.quantity -= item.quantity;
        }
      });
      
      // مسح الفاتورة الحالية
      currentOrderItems = [];
      updateOrderItemsTable();
      
      // مسح الحقول
      document.getElementById('orderCustomer').value = '';
      document.getElementById('orderDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('orderNotes').value = '';
      
      // تحديث الجداول
      updateOrdersTable();
      updateMedicinesTable();
      
      showAlert('success', 'تم حفظ الفاتورة بنجاح');
      
      // تحديث الإحصائيات
      updateDashboardStats();
    }
    
    function addToPrescription() {
      const medicineId = parseInt(document.getElementById('prescriptionMedicine').value);
      const dosage = document.getElementById('prescriptionDosage').value;
      const duration = document.getElementById('prescriptionDuration').value;
      
      if (!medicineId || !dosage || !duration) {
        showAlert('error', 'الرجاء إدخال جميع بيانات الدواء');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showAlert('error', 'الدواء المحدد غير موجود');
        return;
      }
      
      currentPrescriptionItems.push({
        medicineId: medicineId,
        name: medicine.name,
        dosage: dosage,
        duration: duration
      });
      
      updatePrescriptionItemsTable();
      
      // مسح الحقول
      document.getElementById('prescriptionDosage').value = '';
      document.getElementById('prescriptionDuration').value = '';
    }
    
    function savePrescription() {
      const customerId = parseInt(document.getElementById('prescriptionCustomer').value);
      const date = document.getElementById('prescriptionDate').value;
      const doctor = document.getElementById('prescriptionDoctor').value;
      const diagnosis = document.getElementById('prescriptionDiagnosis').value;
      const notes = document.getElementById('prescriptionNotes').value;
      
      if (!customerId) {
        showAlert('error', 'الرجاء اختيار عميل');
        return;
      }
      
      if (!doctor) {
        showAlert('error', 'الرجاء إدخال اسم الطبيب');
        return;
      }
      
      if (currentPrescriptionItems.length === 0) {
        showAlert('error', 'الرجاء إضافة أدوية للوصفة');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showAlert('error', 'العميل المحدد غير موجود');
        return;
      }
      
      const newPrescription = {
        id: Date.now(),
        customerId: customerId,
        customerName: customer.name,
        date: date,
        doctor: doctor,
        diagnosis: diagnosis,
        items: [...currentPrescriptionItems],
        notes: notes
      };
      
      prescriptions.push(newPrescription);
      
      // مسح الوصفة الحالية
      currentPrescriptionItems = [];
      updatePrescriptionItemsTable();
      
      // مسح الحقول
      document.getElementById('prescriptionCustomer').value = '';
      document.getElementById('prescriptionDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('prescriptionDoctor').value = '';
      document.getElementById('prescriptionDiagnosis').value = '';
      document.getElementById('prescriptionNotes').value = '';
      
      // تحديث الجداول
      updatePrescriptionsTable();
      
      showAlert('success', 'تم حفظ الوصفة الطبية بنجاح');
    }
    
    function saveSettings() {
      const name = document.getElementById('pharmacyName').value;
      const phone = document.getElementById('pharmacyPhone').value;
      const email = document.getElementById('pharmacyEmail').value;
      const address = document.getElementById('pharmacyAddress').value;
      const invoicePrefix = document.getElementById('invoicePrefix').value;
      const invoiceStartingNumber = document.getElementById('invoiceStartingNumber').value;
      const invoiceFooter = document.getElementById('invoiceFooter').value;
      
      settings = {
        pharmacyName: name,
        pharmacyPhone: phone,
        pharmacyEmail: email,
        pharmacyAddress: address,
        invoicePrefix: invoicePrefix,
        invoiceStartingNumber: invoiceStartingNumber,
        invoiceFooter: invoiceFooter
      };
      
      // يمكن هنا إضافة حفظ الإعدادات في localStorage أو قاعدة البيانات
      localStorage.setItem('pharmacySettings', JSON.stringify(settings));
      
      showAlert('success', 'تم حفظ الإعدادات بنجاح');
    }
    
    function createBackup() {
      const data = {
        medicines: medicines,
        suppliers: suppliers,
        customers: customers,
        orders: orders,
        prescriptions: prescriptions,
        settings: settings,
        timestamp: new Date().toISOString()
      };
      
      const blob = new Blob([JSON.stringify(data)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `pharmacy-backup-${new Date().toISOString().split('T')[0]}.json`;
      a.click();
      URL.revokeObjectURL(url);
      
      showAlert('success', 'تم إنشاء نسخة احتياطية بنجاح');
    }
    
    function restoreBackup() {
      const fileInput = document.getElementById('restoreFile');
      if (fileInput.files.length === 0) {
        showAlert('error', 'الرجاء اختيار ملف النسخة الاحتياطية');
        return;
      }
      
      const file = fileInput.files[0];
      const reader = new FileReader();
      
      reader.onload = function(e) {
        try {
          const data = JSON.parse(e.target.result);
          
          if (confirm('هل أنت متأكد من استعادة النسخة الاحتياطية؟ سيتم استبدال جميع البيانات الحالية.')) {
            medicines = data.medicines || [];
            suppliers = data.suppliers || [];
            customers = data.customers || [];
            orders = data.orders || [];
            prescriptions = data.prescriptions || [];
            settings = data.settings || {};
            
            // تحديث جميع الجداول والعناصر
            updateMedicinesTable();
            updateSuppliersTable();
            updateCustomersTable();
            updateOrdersTable();
            updatePrescriptionsTable();
            updateSelectOptions();
            loadSettings();
            
            showAlert('success', 'تم استعادة النسخة الاحتياطية بنجاح');
          }
        } catch (error) {
          showAlert('error', 'خطأ في قراءة ملف النسخة الاحتياطية: ' + error.message);
        }
      };
      
      reader.readAsText(file);
    }
    
    function generateStatistics() {
      const fromDate = document.getElementById('statsFromDate').value;
      const toDate = document.getElementById('statsToDate').value;
      
      if (!fromDate || !toDate) {
        showAlert('error', 'الرجاء تحديد تاريخ البداية والنهاية');
        return;
      }
      
      // فلترة الفواتير حسب النطاق الزمني
      const filteredOrders = orders.filter(order => {
        return order.date >= fromDate && order.date <= toDate;
      });
      
      // حساب إجمالي المبيعات
      const totalSales = filteredOrders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('statsTotalSales').textContent = totalSales.toFixed(2) + ' ج.س';
      
      // عدد الفواتير
      document.getElementById('statsTotalOrders').textContent = filteredOrders.length;
      
      // أكثر الأدوية مبيعاً
      const medicineSales = {};
      filteredOrders.forEach(order => {
        order.items.forEach(item => {
          if (medicineSales[item.medicineId]) {
            medicineSales[item.medicineId].quantity += item.quantity;
            medicineSales[item.medicineId].total += item.price * item.quantity;
          } else {
            medicineSales[item.medicineId] = {
              name: item.name,
              quantity: item.quantity,
              total: item.price * item.quantity
            };
          }
        });
      });
      
      let topMedicine = '-';
      if (Object.keys(medicineSales).length > 0) {
        const sorted = Object.entries(medicineSales).sort((a, b) => b[1].quantity - a[1].quantity);
        topMedicine = `${sorted[0][1].name} (${sorted[0][1].quantity} وحدة)`;
      }
      document.getElementById('statsTopMedicine').textContent = topMedicine;
      
      // أفضل العملاء
      const customerSales = {};
      filteredOrders.forEach(order => {
        if (customerSales[order.customerId]) {
          customerSales[order.customerId].count++;
          customerSales[order.customerId].total += order.total;
        } else {
          customerSales[order.customerId] = {
            name: order.customerName,
            count: 1,
            total: order.total
          };
        }
      });
      
      let topCustomer = '-';
      if (Object.keys(customerSales).length > 0) {
        const sorted = Object.entries(customerSales).sort((a, b) => b[1].total - a[1].total);
        topCustomer = `${sorted[0][1].name} (${sorted[0][1].total.toFixed(2)} ج.س)`;
      }
      document.getElementById('statsTopCustomer').textContent = topCustomer;
      
      // تحديث الرسوم البيانية
      updateCharts(filteredOrders, medicineSales);
    }
    
    function searchMedicines() {
      const query = document.getElementById('searchMedicine').value.toLowerCase();
      const filtered = medicines.filter(medicine => 
        medicine.name.toLowerCase().includes(query) || 
        (medicine.scientificName && medicine.scientificName.toLowerCase().includes(query)) ||
        medicine.category.toLowerCase().includes(query)
      );
      
      updateMedicinesTable(filtered);
    }
    
    function searchSuppliers() {
      const query = document.getElementById('searchSupplier').value.toLowerCase();
      const filtered = suppliers.filter(supplier => 
        supplier.name.toLowerCase().includes(query) || 
        (supplier.company && supplier.company.toLowerCase().includes(query)) ||
        (supplier.phone && supplier.phone.includes(query))
      );
      
      updateSuppliersTable(filtered);
    }
    
    function searchCustomers() {
      const query = document.getElementById('searchCustomer').value.toLowerCase();
      const filtered = customers.filter(customer => 
        customer.name.toLowerCase().includes(query) || 
        (customer.phone && customer.phone.includes(query)) ||
        (customer.address && customer.address.toLowerCase().includes(query))
      );
      
      updateCustomersTable(filtered);
    }
    
    function searchOrders() {
      const query = document.getElementById('searchOrder').value.toLowerCase();
      const filtered = orders.filter(order => 
        order.customerName.toLowerCase().includes(query) || 
        order.id.toString().includes(query) ||
        order.date.includes(query)
      );
      
      updateOrdersTable(filtered);
    }
    
    function searchPrescriptions() {
      const query = document.getElementById('searchPrescription').value.toLowerCase();
      const filtered = prescriptions.filter(prescription => 
        prescription.customerName.toLowerCase().includes(query) || 
        prescription.id.toString().includes(query) ||
        (prescription.doctor && prescription.doctor.toLowerCase().includes(query)) ||
        prescription.date.includes(query)
      );
      
      updatePrescriptionsTable(filtered);
    }
    
    // ========== دوال الآلة الحاسبة ==========
    
    function appendToCalculator(value) {
      if (calculatorValue === '0' && value !== '.') {
        calculatorValue = value;
      } else {
        calculatorValue += value;
      }
      updateCalculatorDisplay();
    }
    
    function clearCalculator() {
      calculatorValue = '0';
      calculatorPreviousValue = '0';
      calculatorOperation = null;
      updateCalculatorDisplay();
    }
    
    function backspaceCalculator() {
      if (calculatorValue.length === 1) {
        calculatorValue = '0';
      } else {
        calculatorValue = calculatorValue.slice(0, -1);
      }
      updateCalculatorDisplay();
    }
    
    function calculateResult() {
      try {
        // استبدال علامة الضرب لتفادي مشاكل التقييم
        const expression = calculatorValue.replace(/×/g, '*');
        calculatorValue = eval(expression).toString();
        updateCalculatorDisplay();
      } catch (error) {
        calculatorValue = 'خطأ';
        updateCalculatorDisplay();
        setTimeout(() => {
          calculatorValue = '0';
          updateCalculatorDisplay();
        }, 1000);
      }
    }
    
    function updateCalculatorDisplay() {
      document.getElementById('calculatorDisplay').textContent = calculatorValue;
    }
    
    // ========== دوال التحديث ==========
    
    function updateMedicinesTable(medicinesList = medicines) {
      const tbody = document.querySelector('#medicinesTable tbody');
      tbody.innerHTML = '';
      
      medicinesList.forEach(medicine => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${medicine.name}</td>
          <td>${medicine.scientificName || '-'}</td>
          <td>${medicine.price.toFixed(2)} ج.س</td>
          <td>${medicine.quantity}</td>
          <td>${medicine.category}</td>
          <td class="actions-cell">
            <button class="btn btn-sm" onclick="editMedicine(${medicine.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteMedicine(${medicine.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateSuppliersTable(suppliersList = suppliers) {
      const tbody = document.querySelector('#suppliersTable tbody');
      tbody.innerHTML = '';
      
      suppliersList.forEach(supplier => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${supplier.name}</td>
          <td>${supplier.phone}</td>
          <td>${supplier.company || '-'}</td>
          <td>${supplier.email || '-'}</td>
          <td class="actions-cell">
            <button class="btn btn-sm" onclick="editSupplier(${supplier.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteSupplier(${supplier.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateCustomersTable(customersList = customers) {
      const tbody = document.querySelector('#customersTable tbody');
      tbody.innerHTML = '';
      
      customersList.forEach(customer => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${customer.name}</td>
          <td>${customer.phone}</td>
          <td>${customer.age || '-'}</td>
          <td>${customer.gender}</td>
          <td class="actions-cell">
            <button class="btn btn-sm" onclick="editCustomer(${customer.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteCustomer(${customer.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateOrdersTable(ordersList = orders) {
      const tbody = document.querySelector('#ordersTable tbody');
      tbody.innerHTML = '';
      
      ordersList.forEach(order => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${order.id}</td>
          <td>${order.customerName}</td>
          <td>${order.date}</td>
          <td>${order.total.toFixed(2)} ج.م</td>
          <td class="actions-cell">
            <button class="btn btn-sm" onclick="viewOrder(${order.id})">
              <span class="material-icons">visibility</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteOrder(${order.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updatePrescriptionsTable(prescriptionsList = prescriptions) {
      const tbody = document.querySelector('#prescriptionsTable tbody');
      tbody.innerHTML = '';
      
      prescriptionsList.forEach(prescription => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${prescription.id}</td>
          <td>${prescription.customerName}</td>
          <td>${prescription.doctor}</td>
          <td>${prescription.date}</td>
          <td class="actions-cell">
            <button class="btn btn-sm" onclick="viewPrescription(${prescription.id})">
              <span class="material-icons">visibility</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deletePrescription(${prescription.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateOrderItemsTable() {
      const tbody = document.querySelector('#orderItemsTable tbody');
      tbody.innerHTML = '';
      
      currentOrderItems.forEach(item => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${item.name}</td>
          <td>${item.price.toFixed(2)} ج.م</td>
          <td>${item.quantity}</td>
          <td>${(item.price * item.quantity).toFixed(2)} ج.س</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-danger" onclick="removeFromOrder(${item.medicineId})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
      
      // تحديث الإجمالي
      const total = currentOrderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
      document.getElementById('orderTotal').textContent = total.toFixed(2) + ' ج.س';
    }
    
    function updatePrescriptionItemsTable() {
      const tbody = document.querySelector('#prescriptionItemsTable tbody');
      tbody.innerHTML = '';
      
      currentPrescriptionItems.forEach(item => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${item.name}</td>
          <td>${item.dosage}</td>
          <td>${item.duration}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-danger" onclick="removeFromPrescription(${item.medicineId})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateSelectOptions() {
      // تحديث خيارات الموردين في صفحة الأدوية
      const supplierSelect = document.getElementById('medSupplier');
      supplierSelect.innerHTML = '<option value="">اختر المورد</option>';
      suppliers.forEach(supplier => {
        const option = document.createElement('option');
        option.value = supplier.id;
        option.textContent = supplier.name;
        supplierSelect.appendChild(option);
      });
      
      // تحديث خيارات العملاء في صفحة الفواتير
      const customerSelect = document.getElementById('orderCustomer');
      customerSelect.innerHTML = '<option value="">اختر العميل</option>';
      customers.forEach(customer => {
        const option = document.createElement('option');
        option.value = customer.id;
        option.textContent = customer.name;
        customerSelect.appendChild(option);
      });
      
      // تحديث خيارات الأدوية في صفحة الفواتير
      const medicineOrderSelect = document.getElementById('orderMedicine');
      medicineOrderSelect.innerHTML = '<option value="">اختر الدواء</option>';
      medicines.forEach(medicine => {
        if (medicine.quantity > 0) {
          const option = document.createElement('option');
          option.value = medicine.id;
          option.textContent = `${medicine.name} (${medicine.quantity} متاح)`;
          medicineOrderSelect.appendChild(option);
        }
      });
      
      // تحديث خيارات العملاء في صفحة الوصفات
      const prescriptionCustomerSelect = document.getElementById('prescriptionCustomer');
      prescriptionCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';
      customers.forEach(customer => {
        const option = document.createElement('option');
        option.value = customer.id;
        option.textContent = customer.name;
        prescriptionCustomerSelect.appendChild(option);
      });
      
      // تحديث خيارات الأدوية في صفحة الوصفات
      const medicinePrescriptionSelect = document.getElementById('prescriptionMedicine');
      medicinePrescriptionSelect.innerHTML = '<option value="">اختر الدواء</option>';
      medicines.forEach(medicine => {
        const option = document.createElement('option');
        option.value = medicine.id;
        option.textContent = medicine.name;
        medicinePrescriptionSelect.appendChild(option);
      });
    }
    
    function updateDashboardStats() {
      document.getElementById('totalMedicines').textContent = medicines.length;
      document.getElementById('totalCustomers').textContent = customers.length;
      document.getElementById('totalOrders').textContent = orders.length;
      
      const totalSales = orders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('totalSales').textContent = totalSales.toFixed(2) + ' ج.س';
      
      // تحديث جدول أحدث الفواتير
      const tbody = document.querySelector('#recentOrdersTable tbody');
      tbody.innerHTML = '';
      
      const recentOrders = [...orders].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
      recentOrders.forEach(order => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${order.id}</td>
          <td>${order.customerName}</td>
          <td>${order.date}</td>
          <td>${order.total.toFixed(2)} ج.م</td>
          <td>${order.total > 100 ? 'كبيرة' : 'صغيرة'}</td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateCharts(filteredOrders = orders, medicineSales = {}) {
      // رسم بياني للمبيعات اليومية
      const salesCtx = document.getElementById('salesChart').getContext('2d');
      
      // تجميع المبيعات حسب التاريخ
      const salesByDate = {};
      filteredOrders.forEach(order => {
        if (salesByDate[order.date]) {
          salesByDate[order.date] += order.total;
        } else {
          salesByDate[order.date] = order.total;
        }
      });
      
      const dates = Object.keys(salesByDate).sort();
      const salesData = dates.map(date => salesByDate[date]);
      
      if (window.salesChart) {
        window.salesChart.destroy();
      }
      
      window.salesChart = new Chart(salesCtx, {
        type: 'line',
        data: {
          labels: dates,
          datasets: [{
            label: 'المبيعات اليومية',
            data: salesData,
            backgroundColor: 'rgba(25, 118, 210, 0.2)',
            borderColor: 'rgba(25, 118, 210, 1)',
            borderWidth: 2,
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top',
              rtl: true
            }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
      
      // رسم بياني لأكثر الأدوية مبيعاً
      const medicinesCtx = document.getElementById('medicinesChart').getContext('2d');
      
      const topMedicines = Object.entries(medicineSales)
        .sort((a, b) => b[1].quantity - a[1].quantity)
        .slice(0, 5);
      
      const medicineNames = topMedicines.map(item => item[1].name);
      const medicineQuantities = topMedicines.map(item => item[1].quantity);
      
      if (window.medicinesChart) {
        window.medicinesChart.destroy();
      }
      
      window.medicinesChart = new Chart(medicinesCtx, {
        type: 'bar',
        data: {
          labels: medicineNames,
          datasets: [{
            label: 'الكمية المباعة',
            data: medicineQuantities,
            backgroundColor: 'rgba(56, 142, 60, 0.7)',
            borderColor: 'rgba(56, 142, 60, 1)',
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: {
              position: 'top',
              rtl: true
            }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    }
    
    function loadSettings() {
      const savedSettings = localStorage.getItem('pharmacySettings');
      if (savedSettings) {
        settings = JSON.parse(savedSettings);
        
        document.getElementById('pharmacyName').value = settings.pharmacyName || '';
        document.getElementById('pharmacyPhone').value = settings.pharmacyPhone || '';
        document.getElementById('pharmacyEmail').value = settings.pharmacyEmail || '';
        document.getElementById('pharmacyAddress').value = settings.pharmacyAddress || '';
        document.getElementById('invoicePrefix').value = settings.invoicePrefix || '';
        document.getElementById('invoiceStartingNumber').value = settings.invoiceStartingNumber || '';
        document.getElementById('invoiceFooter').value = settings.invoiceFooter || '';
      }
    }
    
    function loadSampleData() {
      // بيانات عينة للأدوية
      medicines = [
        { id: 1, name: 'بانادول', scientificName: 'Paracetamol', price: 10.50, purchasePrice: 8.00, quantity: 100, expiryDate: '2024-12-31', category: 'مسكنات', supplier: 1, description: 'مسكن للألم وخافض للحرارة' },
        { id: 2, name: 'أموكسيسيلين', scientificName: 'Amoxicillin', price: 15.75, purchasePrice: 12.00, quantity: 50, expiryDate: '2024-10-15', category: 'مضادات حيوية', supplier: 2, description: 'مضاد حيوي واسع المدى' },
        { id: 3, name: 'فولتارين', scientificName: 'Diclofenac', price: 12.00, purchasePrice: 9.50, quantity: 75, expiryDate: '2025-03-20', category: 'مضادات التهاب', supplier: 1, description: 'مضاد التهاب غير ستيرويدي' },
        { id: 4, name: 'فيتامين سي', scientificName: 'Vitamin C', price: 8.25, purchasePrice: 6.00, quantity: 120, expiryDate: '2025-05-10', category: 'فيتامينات', supplier: 3, description: 'مكمل غذائي لفيتامين سي' },
        { id: 5, name: 'لوسيك', scientificName: 'Omeprazole', price: 18.00, purchasePrice: 14.00, quantity: 60, expiryDate: '2024-11-30', category: 'أدوية مزمنة', supplier: 2, description: 'مثبط مضخة البروتون لعلاج الحموضة' }
      ];
      
      // بيانات عينة للموردين
      suppliers = [
        { id: 1, name: 'شركة الأدوية المتحدة', phone: '0123456789', email: 'info@unitedpharma.com', company: 'الأدوية المتحدة', address: 'القاهرة، مصر', notes: 'مورد رئيسي للأدوية' },
        { id: 2, name: 'المصنع العربي للأدوية', phone: '0111222333', email: 'sales@arabianpharma.com', company: 'المصنع العربي', address: 'الإسكندرية، مصر', notes: 'متخصص في المضادات الحيوية' },
        { id: 3, name: 'الدلتا للمستحضرات الطبية', phone: '0100555666', email: 'contact@deltamed.com', company: 'دلتا ميد', address: 'المنصورة، مصر', notes: 'مورد للمكملات الغذائية' }
      ];
      
      // بيانات عينة للعملاء
      customers = [
        { id: 1, name: 'أحمد محمد', phone: '01001234567', age: 35, gender: 'ذكر', address: 'شارع النصر، القاهرة', medicalHistory: 'حساسية من البنسلين' },
        { id: 2, name: 'مريم أحمد', phone: '01112345678', age: 28, gender: 'أنثى', address: 'حي الزهور، الجيزة', medicalHistory: 'ضغط دم مرتفع' },
        { id: 3, name: 'علي محمود', phone: '01223456789', age: 45, gender: 'ذكر', address: 'المعادي، القاهرة', medicalHistory: 'سكري النوع الثاني' },
        { id: 4, name: 'فاطمة إبراهيم', phone: '01501234567', age: 52, gender: 'أنثى', address: 'مدينة نصر، القاهرة', medicalHistory: 'هشاشة عظام' }
      ];
      
      // بيانات عينة للفواتير
      orders = [
        { 
          id: 1001, 
          customerId: 1, 
          customerName: 'أحمد محمد', 
          date: '2023-05-15', 
          items: [
            { medicineId: 1, name: 'بانادول', price: 10.50, quantity: 2 },
            { medicineId: 3, name: 'فولتارين', price: 12.00, quantity: 1 }
          ], 
          total: 33.00, 
          notes: 'فاتورة نقدية' 
        },
        { 
          id: 1002, 
          customerId: 2, 
          customerName: 'مريم أحمد', 
          date: '2023-05-16', 
          items: [
            { medicineId: 2, name: 'أموكسيسيلين', price: 15.75, quantity: 1 },
            { medicineId: 5, name: 'لوسيك', price: 18.00, quantity: 1 }
          ], 
          total: 33.75, 
          notes: 'فاتورة آجل' 
        },
        { 
          id: 1003, 
          customerId: 3, 
          customerName: 'علي محمود', 
          date: '2023-05-17', 
          items: [
            { medicineId: 4, name: 'فيتامين سي', price: 8.25, quantity: 3 },
            { medicineId: 1, name: 'بانادول', price: 10.50, quantity: 1 }
          ], 
          total: 35.25, 
          notes: '' 
        }
      ];
      
      // بيانات عينة للوصفات الطبية
      prescriptions = [
        { 
          id: 2001, 
          customerId: 1, 
          customerName: 'أحمد محمد', 
          date: '2023-05-10', 
          doctor: 'د. محمد عبد الله', 
          diagnosis: 'التهاب الحلق', 
          items: [
            { medicineId: 2, name: 'أموكسيسيلين', dosage: '500mg كل 8 ساعات', duration: '7 أيام' },
            { medicineId: 1, name: 'بانادول', dosage: 'قرص عند اللزوم', duration: 'حتى زوال الألم' }
          ], 
          notes: 'تجنب الأطعمة الباردة' 
        },
        { 
          id: 2002, 
          customerId: 4, 
          customerName: 'فاطمة إبراهيم', 
          date: '2023-05-12', 
          doctor: 'د. هناء محمود', 
          diagnosis: 'آلام المفاصل', 
          items: [
            { medicineId: 3, name: 'فولتارين', dosage: '50mg مرتين يومياً', duration: '10 أيام' },
            { medicineId: 4, name: 'فيتامين سي', dosage: 'قرص يومياً', duration: 'شهر' }
          ], 
          notes: 'مراجعة بعد أسبوعين' 
        }
      ];
      
      // تحميل الإعدادات
      loadSettings();
      
      // تحديث جميع الجداول والعناصر
      updateMedicinesTable();
      updateSuppliersTable();
      updateCustomersTable();
      updateOrdersTable();
      updatePrescriptionsTable();
      updateSelectOptions();
      updateDashboardStats();
    }
    
    // ========== دوال العرض والحذف ==========
    
    window.editMedicine = function(id) {
      const medicine = medicines.find(m => m.id === id);
      if (!medicine) return;
      
      document.getElementById('medName').value = medicine.name;
      document.getElementById('medScientificName').value = medicine.scientificName || '';
      document.getElementById('medPrice').value = medicine.price;
      document.getElementById('medPurchasePrice').value = medicine.purchasePrice;
      document.getElementById('medQuantity').value = medicine.quantity;
      document.getElementById('medExpiryDate').value = medicine.expiryDate || '';
      document.getElementById('medCategory').value = medicine.category || '';
      document.getElementById('medSupplier').value = medicine.supplier || '';
      document.getElementById('medDescription').value = medicine.description || '';
      
      // قم بتغيير زر الإضافة إلى تحديث
      const addBtn = document.getElementById('addMedicineBtn');
      addBtn.innerHTML = '<span class="material-icons">save</span> تحديث الدواء';
      addBtn.onclick = function() {
        const name = document.getElementById('medName').value;
        const scientificName = document.getElementById('medScientificName').value;
        const price = document.getElementById('medPrice').value;
        const purchasePrice = document.getElementById('medPurchasePrice').value;
        const quantity = document.getElementById('medQuantity').value;
        const expiryDate = document.getElementById('medExpiryDate').value;
        const category = document.getElementById('medCategory').value;
        const supplier = document.getElementById('medSupplier').value;
        const description = document.getElementById('medDescription').value;
        
        if (!name || !price || !quantity || !category) {
          showAlert('error', 'الرجاء إدخال جميع البيانات المطلوبة');
          return;
        }
        
        const medicineIndex = medicines.findIndex(m => m.id === id);
        if (medicineIndex !== -1) {
          medicines[medicineIndex] = {
            id: id,
            name: name,
            scientificName: scientificName,
            price: parseFloat(price),
            purchasePrice: parseFloat(purchasePrice) || 0,
            quantity: parseInt(quantity),
            expiryDate: expiryDate,
            category: category,
            supplier: supplier,
            description: description
          };
          
          updateMedicinesTable();
          updateSelectOptions();
          showAlert('success', 'تم تحديث الدواء بنجاح');
          
          // استعادة الزر إلى حالته الأصلية
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة دواء';
          addBtn.onclick = addMedicine;
          
          // مسح الحقول
          document.getElementById('medName').value = '';
          document.getElementById('medScientificName').value = '';
          document.getElementById('medPrice').value = '';
          document.getElementById('medPurchasePrice').value = '';
          document.getElementById('medQuantity').value = '';
          document.getElementById('medExpiryDate').value = '';
          document.getElementById('medCategory').value = '';
          document.getElementById('medSupplier').value = '';
          document.getElementById('medDescription').value = '';
          
          // تحديث الإحصائيات
          updateDashboardStats();
        }
      };
    };
    
    window.deleteMedicine = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
        medicines = medicines.filter(medicine => medicine.id !== id);
        updateMedicinesTable();
        updateSelectOptions();
        showAlert('success', 'تم حذف الدواء بنجاح');
        
        // تحديث الإحصائيات
        updateDashboardStats();
      }
    };
    
    window.editSupplier = function(id) {
      const supplier = suppliers.find(s => s.id === id);
      if (!supplier) return;
      
      document.getElementById('supplierName').value = supplier.name;
      document.getElementById('supplierPhone').value = supplier.phone;
      document.getElementById('supplierEmail').value = supplier.email || '';
      document.getElementById('supplierCompany').value = supplier.company || '';
      document.getElementById('supplierAddress').value = supplier.address || '';
      document.getElementById('supplierNotes').value = supplier.notes || '';
      
      // قم بتغيير زر الإضافة إلى تحديث
      const addBtn = document.getElementById('addSupplierBtn');
      addBtn.innerHTML = '<span class="material-icons">save</span> تحديث المورد';
      addBtn.onclick = function() {
        const name = document.getElementById('supplierName').value;
        const phone = document.getElementById('supplierPhone').value;
        const email = document.getElementById('supplierEmail').value;
        const company = document.getElementById('supplierCompany').value;
        const address = document.getElementById('supplierAddress').value;
        const notes = document.getElementById('supplierNotes').value;
        
        if (!name || !phone) {
          showAlert('error', 'الرجاء إدخال اسم المورد ورقم الهاتف');
          return;
        }
        
        const supplierIndex = suppliers.findIndex(s => s.id === id);
        if (supplierIndex !== -1) {
          suppliers[supplierIndex] = {
            id: id,
            name: name,
            phone: phone,
            email: email,
            company: company,
            address: address,
            notes: notes
          };
          
          updateSuppliersTable();
          updateSelectOptions();
          showAlert('success', 'تم تحديث المورد بنجاح');
          
          // استعادة الزر إلى حالته الأصلية
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة مورد';
          addBtn.onclick = addSupplier;
          
          // مسح الحقول
          document.getElementById('supplierName').value = '';
          document.getElementById('supplierPhone').value = '';
          document.getElementById('supplierEmail').value = '';
          document.getElementById('supplierCompany').value = '';
          document.getElementById('supplierAddress').value = '';
          document.getElementById('supplierNotes').value = '';
          
          // تحديث الإحصائيات
          updateDashboardStats();
        }
      };
    };
    
    window.deleteSupplier = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا المورد؟')) {
        suppliers = suppliers.filter(supplier => supplier.id !== id);
        updateSuppliersTable();
        updateSelectOptions();
        showAlert('success', 'تم حذف المورد بنجاح');
        
        // تحديث الإحصائيات
        updateDashboardStats();
      }
    };
    
    window.editCustomer = function(id) {
      const customer = customers.find(c => c.id === id);
      if (!customer) return;
      
      document.getElementById('customerName').value = customer.name;
      document.getElementById('customerPhone').value = customer.phone;
      document.getElementById('customerAge').value = customer.age || '';
      document.getElementById('customerGender').value = customer.gender || 'ذكر';
      document.getElementById('customerAddress').value = customer.address || '';
      document.getElementById('customerMedicalHistory').value = customer.medicalHistory || '';
      
      // قم بتغيير زر الإضافة إلى تحديث
      const addBtn = document.getElementById('addCustomerBtn');
      addBtn.innerHTML = '<span class="material-icons">save</span> تحديث العميل';
      addBtn.onclick = function() {
        const name = document.getElementById('customerName').value;
        const phone = document.getElementById('customerPhone').value;
        const age = document.getElementById('customerAge').value;
        const gender = document.getElementById('customerGender').value;
        const address = document.getElementById('customerAddress').value;
        const medicalHistory = document.getElementById('customerMedicalHistory').value;
        
        if (!name || !phone) {
          showAlert('error', 'الرجاء إدخال اسم العميل ورقم الهاتف');
          return;
        }
        
        const customerIndex = customers.findIndex(c => c.id === id);
        if (customerIndex !== -1) {
          customers[customerIndex] = {
            id: id,
            name: name,
            phone: phone,
            age: age,
            gender: gender,
            address: address,
            medicalHistory: medicalHistory
          };
          
          updateCustomersTable();
          updateSelectOptions();
          showAlert('success', 'تم تحديث العميل بنجاح');
          
          // استعادة الزر إلى حالته الأصلية
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة عميل';
          addBtn.onclick = addCustomer;
          
          // مسح الحقول
          document.getElementById('customerName').value = '';
          document.getElementById('customerPhone').value = '';
          document.getElementById('customerAge').value = '';
          document.getElementById('customerGender').value = 'ذكر';
          document.getElementById('customerAddress').value = '';
          document.getElementById('customerMedicalHistory').value = '';
          
          // تحديث الإحصائيات
          updateDashboardStats();
        }
      };
    };
    
    window.deleteCustomer = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا العميل؟')) {
        customers = customers.filter(customer => customer.id !== id);
        updateCustomersTable();
        updateSelectOptions();
        showAlert('success', 'تم حذف العميل بنجاح');
        
        // تحديث الإحصائيات
        updateDashboardStats();
      }
    };
    
    window.viewOrder = function(id) {
      const order = orders.find(o => o.id === id);
      if (!order) {
        showAlert('error', 'الفاتورة غير موجودة');
        return;
      }
      
      let itemsHtml = '';
      order.items.forEach(item => {
        itemsHtml += `
          <tr>
            <td>${item.name}</td>
            <td>${item.price.toFixed(2)} ج.م</td>
            <td>${item.quantity}</td>
            <td>${(item.price * item.quantity).toFixed(2)} ج.س</td>
          </tr>
        `;
      });
      
      const modalHtml = `
        <div style="position: fixed; top: 0; right: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; z-index: 2000;">
          <div style="background-color: var(--card-light); padding: 2rem; border-radius: 10px; max-width: 800px; width: 90%; max-height: 90vh; overflow-y: auto;">
            <h3 style="margin-bottom: 1.5rem; color: var(--primary);">تفاصيل الفاتورة #${order.id}</h3>
            <div style="margin-bottom: 1.5rem;">
              <p><strong>العميل:</strong> ${order.customerName}</p>
              <p><strong>التاريخ:</strong> ${order.date}</p>
              ${order.notes ? `<p><strong>ملاحظات:</strong> ${order.notes}</p>` : ''}
            </div>
            <div class="table-responsive">
              <table style="width: 100%; border-collapse: collapse; margin-bottom: 1.5rem;">
                <thead>
                  <tr style="background-color: var(--primary); color: white;">
                    <th style="padding: 0.75rem; text-align: right;">الدواء</th>
                    <th style="padding: 0.75rem; text-align: right;">السعر</th>
                    <th style="padding: 0.75rem; text-align: right;">الكمية</th>
                    <th style="padding: 0.75rem; text-align: right;">المجموع</th>
                  </tr>
                </thead>
                <tbody>
                  ${itemsHtml}
                </tbody>
                <tfoot>
                  <tr>
                    <td colspan="3" style="text-align: left; font-weight: bold; padding: 0.75rem;">الإجمالي:</td>
                    <td style="font-weight: bold; padding: 0.75rem;">${order.total.toFixed(2)} ج.س</td>
                  </tr>
                </tfoot>
              </table>
            </div>
            <button onclick="this.parentElement.parentElement.remove()" class="btn" style="margin-top: 1rem;">
              <span class="material-icons">close</span>
              إغلاق
            </button>
          </div>
        </div>
      `;
      
      document.body.insertAdjacentHTML('beforeend', modalHtml);
    };
    
    window.deleteOrder = function(id) {
      if (confirm('هل أنت متأكد من حذف هذه الفاتورة؟')) {
        orders = orders.filter(order => order.id !== id);
        updateOrdersTable();
        showAlert('success', 'تم حذف الفاتورة بنجاح');
        
        // تحديث الإحصائيات
        updateDashboardStats();
      }
    };
    
    window.viewPrescription = function(id) {
      const prescription = prescriptions.find(p => p.id === id);
      if (!prescription) {
        showAlert('error', 'الوصفة الطبية غير موجودة');
        return;
      }
      
      let itemsHtml = '';
      prescription.items.forEach(item => {
        itemsHtml += `
          <tr>
            <td>${item.name}</td>
            <td>${item.dosage}</td>
            <td>${item.duration}</td>
          </tr>
        `;
      });
      
      const modalHtml = `
        <div style="position: fixed; top: 0; right: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); display: flex; justify-content: center; align-items: center; z-index: 2000;">
          <div style="background-color: var(--card-light); padding: 2rem; border-radius: 10px; max-width: 800px; width: 90%; max-height: 90vh; overflow-y: auto;">
            <h3 style="margin-bottom: 1.5rem; color: var(--primary);">تفاصيل الوصفة الطبية #${prescription.id}</h3>
            <div style="margin-bottom: 1.5rem;">
              <p><strong>العميل:</strong> ${prescription.customerName}</p>
              <p><strong>الطبيب:</strong> ${prescription.doctor}</p>
              <p><strong>التاريخ:</strong> ${prescription.date}</p>
              <p><strong>التشخيص:</strong> ${prescription.diagnosis}</p>
              ${prescription.notes ? `<p><strong>ملاحظات:</strong> ${prescription.notes}</p>` : ''}
            </div>
            <h4 style="margin-bottom: 1rem; color: var(--primary);">الأدوية الموصوفة</h4>
            <div class="table-responsive">
              <table style="width: 100%; border-collapse: collapse; margin-bottom: 1.5rem;">
                <thead>
                  <tr style="background-color: var(--primary); color: white;">
                    <th style="padding: 0.75rem; text-align: right;">الدواء</th>
                    <th style="padding: 0.75rem; text-align: right;">الجرعة</th>
                    <th style="padding: 0.75rem; text-align: right;">المدة</th>
                  </tr>
                </thead>
                <tbody>
                  ${itemsHtml}
                </tbody>
              </table>
            </div>
            <button onclick="this.parentElement.parentElement.remove()" class="btn" style="margin-top: 1rem;">
              <span class="material-icons">close</span>
              إغلاق
            </button>
          </div>
        </div>
      `;
      
      document.body.insertAdjacentHTML('beforeend', modalHtml);
    };
    
    window.deletePrescription = function(id) {
      if (confirm('هل أنت متأكد من حذف هذه الوصفة الطبية؟')) {
        prescriptions = prescriptions.filter(prescription => prescription.id !== id);
        updatePrescriptionsTable();
        showAlert('success', 'تم حذف الوصفة الطبية بنجاح');
      }
    };
    
    window.removeFromOrder = function(medicineId) {
      currentOrderItems = currentOrderItems.filter(item => item.medicineId !== medicineId);
      updateOrderItemsTable();
    };
    
    window.removeFromPrescription = function(medicineId) {
      currentPrescriptionItems = currentPrescriptionItems.filter(item => item.medicineId !== medicineId);
      updatePrescriptionItemsTable();
    };
    
    function showAlert(type, message) {
      const alertContainer = document.getElementById('alertContainer');
      const alert = document.createElement('div');
      alert.className = `alert alert-${type}`;
      alert.innerHTML = `
        <span class="material-icons">${type === 'error' ? 'error' : 'check_circle'}</span>
        ${message}
      `;
      alertContainer.appendChild(alert);
      
      setTimeout(() => {
        alert.remove();
      }, 5000);
    }
  </script>
</body>
</html>
