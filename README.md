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
      --background-light: #ffffff;
      --background-dark: #121212;
      --card-light: #f5f5f5;
      --card-dark: #1e1e1e;
      --text-light: #ffffff;
      --text-dark: #000000;
      --border-light: #e0e0e0;
      --border-dark: #424242;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    body {
      display: flex;
      background-color: var(--background-light);
      color: var(--text-dark);
      transition: background-color 0.3s, color 0.3s;
    }

    body.dark {
      background-color: var(--background-dark);
      color: var(--text-light);
    }

    .sidebar {
      width: 250px;
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
    }

    .sidebar.hidden {
      transform: translateX(100%);
    }

    .sidebar h2 {
      text-align: center;
      margin-bottom: 2rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
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
      border-radius: 4px;
      display: flex;
      align-items: center;
      transition: background-color 0.2s;
    }

    .sidebar button:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .sidebar button .material-icons {
      margin-left: 8px;
    }

    .main {
      margin-left: 250px;
      padding: 2rem;
      flex-grow: 1;
      transition: margin-left 0.3s;
      width: 100%;
    }

    .main.full {
      margin-left: 0;
    }

    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 2rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--border-light);
    }

    body.dark .topbar {
      border-bottom-color: var(--border-dark);
    }

    .topbar-actions {
      display: flex;
      gap: 1rem;
    }

    .toggle-sidebar, .toggle-theme {
      cursor: pointer;
      padding: 0.5rem;
      border-radius: 50%;
      transition: background-color 0.2s;
    }

    .toggle-sidebar:hover, .toggle-theme:hover {
      background-color: rgba(0, 0, 0, 0.1);
    }

    body.dark .toggle-sidebar:hover, 
    body.dark .toggle-theme:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    .page {
      display: none;
    }

    .page.active {
      display: block;
    }

    .card {
      background-color: var(--card-light);
      padding: 1.5rem;
      margin-bottom: 1.5rem;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      transition: background-color 0.3s, box-shadow 0.3s;
    }

    body.dark .card {
      background-color: var(--card-dark);
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    }

    .card h3 {
      margin-bottom: 1rem;
      color: var(--primary);
    }

    body.dark .card h3 {
      color: var(--primary-dark);
    }

    .stats-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 1.5rem;
      margin-bottom: 2rem;
    }

    .stat-card {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 1.5rem;
      border-radius: 8px;
      background-color: var(--primary);
      color: white;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
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

    .stat-card .value {
      font-size: 2.5rem;
      font-weight: bold;
      margin: 0.5rem 0;
    }

    .stat-card .label {
      font-size: 1rem;
    }

    .form-group {
      margin-bottom: 1rem;
    }

    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
    }

    input, select, textarea {
      width: 100%;
      padding: 0.75rem;
      border: 1px solid var(--border-light);
      border-radius: 4px;
      font-size: 1rem;
      transition: border-color 0.3s, background-color 0.3s;
    }

    body.dark input, 
    body.dark select,
    body.dark textarea {
      background-color: var(--card-dark);
      border-color: var(--border-dark);
      color: var(--text-light);
    }

    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 0 2px rgba(25, 118, 210, 0.2);
    }

    button {
      background-color: var(--primary);
      color: white;
      border: none;
      padding: 0.75rem 1.5rem;
      border-radius: 4px;
      cursor: pointer;
      font-size: 1rem;
      transition: background-color 0.2s;
    }

    button:hover {
      background-color: var(--primary-dark);
    }

    button.danger {
      background-color: var(--danger);
    }

    button.danger:hover {
      background-color: #b71c1c;
    }

    button.warning {
      background-color: var(--warning);
    }

    button.warning:hover {
      background-color: #ff8f00;
    }

    .btn-group {
      display: flex;
      gap: 0.5rem;
      margin-top: 1rem;
    }

    .message {
      padding: 0.75rem;
      border-radius: 4px;
      margin-bottom: 1rem;
      display: none;
    }

    .message.success {
      background-color: rgba(56, 142, 60, 0.2);
      color: var(--success);
      border: 1px solid var(--success);
      display: block;
    }

    .message.error {
      background-color: rgba(211, 47, 47, 0.2);
      color: var(--danger);
      border: 1px solid var(--danger);
      display: block;
    }

    .search-container {
      margin-bottom: 1.5rem;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    }

    th, td {
      padding: 12px 15px;
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
    }

    .calculator {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 0.75rem;
      margin-top: 1.5rem;
    }

    .calculator input {
      grid-column: span 4;
      height: 50px;
      font-size: 1.5rem;
      text-align: left;
      padding-right: 1rem;
    }

    .calculator button {
      padding: 1rem;
      font-size: 1.2rem;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .calculator button.operator {
      background-color: var(--primary-dark);
    }

    .calculator button.equals {
      background-color: var(--success);
    }

    .calculator button.clear {
      background-color: var(--danger);
    }

    .chart-container {
      position: relative;
      height: 400px;
      margin-top: 2rem;
    }

    footer {
      text-align: center;
      margin-top: 3rem;
      padding-top: 1rem;
      border-top: 1px solid var(--border-light);
      color: gray;
      font-size: 0.9rem;
    }

    body.dark footer {
      border-top-color: var(--border-dark);
    }

    @media (max-width: 768px) {
      .sidebar {
        transform: translateX(100%);
      }

      .sidebar.visible {
        transform: translateX(0);
      }

      .main {
        margin-left: 0;
        padding: 1rem;
      }

      .stats-grid {
        grid-template-columns: 1fr;
      }
    }

    /* Print styles */
    @media print {
      .sidebar, .topbar {
        display: none;
      }

      .main {
        margin-left: 0;
      }

      .page {
        display: block !important;
      }

      .no-print {
        display: none;
      }
    }
  </style>
</head>
<body>
  <div class="sidebar" id="sidebar">
    <h2>نظام إدارة الصيدلية</h2>
    <button id="dashboardBtn">
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
  
  <div class="main" id="main">
    <div class="topbar">
      <h2 id="page-title">لوحة التحكم</h2>
      <div class="topbar-actions">
        <span class="material-icons toggle-sidebar" id="toggleSidebar">menu</span>
        <span class="material-icons toggle-theme" id="toggleTheme">dark_mode</span>
        <span class="material-icons" id="printBtn">print</span>
      </div>
    </div>

    <!-- Dashboard Page -->
    <div id="dashboard" class="page active">
      <div class="stats-grid">
        <div class="stat-card">
          <div class="value" id="med-count">0</div>
          <div class="label">عدد الأدوية</div>
        </div>
        <div class="stat-card warning">
          <div class="value" id="expiring-soon">0</div>
          <div class="label">أدوية تنتهي قريباً</div>
        </div>
        <div class="stat-card danger">
          <div class="value" id="expired">0</div>
          <div class="label">أدوية منتهية</div>
        </div>
        <div class="stat-card success">
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
            <!-- Filled by JavaScript -->
          </tbody>
        </table>
      </div>
    </div>

    <!-- Medicines Management Page -->
    <div id="medicines" class="page">
      <div class="card">
        <h3>إضافة دواء جديد</h3>
        <div id="med-message" class="message"></div>
        <div class="form-group">
          <label for="medName">اسم الدواء</label>
          <input type="text" id="medName" placeholder="أدخل اسم الدواء">
        </div>
        <div class="form-group">
          <label for="medGeneric">الاسم العلمي</label>
          <input type="text" id="medGeneric" placeholder="أدخل الاسم العلمي">
        </div>
        <div class="form-group">
          <label for="medCategory">الفئة</label>
          <select id="medCategory">
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
          <input type="number" id="medQty" placeholder="أدخل الكمية">
        </div>
        <div class="form-group">
          <label for="medPrice">سعر البيع</label>
          <input type="number" step="0.01" id="medPrice" placeholder="أدخل سعر البيع">
        </div>
        <div class="form-group">
          <label for="medExp">تاريخ الانتهاء</label>
          <input type="date" id="medExp">
        </div>
        <div class="btn-group">
          <button id="addMedicineBtn">إضافة دواء</button>
          <button class="danger" id="clearMedicineBtn">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الأدوية</h3>
        <div class="search-container">
          <input type="text" id="med-search" placeholder="ابحث عن دواء...">
        </div>
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

    <!-- Suppliers Page -->
    <div id="suppliers" class="page">
      <div class="card">
        <h3>إدارة الموردين</h3>
        <div id="supplier-message" class="message"></div>
        <div class="form-group">
          <label for="supplierName">اسم المورد</label>
          <input type="text" id="supplierName" placeholder="أدخل اسم المورد">
        </div>
        <div class="form-group">
          <label for="supplierPhone">رقم الهاتف</label>
          <input type="tel" id="supplierPhone" placeholder="أدخل رقم الهاتف">
        </div>
        <div class="form-group">
          <label for="supplierEmail">البريد الإلكتروني</label>
          <input type="email" id="supplierEmail" placeholder="أدخل البريد الإلكتروني">
        </div>
        <div class="form-group">
          <label for="supplierAddress">العنوان</label>
          <input type="text" id="supplierAddress" placeholder="أدخل العنوان">
        </div>
        <div class="btn-group">
          <button id="addSupplierBtn">إضافة مورد</button>
          <button class="danger" id="clearSupplierBtn">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الموردين</h3>
        <div class="search-container">
          <input type="text" id="supplier-search" placeholder="ابحث عن مورد...">
        </div>
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

    <!-- Customers Page -->
    <div id="customers" class="page">
      <div class="card">
        <h3>إدارة العملاء</h3>
        <div id="customer-message" class="message"></div>
        <div class="form-group">
          <label for="customerName">اسم العميل</label>
          <input type="text" id="customerName" placeholder="أدخل اسم العميل">
        </div>
        <div class="form-group">
          <label for="customerPhone">رقم الهاتف</label>
          <input type="tel" id="customerPhone" placeholder="أدخل رقم الهاتف">
        </div>
        <div class="form-group">
          <label for="customerAddress">العنوان</label>
          <input type="text" id="customerAddress" placeholder="أدخل العنوان">
        </div>
        <div class="form-group">
          <label for="customerMedicalHistory">تاريخ مرضي (اختياري)</label>
          <input type="text" id="customerMedicalHistory" placeholder="أدخل التاريخ المرضي">
        </div>
        <div class="btn-group">
          <button id="addCustomerBtn">إضافة عميل</button>
          <button class="danger" id="clearCustomerBtn">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة العملاء</h3>
        <div class="search-container">
          <input type="text" id="customer-search" placeholder="ابحث عن عميل...">
        </div>
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

    <!-- Orders Page -->
    <div id="orders" class="page">
      <div class="card">
        <h3>إنشا�� فاتورة جديدة</h3>
        <div id="order-message" class="message"></div>
        <div class="form-group">
          <label for="orderCustomer">العميل</label>
          <select id="orderCustomer">
            <option value="">اختر العميل</option>
          </select>
        </div>
        
        <div class="form-group">
          <label for="orderMedicine">الدواء</label>
          <select id="orderMedicine">
            <option value="">اختر الدواء</option>
          </select>
        </div>
        
        <div class="form-group">
          <label for="orderQty">الكمية</label>
          <input type="number" id="orderQty" min="1" value="1">
        </div>
        
        <button id="addMedicineToOrderBtn">إضافة إلى الفاتورة</button>
        
        <table id="orderItems">
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
              <td colspan="3" style="text-align: left;">المجموع:</td>
              <td id="orderTotal">0.00</td>
              <td></td>
            </tr>
          </tfoot>
        </table>
        
        <div class="btn-group">
          <button id="createOrderBtn">إنشاء الفاتورة</button>
          <button class="danger" id="clearOrderBtn">إلغاء</button>
          <button class="no-print" id="printOrderBtn">طباعة الفاتورة</button>
        </div>
      </div>

      <div class="card">
        <h3>سجل الفواتير</h3>
        <div class="search-container">
          <input type="text" id="order-search" placeholder="ابحث عن فاتورة...">
        </div>
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

    <!-- Prescriptions Page -->
    <div id="prescriptions" class="page">
      <div class="card">
        <h3>إضافة وصفة طبية</h3>
        <div id="prescription-message" class="message"></div>
        <div class="form-group">
          <label for="prescriptionCustomer">العميل</label>
          <select id="prescriptionCustomer">
            <option value="">اختر العميل</option>
          </select>
        </div>
        <div class="form-group">
          <label for="prescriptionDoctor">الطبيب</label>
          <input type="text" id="prescriptionDoctor" placeholder="أدخل اسم الطبيب">
        </div>
        <div class="form-group">
          <label for="prescriptionDate">تاريخ الوصفة</label>
          <input type="date" id="prescriptionDate">
        </div>
        <div class="form-group">
          <label for="prescriptionNotes">ملاحظات</label>
          <textarea id="prescriptionNotes" rows="3" placeholder="أدخل أي ملاحظات"></textarea>
        </div>
        
        <h4>أدوية الوصفة</h4>
        <div class="form-group">
          <label for="prescriptionMedicine">الدواء</label>
          <select id="prescriptionMedicine">
            <option value="">اختر الدواء</option>
          </select>
        </div>
        <div class="form-group">
          <label for="prescriptionMedicineQty">الكمية</label>
          <input type="number" id="prescriptionMedicineQty" min="1" value="1">
        </div>
        <div class="form-group">
          <label for="prescriptionDosage">الجرعة</label>
          <input type="text" id="prescriptionDosage" placeholder="أدخل الجرعة">
        </div>
        <button id="addMedicineToPrescriptionBtn">إضافة دواء للوصفة</button>
        
        <table id="prescriptionItems">
          <thead>
            <tr>
              <th>الدواء</th>
              <th>الكمية</th>
              <th>الجرعة</th>
              <th>إجراءات</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
        
        <div class="btn-group">
          <button id="savePrescriptionBtn">حفظ الوصفة</button>
          <button class="danger" id="clearPrescriptionBtn">إلغاء</button>
        </div>
      </div>

      <div class="card">
        <h3>سجل الوصفات الطبية</h3>
        <div class="search-container">
          <input type="text" id="prescription-search" placeholder="ابحث عن وصفة...">
        </div>
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

    <!-- Calculator Page -->
    <div id="calculator" class="page">
      <div class="card">
        <h3>الآلة الحاسبة</h3>
        <div class="calculator">
          <input type="text" id="calc-display" readonly>
          <button onclick="calc('7')">7</button>
          <button onclick="calc('8')">8</button>
          <button onclick="calc('9')">9</button>
          <button class="operator" onclick="calc('/')">÷</button>
          <button onclick="calc('4')">4</button>
          <button onclick="calc('5')">5</button>
          <button onclick="calc('6')">6</button>
          <button class="operator" onclick="calc('*')">×</button>
          <button onclick="calc('1')">1</button>
          <button onclick="calc('2')">2</button>
          <button onclick="calc('3')">3</button>
          <button class="operator" onclick="calc('-')">-</button>
          <button onclick="calc('0')">0</button>
          <button onclick="calc('.')">.</button>
          <button class="equals" onclick="calculate()">=</button>
          <button class="operator" onclick="calc('+')">+</button>
          <button class="clear" onclick="clearCalc()">C</button>
          <button onclick="backspace()">⌫</button>
          <button onclick="calc('(')">(</button>
          <button onclick="calc(')')">)</button>
        </div>
      </div>
    </div>

    <!-- Statistics Page -->
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
        <h3>تقرير الأدوية المنتهية الصلاحية</h3>
        <table id="expiryReportTable">
          <thead>
            <tr>
              <th>اسم الدواء</th>
              <th>الكمية</th>
              <th>تاريخ الانتهاء</th>
              <th>الأيام المتبقية</th>
            </tr>
          </thead>
          <tbody></tbody>
        </table>
      </div>
    </div>

    <!-- Settings Page -->
    <div id="settings" class="page">
      <div class="card">
        <h3>إعدادات النظام</h3>
        <div id="settings-message" class="message"></div>
        
        <div class="form-group">
          <label for="pharmacyName">اسم الصيدلية</label>
          <input type="text" id="pharmacyName" placeholder="أدخل اسم الصيدلية">
        </div>
        
        <div class="form-group">
          <label for="pharmacyAddress">عنوان الصيدلية</label>
          <input type="text" id="pharmacyAddress" placeholder="أدخل عنوان الصيدلية">
        </div>
        
        <div class="form-group">
          <label for="pharmacyPhone">هاتف الصيدلية</label>
          <input type="tel" id="pharmacyPhone" placeholder="أدخل هاتف الصيدلية">
        </div>
        
        <div class="form-group">
          <label for="currency">العملة</label>
          <select id="currency">
            <option value="ج">جنيه (ج)</option>
            <option value="$">دولار ($)</option>
            <option value="€">يورو (€)</option>
            <option value="ريال">ريال</option>
            <option value="د.إ">درهم (د.إ)</option>
          </select>
        </div>
        
        <div class="form-group">
          <label for="taxRate">معدل الضريبة (%)</label>
          <input type="number" id="taxRate" min="0" max="100" step="0.1" value="0">
        </div>
        
        <div class="form-group">
          <label>
            <input type="checkbox" id="enableNotifications"> تمكين الإشعارات
          </label>
        </div>
        
        <div class="form-group">
          <label>
            <input type="checkbox" id="enableExpiryAlerts"> تمكين تنبيهات انتهاء الصلاحية
          </label>
        </div>
        
        <div class="form-group">
          <label for="daysBeforeExpiry">عدد الأيام للتنبيه قبل انتهاء الصلاحية</label>
          <input type="number" id="daysBeforeExpiry" min="1" value="30">
        </div>
        
        <div class="btn-group">
          <button id="saveSettingsBtn">حفظ الإعدادات</button>
          <button class="danger" id="resetSettingsBtn">إعادة تعيين</button>
        </div>
      </div>
    </div>

    <footer>
      © 2023 نظام إدارة الصيدلية | تم التطوير بواسطة عمر بابكر
    </footer>
  </div>

  <script>
    // Global variables
    let darkMode = false;
    let medicines = [];
    let suppliers = [];
    let customers = [];
    let orders = [];
    let prescriptions = [];
    let settings = {
      pharmacyName: "صيدليتي",
      pharmacyAddress: "",
      pharmacyPhone: "",
      currency: "ج",
      taxRate: 0,
      enableNotifications: true,
      enableExpiryAlerts: true,
      daysBeforeExpiry: 30
    };
    let currentPrescriptionId = 1;
    let currentOrderId = 1;

    // DOM Ready
    document.addEventListener('DOMContentLoaded', function() {
      // Initialize the application
      initApp();
      
      // Setup all event listeners
      setupEventListeners();
    });

    function initApp() {
      // Load sample data
      loadSampleData();
      
      // Load settings from localStorage
      loadSettings();
      
      // Initialize charts
      initCharts();
      
      // Set current page title
      document.getElementById('page-title').textContent = 'لوحة التحكم';
      
      // Set current date for date inputs
      const today = new Date().toISOString().split('T')[0];
      document.getElementById('medExp').min = today;
      document.getElementById('prescriptionDate').value = today;
      
      // Populate customer dropdowns
      updateCustomerDropdowns();
      
      // Populate medicine dropdowns
      updateMedicineDropdowns();
      
      // Update dashboard stats
      updateDashboardStats();
    }

    function setupEventListeners() {
      // Navigation buttons
      const navButtons = {
        'dashboardBtn': 'dashboard',
        'medicinesBtn': 'medicines',
        'suppliersBtn': 'suppliers',
        'customersBtn': 'customers',
        'ordersBtn': 'orders',
        'prescriptionsBtn': 'prescriptions',
        'calculatorBtn': 'calculator',
        'statisticsBtn': 'statistics',
        'settingsBtn': 'settings'
      };

      Object.entries(navButtons).forEach(([id, page]) => {
        const element = document.getElementById(id);
        if (element) {
          element.addEventListener('click', () => navigate(page));
        }
      });

      // Topbar actions
      document.getElementById('toggleSidebar')?.addEventListener('click', toggleSidebar);
      document.getElementById('toggleTheme')?.addEventListener('click', toggleTheme);
      document.getElementById('printBtn')?.addEventListener('click', printPage);

      // Medicines page
      document.getElementById('addMedicineBtn')?.addEventListener('click', addMedicine);
      document.getElementById('clearMedicineBtn')?.addEventListener('click', clearMedicineForm);
      document.getElementById('med-search')?.addEventListener('input', function() {
        searchMedicine(this.value);
      });

      // Suppliers page
      document.getElementById('addSupplierBtn')?.addEventListener('click', addSupplier);
      document.getElementById('clearSupplierBtn')?.addEventListener('click', clearSupplierForm);
      document.getElementById('supplier-search')?.addEventListener('input', function() {
        searchSupplier(this.value);
      });

      // Customers page
      document.getElementById('addCustomerBtn')?.addEventListener('click', addCustomer);
      document.getElementById('clearCustomerBtn')?.addEventListener('click', clearCustomerForm);
      document.getElementById('customer-search')?.addEventListener('input', function() {
        searchCustomer(this.value);
      });

      // Orders page
      document.getElementById('addMedicineToOrderBtn')?.addEventListener('click', addMedicineToOrder);
      document.getElementById('createOrderBtn')?.addEventListener('click', createOrder);
      document.getElementById('clearOrderBtn')?.addEventListener('click', clearOrderForm);
      document.getElementById('printOrderBtn')?.addEventListener('click', printOrder);
      document.getElementById('order-search')?.addEventListener('input', function() {
        searchOrder(this.value);
      });

      // Prescriptions page
      document.getElementById('addMedicineToPrescriptionBtn')?.addEventListener('click', addMedicineToPrescription);
      document.getElementById('savePrescriptionBtn')?.addEventListener('click', savePrescription);
      document.getElementById('clearPrescriptionBtn')?.addEventListener('click', clearPrescriptionForm);
      document.getElementById('prescription-search')?.addEventListener('input', function() {
        searchPrescription(this.value);
      });

      // Settings page
      document.getElementById('saveSettingsBtn')?.addEventListener('click', saveSettings);
      document.getElementById('resetSettingsBtn')?.addEventListener('click', resetSettings);
    }

    // Load sample data for demonstration
    function loadSampleData() {
      // Sample medicines
      medicines = [
        { id: 1, name: "باراسيتامول", generic: "Paracetamol", category: "analgesic", qty: 150, price: 5.50, exp: "2023-12-31" },
        { id: 2, name: "أموكسيسيلين", generic: "Amoxicillin", category: "antibiotic", qty: 80, price: 12.75, exp: "2024-06-30" },
        { id: 3, name: "سيتريزين", generic: "Cetirizine", category: "antihistamine", qty: 45, price: 8.25, exp: "2023-11-15" },
        { id: 4, name: "أوميبرازول", generic: "Omeprazole", category: "antacid", qty: 60, price: 15.00, exp: "2024-03-31" },
        { id: 5, name: "ايبوبروفين", generic: "Ibuprofen", category: "analgesic", qty: 90, price: 7.50, exp: "2023-10-31" }
      ];
      
      // Sample suppliers
      suppliers = [
        { id: 1, name: "شركة الأدوية المتحدة", phone: "0123456789", email: "info@unitedmeds.com", address: "القاهرة، مصر" },
        { id: 2, name: "المستورد للأدوية", phone: "0111222333", email: "contact@almoustawared.com", address: "الإسكندرية، مصر" }
      ];
      
      // Sample customers
      customers = [
        { id: 1, name: "أحمد محمد", phone: "01001234567", address: "شارع النصر، المنصورة", medicalHistory: "حساسية من البنسلين" },
        { id: 2, name: "مريم علي", phone: "01159876543", address: "حي الزهور، القاهرة", medicalHistory: "" },
        { id: 3, name: "خالد عبد الرحمن", phone: "01287654321", address: "مدينة نصر، القاهرة", medicalHistory: "ضغط دم مرتفع" }
      ];
      
      // Sample orders
      orders = [
        { id: 1, customerId: 1, date: "2023-05-15", items: [
          { medicineId: 1, qty: 2, price: 5.50 },
          { medicineId: 3, qty: 1, price: 8.25 }
        ], total: 19.25, status: "مكتمل" },
        { id: 2, customerId: 2, date: "2023-05-18", items: [
          { medicineId: 2, qty: 1, price: 12.75 },
          { medicineId: 4, qty: 1, price: 15.00 }
        ], total: 27.75, status: "مكتمل" }
      ];
      
      // Sample prescriptions
      prescriptions = [
        { id: 1, customerId: 3, doctor: "د. وائل عبد الله", date: "2023-05-10", notes: "حساسية موسمية", items: [
          { medicineId: 3, qty: 1, dosage: "قرص يومياً لمدة أسبوع" }
        ] },
        { id: 2, customerId: 1, doctor: "د. هناء محمود", date: "2023-05-12", notes: "آلام أسنان", items: [
          { medicineId: 1, qty: 2, dosage: "قرص كل 6 ساعات عند اللزوم" },
          { medicineId: 5, qty: 1, dosage: "قرص عند اللزوم للألم الشديد" }
        ] }
      ];
      
      // Update all tables
      updateMedTable();
      updateSupplierTable();
      updateCustomerTable();
      updateOrderTable();
      updatePrescriptionTable();
    }

    // Load settings from localStorage
    function loadSettings() {
      const savedSettings = localStorage.getItem('pharmacySettings');
      if (savedSettings) {
        settings = JSON.parse(savedSettings);
        applySettings();
      }
    }

    // Apply settings to the UI
    function applySettings() {
      document.getElementById('pharmacyName').value = settings.pharmacyName;
      document.getElementById('pharmacyAddress').value = settings.pharmacyAddress;
      document.getElementById('pharmacyPhone').value = settings.pharmacyPhone;
      document.getElementById('currency').value = settings.currency;
      document.getElementById('taxRate').value = settings.taxRate;
      document.getElementById('enableNotifications').checked = settings.enableNotifications;
      document.getElementById('enableExpiryAlerts').checked = settings.enableExpiryAlerts;
      document.getElementById('daysBeforeExpiry').value = settings.daysBeforeExpiry;
    }

    // Save settings to localStorage
    function saveSettings() {
      settings.pharmacyName = document.getElementById('pharmacyName').value;
      settings.pharmacyAddress = document.getElementById('pharmacyAddress').value;
      settings.pharmacyPhone = document.getElementById('pharmacyPhone').value;
      settings.currency = document.getElementById('currency').value;
      settings.taxRate = parseFloat(document.getElementById('taxRate').value);
      settings.enableNotifications = document.getElementById('enableNotifications').checked;
      settings.enableExpiryAlerts = document.getElementById('enableExpiryAlerts').checked;
      settings.daysBeforeExpiry = parseInt(document.getElementById('daysBeforeExpiry').value);
      
      localStorage.setItem('pharmacySettings', JSON.stringify(settings));
      
      showMessage('settings-message', 'تم حفظ الإعدادات بنجاح', 'success');
    }

    // Reset settings to defaults
    function resetSettings() {
      if (confirm('هل أنت متأكد من إعادة تعيين جميع الإعدادات إلى القيم الافتراضية؟')) {
        localStorage.removeItem('pharmacySettings');
        settings = {
          pharmacyName: "صيدليتي",
          pharmacyAddress: "",
          pharmacyPhone: "",
          currency: "ج",
          taxRate: 0,
          enableNotifications: true,
          enableExpiryAlerts: true,
          daysBeforeExpiry: 30
        };
        applySettings();
        showMessage('settings-message', 'تم إعادة تعيين الإعدادات', 'success');
      }
    }

    // Initialize charts
    function initCharts() {
      // Sales Chart
      const salesCtx = document.getElementById('salesChart').getContext('2d');
      const salesChart = new Chart(salesCtx, {
        type: 'bar',
        data: {
          labels: ['يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو'],
          datasets: [{
            label: 'المبيعات (' + settings.currency + ')',
            data: [12500, 18700, 14200, 20900, 15800, 23100],
            backgroundColor: 'rgba(25, 118, 210, 0.7)',
            borderColor: 'rgba(25, 118, 210, 1)',
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'top', rtl: true },
            title: { display: true, text: 'مبيعات الأشهر الستة الأخيرة', rtl: true }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });

      // Medicines Chart
      const medsCtx = document.getElementById('medicinesChart').getContext('2d');
      const medicinesChart = new Chart(medsCtx, {
        type: 'pie',
        data: {
          labels: ['مضادات حيوية', 'مسكنات', 'مضادات هستامين', 'مضادات حموضة', 'أخرى'],
          datasets: [{
            data: [35, 25, 15, 10, 15],
            backgroundColor: [
              'rgba(255, 99, 132, 0.7)',
              'rgba(54, 162, 235, 0.7)',
              'rgba(255, 206, 86, 0.7)',
              'rgba(75, 192, 192, 0.7)',
              'rgba(153, 102, 255, 0.7)'
            ],
            borderColor: [
              'rgba(255, 99, 132, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)',
              'rgba(153, 102, 255, 1)'
            ],
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'right', rtl: true },
            title: { display: true, text: 'توزيع الأدوية حسب الفئة', rtl: true }
          }
        }
      });

      // Expiry Chart
      const expiryCtx = document.getElementById('expiryChart').getContext('2d');
      const expiryChart = new Chart(expiryCtx, {
        type: 'line',
        data: {
          labels: ['Q1', 'Q2', 'Q3', 'Q4'],
          datasets: [{
            label: 'الأدوية المنتهية الصلاحية',
            data: [12, 19, 8, 15],
            fill: false,
            backgroundColor: 'rgba(211, 47, 47, 0.7)',
            borderColor: 'rgba(211, 47, 47, 1)',
            tension: 0.1
          }, {
            label: 'الأدوية التي تنتهي قريباً',
            data: [5, 7, 14, 10],
            fill: false,
            backgroundColor: 'rgba(255, 160, 0, 0.7)',
            borderColor: 'rgba(255, 160, 0, 1)',
            tension: 0.1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { position: 'top', rtl: true },
            title: { display: true, text: 'اتجاهات انتهاء صلاحية الأدوية', rtl: true }
          },
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    }

    // Toggle sidebar visibility
    function toggleSidebar() {
      document.getElementById('sidebar').classList.toggle('hidden');
      document.getElementById('main').classList.toggle('full');
    }

    // Toggle dark/light theme
    function toggleTheme() {
      darkMode = !darkMode;
      document.body.classList.toggle('dark', darkMode);
      localStorage.setItem('darkMode', darkMode);
    }

    // Navigate between pages
    function navigate(pageId) {
      const pages = document.querySelectorAll('.page');
      pages.forEach(p => p.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
      
      // Update page title
      const pageTitles = {
        'dashboard': 'لوحة التحكم',
        'medicines': 'إدارة الأدوية',
        'suppliers': 'الموردون',
        'customers': 'العملاء',
        'orders': 'الفواتير',
        'prescriptions': 'الوصفات الطبية',
        'calculator': 'الآلة الحاسبة',
        'statistics': 'الإحصائيات',
        'settings': 'الإعدادات'
      };
      document.getElementById('page-title').textContent = pageTitles[pageId];
      
      // Update charts when navigating to statistics page
      if (pageId === 'statistics') {
        updateCharts();
      }
      
      // Update dashboard stats when navigating to dashboard
      if (pageId === 'dashboard') {
        updateDashboardStats();
        updateExpiryReport();
      }
    }

    // Show message
    function showMessage(elementId, text, type) {
      const element = document.getElementById(elementId);
      element.textContent = text;
      element.className = 'message ' + type;
      
      // Hide message after 5 seconds
      setTimeout(() => {
        element.className = 'message';
        element.textContent = '';
      }, 5000);
    }

    // Add a new medicine
    function addMedicine() {
      const name = document.getElementById('medName').value.trim();
      const generic = document.getElementById('medGeneric').value.trim();
      const category = document.getElementById('medCategory').value;
      const qty = parseInt(document.getElementById('medQty').value);
      const price = parseFloat(document.getElementById('medPrice').value);
      const exp = document.getElementById('medExp').value;
      
      // Validate inputs
      if (!name || !generic || !category || isNaN(qty) || isNaN(price) || !exp) {
        showMessage('med-message', 'يرجى ملء جميع الحقول بشكل صحيح', 'error');
        return;
      }
      
      if (qty <= 0) {
        showMessage('med-message', 'الكمية يجب أن تكون أكبر من الصفر', 'error');
        return;
      }
      
      if (price <= 0) {
        showMessage('med-message', 'السعر يجب أن يكون أكبر من الصفر', 'error');
        return;
      }
      
      // Create new medicine
      const newMedicine = {
        id: medicines.length > 0 ? Math.max(...medicines.map(m => m.id)) + 1 : 1,
        name,
        generic,
        category,
        qty,
        price,
        exp
      };
      
      medicines.push(newMedicine);
      updateMedTable();
      updateMedicineDropdowns();
      updateDashboardStats();
      updateExpiryReport();
      
      // Clear form
      clearMedicineForm();
      
      showMessage('med-message', 'تم إضافة الدواء بنجاح', 'success');
    }

    // Clear medicine form
    function clearMedicineForm() {
      document.getElementById('medName').value = '';
      document.getElementById('medGeneric').value = '';
      document.getElementById('medCategory').value = '';
      document.getElementById('medQty').value = '';
      document.getElementById('medPrice').value = '';
      document.getElementById('medExp').value = '';
    }

    // Update medicines table
    function updateMedTable() {
      const tbody = document.querySelector('#medTable tbody');
      tbody.innerHTML = '';
      
      medicines.forEach(med => {
        const tr = document.createElement('tr');
        
        // Calculate days until expiry
        const today = new Date();
        const expDate = new Date(med.exp);
        const timeDiff = expDate.getTime() - today.getTime();
        const daysDiff = Math.ceil(timeDiff / (1000 * 3600 * 24));
        
        // Add class if expired or expiring soon
        if (daysDiff <= 0) {
          tr.classList.add('expired');
        } else if (daysDiff <= settings.daysBeforeExpiry) {
          tr.classList.add('expiring-soon');
        }
        
        tr.innerHTML = `
          <td>${med.name}</td>
          <td>${med.generic}</td>
          <td>${getCategoryName(med.category)}</td>
          <td>${med.qty}</td>
          <td>${med.price.toFixed(2)} ${settings.currency}</td>
          <td>${formatDate(med.exp)}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="editMedicine(${med.id})">تعديل</button>
            <button class="no-print danger" onclick="deleteMedicine(${med.id})">حذف</button>
          </td>
        `;
        
        tbody.appendChild(tr);
      });
    }

    // Get category name
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

    // Edit medicine
    function editMedicine(id) {
      const medicine = medicines.find(m => m.id === id);
      if (!medicine) return;
      
      document.getElementById('medName').value = medicine.name;
      document.getElementById('medGeneric').value = medicine.generic;
      document.getElementById('medCategory').value = medicine.category;
      document.getElementById('medQty').value = medicine.qty;
      document.getElementById('medPrice').value = medicine.price;
      document.getElementById('medExp').value = medicine.exp;
      
      // Scroll to form
      document.getElementById('medName').scrollIntoView({ behavior: 'smooth' });
      
      // Change button to update
      const addButton = document.querySelector('#medicines .btn-group button:first-child');
      addButton.textContent = 'تحديث الدواء';
      addButton.onclick = function() { updateMedicine(id); };
    }

    // Update medicine
    function updateMedicine(id) {
      const medicineIndex = medicines.findIndex(m => m.id === id);
      if (medicineIndex === -1) return;
      
      const name = document.getElementById('medName').value.trim();
      const generic = document.getElementById('medGeneric').value.trim();
      const category = document.getElementById('medCategory').value;
      const qty = parseInt(document.getElementById('medQty').value);
      const price = parseFloat(document.getElementById('medPrice').value);
      const exp = document.getElementById('medExp').value;
      
      // Validate inputs
      if (!name || !generic || !category || isNaN(qty) || isNaN(price) || !exp) {
        showMessage('med-message', 'يرجى ملء جميع الحقول بشكل صحيح', 'error');
        return;
      }
      
      // Update medicine
      medicines[medicineIndex] = {
        id,
        name,
        generic,
        category,
        qty,
        price,
        exp
      };
      
      updateMedTable();
      updateMedicineDropdowns();
      updateDashboardStats();
      updateExpiryReport();
      
      // Clear form and reset button
      clearMedicineForm();
      const addButton = document.querySelector('#medicines .btn-group button:first-child');
      addButton.textContent = 'إضافة دواء';
      addButton.onclick = function() { addMedicine(); };
      
      showMessage('med-message', 'تم تحديث الدواء بنجاح', 'success');
    }

    // Delete medicine
    function deleteMedicine(id) {
      if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
        medicines = medicines.filter(m => m.id !== id);
        updateMedTable();
        updateMedicineDropdowns();
        updateDashboardStats();
        updateExpiryReport();
        showMessage('med-message', 'تم حذف الدواء بنجاح', 'success');
      }
    }

    // Search medicines
    function searchMedicine(query) {
      const rows = document.querySelectorAll('#medTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const name = row.cells[0].textContent.toLowerCase();
        const generic = row.cells[1].textContent.toLowerCase();
        const category = row.cells[2].textContent.toLowerCase();
        
        if (name.includes(lowerQuery) || generic.includes(lowerQuery) || category.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Add a new supplier
    function addSupplier() {
      const name = document.getElementById('supplierName').value.trim();
      const phone = document.getElementById('supplierPhone').value.trim();
      const email = document.getElementById('supplierEmail').value.trim();
      const address = document.getElementById('supplierAddress').value.trim();
      
      // Validate inputs
      if (!name || !phone) {
        showMessage('supplier-message', 'يرجى إدخال اسم المورد ورقم الهاتف', 'error');
        return;
      }
      
      // Create new supplier
      const newSupplier = {
        id: suppliers.length > 0 ? Math.max(...suppliers.map(s => s.id)) + 1 : 1,
        name,
        phone,
        email,
        address
      };
      
      suppliers.push(newSupplier);
      updateSupplierTable();
      
      // Clear form
      clearSupplierForm();
      
      showMessage('supplier-message', 'تم إضافة المورد بنجاح', 'success');
    }

    // Clear supplier form
    function clearSupplierForm() {
      document.getElementById('supplierName').value = '';
      document.getElementById('supplierPhone').value = '';
      document.getElementById('supplierEmail').value = '';
      document.getElementById('supplierAddress').value = '';
    }

    // Update suppliers table
    function updateSupplierTable() {
      const tbody = document.querySelector('#supplierTable tbody');
      tbody.innerHTML = '';
      
      suppliers.forEach(supplier => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${supplier.name}</td>
          <td>${supplier.phone}</td>
          <td>${supplier.email || '-'}</td>
          <td>${supplier.address || '-'}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="editSupplier(${supplier.id})">تعديل</button>
            <button class="no-print danger" onclick="deleteSupplier(${supplier.id})">حذف</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // Edit supplier
    function editSupplier(id) {
      const supplier = suppliers.find(s => s.id === id);
      if (!supplier) return;
      
      document.getElementById('supplierName').value = supplier.name;
      document.getElementById('supplierPhone').value = supplier.phone;
      document.getElementById('supplierEmail').value = supplier.email;
      document.getElementById('supplierAddress').value = supplier.address;
      
      // Scroll to form
      document.getElementById('supplierName').scrollIntoView({ behavior: 'smooth' });
      
      // Change button to update
      const addButton = document.querySelector('#suppliers .btn-group button:first-child');
      addButton.textContent = 'تحديث المورد';
      addButton.onclick = function() { updateSupplier(id); };
    }

    // Update
