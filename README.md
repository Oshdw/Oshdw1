<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>نظام إدارة الصيدلية</title>
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
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

    /* تحسينات القائمة الجانبية */
    .sidebar {
      width: var(--sidebar-width);
      background-color: var(--primary);
      color: white;
      height: 100vh;
      padding: 1rem;
      position: fixed;
      right: 0;
      top: 0;
      overflow-y: auto;
      transition: transform 0.3s ease-in-out;
      z-index: 1000;
      box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
    }

    .sidebar.hidden {
      transform: translateX(100%);
    }

    .sidebar h2 {
      text-align: center;
      margin: 1rem 0;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
      font-size: 1.5rem;
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
      font-size: 1.2rem;
    }

    /* تحسينات المحتوى الرئيسي */
    .main {
      margin-right: var(--sidebar-width);
      padding: 1.5rem;
      flex-grow: 1;
      transition: margin-right 0.3s;
      width: 100%;
      min-height: 100vh;
    }

    .main.full {
      margin-right: 0;
    }

    /* شريط الأدوات العلوي */
    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--border-light);
      flex-wrap: wrap;
      gap: 1rem;
    }

    body.dark .topbar {
      border-bottom-color: var(--border-dark);
    }

    .topbar h2 {
      font-size: 1.8rem;
      color: var(--primary);
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

    .topbar-action-btn .material-icons {
      font-size: 1.4rem;
    }

    /* إشعارات */
    .notification-badge {
      position: absolute;
      top: -5px;
      right: -5px;
      background-color: var(--danger);
      color: white;
      border-radius: 50%;
      width: 20px;
      height: 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.7rem;
      font-weight: bold;
    }

    .notification-container {
      position: relative;
    }

    /* الصفحات */
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

    /* البطاقات */
    .card {
      background-color: var(--card-light);
      padding: 1.5rem;
      margin-bottom: 1.5rem;
      border-radius: 10px;
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
      transition: all 0.3s;
    }

    body.dark .card {
      background-color: var(--card-dark);
      box-shadow: 0 2px 12px rgba(0, 0, 0, 0.3);
    }

    .card:hover {
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    }

    body.dark .card:hover {
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
    }

    .card h3 {
      margin-bottom: 1.2rem;
      color: var(--primary);
      font-size: 1.3rem;
      padding-bottom: 0.5rem;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark .card h3 {
      border-bottom-color: var(--border-dark);
    }

    /* إحصائيات */
    .stats-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
      gap: 1.5rem;
      margin-bottom: 2rem;
    }

    .stat-card {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      text-align: right;
      padding: 1.5rem;
      border-radius: 10px;
      background-color: var(--primary);
      color: white;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s;
      position: relative;
      overflow: hidden;
    }

    .stat-card:hover {
      transform: translateY(-5px);
    }

    .stat-card::before {
      content: '';
      position: absolute;
      top: 0;
      right: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(135deg, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 100%);
    }

    .stat-card.warning {
      background-color: var(--warning);
    }

    .stat-card.danger {
      background-color: var(--danger);
    }

    .stat-card.success {
      background-color: var(--success);
    }

    .stat-card.info {
      background-color: var(--info);
    }

    .stat-card .icon {
      font-size: 2rem;
      margin-bottom: 1rem;
      opacity: 0.8;
    }

    .stat-card .value {
      font-size: 2rem;
      font-weight: bold;
      margin: 0.3rem 0;
    }

    .stat-card .label {
      font-size: 0.9rem;
      opacity: 0.9;
    }

    /* النماذج */
    .form-container {
      max-width: 800px;
      margin: 0 auto;
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

    textarea.form-control {
      min-height: 120px;
      resize: vertical;
    }

    /* الأزرار */
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
      transform: translateY(-2px);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }

    .btn:active {
      transform: translateY(0);
    }

    .btn-block {
      display: block;
      width: 100%;
    }

    .btn-sm {
      padding: 0.5rem 1rem;
      font-size: 0.9rem;
    }

    .btn-lg {
      padding: 1rem 2rem;
      font-size: 1.1rem;
    }

    .btn-danger {
      background-color: var(--danger);
    }

    .btn-danger:hover {
      background-color: #c62828;
    }

    .btn-warning {
      background-color: var(--warning);
    }

    .btn-warning:hover {
      background-color: #f57c00;
    }

    .btn-success {
      background-color: var(--success);
    }

    .btn-success:hover {
      background-color: #2e7d32;
    }

    .btn-info {
      background-color: var(--info);
    }

    .btn-info:hover {
      background-color: #0277bd;
    }

    .btn-outline {
      background-color: transparent;
      border: 1px solid var(--primary);
      color: var(--primary);
    }

    body.dark .btn-outline {
      color: var(--primary-dark);
      border-color: var(--primary-dark);
    }

    .btn-outline:hover {
      background-color: rgba(25, 118, 210, 0.1);
    }

    .btn-group {
      display: flex;
      gap: 0.8rem;
      margin-top: 1.5rem;
      flex-wrap: wrap;
    }

    .btn-group .btn {
      flex: 1 1 auto;
    }

    /* الرسائل */
    .alert {
      padding: 1rem;
      border-radius: 6px;
      margin-bottom: 1.5rem;
      display: flex;
      align-items: center;
      gap: 0.8rem;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateX(20px); }
      to { opacity: 1; transform: translateX(0); }
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

    .alert-warning {
      background-color: rgba(255, 160, 0, 0.15);
      color: var(--warning);
      border: 1px solid var(--warning);
    }

    .alert-info {
      background-color: rgba(2, 136, 209, 0.15);
      color: var(--info);
      border: 1px solid var(--info);
    }

    .alert .material-icons {
      font-size: 1.5rem;
    }

    /* البحث */
    .search-container {
      margin-bottom: 1.5rem;
      position: relative;
    }

    .search-input {
      padding-right: 3rem;
    }

    .search-icon {
      position: absolute;
      left: 1rem;
      top: 50%;
      transform: translateY(-50%);
      color: #757575;
    }

    /* الجداول */
    .table-responsive {
      overflow-x: auto;
      margin-top: 1.5rem;
      border-radius: 8px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }

    body.dark .table-responsive {
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      min-width: 600px;
    }

    th, td {
      padding: 1rem;
      text-align: right;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark th, 
    body.dark td {
      border-bottom-color: var(--border-dark);
    }

    th {
      background-color: var(--primary);
      color: white;
      font-weight: 500;
      position: sticky;
      top: 0;
    }

    tr {
      transition: background-color 0.2s;
    }

    tr:hover {
      background-color: rgba(25, 118, 210, 0.05);
    }

    body.dark tr:hover {
      background-color: rgba(25, 118, 210, 0.1);
    }

    .actions-cell {
      display: flex;
      gap: 0.5rem;
      justify-content: flex-start;
      white-space: nowrap;
    }

    /* الآلة الحاسبة */
    .calculator {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.8rem;
      margin-top: 1.5rem;
      max-width: 400px;
      margin-left: auto;
      margin-right: auto;
    }

    .calculator-display {
      grid-column: span 4;
      height: 60px;
      font-size: 1.8rem;
      text-align: left;
      padding: 0 1rem;
      border: 1px solid var(--border-light);
      border-radius: 6px;
      background-color: var(--background-light);
      color: var(--text-dark);
      display: flex;
      align-items: center;
      justify-content: flex-end;
      overflow: hidden;
    }

    body.dark .calculator-display {
      background-color: var(--card-dark);
      border-color: var(--border-dark);
      color: var(--text-light);
    }

    .calculator-btn {
      padding: 1.2rem;
      font-size: 1.2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: all 0.2s;
      background-color: var(--card-light);
      color: var(--text-dark);
    }

    body.dark .calculator-btn {
      background-color: var(--card-dark);
      color: var(--text-light);
    }

    .calculator-btn:hover {
      background-color: rgba(0, 0, 0, 0.1);
      transform: translateY(-2px);
    }

    body.dark .calculator-btn:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .calculator-btn.operator {
      background-color: var(--primary-dark);
      color: white;
    }

    .calculator-btn.operator:hover {
      background-color: var(--primary);
    }

    .calculator-btn.equals {
      background-color: var(--success);
      color: white;
    }

    .calculator-btn.equals:hover {
      background-color: #2e7d32;
    }

    .calculator-btn.clear {
      background-color: var(--danger);
      color: white;
    }

    .calculator-btn.clear:hover {
      background-color: #c62828;
    }

    /* الرسوم البيانية */
    .chart-container {
      position: relative;
      height: 400px;
      margin-top: 2rem;
    }

    /* تذييل الصفحة */
    footer {
      text-align: center;
      margin-top: 3rem;
      padding-top: 1.5rem;
      border-top: 1px solid var(--border-light);
      color: #757575;
      font-size: 0.9rem;
    }

    body.dark footer {
      border-top-color: var(--border-dark);
      color: #bdbdbd;
    }

    /* تحسينات للجوال */
    @media (max-width: 992px) {
      .sidebar {
        width: 260px;
      }
      
      .main {
        margin-right: 260px;
      }
      
      .stats-grid {
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      }
    }

    @media (max-width: 768px) {
      .sidebar {
        width: 80%;
        max-width: 300px;
        transform: translateX(100%);
      }

      .sidebar.visible {
        transform: translateX(0);
      }

      .main {
        margin-right: 0;
        padding: 1rem;
      }

      .stats-grid {
        grid-template-columns: 1fr 1fr;
      }
      
      .topbar {
        flex-direction: column;
        align-items: flex-start;
      }
      
      .topbar-actions {
        width: 100%;
        justify-content: space-between;
      }
    }

    @media (max-width: 576px) {
      .stats-grid {
        grid-template-columns: 1fr;
      }
      
      .card {
        padding: 1rem;
      }
      
      .btn-group {
        flex-direction: column;
      }
      
      .btn-group .btn {
        width: 100%;
      }
    }

    /* طباعة */
    @media print {
      .sidebar, .topbar, .no-print {
        display: none !important;
      }

      .main {
        margin-right: 0 !important;
        padding: 0 !important;
      }

      .page {
        display: block !important;
      }
      
      .card {
        box-shadow: none !important;
        border: 1px solid #ddd !important;
        page-break-inside: avoid;
      }
    }
  </style>
</head>
<body>
  <div class="sidebar" id="sidebar">
    <h2>نظام إدارة الصيدلية</h2>
    <div class="sidebar-menu">
      <button id="dashboardBtn" class="active">
        <span class="material-icons">dashboard</span>
        لوحة التحكم
      </button>
      <button id="medicinesBtn">
        <span class="material-icons">medication</span>
        إدارة الأدوية
        <span class="notification-badge" id="expiryBadge" style="display: none;">0</span>
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
      <h2 id="page-title">لوحة التحكم</h2>
      <div class="topbar-actions">
        <button class="topbar-action-btn" id="toggleSidebar">
          <span class="material-icons">menu</span>
        </button>
        <div class="notification-container">
          <button class="topbar-action-btn" id="notificationsBtn">
            <span class="material-icons">notifications</span>
          </button>
          <span class="notification-badge" id="notificationBadge" style="display: none;">0</span>
        </div>
        <button class="topbar-action-btn" id="toggleTheme">
          <span class="material-icons">dark_mode</span>
        </button>
        <button class="topbar-action-btn no-print" id="printBtn">
          <span class="material-icons">print</span>
        </button>
        <button class="topbar-action-btn" id="fullscreenBtn">
          <span class="material-icons">fullscreen</span>
        </button>
      </div>
    </div>

    <!-- إشعارات -->
    <div id="alertContainer"></div>

    <!-- لوحة التحكم -->
    <div id="dashboard" class="page active">
      <div class="stats-grid">
        <div class="stat-card">
          <span class="material-icons icon">medication</span>
          <div class="value" id="med-count">0</div>
          <div class="label">عدد الأدوية</div>
        </div>
        <div class="stat-card warning">
          <span class="material-icons icon">warning</span>
          <div class="value" id="expiring-soon">0</div>
          <div class="label">أدوية تنتهي قريباً</div>
        </div>
        <div class="stat-card danger">
          <span class="material-icons icon">error</span>
          <div class="value" id="expired">0</div>
          <div class="label">أدوية منتهية</div>
        </div>
        <div class="stat-card success">
          <span class="material-icons icon">attach_money</span>
          <div class="value" id="sales">0</div>
          <div class="label">إجمالي المبيعات</div>
        </div>
      </div>

      <div class="card">
        <h3>الأدوية المنتهية الصلاحية</h3>
        <div class="chart-container">
          <canvas id="expiryChart"></canvas>
        </div>
      </div>

      <div class="card">
        <h3>آخر الفواتير</h3>
        <div class="table-responsive">
          <table>
            <thead>
              <tr>
                <th>رقم الفاتورة</th>
                <th>التاريخ</th>
                <th>العميل</th>
                <th>المبلغ</th>
                <th>الحالة</th>
              </tr>
            </thead>
            <tbody id="recent-orders">
              <!-- سيتم ملؤها بواسطة JavaScript -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- إدارة الأدوية -->
    <div id="medicines" class="page">
      <div class="form-container">
        <div class="card">
          <h3>إضافة دواء جديد</h3>
          <div id="med-message" class="alert" style="display: none;"></div>
          <div class="form-group">
            <label for="medName">اسم الدواء</label>
            <input type="text" id="medName" class="form-control" placeholder="أدخل اسم الدواء" required>
          </div>
          <div class="form-group">
            <label for="medGeneric">الاسم العلمي</label>
            <input type="text" id="medGeneric" class="form-control" placeholder="أدخل الاسم العلمي">
          </div>
          <div class="form-group">
            <label for="medCategory">الفئة</label>
            <select id="medCategory" class="form-control" required>
              <option value="">اختر الفئة</option>
              <option value="antibiotic">مضاد حيوي</option>
              <option value="analgesic">مسكن</option>
              <option value="antihistamine">مضاد هستامين</option>
              <option value="antacid">مضاد حموضة</option>
              <option value="other">أخرى</option>
            </select>
          </div>
          <div class="form-group">
            <label for="medQty">الكمية</label>
            <input type="number" id="medQty" class="form-control" placeholder="أدخل الكمية" min="0" required>
          </div>
          <div class="form-group">
            <label for="medPrice">سعر البيع</label>
            <input type="number" step="0.01" id="medPrice" class="form-control" placeholder="أدخل سعر البيع" min="0" required>
          </div>
          <div class="form-group">
            <label for="medExp">تاريخ الانتهاء</label>
            <input type="date" id="medExp" class="form-control" required>
          </div>
          <div class="btn-group">
            <button id="addMedicineBtn" class="btn btn-success">
              <span class="material-icons">save</span>
              إضافة دواء
            </button>
            <button id="clearMedicineBtn" class="btn btn-danger">
              <span class="material-icons">clear</span>
              مسح النموذج
            </button>
          </div>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الأدوية</h3>
        <div class="search-container">
          <input type="text" id="med-search" class="form-control search-input" placeholder="ابحث عن دواء...">
          <span class="material-icons search-icon">search</span>
        </div>
        <div class="table-responsive">
          <table id="medTable">
            <thead>
              <tr>
                <th>الاسم</th>
                <th>الاسم العلمي</th>
                <th>الفئة</th>
                <th>الكمية</th>
                <th>السعر</th>
                <th>تاريخ الانتهاء</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- الموردون -->
    <div id="suppliers" class="page">
      <div class="form-container">
        <div class="card">
          <h3>إدارة الموردين</h3>
          <div id="supplier-message" class="alert" style="display: none;"></div>
          <div class="form-group">
            <label for="supplierName">اسم المورد</label>
            <input type="text" id="supplierName" class="form-control" placeholder="أدخل اسم المورد" required>
          </div>
          <div class="form-group">
            <label for="supplierPhone">رقم الهاتف</label>
            <input type="tel" id="supplierPhone" class="form-control" placeholder="أدخل رقم الهاتف" required>
          </div>
          <div class="form-group">
            <label for="supplierEmail">البريد الإلكتروني</label>
            <input type="email" id="supplierEmail" class="form-control" placeholder="أدخل البريد الإلكتروني">
          </div>
          <div class="form-group">
            <label for="supplierAddress">العنوان</label>
            <input type="text" id="supplierAddress" class="form-control" placeholder="أدخل العنوان">
          </div>
          <div class="btn-group">
            <button id="addSupplierBtn" class="btn btn-success">
              <span class="material-icons">save</span>
              إضافة مورد
            </button>
            <button id="clearSupplierBtn" class="btn btn-danger">
              <span class="material-icons">clear</span>
              مسح النموذج
            </button>
          </div>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الموردين</h3>
        <div class="search-container">
          <input type="text" id="supplier-search" class="form-control search-input" placeholder="ابحث عن مورد...">
          <span class="material-icons search-icon">search</span>
        </div>
        <div class="table-responsive">
          <table id="supplierTable">
            <thead>
              <tr>
                <th>اسم المورد</th>
                <th>رقم الهاتف</th>
                <th>البريد الإلكتروني</th>
                <th>العنوان</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- العملاء -->
    <div id="customers" class="page">
      <div class="form-container">
        <div class="card">
          <h3>إدارة العملاء</h3>
          <div id="customer-message" class="alert" style="display: none;"></div>
          <div class="form-group">
            <label for="customerName">اسم العميل</label>
            <input type="text" id="customerName" class="form-control" placeholder="أدخل اسم العميل" required>
          </div>
          <div class="form-group">
            <label for="customerPhone">رقم الهاتف</label>
            <input type="tel" id="customerPhone" class="form-control" placeholder="أدخل رقم الهاتف" required>
          </div>
          <div class="form-group">
            <label for="customerAddress">العنوان</label>
            <input type="text" id="customerAddress" class="form-control" placeholder="أدخل العنوان">
          </div>
          <div class="form-group">
            <label for="customerMedicalHistory">تاريخ مرضي (اختياري)</label>
            <textarea id="customerMedicalHistory" class="form-control" placeholder="أدخل التاريخ المرضي"></textarea>
          </div>
          <div class="btn-group">
            <button id="addCustomerBtn" class="btn btn-success">
              <span class="material-icons">save</span>
              إضافة عميل
            </button>
            <button id="clearCustomerBtn" class="btn btn-danger">
              <span class="material-icons">clear</span>
              مسح النموذج
            </button>
          </div>
        </div>
      </div>

      <div class="card">
        <h3>قائمة العملاء</h3>
        <div class="search-container">
          <input type="text" id="customer-search" class="form-control search-input" placeholder="ابحث عن عميل...">
          <span class="material-icons search-icon">search</span>
        </div>
        <div class="table-responsive">
          <table id="customerTable">
          <thead>
              <tr>
                <th>اسم العميل</th>
                <th>رقم الهاتف</th>
                <th>العنوان</th>
                <th>التاريخ المرضي</th>
                <th>إجراءات</th>
              </tr>
            </thead>
            <tbody></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- الفواتير -->
    <div id="orders" class="page">
      <div class="form-container">
        <div class="card">
          <h3>إنشاء فاتورة جديدة</h3>
          <div id="order-message" class="alert" style="display: none;"></div>
          <div class="form-group">
            <label for="orderCustomer">العميل</label>
            <select id="orderCustomer" class="form-control" required>
              <option value="">اختر العميل</option>
            </select>
          </div>
 <!-- استمرار الكود من الفواتير -->
<div class="form-group">
  <label for="orderMedicine">الدواء</label>
  <select id="orderMedicine" class="form-control" required>
    <option value="">اختر الدواء</option>
  </select>
</div>

<div class="form-group">
  <label for="orderQty">الكمية</label>
  <input type="number" id="orderQty" class="form-control" min="1" value="1" required>
</div>

<button id="addMedicineToOrderBtn" class="btn btn-primary">
  <span class="material-icons">add</span>
  إضافة إلى الفاتورة
</button>

<div class="table-responsive" style="margin-top: 1.5rem;">
  <table id="orderItems" class="order-items-table">
    <thead>
      <tr>
        <th>الدواء</th>
        <th>الكمية</th>
        <th>السعر</th>
        <th>الإجمالي</th>
        <th>إجراءات</th>
      </tr>
    </thead>
    <tbody></tbody>
    <tfoot>
      <tr>
        <td colspan="3" style="text-align: left; font-weight: bold;">المجموع:</td>
        <td id="orderTotal" style="font-weight: bold;">0.00 ج.م</td>
        <td></td>
      </tr>
      <tr>
        <td colspan="3" style="text-align: left;">الخصم:</td>
        <td>
          <input type="number" id="orderDiscount" class="form-control" min="0" max="100" value="0" style="width: 80px; display: inline-block;"> %
        </td>
        <td></td>
      </tr>
      <tr>
        <td colspan="3" style="text-align: left; font-weight: bold;">الصافي:</td>
        <td id="orderNetTotal" style="font-weight: bold;">0.00 ج.م</td>
        <td></td>
      </tr>
    </tfoot>
  </table>
</div>

<div class="btn-group">
  <button id="createOrderBtn" class="btn btn-success">
    <span class="material-icons">check_circle</span>
    إنشاء الفاتورة
  </button>
  <button id="clearOrderBtn" class="btn btn-danger">
    <span class="material-icons">cancel</span>
    إلغاء
  </button>
  <button id="printOrderBtn" class="btn btn-info no-print">
    <span class="material-icons">print</span>
    طباعة الفاتورة
  </button>
  <button id="saveOrderBtn" class="btn btn-outline no-print">
    <span class="material-icons">save</span>
    حفظ كمسودة
  </button>
</div>
</div>
</div>

<div class="card">
  <h3>سجل الفواتير</h3>
  <div class="search-container">
    <input type="text" id="order-search" class="form-control search-input" placeholder="ابحث عن فاتورة...">
    <span class="material-icons search-icon">search</span>
  </div>
  <div class="table-responsive">
    <table id="orderTable">
      <thead>
        <tr>
          <th>رقم الفاتورة</th>
          <th>التاريخ</th>
          <th>العميل</th>
          <th>المبلغ</th>
          <th>الحالة</th>
          <th>إجراءات</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
</div>
</div>

<!-- الوصفات الطبية -->
<div id="prescriptions" class="page">
  <div class="form-container">
    <div class="card">
      <h3>إضافة وصفة طبية</h3>
      <div id="prescription-message" class="alert" style="display: none;"></div>
      <div class="form-group">
        <label for="prescriptionCustomer">العميل</label>
        <select id="prescriptionCustomer" class="form-control" required>
          <option value="">اختر العميل</option>
        </select>
      </div>
      <div class="form-group">
        <label for="prescriptionDoctor">الطبيب</label>
        <input type="text" id="prescriptionDoctor" class="form-control" placeholder="أدخل اسم الطبيب" required>
      </div>
      <div class="form-group">
        <label for="prescriptionDate">تاريخ الوصفة</label>
        <input type="date" id="prescriptionDate" class="form-control" required>
      </div>
      <div class="form-group">
        <label for="prescriptionNotes">ملاحظات</label>
        <textarea id="prescriptionNotes" class="form-control" rows="4" placeholder="أدخل ملاحظات الوصفة"></textarea>
      </div>
      
      <div class="form-group">
        <label for="prescriptionMedicine">الدواء</label>
        <select id="prescriptionMedicine" class="form-control" required>
          <option value="">اختر الدواء</option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="prescriptionDosage">الجرعة</label>
        <input type="text" id="prescriptionDosage" class="form-control" placeholder="أدخل الجرعة (مثال: قرص مرتين يومياً)" required>
      </div>
      
      <div class="form-group">
        <label for="prescriptionDuration">المدة</label>
        <input type="text" id="prescriptionDuration" class="form-control" placeholder="أدخل المدة (مثال: 5 أيام)" required>
      </div>
      
      <button id="addMedicineToPrescriptionBtn" class="btn btn-primary">
        <span class="material-icons">add</span>
        إضافة دواء للوصفة
      </button>
      
      <div class="table-responsive" style="margin-top: 1.5rem;">
        <table id="prescriptionItems">
          <thead>
            <tr>
              <th>الدواء</th>
              <th>الجرعة</th>
              <th>المدة</th>
              <th>إجراءات</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>
      
      <div class="btn-group">
        <button id="savePrescriptionBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          حفظ الوصفة
        </button>
        <button id="clearPrescriptionBtn" class="btn btn-danger">
          <span class="material-icons">clear</span>
          مسح النموذج
        </button>
        <button id="printPrescriptionBtn" class="btn btn-info no-print">
          <span class="material-icons">print</span>
          طباعة الوصفة
        </button>
      </div>
    </div>
  </div>

  <div class="card">
    <h3>سجل الوصفات الطبية</h3>
    <div class="search-container">
      <input type="text" id="prescription-search" class="form-control search-input" placeholder="ابحث عن وصفة...">
      <span class="material-icons search-icon">search</span>
    </div>
    <div class="table-responsive">
      <table id="prescriptionTable">
        <thead>
          <tr>
            <th>رقم الوصفة</th>
            <th>التاريخ</th>
            <th>العميل</th>
            <th>الطبيب</th>
            <th>إجراءات</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
  </div>
</div>

<!-- الآلة الحاسبة -->
<div id="calculator-page" class="page">
  <div class="card" style="max-width: 400px; margin: 0 auto;">
    <h3>الآلة الحاسبة</h3>
    <div class="calculator">
      <div class="calculator-display" id="calc-display">0</div>
      <button class="calculator-btn clear" onclick="clearCalculator()">AC</button>
      <button class="calculator-btn operator" onclick="appendToDisplay('+')">+</button>
      <button class="calculator-btn operator" onclick="appendToDisplay('-')">-</button>
      <button class="calculator-btn operator" onclick="appendToDisplay('*')">×</button>
      <button class="calculator-btn operator" onclick="appendToDisplay('/')">÷</button>
      <button class="calculator-btn" onclick="appendToDisplay('7')">7</button>
      <button class="calculator-btn" onclick="appendToDisplay('8')">8</button>
      <button class="calculator-btn" onclick="appendToDisplay('9')">9</button>
      <button class="calculator-btn" onclick="appendToDisplay('4')">4</button>
      <button class="calculator-btn" onclick="appendToDisplay('5')">5</button>
      <button class="calculator-btn" onclick="appendToDisplay('6')">6</button>
      <button class="calculator-btn" onclick="appendToDisplay('1')">1</button>
      <button class="calculator-btn" onclick="appendToDisplay('2')">2</button>
      <button class="calculator-btn" onclick="appendToDisplay('3')">3</button>
      <button class="calculator-btn" onclick="appendToDisplay('0')">0</button>
      <button class="calculator-btn" onclick="appendToDisplay('.')">.</button>
      <button class="calculator-btn equals" onclick="calculate()">=</button>
    </div>
  </div>
</div>

<!-- الإحصائيات -->
<div id="statistics" class="page">
  <div class="card">
    <h3>إحصائيات المبيعات</h3>
    <div class="chart-container">
      <canvas id="salesChart"></canvas>
    </div>
  </div>

  <div class="card">
    <h3>إحصائيات الأدوية</h3>
    <div class="chart-container">
      <canvas id="medicinesChart"></canvas>
    </div>
  </div>

  <div class="card">
    <h3>إحصائيات العملاء</h3>
    <div class="chart-container">
      <canvas id="customersChart"></canvas>
    </div>
  </div>
</div>

<!-- الإعدادات -->
<div id="settings" class="page">
  <div class="form-container">
    <div class="card">
      <h3>إعدادات الصيدلية</h3>
      <div id="settings-message" class="alert" style="display: none;"></div>
      <div class="form-group">
        <label for="pharmacyName">اسم الصيدلية</label>
        <input type="text" id="pharmacyName" class="form-control" placeholder="أدخل اسم الصيدلية" required>
      </div>
      <div class="form-group">
        <label for="pharmacyAddress">عنوان الصيدلية</label>
        <input type="text" id="pharmacyAddress" class="form-control" placeholder="أدخل عنوان الصيدلية" required>
      </div>
      <div class="form-group">
        <label for="pharmacyPhone">هاتف الصيدلية</label>
        <input type="tel" id="pharmacyPhone" class="form-control" placeholder="أدخل هاتف الصيدلية" required>
      </div>
      <div class="form-group">
        <label for="pharmacyEmail">البريد الإلكتروني</label>
        <input type="email" id="pharmacyEmail" class="form-control" placeholder="أدخل البريد الإلكتروني">
      </div>
      <div class="form-group">
        <label for="pharmacyLogo">شعار الصيدلية</label>
        <input type="file" id="pharmacyLogo" class="form-control" accept="image/*">
      </div>
      <div class="form-group">
        <label for="pharmacyTaxNumber">الرقم الضريبي</label>
        <input type="text" id="pharmacyTaxNumber" class="form-control" placeholder="أدخل الرقم الضريبي">
      </div>
      <div class="btn-group">
        <button id="saveSettingsBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          حفظ الإعدادات
        </button>
      </div>
    </div>
  </div>

  <div class="card">
    <h3>إعدادات النظام</h3>
    <div class="form-group">
      <label>
        <input type="checkbox" id="enableNotifications" checked>
        تمكين الإشعارات
      </label>
    </div>
    <div class="form-group">
      <label>
        <input type="checkbox" id="enableExpiryAlerts" checked>
        تنبيهات انتهاء الصلاحية
      </label>
    </div>
    <div class="form-group">
      <label>
        <input type="checkbox" id="enableLowStockAlerts" checked>
        تنبيهات نقص الكميات
      </label>
    </div>
    <div class="form-group">
      <label>
        <input type="checkbox" id="enableDarkMode">
        الوضع الليلي
      </label>
    </div>
    <div class="form-group">
      <label for="backupFrequency">تكرار النسخ الاحتياطي</label>
      <select id="backupFrequency" class="form-control">
        <option value="daily">يومي</option>
        <option value="weekly" selected>أسبوعي</option>
        <option value="monthly">شهري</option>
      </select>
    </div>
    <div class="btn-group">
      <button id="backupNowBtn" class="btn btn-info">
        <span class="material-icons">backup</span>
        نسخ احتياطي الآن
      </button>
      <button id="restoreBackupBtn" class="btn btn-warning">
        <span class="material-icons">restore</span>
        استعادة نسخة
      </button>
    </div>
  </div>
</div>

<footer>
  <p>نظام إدارة الصيدلية &copy; <span id="year"></span> - جميع الحقوق محفوظة</p>
  <p id="app-version">الإصدار 2.0.0</p>
</footer>
</div>

<!-- نافذة الإشعارات -->
<div id="notificationsModal" class="modal">
  <div class="modal-content">
    <div class="modal-header">
      <h3>الإشعارات</h3>
      <button class="close-modal">&times;</button>
    </div>
    <div class="modal-body">
      <div id="notificationsList"></div>
    </div>
    <div class="modal-footer">
      <button id="clearNotificationsBtn" class="btn btn-sm btn-danger">مسح الكل</button>
    </div>
  </div>
</div>

<!-- نافذة الطباعة -->
<div id="printModal" class="modal">
  <div class="modal-content" id="printContent">
    <!-- سيتم ملؤها ديناميكياً -->
  </div>
</div>

<script>
  // DOM Elements
  const sidebar = document.getElementById('sidebar');
  const main = document.getElementById('main');
  const toggleSidebar = document.getElementById('toggleSidebar');
  const toggleTheme = document.getElementById('toggleTheme');
  const printBtn = document.getElementById('printBtn');
  const fullscreenBtn = document.getElementById('fullscreenBtn');
  const notificationsBtn = document.getElementById('notificationsBtn');
  const yearSpan = document.getElementById('year');
  const appVersion = document.getElementById('app-version');
  
  // Set current year and version
  yearSpan.textContent = new Date().getFullYear();
  appVersion.textContent = `الإصدار ${window.APP_VERSION || '2.0.0'}`;
  
  // Toggle sidebar
  toggleSidebar.addEventListener('click', () => {
    sidebar.classList.toggle('hidden');
    main.classList.toggle('full');
    localStorage.setItem('sidebarCollapsed', sidebar.classList.contains('hidden'));
  });
  
  // Check for saved sidebar state
  if (localStorage.getItem('sidebarCollapsed') === 'true') {
    sidebar.classList.add('hidden');
    main.classList.add('full');
  }
  
  // Toggle theme
  toggleTheme.addEventListener('click', () => {
    document.body.classList.toggle('dark');
    localStorage.setItem('theme', document.body.classList.contains('dark') ? 'dark' : 'light');
  });
  
  // Check for saved theme preference
  if (localStorage.getItem('theme') === 'dark') {
    document.body.classList.add('dark');
  }
  
  // Fullscreen mode
  fullscreenBtn.addEventListener('click', toggleFullscreen);
  
  function toggleFullscreen() {
    if (!document.fullscreenElement) {
      document.documentElement.requestFullscreen().catch(err => {
        showAlert('error', `خطأ في تفعيل وضع ملء الشاشة: ${err.message}`);
      });
      fullscreenBtn.innerHTML = '<span class="material-icons">fullscreen_exit</span>';
    } else {
      if (document.exitFullscreen) {
        document.exitFullscreen();
        fullscreenBtn.innerHTML = '<span class="material-icons">fullscreen</span>';
      }
    }
  }
  
  // Show alert function
  function showAlert(type, message) {
    const alertContainer = document.getElementById('alertContainer');
    const alert = document.createElement('div');
    alert.className = `alert alert-${type}`;
    alert.innerHTML = `
      <span class="material-icons">${type === 'error' ? 'error' : type === 'success' ? 'check_circle' : 'info'}</span>
      ${message}
    `;
    alertContainer.appendChild(alert);
    
    setTimeout(() => {
      alert.style.opacity = '0';
      setTimeout(() => alert.remove(), 300);
    }, 5000);
  }
  
  // Initialize the app
  document.addEventListener('DOMContentLoaded', () => {
    initCharts();
    loadData();
    setupEventListeners();
    checkExpiryMedicines();
  });
  
  // Load initial data
  async function loadData() {
    try {
      // Load medicines, customers, suppliers, etc.
      await Promise.all([
        loadMedicines(),
        loadCustomers(),
        loadSuppliers(),
        loadOrders(),
        loadPrescriptions()
      ]);
      
      updateDashboardStats();
    } catch (error) {
      showAlert('error', `خطأ في تحميل البيانات: ${error.message}`);
    }
  }
  
  // Check for expiring medicines
  function checkExpiryMedicines() {
    const today = new Date();
    const nextMonth = new Date();
    nextMonth.setMonth(today.getMonth() + 1);
    
    const expiringSoon = medicines.filter(med => {
      const expiryDate = new Date(med.expiry);
      return expiryDate > today && expiryDate < nextMonth;
    });
    
    const expired = medicines.filter(med => {
      return new Date(med.expiry) < today;
    });
    
    if (expiringSoon.length > 0) {
      showAlert('warning', `هناك ${expiringSoon.length} دواء ستنتهي صلاحيته خلال الشهر القادم`);
      document.getElementById('expiryBadge').textContent = expiringSoon.length;
      document.getElementById('expiryBadge').style.display = 'flex';
    }
    
    if (expired.length > 0) {
      showAlert('error', `تحذير: هناك ${expired.length} دواء منتهي الصلاحية`);
    }
  }
  
  // ... (استمرار باقي الدوال والوظائف)
</script>
</body>
</html>
          
