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
      margin-right: 250px; /* Changed from margin-left for RTL */
      padding: 2rem;
      flex-grow: 1;
      transition: margin-right 0.3s; /* Changed from margin-left for RTL */
      width: 100%;
    }

    .main.full {
      margin-right: 0; /* Changed from margin-left for RTL */
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
        margin-right: 0; /* Changed from margin-left for RTL */
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
                      </tbody>
        </table>
      </div>
    </div>

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

        <div id="orders" class="page">
      <div class="card">
        <h3>إنشاء فاتورة جديدة</h3>
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
      pharmacyAddress: "123 الشارع الرئيسي، المدينة",
      pharmacyPhone: "0123456789",
      currency: "ج",
      taxRate: 5,
      enableNotifications: true,
      enableExpiryAlerts: true,
      daysBeforeExpiry: 30
    };
    let currentPrescriptionId = 1;
    let currentOrderId = 1;

    let editingMedicineId = null;
    let editingSupplierId = null;
    let editingCustomerId = null;
    let currentOrderItems = [];
    let currentPrescriptionItems = [];

    let expiryChartInstance;
    let salesChartInstance;
    let medicinesChartInstance;

    // DOM Ready
    document.addEventListener('DOMContentLoaded', function() {
      // Initialize the application
      initApp();
      
      // Setup all event listeners
      setupEventListeners();
    });

    function initApp() {
      // Load data from localStorage or use sample data
      loadDataFromLocalStorage();
      if (medicines.length === 0 && suppliers.length === 0 && customers.length === 0 && orders.length === 0 && prescriptions.length === 0) {
        loadSampleData();
        saveAllDataToLocalStorage();
      }
      
      // Load settings from localStorage
      loadSettings();
      applySettings();
      
      // Initialize charts
      initCharts();
      
      // Set current page title
      document.getElementById('page-title').textContent = 'لوحة التحكم';
      
      // Set current date for date inputs
      const today = new Date().toISOString().split('T')[0];
      document.getElementById('medExp').min = today;
      document.getElementById('prescriptionDate').value = today;
      
      // Populate all dropdowns and tables
      updateCustomerDropdowns();
      updateMedicineDropdowns();
      renderMedicineTable();
      renderSupplierTable();
      renderCustomerTable();
      renderOrderTable();
      renderPrescriptionTable();
      
      // Update dashboard stats and charts
      updateDashboardStats();
      updateExpiryChart();
      updateSalesChart();
      updateMedicinesChart();
      updateExpiryReportTable();
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
      medicines = [
        { id: 1, name: "باراسيتامول", generic: "Paracetamol", category: "مسكن", qty: 150, price: 5.50, exp: "2024-12-31" },
        { id: 2, name: "أموكسيسيلين", generic: "Amoxicillin", category: "مضاد حيوي", qty: 80, price: 12.75, exp: "2024-07-30" },
        { id: 3, name: "سيتريزين", generic: "Cetirizine", category: "مضاد هستامين", qty: 45, price: 8.25, exp: "2024-06-15" },
        { id: 4, name: "أوميبرازول", generic: "Omeprazole", category: "مضاد حموضة", qty: 60, price: 15.00, exp: "2025-03-31" },
        { id: 5, name: "ايبوبروفين", generic: "Ibuprofen", category: "مسكن", qty: 120, price: 7.00, exp: "2025-01-20" },
        { id: 6, name: "ديكلوفيناك", generic: "Diclofenac", category: "مسكن", qty: 30, price: 10.00, exp: "2024-05-01" }, // Expired for testing
        { id: 7, name: "فيتامين سي", generic: "Vitamin C", category: "أخرى", qty: 200, price: 4.00, exp: "2024-07-25" } // Expiring soon
      ];
      suppliers = [
        { id: 1, name: "المورد الدوائي الأول", phone: "0123456789", email: "supplier1@example.com", address: "شارع الصناعة، مبنى 5" },
        { id: 2, name: "شركة الأدوية المتحدة", phone: "0987654321", email: "unitedpharm@example.com", address: "منطقة الأعمال، قطعة 10" }
      ];
      customers = [
        { id: 1, name: "أحمد علي", phone: "0501234567", address: "حي النور، منزل 10", medicalHistory: "حساسية من البنسلين" },
        { id: 2, name: "فاطمة محمد", phone: "0557654321", address: "شارع الروضة، شقة 5", medicalHistory: "ضغط الدم" }
      ];
      orders = [
        { id: 1, date: "2024-06-20", customerId: 1, total: 27.50, status: "مكتمل", items: [{ medId: 1, name: "باراسيتامول", qty: 2, price: 5.50, total: 11.00 }, { medId: 4, name: "أوميبرازول", qty: 1, price: 15.00, total: 15.00 }] },
        { id: 2, date: "2024-06-25", customerId: 2, total: 12.75, status: "معلق", items: [{ medId: 2, name: "أموكسيسيلين", qty: 1, price: 12.75, total: 12.75 }] }
      ];
      prescriptions = [
        { id: 1, customerId: 1, doctor: "د. خالد", date: "2024-06-18", notes: "تناول بعد الأكل", items: [{ medId: 1, name: "باراسيتامول", qty: 1, dosage: "حبتين يومياً" }] }
      ];
      currentPrescriptionId = prescriptions.length > 0 ? Math.max(...prescriptions.map(p => p.id)) + 1 : 1;
      currentOrderId = orders.length > 0 ? Math.max(...orders.map(o => o.id)) + 1 : 1;
    }

    function loadDataFromLocalStorage() {
      medicines = JSON.parse(localStorage.getItem('medicines')) || [];
      suppliers = JSON.parse(localStorage.getItem('suppliers')) || [];
      customers = JSON.parse(localStorage.getItem('customers')) || [];
      orders = JSON.parse(localStorage.getItem('orders')) || [];
      prescriptions = JSON.parse(localStorage.getItem('prescriptions')) || [];
      currentPrescriptionId = JSON.parse(localStorage.getItem('currentPrescriptionId')) || (prescriptions.length > 0 ? Math.max(...prescriptions.map(p => p.id)) + 1 : 1);
      currentOrderId = JSON.parse(localStorage.getItem('currentOrderId')) || (orders.length > 0 ? Math.max(...orders.map(o => o.id)) + 1 : 1);
    }

    function saveAllDataToLocalStorage() {
      localStorage.setItem('medicines', JSON.stringify(medicines));
      localStorage.setItem('suppliers', JSON.stringify(suppliers));
      localStorage.setItem('customers', JSON.stringify(customers));
      localStorage.setItem('orders', JSON.stringify(orders));
      localStorage.setItem('prescriptions', JSON.stringify(prescriptions));
      localStorage.setItem('currentPrescriptionId', JSON.stringify(currentPrescriptionId));
      localStorage.setItem('currentOrderId', JSON.stringify(currentOrderId));
    }

    // UI Navigation
    function navigate(pageId) {
      document.querySelectorAll('.page').forEach(page => {
        page.classList.remove('active');
      });
      document.getElementById(pageId).classList.add('active');
      document.getElementById('page-title').textContent = document.getElementById(pageId + 'Btn').textContent.trim();
      
      // Update data and charts when navigating
      if (pageId === 'dashboard') {
        updateDashboardStats();
        updateExpiryChart();
        renderRecentOrders();
      } else if (pageId === 'medicines') {
        renderMedicineTable();
        clearMedicineForm();
      } else if (pageId === 'suppliers') {
        renderSupplierTable();
        clearSupplierForm();
      } else if (pageId === 'customers') {
        renderCustomerTable();
        clearCustomerForm();
      } else if (pageId === 'orders') {
        updateCustomerDropdowns();
        updateMedicineDropdowns();
        clearOrderForm();
        renderOrderTable();
      } else if (pageId === 'prescriptions') {
        updateCustomerDropdowns();
        updateMedicineDropdowns();
        clearPrescriptionForm();
        renderPrescriptionTable();
      } else if (pageId === 'statistics') {
        updateSalesChart();
        updateMedicinesChart();
        updateExpiryReportTable();
      } else if (pageId === 'settings') {
        loadSettingsToForm();
      }

      // Hide sidebar on mobile after navigation
      if (window.innerWidth <= 768) {
        document.getElementById('sidebar').classList.remove('visible');
        document.getElementById('main').classList.add('full');
      }
    }

    function toggleSidebar() {
      const sidebar = document.getElementById('sidebar');
      const main = document.getElementById('main');
      sidebar.classList.toggle('hidden');
      main.classList.toggle('full');
    }

    function toggleTheme() {
      darkMode = !darkMode;
      applySettings();
      saveSettings(); // Save the theme preference
      updateChartsTheme();
    }

    function printPage() {
      window.print();
    }

    // Helper function to display messages
    function showMessage(elementId, message, type) {
      const element = document.getElementById(elementId);
      element.textContent = message;
      element.className = `message ${type}`;
      setTimeout(() => {
        element.className = 'message';
        element.textContent = '';
      }, 3000);
    }

    // --- Medicines Functions ---
    function addMedicine() {
      const name = document.getElementById('medName').value;
      const generic = document.getElementById('medGeneric').value;
      const category = document.getElementById('medCategory').value;
      const qty = parseInt(document.getElementById('medQty').value);
      const price = parseFloat(document.getElementById('medPrice').value);
      const exp = document.getElementById('medExp').value;

      if (!name || !qty || !price || !exp) {
        showMessage('med-message', 'الرجاء تعبئة جميع الحقول المطلوبة.', 'error');
        return;
      }

      if (editingMedicineId) {
        const medIndex = medicines.findIndex(m => m.id === editingMedicineId);
        if (medIndex !== -1) {
          medicines[medIndex] = { id: editingMedicineId, name, generic, category, qty, price, exp };
          showMessage('med-message', 'تم تحديث الدواء بنجاح!', 'success');
        }
        editingMedicineId = null;
        document.getElementById('addMedicineBtn').textContent = 'إضافة دواء';
      } else {
        const newId = medicines.length > 0 ? Math.max(...medicines.map(m => m.id)) + 1 : 1;
        medicines.push({ id: newId, name, generic, category, qty, price, exp });
        showMessage('med-message', 'تم إضافة الدواء بنجاح!', 'success');
      }
      saveAllDataToLocalStorage();
      renderMedicineTable();
      clearMedicineForm();
      updateMedicineDropdowns();
      updateDashboardStats();
      updateExpiryChart();
      updateMedicinesChart();
      updateExpiryReportTable();
    }

    function renderMedicineTable(filter = '') {
      const tableBody = document.querySelector('#medTable tbody');
      tableBody.innerHTML = '';
      const filteredMedicines = medicines.filter(med =>
        med.name.toLowerCase().includes(filter.toLowerCase()) ||
        med.generic.toLowerCase().includes(filter.toLowerCase()) ||
        med.category.toLowerCase().includes(filter.toLowerCase())
      );

      filteredMedicines.forEach(med => {
        const row = tableBody.insertRow();
        row.insertCell().textContent = med.name;
        row.insertCell().textContent = med.generic;
        row.insertCell().textContent = med.category;
        row.insertCell().textContent = med.qty;
        row.insertCell().textContent = med.price.toFixed(2) + ' ' + settings.currency;
        row.insertCell().textContent = med.exp;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const editBtn = document.createElement('button');
        editBtn.textContent = 'تعديل';
        editBtn.classList.add('warning');
        editBtn.onclick = () => editMedicine(med.id);
        actionsCell.appendChild(editBtn);

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'حذف';
        deleteBtn.classList.add('danger');
        deleteBtn.onclick = () => deleteMedicine(med.id);
        actionsCell.appendChild(deleteBtn);
      });
    }

    function editMedicine(id) {
      const med = medicines.find(m => m.id === id);
      if (med) {
        document.getElementById('medName').value = med.name;
        document.getElementById('medGeneric').value = med.generic;
        document.getElementById('medCategory').value = med.category;
        document.getElementById('medQty').value = med.qty;
        document.getElementById('medPrice').value = med.price;
        document.getElementById('medExp').value = med.exp;
        editingMedicineId = id;
        document.getElementById('addMedicineBtn').textContent = 'تحديث الدواء';
      }
    }

    function deleteMedicine(id) {
      if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
        medicines = medicines.filter(m => m.id !== id);
        saveAllDataToLocalStorage();
        renderMedicineTable();
        updateMedicineDropdowns();
        updateDashboardStats();
        updateExpiryChart();
        updateMedicinesChart();
        updateExpiryReportTable();
        showMessage('med-message', 'تم حذف الدواء بنجاح!', 'success');
      }
    }

    function clearMedicineForm() {
      document.getElementById('medName').value = '';
      document.getElementById('medGeneric').value = '';
      document.getElementById('medCategory').value = '';
      document.getElementById('medQty').value = '';
      document.getElementById('medPrice').value = '';
      document.getElementById('medExp').value = new Date().toISOString().split('T')[0];
      editingMedicineId = null;
      document.getElementById('addMedicineBtn').textContent = 'إضافة دواء';
      showMessage('med-message', '', ''); // Clear messages
    }

    function searchMedicine(query) {
      renderMedicineTable(query);
    }

    // --- Suppliers Functions ---
    function addSupplier() {
      const name = document.getElementById('supplierName').value;
      const phone = document.getElementById('supplierPhone').value;
      const email = document.getElementById('supplierEmail').value;
      const address = document.getElementById('supplierAddress').value;

      if (!name || !phone) {
        showMessage('supplier-message', 'الرجاء تعبئة اسم المورد ورقم الهاتف.', 'error');
        return;
      }

      if (editingSupplierId) {
        const supIndex = suppliers.findIndex(s => s.id === editingSupplierId);
        if (supIndex !== -1) {
          suppliers[supIndex] = { id: editingSupplierId, name, phone, email, address };
          showMessage('supplier-message', 'تم تحديث المورد بنجاح!', 'success');
        }
        editingSupplierId = null;
        document.getElementById('addSupplierBtn').textContent = 'إضافة مورد';
      } else {
        const newId = suppliers.length > 0 ? Math.max(...suppliers.map(s => s.id)) + 1 : 1;
        suppliers.push({ id: newId, name, phone, email, address });
        showMessage('supplier-message', 'تم إضافة المورد بنجاح!', 'success');
      }
      saveAllDataToLocalStorage();
      renderSupplierTable();
      clearSupplierForm();
    }

    function renderSupplierTable(filter = '') {
      const tableBody = document.querySelector('#supplierTable tbody');
      tableBody.innerHTML = '';
      const filteredSuppliers = suppliers.filter(sup =>
        sup.name.toLowerCase().includes(filter.toLowerCase()) ||
        sup.phone.toLowerCase().includes(filter.toLowerCase()) ||
        sup.email.toLowerCase().includes(filter.toLowerCase()) ||
        sup.address.toLowerCase().includes(filter.toLowerCase())
      );

      filteredSuppliers.forEach(sup => {
        const row = tableBody.insertRow();
        row.insertCell().textContent = sup.name;
        row.insertCell().textContent = sup.phone;
        row.insertCell().textContent = sup.email;
        row.insertCell().textContent = sup.address;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const editBtn = document.createElement('button');
        editBtn.textContent = 'تعديل';
        editBtn.classList.add('warning');
        editBtn.onclick = () => editSupplier(sup.id);
        actionsCell.appendChild(editBtn);

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'حذف';
        deleteBtn.classList.add('danger');
        deleteBtn.onclick = () => deleteSupplier(sup.id);
        actionsCell.appendChild(deleteBtn);
      });
    }

    function editSupplier(id) {
      const sup = suppliers.find(s => s.id === id);
      if (sup) {
        document.getElementById('supplierName').value = sup.name;
        document.getElementById('supplierPhone').value = sup.phone;
        document.getElementById('supplierEmail').value = sup.email;
        document.getElementById('supplierAddress').value = sup.address;
        editingSupplierId = id;
        document.getElementById('addSupplierBtn').textContent = 'تحديث المورد';
      }
    }

    function deleteSupplier(id) {
      if (confirm('هل أنت متأكد من حذف هذا المورد؟')) {
        suppliers = suppliers.filter(s => s.id !== id);
        saveAllDataToLocalStorage();
        renderSupplierTable();
        showMessage('supplier-message', 'تم حذف المورد بنجاح!', 'success');
      }
    }

    function clearSupplierForm() {
      document.getElementById('supplierName').value = '';
      document.getElementById('supplierPhone').value = '';
      document.getElementById('supplierEmail').value = '';
      document.getElementById('supplierAddress').value = '';
      editingSupplierId = null;
      document.getElementById('addSupplierBtn').textContent = 'إضافة مورد';
      showMessage('supplier-message', '', ''); // Clear messages
    }

    function searchSupplier(query) {
      renderSupplierTable(query);
    }

    // --- Customers Functions ---
    function addCustomer() {
      const name = document.getElementById('customerName').value;
      const phone = document.getElementById('customerPhone').value;
      const address = document.getElementById('customerAddress').value;
      const medicalHistory = document.getElementById('customerMedicalHistory').value;

      if (!name || !phone) {
        showMessage('customer-message', 'الرجاء تعبئة اسم العميل ورقم الهاتف.', 'error');
        return;
      }

      if (editingCustomerId) {
        const custIndex = customers.findIndex(c => c.id === editingCustomerId);
        if (custIndex !== -1) {
          customers[custIndex] = { id: editingCustomerId, name, phone, address, medicalHistory };
          showMessage('customer-message', 'تم تحديث العميل بنجاح!', 'success');
        }
        editingCustomerId = null;
        document.getElementById('addCustomerBtn').textContent = 'إضافة عميل';
      } else {
        const newId = customers.length > 0 ? Math.max(...customers.map(c => c.id)) + 1 : 1;
        customers.push({ id: newId, name, phone, address, medicalHistory });
        showMessage('customer-message', 'تم إضافة العميل بنجاح!', 'success');
      }
      saveAllDataToLocalStorage();
      renderCustomerTable();
      clearCustomerForm();
      updateCustomerDropdowns();
    }

    function renderCustomerTable(filter = '') {
      const tableBody = document.querySelector('#customerTable tbody');
      tableBody.innerHTML = '';
      const filteredCustomers = customers.filter(cust =>
        cust.name.toLowerCase().includes(filter.toLowerCase()) ||
        cust.phone.toLowerCase().includes(filter.toLowerCase()) ||
        cust.address.toLowerCase().includes(filter.toLowerCase()) ||
        cust.medicalHistory.toLowerCase().includes(filter.toLowerCase())
      );

      filteredCustomers.forEach(cust => {
        const row = tableBody.insertRow();
        row.insertCell().textContent = cust.name;
        row.insertCell().textContent = cust.phone;
        row.insertCell().textContent = cust.address;
        row.insertCell().textContent = cust.medicalHistory;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const editBtn = document.createElement('button');
        editBtn.textContent = 'تعديل';
        editBtn.classList.add('warning');
        editBtn.onclick = () => editCustomer(cust.id);
        actionsCell.appendChild(editBtn);

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'حذف';
        deleteBtn.classList.add('danger');
        deleteBtn.onclick = () => deleteCustomer(cust.id);
        actionsCell.appendChild(deleteBtn);
      });
    }

    function editCustomer(id) {
      const cust = customers.find(c => c.id === id);
      if (cust) {
        document.getElementById('customerName').value = cust.name;
        document.getElementById('customerPhone').value = cust.phone;
        document.getElementById('customerAddress').value = cust.address;
        document.getElementById('customerMedicalHistory').value = cust.medicalHistory;
        editingCustomerId = id;
        document.getElementById('addCustomerBtn').textContent = 'تحديث العميل';
      }
    }

    function deleteCustomer(id) {
      if (confirm('هل أنت متأكد من حذف هذا العميل؟')) {
        customers = customers.filter(c => c.id !== id);
        saveAllDataToLocalStorage();
        renderCustomerTable();
        updateCustomerDropdowns();
        showMessage('customer-message', 'تم حذف العميل بنجاح!', 'success');
      }
    }

    function clearCustomerForm() {
      document.getElementById('customerName').value = '';
      document.getElementById('customerPhone').value = '';
      document.getElementById('customerAddress').value = '';
      document.getElementById('customerMedicalHistory').value = '';
      editingCustomerId = null;
      document.getElementById('addCustomerBtn').textContent = 'إضافة عميل';
      showMessage('customer-message', '', ''); // Clear messages
    }

    function searchCustomer(query) {
      renderCustomerTable(query);
    }

    // --- Orders Functions ---
    function updateCustomerDropdowns() {
      const orderCustomerSelect = document.getElementById('orderCustomer');
      const prescriptionCustomerSelect = document.getElementById('prescriptionCustomer');

      // Clear existing options
      orderCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';
      prescriptionCustomerSelect.innerHTML = '<option value="">اختر العميل</option>';

      customers.forEach(cust => {
        const option1 = document.createElement('option');
        option1.value = cust.id;
        option1.textContent = cust.name;
        orderCustomerSelect.appendChild(option1);

        const option2 = document.createElement('option');
        option2.value = cust.id;
        option2.textContent = cust.name;
        prescriptionCustomerSelect.appendChild(option2);
      });
    }

    function updateMedicineDropdowns() {
      const orderMedicineSelect = document.getElementById('orderMedicine');
      const prescriptionMedicineSelect = document.getElementById('prescriptionMedicine');

      // Clear existing options
      orderMedicineSelect.innerHTML = '<option value="">اختر الدواء</option>';
      prescriptionMedicineSelect.innerHTML = '<option value="">اختر الدواء</option>';

      medicines.forEach(med => {
        const option1 = document.createElement('option');
        option1.value = med.id;
        option1.textContent = `${med.name} (متوفر: ${med.qty})`;
        orderMedicineSelect.appendChild(option1);

        const option2 = document.createElement('option');
        option2.value = med.id;
        option2.textContent = med.name;
        prescriptionMedicineSelect.appendChild(option2);
      });
    }

    function addMedicineToOrder() {
      const medId = parseInt(document.getElementById('orderMedicine').value);
      const qty = parseInt(document.getElementById('orderQty').value);

      if (!medId || !qty || qty <= 0) {
        showMessage('order-message', 'الرجاء اختيار دواء وإدخال كمية صحيحة.', 'error');
        return;
      }

      const medicine = medicines.find(m => m.id === medId);
      if (!medicine) {
        showMessage('order-message', 'الدواء المحدد غير موجود.', 'error');
        return;
      }

      if (medicine.qty < qty) {
        showMessage('order-message', `الكمية المطلوبة من ${medicine.name} غير متوفرة. المتوفر: ${medicine.qty}`, 'error');
        return;
      }

      const existingItemIndex = currentOrderItems.findIndex(item => item.medId === medId);
      if (existingItemIndex !== -1) {
        currentOrderItems[existingItemIndex].qty += qty;
        currentOrderItems[existingItemIndex].total = currentOrderItems[existingItemIndex].qty * medicine.price;
      } else {
        currentOrderItems.push({
          medId: medicine.id,
          name: medicine.name,
          qty: qty,
          price: medicine.price,
          total: qty * medicine.price
        });
      }
      renderOrderItems();
      showMessage('order-message', 'تم إضافة الدواء إلى الفاتورة.', 'success');
    }

    function removeMedicineFromOrder(index) {
      currentOrderItems.splice(index, 1);
      renderOrderItems();
      showMessage('order-message', 'تم إزالة الدواء من الفاتورة.', 'success');
    }

    function renderOrderItems() {
      const tableBody = document.querySelector('#orderItems tbody');
      tableBody.innerHTML = '';
      let total = 0;

      currentOrderItems.forEach((item, index) => {
        const row = tableBody.insertRow();
        row.insertCell().textContent = item.name;
        row.insertCell().textContent = item.qty;
        row.insertCell().textContent = item.price.toFixed(2) + ' ' + settings.currency;
        row.insertCell().textContent = item.total.toFixed(2) + ' ' + settings.currency;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const removeBtn = document.createElement('button');
        removeBtn.textContent = 'إزالة';
        removeBtn.classList.add('danger');
        removeBtn.onclick = () => removeMedicineFromOrder(index);
        actionsCell.appendChild(removeBtn);
        total += item.total;
      });
      document.getElementById('orderTotal').textContent = total.toFixed(2) + ' ' + settings.currency;
    }

    function createOrder() {
      const customerId = parseInt(document.getElementById('orderCustomer').value);
      if (!customerId) {
        showMessage('order-message', 'الرجاء اختيار عميل.', 'error');
        return;
      }
      if (currentOrderItems.length === 0) {
        showMessage('order-message', 'الرجاء إضافة أدوية إلى الفاتورة.', 'error');
        return;
      }

      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showMessage('order-message', 'العميل المحدد غير موجود.', 'error');
        return;
      }

      let totalAmount = parseFloat(document.getElementById('orderTotal').textContent);
      const taxAmount = totalAmount * (settings.taxRate / 100);
      totalAmount += taxAmount;

      const newOrder = {
        id: currentOrderId++,
        date: new Date().toISOString().split('T')[0],
        customerId: customerId,
        customerName: customer.name,
        total: totalAmount,
        status: "مكتمل", // Default status
        items: currentOrderItems
      };

      // Deduct medicine quantities from stock
      for (const orderItem of currentOrderItems) {
        const med = medicines.find(m => m.id === orderItem.medId);
        if (med) {
          med.qty -= orderItem.qty;
        }
      }

      orders.push(newOrder);
      saveAllDataToLocalStorage();
      showMessage('order-message', 'تم إنشاء الفاتورة بنجاح!', 'success');
      clearOrderForm();
      renderOrderTable();
      updateDashboardStats();
      updateMedicineDropdowns(); // To reflect updated quantities
      updateSalesChart();
      updateMedicinesChart();
    }

    function clearOrderForm() {
      document.getElementById('orderCustomer').value = '';
      document.getElementById('orderMedicine').value = '';
      document.getElementById('orderQty').value = '1';
      currentOrderItems = [];
      renderOrderItems();
      showMessage('order-message', '', ''); // Clear messages
    }

    function renderOrderTable(filter = '') {
      const tableBody = document.querySelector('#orderTable tbody');
      tableBody.innerHTML = '';
      const filteredOrders = orders.filter(order => {
        const customerName = customers.find(c => c.id === order.customerId)?.name || "عميل غير معروف";
        return order.id.toString().includes(filter) ||
          order.date.includes(filter) ||
          customerName.toLowerCase().includes(filter.toLowerCase()) ||
          order.status.toLowerCase().includes(filter.toLowerCase());
      });

      filteredOrders.forEach(order => {
        const row = tableBody.insertRow();
        const customerName = customers.find(c => c.id === order.customerId)?.name || "عميل غير معروف";
        row.insertCell().textContent = order.id;
        row.insertCell().textContent = order.date;
        row.insertCell().textContent = customerName;
        row.insertCell().textContent = order.total.toFixed(2) + ' ' + settings.currency;
        row.insertCell().textContent = order.status;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const viewBtn = document.createElement('button');
        viewBtn.textContent = 'عرض';
        viewBtn.classList.add('primary');
        viewBtn.onclick = () => viewOrderDetails(order.id);
        actionsCell.appendChild(viewBtn);

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'حذف';
        deleteBtn.classList.add('danger');
        deleteBtn.onclick = () => deleteOrder(order.id);
        actionsCell.appendChild(deleteBtn);
      });
    }

    function searchOrder(query) {
      renderOrderTable(query);
    }

    function viewOrderDetails(id) {
      const order = orders.find(o => o.id === id);
      if (order) {
        let details = `
          <h3>تفاصيل الفاتورة رقم ${order.id}</h3>
          <p><strong>التاريخ:</strong> ${order.date}</p>
          <p><strong>العميل:</strong> ${order.customerName}</p>
          <p><strong>الحالة:</strong> ${order.status}</p>
          <h4>الأدوية:</h4>
          <table>
            <thead>
              <tr>
                <th>الدواء</th>
                <th>الكمية</th>
                <th>السعر</th>
                <th>الإجمالي</th>
              </tr>
            </thead>
            <tbody>
        `;
        order.items.forEach(item => {
          details += `
            <tr>
              <td>${item.name}</td>
              <td>${item.qty}</td>
              <td>${item.price.toFixed(2)} ${settings.currency}</td>
              <td>${item.total.toFixed(2)} ${settings.currency}</td>
            </tr>
          `;
        });
        details += `
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" style="text-align: left;">المجموع الكلي (مع الضريبة):</td>
                <td>${order.total.toFixed(2)} ${settings.currency}</td>
              </tr>
            </tfoot>
          </table>
        `;
        alert(details); // For a simple popup, consider a modal for better UI
      }
    }

    function deleteOrder(id) {
      if (confirm('هل أنت متأكد من حذف هذه الفاتورة؟')) {
        // Return stock to medicines
        const orderToDelete = orders.find(o => o.id === id);
        if (orderToDelete) {
          for (const item of orderToDelete.items) {
            const med = medicines.find(m => m.id === item.medId);
            if (med) {
              med.qty += item.qty;
            }
          }
        }

        orders = orders.filter(o => o.id !== id);
        saveAllDataToLocalStorage();
        renderOrderTable();
        updateDashboardStats();
        updateMedicineDropdowns();
        updateSalesChart();
        updateMedicinesChart();
        showMessage('order-message', 'تم حذف الفاتورة بنجاح!', 'success');
      }
    }

    function printOrder() {
      // This function would ideally generate a printable receipt
      // For this example, we'll just trigger the browser print
      window.print();
    }

    // --- Prescriptions Functions ---
    function addMedicineToPrescription() {
      const medId = parseInt(document.getElementById('prescriptionMedicine').value);
      const qty = parseInt(document.getElementById('prescriptionMedicineQty').value);
      const dosage = document.getElementById('prescriptionDosage').value;

      if (!medId || !qty || qty <= 0 || !dosage) {
        showMessage('prescription-message', 'الرجاء اختيار دواء، إدخال كمية صحيحة، وتحديد الجرعة.', 'error');
        return;
      }

      const medicine = medicines.find(m => m.id === medId);
      if (!medicine) {
        showMessage('prescription-message', 'الدواء المحدد غير موجود.', 'error');
        return;
      }

      const existingItemIndex = currentPrescriptionItems.findIndex(item => item.medId === medId);
      if (existingItemIndex !== -1) {
        currentPrescriptionItems[existingItemIndex].qty += qty;
        currentPrescriptionItems[existingItemIndex].dosage = dosage; // Update dosage if exists
      } else {
        currentPrescriptionItems.push({
          medId: medicine.id,
          name: medicine.name,
          qty: qty,
          dosage: dosage
        });
      }
      renderPrescriptionItems();
      showMessage('prescription-message', 'تم إضافة الدواء إلى الوصفة.', 'success');
    }

    function removeMedicineFromPrescription(index) {
      currentPrescriptionItems.splice(index, 1);
      renderPrescriptionItems();
      showMessage('prescription-message', 'تم إزالة الدواء من الوصفة.', 'success');
    }

    function renderPrescriptionItems() {
      const tableBody = document.querySelector('#prescriptionItems tbody');
      tableBody.innerHTML = '';

      currentPrescriptionItems.forEach((item, index) => {
        const row = tableBody.insertRow();
        row.insertCell().textContent = item.name;
        row.insertCell().textContent = item.qty;
        row.insertCell().textContent = item.dosage;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const removeBtn = document.createElement('button');
        removeBtn.textContent = 'إزالة';
        removeBtn.classList.add('danger');
        removeBtn.onclick = () => removeMedicineFromPrescription(index);
        actionsCell.appendChild(removeBtn);
      });
    }

    function savePrescription() {
      const customerId = parseInt(document.getElementById('prescriptionCustomer').value);
      const doctor = document.getElementById('prescriptionDoctor').value;
      const date = document.getElementById('prescriptionDate').value;
      const notes = document.getElementById('prescriptionNotes').value;

      if (!customerId || !doctor || !date || currentPrescriptionItems.length === 0) {
        showMessage('prescription-message', 'الرجاء تعبئة جميع الحقول المطلوبة وإضافة أدوية للوصفة.', 'error');
        return;
      }

      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showMessage('prescription-message', 'العميل المحدد غير موجود.', 'error');
        return;
      }

      const newPrescription = {
        id: currentPrescriptionId++,
        customerId: customerId,
        customerName: customer.name,
        doctor: doctor,
        date: date,
        notes: notes,
        items: currentPrescriptionItems
      };

      prescriptions.push(newPrescription);
      saveAllDataToLocalStorage();
      showMessage('prescription-message', 'تم حفظ الوصفة بنجاح!', 'success');
      clearPrescriptionForm();
      renderPrescriptionTable();
    }

    function clearPrescriptionForm() {
      document.getElementById('prescriptionCustomer').value = '';
      document.getElementById('prescriptionDoctor').value = '';
      document.getElementById('prescriptionDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('prescriptionNotes').value = '';
      document.getElementById('prescriptionMedicine').value = '';
      document.getElementById('prescriptionMedicineQty').value = '1';
      document.getElementById('prescriptionDosage').value = '';
      currentPrescriptionItems = [];
      renderPrescriptionItems();
      showMessage('prescription-message', '', ''); // Clear messages
    }

    function renderPrescriptionTable(filter = '') {
      const tableBody = document.querySelector('#prescriptionTable tbody');
      tableBody.innerHTML = '';
      const filteredPrescriptions = prescriptions.filter(pres => {
        const customerName = customers.find(c => c.id === pres.customerId)?.name || "عميل غير معروف";
        return pres.id.toString().includes(filter) ||
          pres.date.includes(filter) ||
          customerName.toLowerCase().includes(filter.toLowerCase()) ||
          pres.doctor.toLowerCase().includes(filter.toLowerCase());
      });

      filteredPrescriptions.forEach(pres => {
        const row = tableBody.insertRow();
        const customerName = customers.find(c => c.id === pres.customerId)?.name || "عميل غير معروف";
        row.insertCell().textContent = pres.id;
        row.insertCell().textContent = pres.date;
        row.insertCell().textContent = customerName;
        row.insertCell().textContent = pres.doctor;

        const actionsCell = row.insertCell();
        actionsCell.classList.add('actions-cell');
        const viewBtn = document.createElement('button');
        viewBtn.textContent = 'عرض';
        viewBtn.classList.add('primary');
        viewBtn.onclick = () => viewPrescriptionDetails(pres.id);
        actionsCell.appendChild(viewBtn);

        const deleteBtn = document.createElement('button');
        deleteBtn.textContent = 'حذف';
        deleteBtn.classList.add('danger');
        deleteBtn.onclick = () => deletePrescription(pres.id);
        actionsCell.appendChild(deleteBtn);
      });
    }

    function searchPrescription(query) {
      renderPrescriptionTable(query);
    }

    function viewPrescriptionDetails(id) {
      const prescription = prescriptions.find(p => p.id === id);
      if (prescription) {
        let details = `
          <h3>تفاصيل الوصفة رقم ${prescription.id}</h3>
          <p><strong>التاريخ:</strong> ${prescription.date}</p>
          <p><strong>العميل:</strong> ${prescription.customerName}</p>
          <p><strong>الطبيب:</strong> ${prescription.doctor}</p>
          <p><strong>ملاحظات:</strong> ${prescription.notes || 'لا يوجد'}</p>
          <h4>الأدوية الموصوفة:</h4>
          <table>
            <thead>
              <tr>
                <th>الدواء</th>
                <th>الكمية</th>
                <th>الجرعة</th>
              </tr>
            </thead>
            <tbody>
        `;
        prescription.items.forEach(item => {
          details += `
            <tr>
              <td>${item.name}</td>
              <td>${item.qty}</td>
              <td>${item.dosage}</td>
            </tr>
          `;
        });
        details += `
            </tbody>
          </table>
        `;
        alert(details); // For a simple popup, consider a modal for better UI
      }
    }

    function deletePrescription(id) {
      if (confirm('هل أنت متأكد من حذف هذه الوصفة؟')) {
        prescriptions = prescriptions.filter(p => p.id !== id);
        saveAllDataToLocalStorage();
        renderPrescriptionTable();
        showMessage('prescription-message', 'تم حذف الوصفة بنجاح!', 'success');
      }
    }

    // --- Calculator Functions ---
    let display = document.getElementById('calc-display');
    let currentExpression = '';

    function calc(value) {
      currentExpression += value;
      display.value = currentExpression;
    }

    function calculate() {
      try {
        const result = eval(currentExpression.replace('×', '*').replace('÷', '/'));
        display.value = result;
        currentExpression = result.toString();
      } catch (e) {
        display.value = 'خطأ';
        currentExpression = '';
      }
    }

    function clearCalc() {
      currentExpression = '';
      display.value = '';
    }

    function backspace() {
      currentExpression = currentExpression.slice(0, -1);
      display.value = currentExpression;
    }

    // --- Dashboard Functions ---
    function updateDashboardStats() {
      document.getElementById('med-count').textContent = medicines.length;

      const today = new Date();
      const expiringSoonThreshold = new Date();
      expiringSoonThreshold.setDate(today.getDate() + settings.daysBeforeExpiry);

      let expiringSoonCount = 0;
      let expiredCount = 0;

      medicines.forEach(med => {
        const expDate = new Date(med.exp);
        if (expDate < today) {
          expiredCount++;
        } else if (expDate <= expiringSoonThreshold) {
          expiringSoonCount++;
        }
      });

      document.getElementById('expiring-soon').textContent = expiringSoonCount;
      document.getElementById('expired').textContent = expiredCount;

      const totalSales = orders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('sales').textContent = totalSales.toFixed(2) + ' ' + settings.currency;

      renderRecentOrders();
    }

    function renderRecentOrders() {
      const tableBody = document.getElementById('recent-orders');
      tableBody.innerHTML = '';
      const recentFiveOrders = orders.slice(-5).reverse(); // Get last 5 and reverse to show most recent first

      if (recentFiveOrders.length === 0) {
        tableBody.innerHTML = '<tr><td colspan="5">لا توجد فواتير حديثة لعرضها.</td></tr>';
        return;
      }

      recentFiveOrders.forEach(order => {
        const row = tableBody.insertRow();
        const customerName = customers.find(c => c.id === order.customerId)?.name || "عميل غير معروف";
        row.insertCell().textContent = order.id;
        row.insertCell().textContent = order.date;
        row.insertCell().textContent = customerName;
        row.insertCell().textContent = order.total.toFixed(2) + ' ' + settings.currency;
        row.insertCell().textContent = order.status;
      });
    }

    // --- Chart Functions ---
    function initCharts() {
      const expiryCtx = document.getElementById('expiryChart').getContext('2d');
      expiryChartInstance = new Chart(expiryCtx, {
        type: 'pie',
        data: {
          labels: ['صالح', 'تنتهي قريباً', 'منتهي الصلاحية'],
          datasets: [{
            data: [0, 0, 0],
            backgroundColor: ['#388e3c', '#ffa000', '#d32f2f'],
            hoverOffset: 4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'top',
              labels: {
                color: darkMode ? 'white' : 'black'
              }
            },
            title: {
              display: false,
              text: 'حالة انتهاء صلاحية الأدوية'
            }
          }
        }
      });

      const salesCtx = document.getElementById('salesChart').getContext('2d');
      salesChartInstance = new Chart(salesCtx, {
        type: 'bar',
        data: {
          labels: [], // Will be populated dynamically
          datasets: [{
            label: 'إجمالي المبيعات',
            data: [], // Will be populated dynamically
            backgroundColor: var_to_rgb('--primary'),
            borderColor: var_to_rgb('--primary-dark'),
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              display: true,
              labels: {
                color: darkMode ? 'white' : 'black'
              }
            }
          },
          scales: {
            x: {
              ticks: { color: darkMode ? 'white' : 'black' },
              grid: { color: darkMode ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)' }
            },
            y: {
              beginAtZero: true,
              ticks: { color: darkMode ? 'white' : 'black' },
              grid: { color: darkMode ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)' }
            }
          }
        }
      });

      const medicinesCtx = document.getElementById('medicinesChart').getContext('2d');
      medicinesChartInstance = new Chart(medicinesCtx, {
        type: 'line',
        data: {
          labels: ['مضاد حيوي', 'مسكن', 'مضاد هستامين', 'مضاد حموضة', 'أخرى'],
          datasets: [{
            label: 'عدد الأدوية حسب الفئة',
            data: [0, 0, 0, 0, 0], // Will be populated dynamically
            borderColor: var_to_rgb('--success'),
            tension: 0.1,
            fill: false
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              display: true,
              labels: {
                color: darkMode ? 'white' : 'black'
              }
            }
          },
          scales: {
            x: {
              ticks: { color: darkMode ? 'white' : 'black' },
              grid: { color: darkMode ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)' }
            },
            y: {
              beginAtZero: true,
              ticks: { color: darkMode ? 'white' : 'black', stepSize: 1 },
              grid: { color: darkMode ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)' }
            }
          }
        }
      });
      updateChartsTheme();
    }

    function updateChartsTheme() {
      const textColor = darkMode ? 'white' : 'black';
      const gridColor = darkMode ? 'rgba(255,255,255,0.1)' : 'rgba(0,0,0,0.1)';

      [expiryChartInstance, salesChartInstance, medicinesChartInstance].forEach(chart => {
        if (chart) {
          chart.options.plugins.legend.labels.color = textColor;
          if (chart.options.scales && chart.options.scales.x) {
            chart.options.scales.x.ticks.color = textColor;
            chart.options.scales.x.grid.color = gridColor;
          }
          if (chart.options.scales && chart.options.scales.y) {
            chart.options.scales.y.ticks.color = textColor;
            chart.options.scales.y.grid.color = gridColor;
          }
          chart.update();
        }
      });
    }

    function updateExpiryChart() {
      const today = new Date();
      const expiringSoonThreshold = new Date();
      expiringSoonThreshold.setDate(today.getDate() + settings.daysBeforeExpiry);

      let validCount = 0;
      let expiringSoonCount = 0;
      let expiredCount = 0;

      medicines.forEach(med => {
        const expDate = new Date(med.exp);
        if (expDate < today) {
          expiredCount++;
        } else if (expDate <= expiringSoonThreshold) {
          expiringSoonCount++;
        } else {
          validCount++;
        }
      });

      if (expiryChartInstance) {
        expiryChartInstance.data.datasets[0].data = [validCount, expiringSoonCount, expiredCount];
        expiryChartInstance.update();
      }
    }

    function updateSalesChart() {
      const salesByMonth = {};
      orders.forEach(order => {
        const month = order.date.substring(0, 7); // YYYY-MM
        if (!salesByMonth[month]) {
          salesByMonth[month] = 0;
        }
        salesByMonth[month] += order.total;
      });

      const sortedMonths = Object.keys(salesByMonth).sort();
      const salesData = sortedMonths.map(month => salesByMonth[month]);

      if (salesChartInstance) {
        salesChartInstance.data.labels = sortedMonths;
        salesChartInstance.data.datasets[0].data = salesData;
        salesChartInstance.update();
      }
    }

    function updateMedicinesChart() {
      const categories = ['مضاد حيوي', 'مسكن', 'مضاد هستامين', 'مضاد حموضة', 'أخرى'];
      const categoryCounts = categories.map(cat => 
        medicines.filter(med => med.category === cat).length
      );

      if (medicinesChartInstance) {
        medicinesChartInstance.data.datasets[0].data = categoryCounts;
        medicinesChartInstance.update();
      }
    }

    function updateExpiryReportTable() {
      const tableBody = document.querySelector('#expiryReportTable tbody');
      tableBody.innerHTML = '';
      const today = new Date();
      const expiringMedicines = medicines.filter(med => {
        const expDate = new Date(med.exp);
        return expDate < today || (expDate <= new Date(today.getTime() + settings.daysBeforeExpiry * 24 * 60 * 60 * 1000));
      }).sort((a, b) => new Date(a.exp) - new Date(b.exp));

      if (expiringMedicines.length === 0) {
        tableBody.innerHTML = '<tr><td colspan="4">لا توجد أدوية منتهية الصلاحية أو تنتهي قريباً.</td></tr>';
        return;
      }

      expiringMedicines.forEach(med => {
        const expDate = new Date(med.exp);
        const diffTime = expDate - today;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        const row = tableBody.insertRow();
        row.insertCell().textContent = med.name;
        row.insertCell().textContent = med.qty;
        row.insertCell().textContent = med.exp;
        const daysRemainingCell = row.insertCell();
        daysRemainingCell.textContent = diffDays <= 0 ? 'منتهي الصلاحية' : `${diffDays} يوماً`;
        if (diffDays <= 0) {
          daysRemainingCell.style.color = 'var(--danger)';
          daysRemainingCell.style.fontWeight = 'bold';
        } else if (diffDays <= settings.daysBeforeExpiry) {
          daysRemainingCell.style.color = 'var(--warning)';
          daysRemainingCell.style.fontWeight = 'bold';
        }
      });
    }

    // --- Settings Functions ---
    function loadSettings() {
      const savedSettings = JSON.parse(localStorage.getItem('pharmacySettings'));
      if (savedSettings) {
        settings = { ...settings, ...savedSettings };
        darkMode = savedSettings.darkMode || false; // Load dark mode preference
      }
      applySettings();
    }

    function saveSettings() {
      settings.pharmacyName = document.getElementById('pharmacyName').value;
      settings.pharmacyAddress = document.getElementById('pharmacyAddress').value;
      settings.pharmacyPhone = document.getElementById('pharmacyPhone').value;
      settings.currency = document.getElementById('currency').value;
      settings.taxRate = parseFloat(document.getElementById('taxRate').value);
      settings.enableNotifications = document.getElementById('enableNotifications').checked;
      settings.enableExpiryAlerts = document.getElementById('enableExpiryAlerts').checked;
      settings.daysBeforeExpiry = parseInt(document.getElementById('daysBeforeExpiry').value);
      settings.darkMode = darkMode; // Save dark mode state

      localStorage.setItem('pharmacySettings', JSON.stringify(settings));
      showMessage('settings-message', 'تم حفظ الإعدادات بنجاح!', 'success');
      applySettings();
      updateDashboardStats(); // To update currency display
      updateExpiryChart(); // In case daysBeforeExpiry changed
      updateExpiryReportTable(); // In case daysBeforeExpiry changed
    }

    function loadSettingsToForm() {
      document.getElementById('pharmacyName').value = settings.pharmacyName;
      document.getElementById('pharmacyAddress').value = settings.pharmacyAddress;
      document.getElementById('pharmacyPhone').value = settings.pharmacyPhone;
      document.getElementById('currency').value = settings.currency;
      document.getElementById('taxRate').value = settings.taxRate;
      document.getElementById('enableNotifications').checked = settings.enableNotifications;
      document.getElementById('enableExpiryAlerts').checked = settings.enableExpiryAlerts;
      document.getElementById('daysBeforeExpiry').value = settings.daysBeforeExpiry;
      showMessage('settings-message', '', ''); // Clear messages
    }

    function resetSettings() {
      if (confirm('هل أنت متأكد من إعادة تعيين جميع الإعدادات إلى الافتراضيات؟')) {
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
        darkMode = false; // Reset dark mode as well
        localStorage.removeItem('pharmacySettings');
        loadSettingsToForm();
        applySettings();
        updateDashboardStats();
        updateExpiryChart();
        updateExpiryReportTable();
        showMessage('settings-message', 'تم إعادة تعيين الإعدادات بنجاح!', 'success');
      }
    }

    function applySettings() {
      document.body.classList.toggle('dark', darkMode);
      document.getElementById('toggleTheme').textContent = darkMode ? 'light_mode' : 'dark_mode';
      // Update dynamic CSS variables if needed, though with :root variables this is less direct.
      // For Chart.js, we need to explicitly update chart options.
      updateChartsTheme();
    }

    // Helper function to convert CSS variable to RGB string for Chart.js
    function var_to_rgb(variable) {
      const style = getComputedStyle(document.body);
      const color = style.getPropertyValue(variable).trim();
      return color;
    }
  </script>
</body>
</html>
