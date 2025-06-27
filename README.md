<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
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

    /* النوافذ المنبثقة */
    .modal {
      display: none;
      position: fixed;
      z-index: 2000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      overflow: auto;
    }

    .modal-content {
      background-color: var(--background-light);
      margin: 5% auto;
      padding: 20px;
      border-radius: 8px;
      width: 80%;
      max-width: 600px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
    }

    body.dark .modal-content {
      background-color: var(--card-dark);
    }

    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark .modal-header {
      border-bottom-color: var(--border-dark);
    }

    .close-modal {
      font-size: 28px;
      font-weight: bold;
      cursor: pointer;
      background: none;
      border: none;
      color: var(--text-dark);
    }

    body.dark .close-modal {
      color: var(--text-light);
    }

    .modal-footer {
      margin-top: 20px;
      padding-top: 10px;
      border-top: 1px solid var(--border-light);
      text-align: left;
    }

    body.dark .modal-footer {
      border-top-color: var(--border-dark);
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
    
    // Navigation buttons
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
    
    // Pages
    const pages = {
      dashboard: document.getElementById('dashboard'),
      medicines: document.getElementById('medicines'),
      suppliers: document.getElementById('suppliers'),
      customers: document.getElementById('customers'),
      orders: document.getElementById('orders'),
      prescriptions: document.getElementById('prescriptions'),
      calculator: document.getElementById('calculator-page'),
      statistics: document.getElementById('statistics'),
      settings: document.getElementById('settings')
    };
    
    // Sample data
    let medicines = [];
    let suppliers = [];
    let customers = [];
    let orders = [];
    let prescriptions = [];
    let notifications = [];
    
    // Set current year and version
    yearSpan.textContent = new Date().getFullYear();
    appVersion.textContent = `الإصدار 2.0.0`;
    
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
    
    // Navigation
    Object.keys(navButtons).forEach(key => {
      navButtons[key].addEventListener('click', () => {
        // Hide all pages
        Object.values(pages).forEach(page => {
          page.classList.remove('active');
        });
        
        // Remove active class from all buttons
        Object.values(navButtons).forEach(btn => {
          btn.classList.remove('active');
        });
        
        // Show selected page
        pages[key].classList.add('active');
        
        // Add active class to clicked button
        navButtons[key].classList.add('active');
        
        // Update page title
        document.getElementById('page-title').textContent = navButtons[key].textContent.trim();
        
        // Close sidebar on mobile
        if (window.innerWidth <= 768) {
          sidebar.classList.add('hidden');
          main.classList.add('full');
        }
      });
    });
    
    // Notifications modal
    notificationsBtn.addEventListener('click', () => {
      document.getElementById('notificationsModal').style.display = 'block';
    });
    
    // Close modal when clicking on X
    document.querySelector('.close-modal').addEventListener('click', () => {
      document.getElementById('notificationsModal').style.display = 'none';
    });
    
    // Close modal when clicking outside
    window.addEventListener('click', (event) => {
      if (event.target === document.getElementById('notificationsModal')) {
        document.getElementById('notificationsModal').style.display = 'none';
      }
      if (event.target === document.getElementById('printModal')) {
        document.getElementById('printModal').style.display = 'none';
      }
    });
    
    // Clear notifications
    document.getElementById('clearNotificationsBtn').addEventListener('click', () => {
      notifications = [];
      document.getElementById('notificationsList').innerHTML = '';
      document.getElementById('notificationBadge').style.display = 'none';
      showAlert('success', 'تم مسح جميع الإشعارات');
    });
    
    // Print functionality
    printBtn.addEventListener('click', () => {
      window.print();
    });
    
    // Calculator functionality
    function appendToDisplay(value) {
      const display = document.getElementById('calc-display');
      if (display.textContent === '0' && value !== '.') {
        display.textContent = value;
      } else {
        display.textContent += value;
      }
    }
    
    function clearCalculator() {
      document.getElementById('calc-display').textContent = '0';
    }
    
    function calculate() {
      const display = document.getElementById('calc-display');
      try {
        // Replace × with * for evaluation
        const expression = display.textContent.replace(/×/g, '*');
        display.textContent = eval(expression);
      } catch (error) {
        display.textContent = 'Error';
      }
    }
    
    // Initialize charts
    function initCharts() {
      // Expiry Chart
      const expiryCtx = document.getElementById('expiryChart').getContext('2d');
      new Chart(expiryCtx, {
        type: 'bar',
        data: {
          labels: ['يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو'],
          datasets: [{
            label: 'أدوية تنتهي',
            data: [12, 19, 3, 5, 2, 3],
            backgroundColor: 'rgba(255, 159, 64, 0.7)',
            borderColor: 'rgba(255, 159, 64, 1)',
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
      
      // Sales Chart
      const salesCtx = document.getElementById('salesChart').getContext('2d');
      new Chart(salesCtx, {
        type: 'line',
        data: {
          labels: ['يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو'],
          datasets: [{
            label: 'المبيعات',
            data: [12000, 19000, 3000, 5000, 2000, 3000],
            backgroundColor: 'rgba(56, 142, 60, 0.2)',
            borderColor: 'rgba(56, 142, 60, 1)',
            borderWidth: 2,
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
      
      // Medicines Chart
      const medicinesCtx = document.getElementById('medicinesChart').getContext('2d');
      new Chart(medicinesCtx, {
        type: 'pie',
        data: {
          labels: ['مضادات حيوية', 'مسكنات', 'مضادات هستامين', 'أخرى'],
          datasets: [{
            data: [300, 150, 100, 50],
            backgroundColor: [
              'rgba(25, 118, 210, 0.7)',
              'rgba(255, 159, 64, 0.7)',
              'rgba(211, 47, 47, 0.7)',
              'rgba(121, 85, 72, 0.7)'
            ],
            borderColor: [
              'rgba(25, 118, 210, 1)',
              'rgba(255, 159, 64, 1)',
              'rgba(211, 47, 47, 1)',
              'rgba(121, 85, 72, 1)'
            ],
            borderWidth: 1
          }]
        },
        options: {
          responsive: true
        }
      });
      
      // Customers Chart
      const customersCtx = document.getElementById('customersChart').getContext('2d');
      new Chart(customersCtx, {
        type: 'doughnut',
        data: {
          labels: ['عملاء جدد', 'عملاء متكررين', 'عملاء غير نشطين'],
          datasets: [{
            data: [150, 80, 20],
            backgroundColor: [
              'rgba(2, 136, 209, 0.7)',
              'rgba(56, 142, 60, 0.7)',
              'rgba(158, 158, 158, 0.7)'
            ],
            borderColor: [
              'rgba(2, 136, 209, 1)',
              'rgba(56, 142, 60, 1)',
              'rgba(158, 158, 158, 1)'
            ],
            borderWidth: 1
          }]
        },
        options: {
          responsive: true
        }
      });
    }
    
    // Load sample data
    function loadSampleData() {
      medicines = [
        { id: 1, name: 'بانادول', generic: 'باراسيتامول', category: 'analgesic', quantity: 50, price: 5.5, expiry: '2023-12-31' },
        { id: 2, name: 'أموكسيسيلين', generic: 'أموكسيسيلين', category: 'antibiotic', quantity: 30, price: 12.0, expiry: '2024-06-30' },
        { id: 3, name: 'كلاريتين', generic: 'لوراتادين', category: 'antihistamine', quantity: 25, price: 8.75, expiry: '2023-11-15' },
        { id: 4, name: 'جافيسكون', generic: 'ألجينات الصوديوم', category: 'antacid', quantity: 40, price: 6.25, expiry: '2024-03-31' },
        { id: 5, name: 'فولتارين', generic: 'ديكلوفيناك', category: 'analgesic', quantity: 20, price: 15.0, expiry: '2023-10-31' }
      ];
      
      suppliers = [
        { id: 1, name: 'شركة الأدوية المتحدة', phone: '0123456789', email: 'info@unitedpharma.com', address: 'القاهرة، مصر' },
        { id: 2, name: 'المجموعة الطبية', phone: '0111222333', email: 'contact@medicalgroup.com', address: 'الإسكندرية، مصر' }
      ];
      
      customers = [
        { id: 1, name: 'أحمد محمد', phone: '01001234567', address: 'شارع النصر، القاهرة', medicalHistory: 'حساسية من البنسلين' },
        { id: 2, name: 'مريم علي', phone: '01111222333', address: 'حي الزهور، الجيزة', medicalHistory: '' },
        { id: 3, name: 'خالد عبدالله', phone: '0122223344', address: 'مدينة نصر، القاهرة', medicalHistory: 'ضغط دم مرتفع' }
      ];
      
      orders = [
        { id: 1001, date: '2023-05-15', customer: 'أحمد محمد', amount: 120.5, status: 'مكتمل' },
        { id: 1002, date: '2023-05-14', customer: 'مريم علي', amount: 85.75, status: 'مكتمل' },
        { id: 1003, date: '2023-05-13', customer: 'خالد عبدالله', amount: 150.0, status: 'ملغى' }
      ];
      
      prescriptions = [
        { id: 5001, date: '2023-05-10', customer: 'أحمد محمد', doctor: 'د. محمد أحمد', items: [
          { medicine: 'أموكسيسيلين', dosage: 'كبسولة كل 8 ساعات', duration: '7 أيام' },
          { medicine: 'بانادول', dosage: 'قرص عند اللزوم', duration: '5 أيام' }
        ]}
      ];
      
      notifications = [
        { id: 1, type: 'warning', message: '5 أدوية ستنتهي صلاحيتها خلال شهر', date: '2023-05-15' },
        { id: 2, type: 'info', message: 'فاتورة جديدة #1001 تم إنشاؤها', date: '2023-05-15' }
      ];
    }
    
    // Render data in tables
    function renderData() {
      // Render medicines table
      const medTableBody = document.querySelector('#medTable tbody');
      medTableBody.innerHTML = medicines.map(med => `
        <tr>
          <td>${med.name}</td>
          <td>${med.generic}</td>
          <td>${getCategoryName(med.category)}</td>
          <td>${med.quantity}</td>
          <td>${med.price.toFixed(2)} ج.م</td>
          <td>${med.expiry}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-outline" onclick="editMedicine(${med.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteMedicine(${med.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        </tr>
      `).join('');
      
      // Render suppliers table
      const supplierTableBody = document.querySelector('#supplierTable tbody');
      supplierTableBody.innerHTML = suppliers.map(supplier => `
        <tr>
          <td>${supplier.name}</td>
          <td>${supplier.phone}</td>
          <td>${supplier.email}</td>
          <td>${supplier.address}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-outline" onclick="editSupplier(${supplier.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteSupplier(${supplier.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        </tr>
      `).join('');
      
      // Render customers table
      const customerTableBody = document.querySelector('#customerTable tbody');
      customerTableBody.innerHTML = customers.map(customer => `
        <tr>
          <td>${customer.name}</td>
          <td>${customer.phone}</td>
          <td>${customer.address}</td>
          <td>${customer.medicalHistory || 'لا يوجد'}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-outline" onclick="editCustomer(${customer.id})">
              <span class="material-icons">edit</span>
            </button>
            <button class="btn btn-sm btn-danger" onclick="deleteCustomer(${customer.id})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        </tr>
      `).join('');
      
      // Render orders table
      const orderTableBody = document.querySelector('#orderTable tbody');
      orderTableBody.innerHTML = orders.map(order => `
        <tr>
          <td>#${order.id}</td>
          <td>${order.date}</td>
          <td>${order.customer}</td>
          <td>${order.amount.toFixed(2)} ج.م</td>
          <td>${order.status}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-outline" onclick="viewOrder(${order.id})">
              <span class="material-icons">visibility</span>
            </button>
            <button class="btn btn-sm btn-info" onclick="printOrder(${order.id})">
              <span class="material-icons">print</span>
            </button>
          </td>
        </tr>
      `).join('');
      
      // Render recent orders in dashboard
      const recentOrdersBody = document.querySelector('#recent-orders');
      recentOrdersBody.innerHTML = orders.slice(0, 3).map(order => `
        <tr>
          <td>#${order.id}</td>
          <td>${order.date}</td>
          <td>${order.customer}</td>
          <td>${order.amount.toFixed(2)} ج.م</td>
          <td>${order.status}</td>
        </tr>
      `).join('');
      
      // Render prescriptions table
      const prescriptionTableBody = document.querySelector('#prescriptionTable tbody');
      prescriptionTableBody.innerHTML = prescriptions.map(prescription => `
        <tr>
          <td>#${prescription.id}</td>
          <td>${prescription.date}</td>
          <td>${prescription.customer}</td>
          <td>${prescription.doctor}</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-outline" onclick="viewPrescription(${prescription.id})">
              <span class="material-icons">visibility</span>
            </button>
            <button class="btn btn-sm btn-info" onclick="printPrescription(${prescription.id})">
              <span class="material-icons">print</span>
            </button>
          </td>
        </tr>
      `).join('');
      
      // Render notifications
      const notificationsList = document.getElementById('notificationsList');
      notificationsList.innerHTML = notifications.map(notification => `
        <div class="alert alert-${notification.type}">
          <span class="material-icons">${notification.type === 'error' ? 'error' : notification.type === 'success' ? 'check_circle' : 'info'}</span>
          ${notification.message} - ${notification.date}
        </div>
      `).join('');
      
      // Update notification badge
      document.getElementById('notificationBadge').textContent = notifications.length;
      document.getElementById('notificationBadge').style.display = notifications.length > 0 ? 'flex' : 'none';
      
      // Update customer dropdowns
      updateCustomerDropdowns();
      
      // Update medicine dropdowns
      updateMedicineDropdowns();
    }
    
    // Helper function to get category name
    function getCategoryName(category) {
      const categories = {
        'antibiotic': 'مضاد حيوي',
        'analgesic': 'مسكن',
        'antihistamine': 'مضاد هستامين',
        'antacid': 'مضاد حموضة',
        'other': 'أخرى'
      };
      return categories[category] || category;
    }
    
    // Update customer dropdowns
    function updateCustomerDropdowns() {
      const orderCustomerSelect = document.getElementById('orderCustomer');
      const prescriptionCustomerSelect = document.getElementById('prescriptionCustomer');
      
      orderCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';
      prescriptionCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';
      
      customers.forEach(customer => {
        orderCustomerSelect.innerHTML += `<option value="${customer.id}">${customer.name}</option>`;
        prescriptionCustomerSelect.innerHTML += `<option value="${customer.id}">${customer.name}</option>`;
      });
    }
    
    // Update medicine dropdowns
    function updateMedicineDropdowns() {
      const orderMedicineSelect = document.getElementById('orderMedicine');
      const prescriptionMedicineSelect = document.getElementById('prescriptionMedicine');
      
      orderMedicineSelect.innerHTML = '<option value="">اختر الدواء</option>';
      prescriptionMedicineSelect.innerHTML = '<option value="">اختر الدواء</option>';
      
      medicines.forEach(medicine => {
        orderMedicineSelect.innerHTML += `<option value="${medicine.id}">${medicine.name} (${medicine.generic})</option>`;
        prescriptionMedicineSelect.innerHTML += `<option value="${medicine.id}">${medicine.name} (${medicine.generic})</option>`;
      });
    }
    
    // Add medicine
    document.getElementById('addMedicineBtn').addEventListener('click', () => {
      const name = document.getElementById('medName').value;
      const generic = document.getElementById('medGeneric').value;
      const category = document.getElementById('medCategory').value;
      const quantity = parseInt(document.getElementById('medQty').value);
      const price = parseFloat(document.getElementById('medPrice').value);
      const expiry = document.getElementById('medExp').value;
      
      if (!name || !category || isNaN(quantity) || isNaN(price) || !expiry) {
        showAlert('error', 'الرجاء ملء جميع الحقول المطلوبة');
        return;
      }
      
      const newId = medicines.length > 0 ? Math.max(...medicines.map(m => m.id)) + 1 : 1;
      
      medicines.push({
        id: newId,
        name,
        generic,
        category,
        quantity,
        price,
        expiry
      });
      
      renderData();
      showAlert('success', 'تم إضافة الدواء بنجاح');
      document.getElementById('med-message').style.display = 'none';
      document.getElementById('clearMedicineBtn').click();
      
      // Check for expiry notifications
      checkExpiryMedicines();
    });
    
    // Clear medicine form
    document.getElementById('clearMedicineBtn').addEventListener('click', () => {
      document.getElementById('medName').value = '';
      document.getElementById('medGeneric').value = '';
      document.getElementById('medCategory').value = '';
      document.getElementById('medQty').value = '';
      document.getElementById('medPrice').value = '';
      document.getElementById('medExp').value = '';
    });
    
    // Add supplier
    document.getElementById('addSupplierBtn').addEventListener('click', () => {
      const name = document.getElementById('supplierName').value;
      const phone = document.getElementById('supplierPhone').value;
      const email = document.getElementById('supplierEmail').value;
      const address = document.getElementById('supplierAddress').value;
      
      if (!name || !phone) {
        showAlert('error', 'الرجاء إدخال اسم المورد ورقم الهاتف');
        return;
      }
      
      const newId = suppliers.length > 0 ? Math.max(...suppliers.map(s => s.id)) + 1 : 1;
      
      suppliers.push({
        id: newId,
        name,
        phone,
        email,
        address
      });
      
      renderData();
      showAlert('success', 'تم إضافة المورد بنجاح');
      document.getElementById('supplier-message').style.display = 'none';
      document.getElementById('clearSupplierBtn').click();
    });
    
    // Clear supplier form
    document.getElementById('clearSupplierBtn').addEventListener('click', () => {
      document.getElementById('supplierName').value = '';
      document.getElementById('supplierPhone').value = '';
      document.getElementById('supplierEmail').value = '';
      document.getElementById('supplierAddress').value = '';
    });
    
    // Add customer
    document.getElementById('addCustomerBtn').addEventListener('click', () => {
      const name = document.getElementById('customerName').value;
      const phone = document.getElementById('customerPhone').value;
      const address = document.getElementById('customerAddress').value;
      const medicalHistory = document.getElementById('customerMedicalHistory').value;
      
      if (!name || !phone) {
        showAlert('error', 'الرجاء إدخال اسم العميل ورقم الهاتف');
        return;
      }
      
      const newId = customers.length > 0 ? Math.max(...customers.map(c => c.id)) + 1 : 1;
      
      customers.push({
        id: newId,
        name,
        phone,
        address,
        medicalHistory
      });
      
      renderData();
      showAlert('success', 'تم إضافة العميل بنجاح');
      document.getElementById('customer-message').style.display = 'none';
      document.getElementById('clearCustomerBtn').click();
    });
    
    // Clear customer form
    document.getElementById('clearCustomerBtn').addEventListener('click', () => {
      document.getElementById('customerName').value = '';
      document.getElementById('customerPhone').value = '';
      document.getElementById('customerAddress').value = '';
      document.getElementById('customerMedicalHistory').value = '';
    });
    
    // Add medicine to order
    document.getElementById('addMedicineToOrderBtn').addEventListener('click', () => {
      const medicineId = parseInt(document.getElementById('orderMedicine').value);
      const quantity = parseInt(document.getElementById('orderQty').value);
      
      if (!medicineId || isNaN(quantity) || quantity < 1) {
        showAlert('error', 'الرجاء اختيار دواء وإدخال كمية صحيحة');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showAlert('error', 'الدواء المحدد غير موجود');
        return;
      }
      
      if (medicine.quantity < quantity) {
        showAlert('error', 'الكمية المطلوبة غير متوفرة في المخزن');
        return;
      }
      
      const orderItemsBody = document.querySelector('#orderItems tbody');
      const existingItem = Array.from(orderItemsBody.querySelectorAll('tr')).find(tr => 
        parseInt(tr.dataset.medicineId) === medicineId
      );
      
      if (existingItem) {
        const existingQty = parseInt(existingItem.querySelector('.item-qty').textContent);
        const newQty = existingQty + quantity;
        existingItem.querySelector('.item-qty').textContent = newQty;
        existingItem.querySelector('.item-total').textContent = (newQty * medicine.price).toFixed(2) + ' ج.م';
      } else {
        const newRow = document.createElement('tr');
        newRow.dataset.medicineId = medicineId;
        newRow.innerHTML = `
          <td>${medicine.name}</td>
          <td class="item-qty">${quantity}</td>
          <td>${medicine.price.toFixed(2)} ج.م</td>
          <td class="item-total">${(quantity * medicine.price).toFixed(2)} ج.م</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-danger" onclick="removeOrderItem(this)">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        orderItemsBody.appendChild(newRow);
      }
      
      updateOrderTotal();
    });
    
    // Remove order item
    window.removeOrderItem = function(button) {
      const row = button.closest('tr');
      row.remove();
      updateOrderTotal();
    };
    
    // Update order total
    function updateOrderTotal() {
      const rows = document.querySelectorAll('#orderItems tbody tr');
      let total = 0;
      
      rows.forEach(row => {
        const itemTotal = parseFloat(row.querySelector('.item-total').textContent);
        total += itemTotal;
      });
      
      const discount = parseInt(document.getElementById('orderDiscount').value) || 0;
      const discountAmount = total * (discount / 100);
      const netTotal = total - discountAmount;
      
      document.getElementById('orderTotal').textContent = total.toFixed(2) + ' ج.م';
      document.getElementById('orderNetTotal').textContent = netTotal.toFixed(2) + ' ج.م';
    }
    
    // Order discount change
    document.getElementById('orderDiscount').addEventListener('change', updateOrderTotal);
    
    // Create order
    document.getElementById('createOrderBtn').addEventListener('click', () => {
      const customerId = parseInt(document.getElementById('orderCustomer').value);
      const rows = document.querySelectorAll('#orderItems tbody tr');
      
      if (!customerId) {
        showAlert('error', 'الرجاء اختيار عميل');
        return;
      }
      
      if (rows.length === 0) {
        showAlert('error', 'الرجاء إضافة أدوية إلى الفاتورة');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showAlert('error', 'العميل المحدد غير موجود');
        return;
      }
      
      // Calculate total
      let total = 0;
      rows.forEach(row => {
        const itemTotal = parseFloat(row.querySelector('.item-total').textContent);
        total += itemTotal;
      });
      
      const discount = parseInt(document.getElementById('orderDiscount').value) || 0;
      const discountAmount = total * (discount / 100);
      const netTotal = total - discountAmount;
      
      // Create new order
      const newId = orders.length > 0 ? Math.max(...orders.map(o => o.id)) + 1 : 1001;
      const today = new Date().toISOString().split('T')[0];
      
      orders.unshift({
        id: newId,
        date: today,
        customer: customer.name,
        amount: netTotal,
        status: 'مكتمل'
      });
      
      // Update medicines quantity
      rows.forEach(row => {
        const medicineId = parseInt(row.dataset.medicineId);
        const quantity = parseInt(row.querySelector('.item-qty').textContent);
        
        const medicineIndex = medicines.findIndex(m => m.id === medicineId);
        if (medicineIndex !== -1) {
          medicines[medicineIndex].quantity -= quantity;
        }
      });
      
      // Show success message
      showAlert('success', `تم إنشاء الفاتورة #${newId} بنجاح`);
      
      // Reset form
      document.getElementById('clearOrderBtn').click();
      
      // Update dashboard
      renderData();
    });
    
    // Clear order form
    document.getElementById('clearOrderBtn').addEventListener('click', () => {
      document.getElementById('orderCustomer').value = '';
      document.querySelector('#orderItems tbody').innerHTML = '';
      document.getElementById('orderTotal').textContent = '0.00 ج.م';
      document.getElementById('orderDiscount').value = '0';
      document.getElementById('orderNetTotal').textContent = '0.00 ج.م';
    });
    
    // Add medicine to prescription
    document.getElementById('addMedicineToPrescriptionBtn').addEventListener('click', () => {
      const medicineId = parseInt(document.getElementById('prescriptionMedicine').value);
      const dosage = document.getElementById('prescriptionDosage').value;
      const duration = document.getElementById('prescriptionDuration').value;
      
      if (!medicineId || !dosage || !duration) {
        showAlert('error', 'الرجاء اختيار دواء وإدخال الجرعة والمدة');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showAlert('error', 'الدواء المحدد غير موجود');
        return;
      }
      
      const prescriptionItemsBody = document.querySelector('#prescriptionItems tbody');
      const newRow = document.createElement('tr');
      newRow.dataset.medicineId = medicineId;
      newRow.innerHTML = `
        <td>${medicine.name}</td>
        <td>${dosage}</td>
        <td>${duration}</td>
        <td class="actions-cell">
          <button class="btn btn-sm btn-danger" onclick="removePrescriptionItem(this)">
            <span class="material-icons">delete</span>
          </button>
        </td>
      `;
      prescriptionItemsBody.appendChild(newRow);
    });
    
    // Remove prescription item
    window.removePrescriptionItem = function(button) {
      button.closest('tr').remove();
    };
    
    // Save prescription
    document.getElementById('savePrescriptionBtn').addEventListener('click', () => {
      const customerId = parseInt(document.getElementById('prescriptionCustomer').value);
      const doctor = document.getElementById('prescriptionDoctor').value;
      const date = document.getElementById('prescriptionDate').value;
      const notes = document.getElementById('prescriptionNotes').value;
      const items = Array.from(document.querySelectorAll('#prescriptionItems tbody tr')).map(row => ({
        medicine: row.cells[0].textContent,
        dosage: row.cells[1].textContent,
        duration: row.cells[2].textContent
      }));
      
      if (!customerId || !doctor || !date || items.length === 0) {
        showAlert('error', 'الرجاء ملء جميع الحقول المطلوبة وإضافة أدوية للوصفة');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showAlert('error', 'العميل المحدد غير موجود');
        return;
      }
      
      // Create new prescription
      const newId = prescriptions.length > 0 ? Math.max(...prescriptions.map(p => p.id)) + 1 : 5001;
      
      prescriptions.unshift({
        id: newId,
        date,
        customer: customer.name,
        doctor,
        notes,
        items
      });
      
      // Show success message
      showAlert('success', `تم حفظ الوصفة #${newId} بنجاح`);
      
      // Reset form
      document.getElementById('clearPrescriptionBtn').click();
      
      // Update dashboard
      renderData();
    });
    
    // Clear prescription form
    document.getElementById('clearPrescriptionBtn').addEventListener('click', () => {
      document.getElementById('prescriptionCustomer').value = '';
      document.getElementById('prescriptionDoctor').value = '';
      document.getElementById('prescriptionDate').value = '';
      document.getElementById('prescriptionNotes').value = '';
      document.querySelector('#prescriptionItems tbody').innerHTML = '';
    });
    
    // Save settings
    document.getElementById('saveSettingsBtn').addEventListener('click', () => {
      const name = document.getElementById('pharmacyName').value;
      const address = document.getElementById('pharmacyAddress').value;
      const phone = document.getElementById('pharmacyPhone').value;
      const email = document.getElementById('pharmacyEmail').value;
      const taxNumber = document.getElementById('pharmacyTaxNumber').value;
      
      if (!name || !address || !phone) {
        showAlert('error', 'الرجاء إدخال اسم الصيدلية والعنوان ورقم الهاتف');
        return;
      }
      
      // Save settings to localStorage
      localStorage.setItem('pharmacyName', name);
      localStorage.setItem('pharmacyAddress', address);
      localStorage.setItem('pharmacyPhone', phone);
      localStorage.setItem('pharmacyEmail', email);
      localStorage.setItem('pharmacyTaxNumber', taxNumber);
      
      // Save system settings
      localStorage.setItem('enableNotifications', document.getElementById('enableNotifications').checked);
      localStorage.setItem('enableExpiryAlerts', document.getElementById('enableExpiryAlerts').checked);
      localStorage.setItem('enableLowStockAlerts', document.getElementById('enableLowStockAlerts').checked);
      localStorage.setItem('enableDarkMode', document.getElementById('enableDarkMode').checked);
      localStorage.setItem('backupFrequency', document.getElementById('backupFrequency').value);
      
      showAlert('success', 'تم حفظ الإعدادات بنجاح');
    });
    
    // Load settings
    function loadSettings() {
      document.getElementById('pharmacyName').value = localStorage.getItem('pharmacyName') || '';
      document.getElementById('pharmacyAddress').value = localStorage.getItem('pharmacyAddress') || '';
      document.getElementById('pharmacyPhone').value = localStorage.getItem('pharmacyPhone') || '';
      document.getElementById('pharmacyEmail').value = localStorage.getItem('pharmacyEmail') || '';
      document.getElementById('pharmacyTaxNumber').value = localStorage.getItem('pharmacyTaxNumber') || '';
      
      document.getElementById('enableNotifications').checked = localStorage.getItem('enableNotifications') !== 'false';
      document.getElementById('enableExpiryAlerts').checked = localStorage.getItem('enableExpiryAlerts') !== 'false';
      document.getElementById('enableLowStockAlerts').checked = localStorage.getItem('enableLowStockAlerts') !== 'false';
      document.getElementById('enableDarkMode').checked = localStorage.getItem('enableDarkMode') === 'true';
      document.getElementById('backupFrequency').value = localStorage.getItem('backupFrequency') || 'weekly';
    }
    
    // Backup now
    document.getElementById('backupNowBtn').addEventListener('click', () => {
      const backupData = {
        medicines,
        suppliers,
        customers,
        orders,
        prescriptions,
        settings: {
          pharmacyName: localStorage.getItem('pharmacyName'),
          pharmacyAddress: localStorage.getItem('pharmacyAddress'),
          pharmacyPhone: localStorage.getItem('pharmacyPhone'),
          pharmacyEmail: localStorage.getItem('pharmacyEmail'),
          pharmacyTaxNumber: localStorage.getItem('pharmacyTaxNumber')
        },
        timestamp: new Date().toISOString()
      };
      
      localStorage.setItem('pharmacyBackup', JSON.stringify(backupData));
      showAlert('success', 'تم إنشاء نسخة احتياطية بنجاح');
    });
    
    // Restore backup
    document.getElementById('restoreBackupBtn').addEventListener('click', () => {
      const backupData = localStorage.getItem('pharmacyBackup');
      if (!backupData) {
        showAlert('error', 'لا توجد نسخة احتياطية متاحة');
        return;
      }
      
      if (confirm('هل أنت متأكد من استعادة النسخة الاحتياطية؟ سيتم استبدال جميع البيانات الحالية.')) {
        try {
          const data = JSON.parse(backupData);
          
          medicines = data.medicines || [];
          suppliers = data.suppliers || [];
          customers = data.customers || [];
          orders = data.orders || [];
          prescriptions = data.prescriptions || [];
          
          if (data.settings) {
            localStorage.setItem('pharmacyName', data.settings.pharmacyName || '');
            localStorage.setItem('pharmacyAddress', data.settings.pharmacyAddress || '');
            localStorage.setItem('pharmacyPhone', data.settings.pharmacyPhone || '');
            localStorage.setItem('pharmacyEmail', data.settings.pharmacyEmail || '');
            localStorage.setItem('pharmacyTaxNumber', data.settings.pharmacyTaxNumber || '');
          }
          
          renderData();
          loadSettings();
          showAlert('success', 'تم استعادة النسخة الاحتياطية بنجاح');
        } catch (error) {
          showAlert('error', 'حدث خطأ أثناء استعادة النسخة الاحتياطية');
        }
      }
    });
    
    // Print order
    window.printOrder = function(orderId) {
      const order = orders.find(o => o.id === orderId);
      if (!order) return;
      
      const printContent = document.getElementById('printContent');
      printContent.innerHTML = `
        <div style="text-align: center; margin-bottom: 20px;">
          <h2>${localStorage.getItem('pharmacyName') || 'صيدليتنا'}</h2>
          <p>${localStorage.getItem('pharmacyAddress') || ''}</p>
          <p>هاتف: ${localStorage.getItem('pharmacyPhone') || ''}</p>
        </div>
        
        <div style="margin-bottom: 30px;">
          <h3 style="text-align: center; border-bottom: 1px solid #ddd; padding-bottom: 10px;">فاتورة بيع</h3>
          <div style="display: flex; justify-content: space-between; margin-bottom: 15px;">
            <div>
              <p><strong>رقم الفاتورة:</strong> #${order.id}</p>
              <p><strong>التاريخ:</strong> ${order.date}</p>
            </div>
            <div>
              <p><strong>العميل:</strong> ${order.customer}</p>
            </div>
          </div>
          
          <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
            <thead>
              <tr style="background-color: #f5f5f5;">
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">الدواء</th>
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">الكمية</th>
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">السعر</th>
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">الإجمالي</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td style="padding: 10px; border: 1px solid #ddd;">أدوية متنوعة</td>
                <td style="padding: 10px; border: 1px solid #ddd;">1</td>
                <td style="padding: 10px; border: 1px solid #ddd;">${order.amount.toFixed(2)} ج.م</td>
                <td style="padding: 10px; border: 1px solid #ddd;">${order.amount.toFixed(2)} ج.م</td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" style="padding: 10px; text-align: left; border: 1px solid #ddd;"><strong>المجموع:</strong></td>
                <td style="padding: 10px; border: 1px solid #ddd;"><strong>${order.amount.toFixed(2)} ج.م</strong></td>
              </tr>
            </tfoot>
          </table>
          
          <div style="text-align: left; margin-top: 30px;">
            <p>شكراً لزيارتكم</p>
            <p>${localStorage.getItem('pharmacyName') || 'صيدليتنا'}</p>
          </div>
        </div>
      `;
      
      document.getElementById('printModal').style.display = 'block';
      setTimeout(() => {
        const printWindow = window.open('', '', 'width=800,height=600');
        printWindow.document.write(`
          <html>
            <head>
              <title>فاتورة #${order.id}</title>
              <style>
                body { font-family: Arial, sans-serif; margin: 0; padding: 20px; direction: rtl; }
                table { width: 100%; border-collapse: collapse; }
                th, td { padding: 10px; text-align: right; border: 1px solid #ddd; }
                h2, h3 { text-align: center; }
              </style>
            </head>
            <body>
              ${printContent.innerHTML}
              <script>
                window.onload = function() {
                  window.print();
                  setTimeout(function() {
                    window.close();
                  }, 100);
                };
              </script>
            </body>
          </html>
        `);
        printWindow.document.close();
      }, 100);
    };
    
    // Print prescription
    window.printPrescription = function(prescriptionId) {
      const prescription = prescriptions.find(p => p.id === prescriptionId);
      if (!prescription) return;
      
      const printContent = document.getElementById('printContent');
      printContent.innerHTML = `
        <div style="text-align: center; margin-bottom: 20px;">
          <h2>${localStorage.getItem('pharmacyName') || 'صيدليتنا'}</h2>
          <p>${localStorage.getItem('pharmacyAddress') || ''}</p>
          <p>هاتف: ${localStorage.getItem('pharmacyPhone') || ''}</p>
        </div>
        
        <div style="margin-bottom: 30px;">
          <h3 style="text-align: center; border-bottom: 1px solid #ddd; padding-bottom: 10px;">وصفة طبية</h3>
          <div style="display: flex; justify-content: space-between; margin-bottom: 15px;">
            <div>
              <p><strong>رقم الوصفة:</strong> #${prescription.id}</p>
              <p><strong>التاريخ:</strong> ${prescription.date}</p>
            </div>
            <div>
              <p><strong>العميل:</strong> ${prescription.customer}</p>
              <p><strong>الطبيب:</strong> ${prescription.doctor}</p>
            </div>
          </div>
          
          <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
            <thead>
              <tr style="background-color: #f5f5f5;">
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">الدواء</th>
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">الجرعة</th>
                <th style="padding: 10px; text-align: right; border: 1px solid #ddd;">المدة</th>
              </tr>
            </thead>
            <tbody>
              ${prescription.items.map(item => `
                <tr>
                  <td style="padding: 10px; border: 1px solid #ddd;">${item.medicine}</td>
                  <td style="padding: 10px; border: 1px solid #ddd;">${item.dosage}</td>
                  <td style="padding: 10px; border: 1px solid #ddd;">${item.duration}</td>
                </tr>
              `).join('')}
            </tbody>
          </table>
          
          ${prescription.notes ? `
            <div style="margin-bottom: 20px;">
              <h4 style="border-bottom: 1px solid #ddd; padding-bottom: 5px;">ملاحظات:</h4>
              <p>${prescription.notes}</p>
            </div>
          ` : ''}
          
          <div style="text-align: left; margin-top: 30px;">
            <p>شكراً لزيارتكم</p>
            <p>${localStorage.getItem('pharmacyName') || 'صيدليتنا'}</p>
          </div>
        </div>
      `;
      
      document.getElementById('printModal').style.display = 'block';
      setTimeout(() => {
        const printWindow = window.open('', '', 'width=800,height=600');
        printWindow.document.write(`
          <html>
            <head>
              <title>وصفة طبية #${prescription.id}</title>
              <style>
                body { font-family: Arial, sans-serif; margin: 0; padding: 20px; direction: rtl; }
                table { width: 100%; border-collapse: collapse; }
                th, td { padding: 10px; text-align: right; border: 1px solid #ddd; }
                h2, h3 { text-align: center; }
              </style>
            </head>
            <body>
              ${printContent.innerHTML}
              <script>
                window.onload = function() {
                  window.print();
                  setTimeout(function() {
                    window.close();
                  }, 100);
                };
              </script>
            </body>
          </html>
        `);
        printWindow.document.close();
      }, 100);
    };
    
    // Initialize the app
    document.addEventListener('DOMContentLoaded', () => {
      loadSampleData();
      loadSettings();
      initCharts();
      renderData();
      checkExpiryMedicines();
      
      // Set up event listeners for print buttons
      document.getElementById('printOrderBtn').addEventListener('click', () => {
        // This would print the current order being created
        showAlert('info', 'سيتم تنفيذ هذه الوظيفة في النسخة الكاملة');
      });
      
      document.getElementById('printPrescriptionBtn').addEventListener('click', () => {
        // This would print the current prescription being created
        showAlert('info', 'سيتم تنفيذ هذه الوظيفة في النسخة الكاملة');
      });
    });
    
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
      
      if (expiringSoon.length > 0 && localStorage.getItem('enableExpiryAlerts') !== 'false') {
        showAlert('warning', `هناك ${expiringSoon.length} دواء ستنتهي صلاحيته خلال الشهر القادم`);
        document.getElementById('expiryBadge').textContent = expiringSoon.length;
        document.getElementById('expiryBadge').style.display = 'flex';
      }
      
      if (expired.length > 0 && localStorage.getItem('enableExpiryAlerts') !== 'false') {
        showAlert('error', `تحذير: هناك ${expired.length} دواء منتهي الصلاحية`);
      }
      
      // Update dashboard stats
      document.getElementById('med-count').textContent = medicines.length;
      document.getElementById('expiring-soon').textContent = expiringSoon.length;
      document.getElementById('expired').textContent = expired.length;
      
      // Calculate total sales
      const totalSales = orders.reduce((sum, order) => sum + order.amount, 0);
      document.getElementById('sales').textContent = totalSales.toFixed(2);
    }
    
    // Edit functions (to be implemented)
    window.editMedicine = function(id) {
      showAlert('info', 'سيتم تنفيذ وظيفة التعديل في النسخة الكاملة');
    };
    
    window.deleteMedicine = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
        medicines = medicines.filter(med => med.id !== id);
        renderData();
        showAlert('success', 'تم حذف الدواء بنجاح');
      }
    };
    
    window.editSupplier = function(id) {
      showAlert('info', 'سيتم تنفيذ وظيفة التعديل في النسخة الكاملة');
    };
    
    window.deleteSupplier = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا المورد؟')) {
        suppliers = suppliers.filter(supplier => supplier.id !== id);
        renderData();
        showAlert('success', 'تم حذف المورد بنجاح');
      }
    };
    
    window.editCustomer = function(id) {
      showAlert('info', 'سيتم تنفيذ وظيفة التعديل في النسخة الكاملة');
    };
    
    window.deleteCustomer = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا العميل؟')) {
        customers = customers.filter(customer => customer.id !== id);
        renderData();
        showAlert('success', 'تم حذف العميل بنجاح');
      }
    };
    
    window.viewOrder = function(id) {
      showAlert('info', 'سيتم تنفيذ وظيفة العرض في النسخة الكاملة');
    };
    
    window.viewPrescription = function(id) {
      showAlert('info', 'سيتم تنفيذ وظيفة العرض في النسخة الكاملة');
    };
  </script>
</body>
</html>
