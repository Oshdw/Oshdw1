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
      --sidebar-overlay: rgba(0,0,0,0.5);
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
      position: relative;
    }

    body.dark {
      background-color: var(--background-dark);
      color: var(--text-light);
    }

    body.sidebar-open {
      overflow: hidden;
    }

    .sidebar-overlay {
      position: fixed;
      top: 0;
      right: 0;
      width: 100%;
      height: 100%;
      background-color: var(--sidebar-overlay);
      z-index: 999;
      display: none;
      transition: opacity 0.3s;
    }

    .sidebar-overlay.visible {
      display: block;
      opacity: 1;
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
      box-shadow: -2px 0 10px rgba(0,0,0,0.2);
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
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
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
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }

    .btn:active {
      transform: translateY(0);
      box-shadow: none;
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
      border-radius: 8px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
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
      position: sticky;
      top: 0;
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
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { transform: translateX(20px); opacity: 0; }
      to { transform: translateX(0); opacity: 1; }
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
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .stat-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
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
      background-color: var(--card-light);
      padding: 1rem;
      border-radius: 10px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    body.dark .chart-container {
      background-color: var(--card-dark);
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
      transition: background-color 0.2s, transform 0.2s;
    }

    .calculator-btn:hover {
      background-color: var(--primary-dark);
      transform: translateY(-2px);
    }

    .calculator-btn:active {
      transform: translateY(0);
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

    .quick-sale-item {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin-bottom: 1rem;
      padding: 1rem;
      background-color: var(--card-light);
      border-radius: 8px;
      transition: transform 0.2s;
    }

    body.dark .quick-sale-item {
      background-color: var(--card-dark);
    }

    .quick-sale-item:hover {
      transform: translateY(-3px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }

    .quick-sale-item img {
      width: 50px;
      height: 50px;
      object-fit: cover;
      border-radius: 8px;
    }

    .quick-sale-item-details {
      flex: 1;
    }

    .quick-sale-item-actions {
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .quick-sale-item-actions input {
      width: 60px;
      text-align: center;
      padding: 0.5rem;
    }

    .user-profile {
      display: flex;
      align-items: center;
      gap: 1rem;
      padding: 1rem;
      margin-bottom: 1.5rem;
      background-color: var(--card-light);
      border-radius: 10px;
    }

    body.dark .user-profile {
      background-color: var(--card-dark);
    }

    .user-profile img {
      width: 80px;
      height: 80px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid var(--primary);
    }

    .user-profile-info h4 {
      margin-bottom: 0.5rem;
      color: var(--primary);
    }

    .user-profile-info p {
      color: var(--text-gray);
      margin-bottom: 0.5rem;
    }

    .user-stats {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1rem;
      margin-bottom: 1.5rem;
    }

    .user-stat {
      background-color: var(--card-light);
      padding: 1rem;
      border-radius: 8px;
      text-align: center;
    }

    body.dark .user-stat {
      background-color: var(--card-dark);
    }

    .user-stat h5 {
      color: var(--primary);
      margin-bottom: 0.5rem;
    }

    .user-stat p {
      font-size: 1.2rem;
      font-weight: bold;
    }

    .admin-controls {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 1.5rem;
      margin-top: 1.5rem;
    }

    .admin-control-card {
      background-color: var(--card-light);
      padding: 1.5rem;
      border-radius: 10px;
      text-align: center;
      cursor: pointer;
      transition: all 0.3s;
    }

    body.dark .admin-control-card {
      background-color: var(--card-dark);
    }

    .admin-control-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
    }

    .admin-control-card .material-icons {
      font-size: 2.5rem;
      color: var(--primary);
      margin-bottom: 1rem;
    }

    .admin-control-card h4 {
      margin-bottom: 0.5rem;
    }

    .admin-control-card p {
      color: var(--text-gray);
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

      .grid, .user-stats, .admin-controls {
        grid-template-columns: 1fr;
      }
      
      .calculator-container {
        grid-template-columns: repeat(4, 1fr);
      }

      .user-profile {
        flex-direction: column;
        text-align: center;
      }
    }

    @media (max-width: 480px) {
      .calculator-btn {
        padding: 0.8rem;
        font-size: 1rem;
      }

      .search-container {
        flex-direction: column;
      }

      .quick-sale-item {
        flex-direction: column;
        text-align: center;
      }

      .quick-sale-item-actions {
        width: 100%;
        justify-content: center;
      }
    }
  </style>
</head>
<body>
  <div class="sidebar-overlay" id="sidebarOverlay"></div>
  
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
      <button id="quickSaleBtn">
        <span class="material-icons">local_pharmacy</span>
        بيع مباشر
      </button>
      <button id="calculatorBtn">
        <span class="material-icons">calculate</span>
        الآلة الحاسبة
      </button>
      <button id="statisticsBtn">
        <span class="material-icons">bar_chart</span>
        الإحصائيات
      </button>
      <button id="adminBtn">
        <span class="material-icons">admin_panel_settings</span>
        لوحة التحكم
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
        <button class="topbar-action-btn" id="userMenuBtn">
          <span class="material-icons">account_circle</span>
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
          <div class="value" id="totalSales">0 ج.س</div>
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
      <!-- محتوى صفحة الأدوية -->
    </div>

    <div id="suppliers" class="page">
      <!-- محتوى صفحة الموردين -->
    </div>

    <div id="customers" class="page">
      <!-- محتوى صفحة العملاء -->
    </div>

    <div id="orders" class="page">
      <!-- محتوى صفحة الفواتير -->
    </div>

    <div id="prescriptions" class="page">
      <!-- محتوى صفحة الوصفات الطبية -->
    </div>

    <div id="quickSale" class="page">
      <div class="card">
        <h3>البيع المباشر</h3>
        <div class="search-container">
          <input type="text" id="quickSaleSearch" class="form-control" placeholder="ابحث عن دواء...">
          <button class="btn btn-info" id="quickSaleSearchBtn">
            <span class="material-icons">search</span>
            بحث
          </button>
        </div>
        
        <div id="quickSaleItems">
          <!-- سيتم ملؤها بالبيانات -->
        </div>
        
        <div class="card" style="margin-top: 1.5rem;">
          <h3>فاتورة البيع المباشر</h3>
          <div class="table-responsive">
            <table id="quickSaleTable">
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
                  <td id="quickSaleTotal" style="font-weight: bold;">0 ج.س</td>
                  <td></td>
                </tr>
              </tfoot>
            </table>
          </div>
          
          <button id="completeQuickSaleBtn" class="btn btn-success" style="margin-top: 1.5rem;">
            <span class="material-icons">check_circle</span>
            إتمام البيع
          </button>
        </div>
      </div>
    </div>

    <div id="calculator" class="page">
      <!-- محتوى صفحة الآلة الحاسبة -->
    </div>

    <div id="statistics" class="page">
      <!-- محتوى صفحة الإحصائيات -->
    </div>

    <div id="admin" class="page">
      <div class="card">
        <div class="user-profile">
          <img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iIzE5NzZkMiIgZD0iTTEyLDJBMTAsMTAgMCAwLDAgMiwxMkExMCwxMCAwIDAsMCAxMiwyMkExMCwxMCAwIDAsMCAyMiwxMkExMCwxMCAwIDAsMCAxMiwyTTEyLDRBOCw4IDAgMCwxIDIwLDEyQTgsOCAwIDAsMSAxMiwyMEE4LDggMCAwLDEgNCwxMkE4LDggMCAwLDEgMTIsNE0xMiw2QTIsMiAwIDAsMCAxMCw4QTIsMiAwIDAsMCAxMiwxMEEyLDIgMCAwLDAgMTQsOEEyLDIgMCAwLDAgMTIsNk0xMiwxM0E1LDUgMCAwLDAgNywxOEE1LDUgMCAwLDAgMTIsMjNBNSw1IDAgMCwwIDE3LDE4QTUsNSAwIDAsMCAxMiwxM1oiIC8+PC9zdmc+" alt="صورة المستخدم">
          <div class="user-profile-info">
            <h4>مدير النظام</h4>
            <p>آخر دخول: اليوم في 10:30 ص</p>
            <p>صلاحية: مدير</p>
          </div>
        </div>
        
        <div class="user-stats">
          <div class="user-stat">
            <h5>عدد المستخدمين</h5>
            <p>3</p>
          </div>
          <div class="user-stat">
            <h5>آخر نشاط</h5>
            <p>منذ 5 دقائق</p>
          </div>
          <div class="user-stat">
            <h5>حالة النظام</h5>
            <p>يعمل بكفاءة</p>
          </div>
        </div>
        
        <h3>أدوات التحكم</h3>
        <div class="admin-controls">
          <div class="admin-control-card" onclick="navigateTo('medicines')">
            <span class="material-icons">medication</span>
            <h4>إدارة الأدوية</h4>
            <p>إضافة وتعديل وحذف الأدوية</p>
          </div>
          <div class="admin-control-card" onclick="navigateTo('customers')">
            <span class="material-icons">people</span>
            <h4>إدارة العملاء</h4>
            <p>إدارة بيانات العملاء</p>
          </div>
          <div class="admin-control-card" onclick="navigateTo('orders')">
            <span class="material-icons">receipt</span>
            <h4>إدارة الفواتير</h4>
            <p>عرض وتعديل الفواتير</p>
          </div>
          <div class="admin-control-card" onclick="navigateTo('statistics')">
            <span class="material-icons">bar_chart</span>
            <h4>الإحصائيات</h4>
            <p>عرض تقارير المبيعات</p>
          </div>
          <div class="admin-control-card" onclick="navigateTo('settings')">
            <span class="material-icons">settings</span>
            <h4>إعدادات النظام</h4>
            <p>تخصيص إعدادات الصيدلية</p>
          </div>
          <div class="admin-control-card" onclick="showBackupModal()">
            <span class="material-icons">backup</span>
            <h4>النسخ الاحتياطي</h4>
            <p>إنشاء واستعادة النسخ</p>
          </div>
        </div>
      </div>
    </div>

    <div id="settings" class="page">
      <!-- محتوى صفحة الإعدادات -->
    </div>

    <div class="signature">
      <p>نظام إدارة الصيدلية &copy; <span id="currentYear"></span></p>
      <p>تمت البرمجة بواسطة عمر بابكر - Omer Bk</p>
    </div>
  </div>
<!-- استمرار لصفحة HTML -->

    <!-- Modal للنسخ الاحتياطي -->
    <div id="backupModal" style="position: fixed; top: 0; right: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 2000;">
      <div style="background-color: var(--card-light); padding: 2rem; border-radius: 10px; max-width: 500px; width: 90%;">
        <h3 style="margin-bottom: 1.5rem; color: var(--primary);">النسخ الاحتياطي</h3>
        
        <div style="margin-bottom: 1.5rem;">
          <button id="createBackupBtn" class="btn btn-info" style="width: 100%; margin-bottom: 1rem;">
            <span class="material-icons">backup</span>
            إنشاء نسخة احتياطية
          </button>
          
          <div style="margin-bottom: 1rem;">
            <label for="restoreFile" style="display: block; margin-bottom: 0.5rem; font-weight: 500; color: var(--primary);">استعادة نسخة احتياطية</label>
            <input type="file" id="restoreFile" class="form-control" style="margin-bottom: 1rem;">
            <button id="restoreBackupBtn" class="btn btn-warning" style="width: 100%;">
              <span class="material-icons">restore</span>
              استعادة البيانات
            </button>
          </div>
        </div>
        
        <button onclick="document.getElementById('backupModal').style.display = 'none'" class="btn" style="width: 100%;">
          <span class="material-icons">close</span>
          إغلاق
        </button>
      </div>
    </div>

    <!-- Modal قائمة المستخدم -->
    <div id="userMenuModal" style="position: fixed; top: 60px; left: 10px; background-color: var(--card-light); border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); z-index: 2000; display: none; min-width: 200px;">
      <div style="padding: 1rem; border-bottom: 1px solid var(--border-light);">
        <div style="display: flex; align-items: center; gap: 0.5rem; margin-bottom: 0.5rem;">
          <span class="material-icons" style="color: var(--primary);">account_circle</span>
          <span>مدير النظام</span>
        </div>
        <div style="font-size: 0.9rem; color: var(--text-gray);">admin@example.com</div>
      </div>
      <div style="padding: 0.5rem 0;">
        <button onclick="navigateTo('settings')" style="width: 100%; text-align: right; padding: 0.75rem 1rem; background: none; border: none; cursor: pointer; display: flex; align-items: center; gap: 0.5rem; color: var(--text-dark);">
          <span class="material-icons">settings</span>
          الإعدادات
        </button>
        <button onclick="logout()" style="width: 100%; text-align: right; padding: 0.75rem 1rem; background: none; border: none; cursor: pointer; display: flex; align-items: center; gap: 0.5rem; color: var(--danger);">
          <span class="material-icons">logout</span>
          تسجيل الخروج
        </button>
      </div>
    </div>

    <!-- Modal تأكيد البيع -->
    <div id="confirmSaleModal" style="position: fixed; top: 0; right: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 2000;">
      <div style="background-color: var(--card-light); padding: 2rem; border-radius: 10px; max-width: 400px; width: 90%;">
        <h3 style="margin-bottom: 1.5rem; color: var(--primary); text-align: center;">تأكيد عملية البيع</h3>
        
        <div style="margin-bottom: 1.5rem; text-align: center;">
          <p style="margin-bottom: 1rem;">هل أنت متأكد من إتمام عملية البيع؟</p>
          <p style="font-weight: bold; font-size: 1.2rem;">المبلغ الإجمالي: <span id="confirmSaleTotal">0</span> ج.س</p>
        </div>
        
        <div style="display: flex; gap: 1rem;">
          <button onclick="document.getElementById('confirmSaleModal').style.display = 'none'" class="btn btn-danger" style="flex: 1;">
            <span class="material-icons">close</span>
            إلغاء
          </button>
          <button id="finalConfirmSaleBtn" class="btn btn-success" style="flex: 1;">
            <span class="material-icons">check</span>
            تأكيد
          </button>
        </div>
      </div>
    </div>

    <!-- Modal إشعارات -->
    <div id="notificationsModal" style="position: fixed; top: 60px; left: 10px; background-color: var(--card-light); border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.15); z-index: 2000; display: none; width: 300px; max-height: 400px; overflow-y: auto;">
      <div style="padding: 1rem; border-bottom: 1px solid var(--border-light); display: flex; justify-content: space-between; align-items: center;">
        <h4 style="color: var(--primary);">الإشعارات</h4>
        <button onclick="document.getElementById('notificationsModal').style.display = 'none'" style="background: none; border: none; cursor: pointer; color: var(--text-gray);">
          <span class="material-icons">close</span>
        </button>
      </div>
      <div id="notificationsList" style="padding: 0.5rem;">
        <div style="padding: 0.75rem; border-bottom: 1px solid var(--border-light); display: flex; gap: 0.75rem;">
          <span class="material-icons" style="color: var(--success);">inventory</span>
          <div>
            <p style="font-weight: 500;">طلب جديد #1005</p>
            <p style="font-size: 0.8rem; color: var(--text-gray);">منذ 5 دقائق</p>
          </div>
        </div>
        <div style="padding: 0.75rem; border-bottom: 1px solid var(--border-light); display: flex; gap: 0.75rem;">
          <span class="material-icons" style="color: var(--warning);">warning</span>
          <div>
            <p style="font-weight: 500;">أدوية قاربت على النفاذ</p>
            <p style="font-size: 0.8rem; color: var(--text-gray);">منذ ساعة</p>
          </div>
        </div>
        <div style="padding: 0.75rem; display: flex; gap: 0.75rem;">
          <span class="material-icons" style="color: var(--info);">event</span>
          <div>
            <p style="font-weight: 500;">موعد تجديد الرخصة</p>
            <p style="font-size: 0.8rem; color: var(--text-gray);">يومين مضوا</p>
          </div>
        </div>
      </div>
      <div style="padding: 0.75rem; text-align: center; border-top: 1px solid var(--border-light);">
        <button onclick="markAllAsRead()" style="background: none; border: none; color: var(--primary); cursor: pointer; font-weight: 500;">
          تعيين الكل كمقروء
        </button>
      </div>
    </div>

    <!-- Modal الأدوية منتهية الصلاحية -->
    <div id="expiredMedicinesModal" style="position: fixed; top: 0; right: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 2000;">
      <div style="background-color: var(--card-light); padding: 1.5rem; border-radius: 10px; max-width: 600px; width: 90%; max-height: 80vh; overflow-y: auto;">
        <h3 style="margin-bottom: 1.5rem; color: var(--danger); display: flex; align-items: center; gap: 0.5rem;">
          <span class="material-icons">warning</span>
          أدوية منتهية الصلاحية
        </h3>
        
        <div class="table-responsive">
          <table style="width: 100%;">
            <thead>
              <tr style="background-color: var(--danger); color: white;">
                <th style="padding: 0.75rem;">اسم الدواء</th>
                <th style="padding: 0.75rem;">تاريخ الانتهاء</th>
                <th style="padding: 0.75rem;">الكمية</th>
              </tr>
            </thead>
            <tbody id="expiredMedicinesList">
              <!-- سيتم ملؤها بالبيانات -->
            </tbody>
          </table>
        </div>
        
        <button onclick="document.getElementById('expiredMedicinesModal').style.display = 'none'" class="btn" style="margin-top: 1.5rem; width: 100%;">
          <span class="material-icons">close</span>
          إغلاق
        </button>
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
    let currentQuickSaleItems = [];
    let calculatorValue = '0';
    let calculatorPreviousValue = '0';
    let calculatorOperation = null;
    let currentUser = {
      name: "مدير النظام",
      email: "admin@example.com",
      role: "admin",
      lastLogin: new Date()
    };
    
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
      
      // معالج حدث فتح/إغلاق القائمة الجانبية
      document.getElementById('toggleSidebar').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('visible');
        document.getElementById('sidebarOverlay').classList.toggle('visible');
        document.body.classList.toggle('sidebar-open');
      });
      
      // معالج حدث النقر على overlay لإغلاق القائمة الجانبية
      document.getElementById('sidebarOverlay').addEventListener('click', function() {
        document.getElementById('sidebar').classList.remove('visible');
        this.classList.remove('visible');
        document.body.classList.remove('sidebar-open');
      });

      // معالج حدث لتبديل الوضع الليلي
      document.getElementById('toggleTheme').addEventListener('click', function() {
        document.body.classList.toggle('dark');
        localStorage.setItem('darkMode', document.body.classList.contains('dark'));
      });
      
      // معالج حدث قائمة المستخدم
      document.getElementById('userMenuBtn').addEventListener('click', function(e) {
        e.stopPropagation();
        const modal = document.getElementById('userMenuModal');
        modal.style.display = modal.style.display === 'block' ? 'none' : 'block';
      });
      
      // إغلاق قا��مة المستخدم عند النقر خارجها
      document.addEventListener('click', function() {
        document.getElementById('userMenuModal').style.display = 'none';
      });
      
      // تحميل وضع العرض المظلم من localStorage
      if (localStorage.getItem('darkMode') === 'true') {
        document.body.classList.add('dark');
      }
      
      // تعيين معالج الأحداث لأزرار التنقل
      const navButtons = {
        dashboard: document.getElementById('dashboardBtn'),
        medicines: document.getElementById('medicinesBtn'),
        suppliers: document.getElementById('suppliersBtn'),
        customers: document.getElementById('customersBtn'),
        orders: document.getElementById('ordersBtn'),
        prescriptions: document.getElementById('prescriptionsBtn'),
        quickSale: document.getElementById('quickSaleBtn'),
        calculator: document.getElementById('calculatorBtn'),
        statistics: document.getElementById('statisticsBtn'),
        admin: document.getElementById('adminBtn'),
        settings: document.getElementById('settingsBtn')
      };

      const pages = {
        dashboard: document.getElementById('dashboard'),
        medicines: document.getElementById('medicines'),
        suppliers: document.getElementById('suppliers'),
        customers: document.getElementById('customers'),
        orders: document.getElementById('orders'),
        prescriptions: document.getElementById('prescriptions'),
        quickSale: document.getElementById('quickSale'),
        calculator: document.getElementById('calculator'),
        statistics: document.getElementById('statistics'),
        admin: document.getElementById('admin'),
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
          
          // إغلاق القائمة الجانبية بعد اختيار الصفحة (للهواتف)
          if (window.innerWidth <= 768) {
            document.getElementById('sidebar').classList.remove('visible');
            document.getElementById('sidebarOverlay').classList.remove('visible');
            document.body.classList.remove('sidebar-open');
          }
          
          // إذا كانت صفحة الإحصائيات، قم بتحديث الرسوم البيانية
          if (key === 'statistics') {
            updateCharts();
          }
          
          // إذا كانت صفحة البيع المباشر، قم بتحديث قائمة الأدوية
          if (key === 'quickSale') {
            updateQuickSaleItems();
          }
        });
      });

      // معالج حدث إضافة دواء جديد
      document.getElementById('addMedicineBtn')?.addEventListener('click', addMedicine);
      
      // معالج حدث إضافة مورد جديد
      document.getElementById('addSupplierBtn')?.addEventListener('click', addSupplier);
      
      // معالج حدث إضافة عميل جديد
      document.getElementById('addCustomerBtn')?.addEventListener('click', addCustomer);
      
      // معالج حدث إضافة عنصر للفاتورة
      document.getElementById('addToOrderBtn')?.addEventListener('click', addToOrder);
      
      // معالج حدث حفظ الفاتورة
      document.getElementById('saveOrderBtn')?.addEventListener('click', saveOrder);
      
      // معالج حدث إضافة عنصر للوصفة
      document.getElementById('addToPrescriptionBtn')?.addEventListener('click', addToPrescription);
      
      // معالج حدث حفظ الوصفة
      document.getElementById('savePrescriptionBtn')?.addEventListener('click', savePrescription);
      
      // معالج حدث حفظ الإعدادات
      document.getElementById('saveSettingsBtn')?.addEventListener('click', saveSettings);
      
      // معالج حدث إنشاء نسخة احتياطية
      document.getElementById('createBackupBtn')?.addEventListener('click', createBackup);
      
      // معالج حدث استعادة النسخة الاحتياطية
      document.getElementById('restoreBackupBtn')?.addEventListener('click', restoreBackup);
      
      // معالج حدث توليد الإحصائيات
      document.getElementById('generateStatsBtn')?.addEventListener('click', generateStatistics);
      
      // معالج أحداث البحث
      document.getElementById('searchMedicineBtn')?.addEventListener('click', searchMedicines);
      document.getElementById('searchSupplierBtn')?.addEventListener('click', searchSuppliers);
      document.getElementById('searchCustomerBtn')?.addEventListener('click', searchCustomers);
      document.getElementById('searchOrderBtn')?.addEventListener('click', searchOrders);
      document.getElementById('searchPrescriptionBtn')?.addEventListener('click', searchPrescriptions);
      document.getElementById('quickSaleSearchBtn')?.addEventListener('click', searchQuickSaleItems);
      
      // معالج حدث إتمام البيع المباشر
      document.getElementById('completeQuickSaleBtn')?.addEventListener('click', confirmQuickSale);
      document.getElementById('finalConfirmSaleBtn')?.addEventListener('click', completeQuickSale);

      // تحميل البيانات الأولية
      loadSampleData();
      
      // تحديث الإحصائيات على لوحة التحكم
      updateDashboardStats();
      
      // تحديث شاشة الآلة الحاسبة
      updateCalculatorDisplay();
      
      // تحميل الإعدادات
      loadSettings();
      
      // التحقق من الأدوية المنتهية الصلاحية
      checkExpiredMedicines();
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
        description: description,
        barcode: generateBarcode()
      };
      
      medicines.push(newMedicine);
      updateMedicinesTable();
      updateSelectOptions();
      
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
      
      document.getElementById('supplierName').value = '';
      document.getElementById('supplierPhone').value = '';
      document.getElementById('supplierEmail').value = '';
      document.getElementById('supplierCompany').value = '';
      document.getElementById('supplierAddress').value = '';
      document.getElementById('supplierNotes').value = '';
      
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
      
      document.getElementById('customerName').value = '';
      document.getElementById('customerPhone').value = '';
      document.getElementById('customerAge').value = '';
      document.getElementById('customerGender').value = 'ذكر';
      document.getElementById('customerAddress').value = '';
      document.getElementById('customerMedicalHistory').value = '';
      
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
      
      currentOrderItems.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          medicine.quantity -= item.quantity;
        }
      });
      
      currentOrderItems = [];
      updateOrderItemsTable();
      
      document.getElementById('orderCustomer').value = '';
      document.getElementById('orderDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('orderNotes').value = '';
      
      updateOrdersTable();
      updateMedicinesTable();
      
      showAlert('success', 'تم حفظ الفاتورة بنجاح');
      
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
      
      currentPrescriptionItems = [];
      updatePrescriptionItemsTable();
      
      document.getElementById('prescriptionCustomer').value = '';
      document.getElementById('prescriptionDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('prescriptionDoctor').value = '';
      document.getElementById('prescriptionDiagnosis').value = '';
      document.getElementById('prescriptionNotes').value = '';
      
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
        invoiceFooter: invoiceFooter,
        currency: 'ج.س'
      };
      
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
      
      const filteredOrders = orders.filter(order => {
        return order.date >= fromDate && order.date <= toDate;
      });
      
      const totalSales = filteredOrders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('statsTotalSales').textContent = totalSales.toFixed(2) + ' ج.س';
      
      document.getElementById('statsTotalOrders').textContent = filteredOrders.length;
      
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
    
    function searchQuickSaleItems() {
      const query = document.getElementById('quickSaleSearch').value.toLowerCase();
      updateQuickSaleItems(query);
    }
    
    function updateQuickSaleItems(query = '') {
      const container = document.getElementById('quickSaleItems');
      container.innerHTML = '';
      
      let filteredMedicines = medicines;
      if (query) {
        filteredMedicines = medicines.filter(medicine => 
          medicine.name.toLowerCase().includes(query) ||
          (medicine.scientificName && medicine.scientificName.toLowerCase().includes(query))
        );
      }
      
      filteredMedicines.forEach(medicine => {
        if (medicine.quantity > 0) {
          const item = document.createElement('div');
          item.className = 'quick-sale-item';
          item.innerHTML = `
            <img src="https://via.placeholder.com/50" alt="${medicine.name}">
            <div class="quick-sale-item-details">
              <h4>${medicine.name}</h4>
              <p>${medicine.price.toFixed(2)} ج.س | متاح: ${medicine.quantity}</p>
            </div>
            <div class="quick-sale-item-actions">
              <input type="number" min="1" max="${medicine.quantity}" value="1" id="qty-${medicine.id}">
              <button class="btn btn-sm" onclick="addToQuickSale(${medicine.id})">
                <span class="material-icons">add</span>
              </button>
            </div>
          `;
          container.appendChild(item);
        }
      });
    }
    
    function addToQuickSale(medicineId) {
      const quantityInput = document.getElementById(`qty-${medicineId}`);
      const quantity = parseInt(quantityInput.value);
      
      if (!quantity || quantity < 1) {
        showAlert('error', 'الرجاء إدخال كمية صحيحة');
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
      
      const existingItem = currentQuickSaleItems.find(item => item.medicineId === medicineId);
      if (existingItem) {
        existingItem.quantity += quantity;
      } else {
        currentQuickSaleItems.push({
          medicineId: medicineId,
          name: medicine.name,
          price: medicine.price,
          quantity: quantity
        });
      }
      
      updateQuickSaleTable();
      quantityInput.value = '1';
    }
    
    function updateQuickSaleTable() {
      const tbody = document.querySelector('#quickSaleTable tbody');
      tbody.innerHTML = '';
      
      currentQuickSaleItems.forEach(item => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${item.name}</td>
          <td>${item.price.toFixed(2)} ج.س</td>
          <td>${item.quantity}</td>
          <td>${(item.price * item.quantity).toFixed(2)} ج.س</td>
          <td class="actions-cell">
            <button class="btn btn-sm btn-danger" onclick="removeFromQuickSale(${item.medicineId})">
              <span class="material-icons">delete</span>
            </button>
          </td>
        `;
        tbody.appendChild(row);
      });
      
      const total = currentQuickSaleItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
      document.getElementById('quickSaleTotal').textContent = total.toFixed(2) + ' ج.س';
    }
    
    function confirmQuickSale() {
      if (currentQuickSaleItems.length === 0) {
        showAlert('error', 'الرجاء إضافة أدوية للبيع');
        return;
      }
      
      const total = currentQuickSaleItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
      document.getElementById('confirmSaleTotal').textContent = total.toFixed(2);
      document.getElementById('confirmSaleModal').style.display = 'flex';
    }
    
    function completeQuickSale() {
      const total = currentQuickSaleItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
      
      const newOrder = {
        id: Date.now(),
        customerId: null,
        customerName: 'عميل مباشر',
        date: new Date().toISOString().split('T')[0],
        items: [...currentQuickSaleItems],
        total: total,
        notes: 'بيع مباشر'
      };
      
      orders.push(newOrder);
      
      currentQuickSaleItems.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          medicine.quantity -= item.quantity;
        }
      });
      
      currentQuickSaleItems = [];
      updateQuickSaleTable();
      updateQuickSaleItems();
      updateOrdersTable();
      updateMedicinesTable();
      
      document.getElementById('confirmSaleModal').style.display = 'none';
      showAlert('success', 'تم إتمام عملية البيع بنجاح');
      
      updateDashboardStats();
    }
    
    function removeFromQuickSale(medicineId) {
      currentQuickSaleItems = currentQuickSaleItems.filter(item => item.medicineId !== medicineId);
      updateQuickSaleTable();
    }
    
    function generateBarcode() {
      return 'BC' + Math.floor(100000 + Math.random() * 900000);
    }
    
    function checkExpiredMedicines() {
      const today = new Date().toISOString().split('T')[0];
      const expiredMedicines = medicines.filter(medicine => 
        medicine.expiryDate && medicine.expiryDate < today
      );
      
      if (expiredMedicines.length > 0) {
        const container = document.getElementById('expiredMedicinesList');
        container.innerHTML = '';
        
        expiredMedicines.forEach(medicine => {
          const row = document.createElement('tr');
          row.innerHTML = `
            <td>${medicine.name}</td>
            <td>${medicine.expiryDate}</td>
            <td>${medicine.quantity}</td>
          `;
          container.appendChild(row);
        });
        
        document.getElementById('expiredMedicinesModal').style.display = 'flex';
      }
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
      medicines = [
        { id: 1, name: 'بانادول', scientificName: 'Paracetamol', price: 10.50, purchasePrice: 8.00, quantity: 100, expiryDate: '2024-12-31', category: 'مسكنات', supplier: 1, description: 'مسكن للألم وخافض للحرارة', barcode: generateBarcode() },
        { id: 2, name: 'أموكسيسيلين', scientificName: 'Amoxicillin', price: 15.75, purchasePrice: 12.00, quantity: 50, expiryDate: '2024-10-15', category: 'مضادات حيوية', supplier: 2, description: 'مضاد حيوي واسع المدى', barcode: generateBarcode() },
        { id: 3, name: 'فولتارين', scientificName: 'Diclofenac', price: 12.00, purchasePrice: 9.50, quantity: 75, expiryDate: '2025-03-20', category: 'مضادات التهاب', supplier: 1, description: 'مضاد التهاب غير ستيرويدي', barcode: generateBarcode() },
        { id: 4, name: 'فيتامين سي', scientificName: 'Vitamin C', price: 8.25, purchasePrice: 6.00, quantity: 120, expiryDate: '2025-05-10', category: 'فيتامينات', supplier: 3, description: 'مكمل غذائي لفيتامين سي', barcode: generateBarcode() },
        { id: 5, name: 'لوسيك', scientificName: 'Omeprazole', price: 18.00, purchasePrice: 14.00, quantity: 60, expiryDate: '2024-11-30', category: 'أدوية مزمنة', supplier: 2, description: 'مثبط مضخة البروتون لعلاج الحموضة', barcode: generateBarcode() }
      ];
      
      suppliers = [
        { id: 1, name: 'شركة الأدوية المتحدة', phone: '0123456789', email: 'info@unitedpharma.com', company: 'الأدوية المتحدة', address: 'القاهرة، مصر', notes: 'مورد رئيسي للأدوية' },
        { id: 2, name: 'المصنع العربي للأدوية', phone: '0111222333', email: 'sales@arabianpharma.com', company: 'المصنع العربي', address: 'الإسكندرية، مصر', notes: 'متخصص في المضادات الحيوية' },
        { id: 3, name: 'الدلتا للمستح��رات الطبية', phone: '0100555666', email: 'contact@deltamed.com', company: 'دلتا ميد', address: 'المنصورة، مصر', notes: 'مورد للمكملات الغذائية' }
      ];
      
      customers = [
        { id: 1, name: 'أحمد محمد', phone: '01001234567', age: 35, gender: 'ذكر', address: 'شارع النصر، القاهرة', medicalHistory: 'حساسية من البنسلين' },
        { id: 2, name: 'مريم أحمد', phone: '01112345678', age: 28, gender: 'أنثى', address: 'حي الزهور، الجيزة', medicalHistory: 'ضغط دم مرتفع' },
        { id: 3, name: 'علي محمود', phone: '01223456789', age: 45, gender: 'ذكر', address: 'المعادي، القاهرة', medicalHistory: 'سكري النوع الثاني' },
        { id: 4, name: 'فاطمة إبراهيم', phone: '01501234567', age: 52, gender: 'أنثى', address: 'مدينة نصر، القاهرة', medicalHistory: 'هشاشة عظام' }
      ];
      
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
      
      settings = {
        pharmacyName: "صيدلية النور",
        pharmacyPhone: "0912345678",
        pharmacyEmail: "info@alnoor-pharmacy.com",
        pharmacyAddress: "الخرطوم، السودان",
        invoicePrefix: "INV-",
        invoiceStartingNumber: "1001",
        invoiceFooter: "شكراً لزيارتكم\nصيدلية النور - الخرطوم",
        currency: "ج.س"
      };
      
      loadSettings();
      
      updateMedicinesTable();
      updateSuppliersTable();
      updateCustomersTable();
      updateOrdersTable();
      updatePrescriptionsTable();
      updateSelectOptions();
      updateDashboardStats();
      updateQuickSaleItems();
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
          <td>${order.total.toFixed(2)} ج.س</td>
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
          <td>${item.price.toFixed(2)} ج.س</td>
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
      const supplierSelect = document.getElementById('medSupplier');
      if (supplierSelect) {
        supplierSelect.innerHTML = '<option value="">اختر المورد</option>';
        suppliers.forEach(supplier => {
          const option = document.createElement('option');
          option.value = supplier.id;
          option.textContent = supplier.name;
          supplierSelect.appendChild(option);
        });
      }
      
      const customerSelect = document.getElementById('orderCustomer');
      if (customerSelect) {
        customerSelect.innerHTML = '<option value="">اختر العميل</option>';
        customers.forEach(customer => {
          const option = document.createElement('option');
          option.value = customer.id;
          option.textContent = customer.name;
          customerSelect.appendChild(option);
        });
      }
      
      const medicineOrderSelect = document.getElementById('orderMedicine');
      if (medicineOrderSelect) {
        medicineOrderSelect.innerHTML = '<option value="">اختر الدواء</option>';
        medicines.forEach(medicine => {
          if (medicine.quantity > 0) {
            const option = document.createElement('option');
            option.value = medicine.id;
            option.textContent = `${medicine.name} (${medicine.quantity} متاح)`;
            medicineOrderSelect.appendChild(option);
          }
        });
      }
      
      const prescriptionCustomerSelect = document.getElementById('prescriptionCustomer');
      if (prescriptionCustomerSelect) {
        prescriptionCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';
        customers.forEach(customer => {
          const option = document.createElement('option');
          option.value = customer.id;
          option.textContent = customer.name;
          prescriptionCustomerSelect.appendChild(option);
        });
      }
      
      const medicinePrescriptionSelect = document.getElementById('prescriptionMedicine');
      if (medicinePrescriptionSelect) {
        medicinePrescriptionSelect.innerHTML = '<option value="">اختر الدواء</option>';
        medicines.forEach(medicine => {
          const option = document.createElement('option');
          option.value = medicine.id;
          option.textContent = medicine.name;
          medicinePrescriptionSelect.appendChild(option);
        });
      }
    }
    
    function updateDashboardStats() {
      document.getElementById('totalMedicines').textContent = medicines.length;
      document.getElementById('totalCustomers').textContent = customers.length;
      document.getElementById('totalOrders').textContent = orders.length;
      
      const totalSales = orders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('totalSales').textContent = totalSales.toFixed(2) + ' ج.س';
      
      const tbody = document.querySelector('#recentOrdersTable tbody');
      tbody.innerHTML = '';
      
      const recentOrders = [...orders].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
      recentOrders.forEach(order => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${order.id}</td>
          <td>${order.customerName}</td>
          <td>${order.date}</td>
          <td>${order.total.toFixed(2)} ج.س</td>
          <td>${order.total > 100 ? 'كبيرة' : 'صغيرة'}</td>
        `;
        tbody.appendChild(row);
      });
    }
    
    function updateCharts(filteredOrders = orders, medicineSales = {}) {
      const salesCtx = document.getElementById('salesChart').getContext('2d');
      
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
            description: description,
            barcode: medicine.barcode
          };
          
          updateMedicinesTable();
          updateSelectOptions();
          showAlert('success', 'تم تحديث الدواء بنجاح');
          
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة دواء';
          addBtn.onclick = addMedicine;
          
          document.getElementById('medName').value = '';
          document.getElementById('medScientificName').value = '';
          document.getElementById('medPrice').value = '';
          document.getElementById('medPurchasePrice').value = '';
          document.getElementById('medQuantity').value = '';
          document.getElementById('medExpiryDate').value = '';
          document.getElementById('medCategory').value = '';
          document.getElementById('medSupplier').value = '';
          document.getElementById('medDescription').value = '';
          
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
          
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة مورد';
          addBtn.onclick = addSupplier;
          
          document.getElementById('supplierName').value = '';
          document.getElementById('supplierPhone').value = '';
          document.getElementById('supplierEmail').value = '';
          document.getElementById('supplierCompany').value = '';
          document.getElementById('supplierAddress').value = '';
          document.getElementById('supplierNotes').value = '';
          
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
          
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة عميل';
          addBtn.onclick = addCustomer;
          
          document.getElementById('customerName').value = '';
          document.getElementById('customerPhone').value = '';
          document.getElementById('customerAge').value = '';
          document.getElementById('customerGender').value = 'ذكر';
          document.getElementById('customerAddress').value = '';
          document.getElementById('customerMedicalHistory').value = '';
          
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
            <td>${item.price.toFixed(2)} ج.س</td>
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
    
    window.navigateTo = function(pageId) {
      document.getElementById(`${pageId}Btn`).click();
    };
    
    window.showBackupModal = function() {
      document.getElementById('backupModal').style.display = 'flex';
    };
    
    window.logout = function() {
      if (confirm('هل أنت متأكد من تسجيل الخروج؟')) {
        window.location.reload();
      }
    };
    
    window.markAllAsRead = function() {
      document.getElementById('notificationsModal').style.display = 'none';
      showAlert('success', 'تم تعيين جميع الإشعارات كمقروءة');
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
    
    // ========== دوال الآلة الحاسبة ==========
    
    window.appendToCalculator = function(value) {
      if (calculatorValue === '0' && value !== '.') {
        calculatorValue = value;
      } else {
        calculatorValue += value;
      }
      updateCalculatorDisplay();
    };
    
    window.clearCalculator = function() {
      calculatorValue = '0';
      calculatorPreviousValue = '0';
      calculatorOperation = null;
      updateCalculatorDisplay();
    };
    
    window.backspaceCalculator = function() {
      if (calculatorValue.length === 1) {
        calculatorValue = '0';
      } else {
        calculatorValue = calculatorValue.slice(0, -1);
      }
      updateCalculatorDisplay();
    };
    
    window.calculateResult = function() {
      try {
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
    };
    
    function updateCalculatorDisplay() {
      document.getElementById('calculatorDisplay').textContent = calculatorValue;
    }
  </script>
</body>
</html>
