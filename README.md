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
      left: 0;
      top: 0;
      overflow-y: auto;
      transition: transform 0.3s ease-in-out;
      z-index: 1000;
    }

    .sidebar.hidden {
      transform: translateX(-100%);
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
      margin-right: 250px;
      padding: 2rem;
      flex-grow: 1;
      transition: margin-right 0.3s;
      width: 100%;
    }

    .main.full {
      margin-right: 0;
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
        transform: translateX(-100%);
      }

      .sidebar.visible {
        transform: translateX(0);
      }

      .main {
        margin-right: 0;
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
        margin-right: 0;
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
              <th>العنوا��</th>
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

    <!-- Prescriptions Page -->
    <div id="prescriptions" class="page">
      <div class="card">
        <h3>إضافة وصفة طبية</h3>
        <div id="prescription-message" class="message"></div>
        <div class="form-group">
          <label for="prescriptionCustomer">العميل</label>
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

    // Update supplier
    function updateSupplier(id) {
      const supplierIndex = suppliers.findIndex(s => s.id === id);
      if (supplierIndex === -1) return;
      
      const name = document.getElementById('supplierName').value.trim();
      const phone = document.getElementById('supplierPhone').value.trim();
      const email = document.getElementById('supplierEmail').value.trim();
      const address = document.getElementById('supplierAddress').value.trim();
      
      // Validate inputs
      if (!name || !phone) {
        showMessage('supplier-message', 'يرجى إدخال اسم المورد ورقم الهاتف', 'error');
        return;
      }
      
      // Update supplier
      suppliers[supplierIndex] = {
        id,
        name,
        phone,
        email,
        address
      };
      
      updateSupplierTable();
      
      // Clear form and reset button
      clearSupplierForm();
      const addButton = document.querySelector('#suppliers .btn-group button:first-child');
      addButton.textContent = 'إضافة مورد';
      addButton.onclick = function() { addSupplier(); };
      
      showMessage('supplier-message', 'تم تحديث المورد بنجاح', 'success');
    }

    // Delete supplier
    function deleteSupplier(id) {
      if (confirm('هل أنت متأكد من حذف هذا المورد؟')) {
        suppliers = suppliers.filter(s => s.id !== id);
        updateSupplierTable();
        showMessage('supplier-message', 'تم حذف المورد بنجاح', 'success');
      }
    }

    // Search suppliers
    function searchSupplier(query) {
      const rows = document.querySelectorAll('#supplierTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const name = row.cells[0].textContent.toLowerCase();
        const phone = row.cells[1].textContent.toLowerCase();
        const email = row.cells[2].textContent.toLowerCase();
        const address = row.cells[3].textContent.toLowerCase();
        
        if (name.includes(lowerQuery) || phone.includes(lowerQuery) || 
            email.includes(lowerQuery) || address.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Add a new customer
    function addCustomer() {
      const name = document.getElementById('customerName').value.trim();
      const phone = document.getElementById('customerPhone').value.trim();
      const address = document.getElementById('customerAddress').value.trim();
      const medicalHistory = document.getElementById('customerMedicalHistory').value.trim();
      
      // Validate inputs
      if (!name || !phone) {
        showMessage('customer-message', 'يرجى إدخال اسم العميل ورقم الهاتف', 'error');
        return;
      }
      
      // Create new customer
      const newCustomer = {
        id: customers.length > 0 ? Math.max(...customers.map(c => c.id)) + 1 : 1,
        name,
        phone,
        address,
        medicalHistory
      };
      
      customers.push(newCustomer);
      updateCustomerTable();
      updateCustomerDropdowns();
      
      // Clear form
      clearCustomerForm();
      
      showMessage('customer-message', 'تم إضافة العميل بنجاح', 'success');
    }

    // Clear customer form
    function clearCustomerForm() {
      document.getElementById('customerName').value = '';
      document.getElementById('customerPhone').value = '';
      document.getElementById('customerAddress').value = '';
      document.getElementById('customerMedicalHistory').value = '';
    }

    // Update customers table
    function updateCustomerTable() {
      const tbody = document.querySelector('#customerTable tbody');
      tbody.innerHTML = '';
      
      customers.forEach(customer => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${customer.name}</td>
          <td>${customer.phone}</td>
          <td>${customer.address || '-'}</td>
          <td>${customer.medicalHistory || '-'}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="editCustomer(${customer.id})">تعديل</button>
            <button class="no-print danger" onclick="deleteCustomer(${customer.id})">حذف</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // Edit customer
    function editCustomer(id) {
      const customer = customers.find(c => c.id === id);
      if (!customer) return;
      
      document.getElementById('customerName').value = customer.name;
      document.getElementById('customerPhone').value = customer.phone;
      document.getElementById('customerAddress').value = customer.address;
      document.getElementById('customerMedicalHistory').value = customer.medicalHistory;
      
      // Scroll to form
      document.getElementById('customerName').scrollIntoView({ behavior: 'smooth' });
      
      // Change button to update
      const addButton = document.querySelector('#customers .btn-group button:first-child');
      addButton.textContent = 'تحديث العميل';
      addButton.onclick = function() { updateCustomer(id); };
    }

    // Update customer
    function updateCustomer(id) {
      const customerIndex = customers.findIndex(c => c.id === id);
      if (customerIndex === -1) return;
      
      const name = document.getElementById('customerName').value.trim();
      const phone = document.getElementById('customerPhone').value.trim();
      const address = document.getElementById('customerAddress').value.trim();
      const medicalHistory = document.getElementById('customerMedicalHistory').value.trim();
      
      // Validate inputs
      if (!name || !phone) {
        showMessage('customer-message', 'يرجى إدخال اسم العميل ورقم الهاتف', 'error');
        return;
      }
      
      // Update customer
      customers[customerIndex] = {
        id,
        name,
        phone,
        address,
        medicalHistory
      };
      
      updateCustomerTable();
      updateCustomerDropdowns();
      
      // Clear form and reset button
      clearCustomerForm();
      const addButton = document.querySelector('#customers .btn-group button:first-child');
      addButton.textContent = 'إضافة عميل';
      addButton.onclick = function() { addCustomer(); };
      
      showMessage('customer-message', 'تم تحديث العميل بنجاح', 'success');
    }

    // Delete customer
    function deleteCustomer(id) {
      if (confirm('هل أنت متأكد من حذف هذا العميل؟')) {
        customers = customers.filter(c => c.id !== id);
        updateCustomerTable();
        updateCustomerDropdowns();
        showMessage('customer-message', 'تم حذف العميل بنجاح', 'success');
      }
    }

    // Search customers
    function searchCustomer(query) {
      const rows = document.querySelectorAll('#customerTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const name = row.cells[0].textContent.toLowerCase();
        const phone = row.cells[1].textContent.toLowerCase();
        const address = row.cells[2].textContent.toLowerCase();
        const history = row.cells[3].textContent.toLowerCase();
        
        if (name.includes(lowerQuery) || phone.includes(lowerQuery) || 
            address.includes(lowerQuery) || history.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Update customer dropdowns
    function updateCustomerDropdowns() {
      const dropdowns = [
        document.getElementById('orderCustomer'),
        document.getElementById('prescriptionCustomer')
      ];
      
      dropdowns.forEach(dropdown => {
        if (!dropdown) return;
        
        // Save current value
        const currentValue = dropdown.value;
        
        // Clear options except first
        while (dropdown.options.length > 1) {
          dropdown.remove(1);
        }
        
        // Add customers
        customers.forEach(customer => {
          const option = document.createElement('option');
          option.value = customer.id;
          option.textContent = customer.name + ' - ' + customer.phone;
          dropdown.appendChild(option);
        });
        
        // Restore current value if still exists
        if (currentValue && dropdown.querySelector(`option[value="${currentValue}"]`)) {
          dropdown.value = currentValue;
        }
      });
    }

    // Update medicine dropdowns
    function updateMedicineDropdowns() {
      const dropdowns = [
        document.getElementById('orderMedicine'),
        document.getElementById('prescriptionMedicine')
      ];
      
      dropdowns.forEach(dropdown => {
        if (!dropdown) return;
        
        // Save current value
        const currentValue = dropdown.value;
        
        // Clear options except first
        while (dropdown.options.length > 1) {
          dropdown.remove(1);
        }
        
        // Add medicines
        medicines.forEach(medicine => {
          if (medicine.qty > 0) { // Only show medicines with available quantity
            const option = document.createElement('option');
            option.value = medicine.id;
            option.textContent = medicine.name + ' (' + medicine.generic + ') - ' + medicine.price.toFixed(2) + settings.currency;
            dropdown.appendChild(option);
          }
        });
        
        // Restore current value if still exists
        if (currentValue && dropdown.querySelector(`option[value="${currentValue}"]`)) {
          dropdown.value = currentValue;
        }
      });
    }

    // Add medicine to order
    function addMedicineToOrder() {
      const medicineId = parseInt(document.getElementById('orderMedicine').value);
      const qty = parseInt(document.getElementById('orderQty').value);
      
      if (!medicineId || isNaN(qty) || qty <= 0) {
        showMessage('order-message', 'يرجى اختيار دواء وإدخال كمية صحيحة', 'error');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showMessage('order-message', 'الدواء المحدد غير موجود', 'error');
        return;
      }
      
      if (qty > medicine.qty) {
        showMessage('order-message', 'الكمية المطلوبة غير متوفرة في المخزون', 'error');
        return;
      }
      
      // Check if medicine already exists in order
      const tbody = document.querySelector('#orderItems tbody');
      let existingRow = null;
      
      tbody.querySelectorAll('tr').forEach(row => {
        if (parseInt(row.dataset.medicineId) === medicineId) {
          existingRow = row;
        }
      });
      
      if (existingRow) {
        // Update existing row
        const existingQty = parseInt(existingRow.dataset.qty);
        const newQty = existingQty + qty;
        
        if (newQty > medicine.qty) {
          showMessage('order-message', 'الكمية الإجمالية تتجاوز الكمية المتوفرة في المخزون', 'error');
          return;
        }
        
        existingRow.dataset.qty = newQty;
        existingRow.cells[1].textContent = newQty;
        existingRow.cells[3].textContent = (newQty * medicine.price).toFixed(2);
      } else {
        // Add new row
        const tr = document.createElement('tr');
        tr.dataset.medicineId = medicineId;
        tr.dataset.qty = qty;
        tr.dataset.price = medicine.price;
        
        tr.innerHTML = `
          <td>${medicine.name} (${medicine.generic})</td>
          <td>${qty}</td>
          <td>${medicine.price.toFixed(2)}</td>
          <td>${(qty * medicine.price).toFixed(2)}</td>
          <td class="actions-cell">
            <button class="no-print danger" onclick="removeOrderItem(this)">حذف</button>
          </td>
        `;
        
        tbody.appendChild(tr);
      }
      
      // Update total
      updateOrderTotal();
      
      // Clear quantity field
      document.getElementById('orderQty').value = 1;
    }

    // Remove item from order
    function removeOrderItem(button) {
      const row = button.closest('tr');
      row.remove();
      updateOrderTotal();
    }

    // Update order total
    function updateOrderTotal() {
      const rows = document.querySelectorAll('#orderItems tbody tr');
      let total = 0;
      
      rows.forEach(row => {
        total += parseFloat(row.cells[3].textContent);
      });
      
      document.getElementById('orderTotal').textContent = total.toFixed(2);
    }

    // Create order
    function createOrder() {
      const customerId = parseInt(document.getElementById('orderCustomer').value);
      const rows = document.querySelectorAll('#orderItems tbody tr');
      
      if (!customerId) {
        showMessage('order-message', 'يرجى اختيار عميل', 'error');
        return;
      }
      
      if (rows.length === 0) {
        showMessage('order-message', 'لا توجد أدوية في الفاتورة', 'error');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showMessage('order-message', 'العميل المحدد غير موجود', 'error');
        return;
      }
      
      // Prepare order items
      const items = [];
      let total = 0;
      
      rows.forEach(row => {
        const medicineId = parseInt(row.dataset.medicineId);
        const qty = parseInt(row.dataset.qty);
        const price = parseFloat(row.dataset.price);
        
        items.push({
          medicineId,
          qty,
          price
        });
        
        total += qty * price;
      });
      
      // Create new order
      const newOrder = {
        id: currentOrderId++,
        customerId,
        date: new Date().toISOString().split('T')[0],
        items,
        total,
        status: 'مكتمل'
      };
      
      orders.push(newOrder);
      updateOrderTable();
      
      // Update medicine quantities
      items.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          medicine.qty -= item.qty;
        }
      });
      
      updateMedTable();
      updateMedicineDropdowns();
      updateDashboardStats();
      
      // Clear form
      clearOrderForm();
      
      showMessage('order-message', 'تم إنشاء الفاتورة بنجاح', 'success');
    }

    // Clear order form
    function clearOrderForm() {
      document.getElementById('orderCustomer').value = '';
      document.querySelector('#orderItems tbody').innerHTML = '';
      document.getElementById('orderTotal').textContent = '0.00';
      document.getElementById('orderQty').value = 1;
    }

    // Update orders table
    function updateOrderTable() {
      const tbody = document.querySelector('#orderTable tbody');
      tbody.innerHTML = '';
      
      orders.forEach(order => {
        const customer = customers.find(c => c.id === order.customerId);
        const customerName = customer ? customer.name : 'غير معروف';
        
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${order.id}</td>
          <td>${formatDate(order.date)}</td>
          <td>${customerName}</td>
          <td>${order.total.toFixed(2)} ${settings.currency}</td>
          <td>${order.status}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="viewOrder(${order.id})">عرض</button>
            <button class="no-print danger" onclick="deleteOrder(${order.id})">حذف</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // View order details
    function viewOrder(id) {
      const order = orders.find(o => o.id === id);
      if (!order) return;
      
      const customer = customers.find(c => c.id === order.customerId);
      
      let itemsHtml = '';
      order.items.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          itemsHtml += `
            <tr>
              <td>${medicine.name} (${medicine.generic})</td>
              <td>${item.qty}</td>
              <td>${item.price.toFixed(2)}</td>
              <td>${(item.qty * item.price).toFixed(2)}</td>
            </tr>
          `;
        }
      });
      
      const orderDetails = `
        <h3>تفاصيل الفاتورة #${order.id}</h3>
        <p><strong>التاريخ:</strong> ${formatDate(order.date)}</p>
        <p><strong>العميل:</strong> ${customer ? customer.name : 'غير معروف'}</p>
        
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
            ${itemsHtml}
          </tbody>
          <tfoot>
            <tr>
              <td colspan="3" style="text-align: left;"><strong>المجموع:</strong></td>
              <td><strong>${order.total.toFixed(2)} ${settings.currency}</strong></td>
            </tr>
          </tfoot>
        </table>
      `;
      
      alert(orderDetails);
    }

    // Delete order
    function deleteOrder(id) {
      if (confirm('هل أنت متأكد من حذف هذه الفاتورة؟')) {
        orders = orders.filter(o => o.id !== id);
        updateOrderTable();
        showMessage('order-message', 'تم حذف الفاتورة بنجاح', 'success');
      }
    }

    // Search orders
    function searchOrder(query) {
      const rows = document.querySelectorAll('#orderTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const orderId = row.cells[0].textContent.toLowerCase();
        const date = row.cells[1].textContent.toLowerCase();
        const customer = row.cells[2].textContent.toLowerCase();
        const total = row.cells[3].textContent.toLowerCase();
        const status = row.cells[4].textContent.toLowerCase();
        
        if (orderId.includes(lowerQuery) || date.includes(lowerQuery) || 
            customer.includes(lowerQuery) || total.includes(lowerQuery) || 
            status.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Add medicine to prescription
    function addMedicineToPrescription() {
      const medicineId = parseInt(document.getElementById('prescriptionMedicine').value);
      const qty = parseInt(document.getElementById('prescriptionMedicineQty').value);
      const dosage = document.getElementById('prescriptionDosage').value.trim();
      
      if (!medicineId || isNaN(qty) || qty <= 0 || !dosage) {
        showMessage('prescription-message', 'يرجى اختيار دواء وإدخال كمية وجرعة صحيحة', 'error');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) {
        showMessage('prescription-message', 'الدواء المحدد غير موجود', 'error');
        return;
      }
      
      // Add to prescription items table
      const tbody = document.querySelector('#prescriptionItems tbody');
      const tr = document.createElement('tr');
      tr.dataset.medicineId = medicineId;
      
      tr.innerHTML = `
        <td>${medicine.name} (${medicine.generic})</td>
        <td>${qty}</td>
        <td>${dosage}</td>
        <td class="actions-cell">
          <button class="no-print danger" onclick="removePrescriptionItem(this)">حذف</button>
        </td>
      `;
      
      tbody.appendChild(tr);
      
      // Clear medicine fields
      document.getElementById('prescriptionMedicine').value = '';
      document.getElementById('prescriptionMedicineQty').value = 1;
      document.getElementById('prescriptionDosage').value = '';
    }

    // Remove item from prescription
    function removePrescriptionItem(button) {
      button.closest('tr').remove();
    }

    // Save prescription
    function savePrescription() {
      const customerId = parseInt(document.getElementById('prescriptionCustomer').value);
      const doctor = document.getElementById('prescriptionDoctor').value.trim();
      const date = document.getElementById('prescriptionDate').value;
      const notes = document.getElementById('prescriptionNotes').value.trim();
      const items = document.querySelectorAll('#prescriptionItems tbody tr');
      
      if (!customerId) {
        showMessage('prescription-message', 'يرجى اختيار عميل', 'error');
        return;
      }
      
      if (!doctor) {
        showMessage('prescription-message', 'يرجى إدخال اسم الطبيب', 'error');
        return;
      }
      
      if (items.length === 0) {
        showMessage('prescription-message', 'لا توجد أدوية في الوصفة', 'error');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      if (!customer) {
        showMessage('prescription-message', 'العميل المحدد غير موجود', 'error');
        return;
      }
      
      // Prepare prescription items
      const prescriptionItems = [];
      
      items.forEach(row => {
        prescriptionItems.push({
          medicineId: parseInt(row.dataset.medicineId),
          qty: parseInt(row.cells[1].textContent),
          dosage: row.cells[2].textContent
        });
      });
      
      // Create new prescription
      const newPrescription = {
        id: currentPrescriptionId++,
        customerId,
        doctor,
        date,
        notes,
        items: prescriptionItems
      };
      
      prescriptions.push(newPrescription);
      updatePrescriptionTable();
      
      // Clear form
      clearPrescriptionForm();
      
      showMessage('prescription-message', 'تم حفظ الوصفة الطبية بنجاح', 'success');
    }

    // Clear prescription form
    function clearPrescriptionForm() {
      document.getElementById('prescriptionCustomer').value = '';
      document.getElementById('prescriptionDoctor').value = '';
      document.getElementById('prescriptionDate').value = new Date().toISOString().split('T')[0];
      document.getElementById('prescriptionNotes').value = '';
      document.querySelector('#prescriptionItems tbody').innerHTML = '';
      document.getElementById('prescriptionMedicine').value = '';
      document.getElementById('prescriptionMedicineQty').value = 1;
      document.getElementById('prescriptionDosage').value = '';
    }

    // Update prescriptions table
    function updatePrescriptionTable() {
      const tbody = document.querySelector('#prescriptionTable tbody');
      tbody.innerHTML = '';
      
      prescriptions.forEach(prescription => {
        const customer = customers.find(c => c.id === prescription.customerId);
        const customerName = customer ? customer.name : 'غير معروف';
        
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${prescription.id}</td>
          <td>${formatDate(prescription.date)}</td>
          <td>${customerName}</td>
          <td>${prescription.doctor}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="viewPrescription(${prescription.id})">عرض</button>
            <button class="no-print danger" onclick="deletePrescription(${prescription.id})">حذف</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // View prescription details
    function viewPrescription(id) {
      const prescription = prescriptions.find(p => p.id === id);
      if (!prescription) return;
      
      const customer = customers.find(c => c.id === prescription.customerId);
      
      let itemsHtml = '';
      prescription.items.forEach(item => {
        const medicine = medicines.find(m => m.id === item.medicineId);
        if (medicine) {
          itemsHtml += `
            <tr>
              <td>${medicine.name} (${medicine.generic})</td>
              <td>${item.qty}</td>
              <td>${item.dosage}</td>
            </tr>
          `;
        }
      });
      
      const prescriptionDetails = `
        <h3>تفاصيل الوصفة الطبية #${prescription.id}</h3>
        <p><strong>التاريخ:</strong> ${formatDate(prescription.date)}</p>
        <p><strong>العميل:</strong> ${customer ? customer.name : 'غير معروف'}</p>
        <p><strong>الطبيب:</strong> ${prescription.doctor}</p>
        <p><strong>ملاحظات:</strong> ${prescription.notes || 'لا توجد ملاحظات'}</p>
        
        <h4>أدوية الوصفة:</h4>
        <table>
          <thead>
            <tr>
              <th>الدواء</th>
              <th>الكمية</th>
              <th>الجرعة</th>
            </tr>
          </thead>
          <tbody>
            ${itemsHtml}
          </tbody>
        </table>
      `;
      
      alert(prescriptionDetails);
    }

    // Delete prescription
    function deletePrescription(id) {
      if (confirm('هل أنت متأكد من حذف هذه الوصفة الطبية؟')) {
        prescriptions = prescriptions.filter(p => p.id !== id);
        updatePrescriptionTable();
        showMessage('prescription-message', 'تم حذف الوصفة الطبية بنجاح', 'success');
      }
    }

    // Search prescriptions
    function searchPrescription(query) {
      const rows = document.querySelectorAll('#prescriptionTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const prescriptionId = row.cells[0].textContent.toLowerCase();
        const date = row.cells[1].textContent.toLowerCase();
        const customer = row.cells[2].textContent.toLowerCase();
        const doctor = row.cells[3].textContent.toLowerCase();
        
        if (prescriptionId.includes(lowerQuery) || date.includes(lowerQuery) || 
            customer.includes(lowerQuery) || doctor.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Calculator functions
    function calc(value) {
      document.getElementById('calc-display').value += value;
    }

    function clearCalc() {
      document.getElementById('calc-display').value = '';
    }

    function backspace() {
      const display = document.getElementById('calc-display');
      display.value = display.value.slice(0, -1);
    }

    function calculate() {
      const display = document.getElementById('calc-display');
      try {
        // Replace Arabic/English numbers and symbols
        let expression = display.value
          .replace(/×/g, '*')
          .replace(/÷/g, '/')
          .replace(/٫/g, '.')
          .replace(/[٠١٢٣٤٥٦٧٨٩]/g, function(d) {
            return '٠١٢٣٤٥٦٧٨٩'.indexOf(d);
          });
        
        display.value = eval(expression);
      } catch (e) {
        display.value = 'خطأ';
      }
    }

    // Update dashboard statistics
    function updateDashboardStats() {
      // Medicine count
      document.getElementById('med-count').textContent = medicines.length;
      
      // Expiring soon and expired medicines
      const today = new Date();
      let expiringSoon = 0;
      let expired = 0;
      
      medicines.forEach(med => {
        const expDate = new Date(med.exp);
        const timeDiff = expDate.getTime() - today.getTime();
        const daysDiff = Math.ceil(timeDiff / (1000 * 3600 * 24));
        
        if (daysDiff <= 0) {
          expired++;
        } else if (daysDiff <= settings.daysBeforeExpiry) {
          expiringSoon++;
        }
      });
      
      document.getElementById('expiring-soon').textContent = expiringSoon;
      document.getElementById('expired').textContent = expired;
      
      // Total sales
      const totalSales = orders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('sales').textContent = totalSales.toFixed(2);
      
      // Recent orders
      const recentOrders = [...orders].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
      const tbody = document.querySelector('#recent-orders');
      tbody.innerHTML = '';
      
      recentOrders.forEach(order => {
        const customer = customers.find(c => c.id === order.customerId);
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${order.id}</td>
          <td>${formatDate(order.date)}</td>
          <td>${customer ? customer.name : 'غير معروف'}</td>
          <td>${order.total.toFixed(2)} ${settings.currency}</td>
          <td>${order.status}</td>
        `;
        tbody.appendChild(tr);
      });
    }

    // Update expiry report
    function updateExpiryReport() {
      const tbody = document.querySelector('#expiryReportTable tbody');
      tbody.innerHTML = '';
      
      const today = new Date();
      
      medicines.forEach(med => {
        const expDate = new Date(med.exp);
        const timeDiff = expDate.getTime() - today.getTime();
        const daysDiff = Math.ceil(timeDiff / (1000 * 3600 * 24));
        
        if (daysDiff <= settings.daysBeforeExpiry) {
          const tr = document.createElement('tr');
          tr.innerHTML = `
            <td>${med.name}</td>
            <td>${med.qty}</td>
            <td>${formatDate(med.exp)}</td>
            <td>${daysDiff > 0 ? daysDiff : 'منتهية'}</td>
          `;
          
          if (daysDiff <= 0) {
            tr.classList.add('expired');
          } else {
            tr.classList.add('expiring-soon');
          }
          
          tbody.appendChild(tr);
        }
      });
    }

    // Update charts
    function updateCharts() {
      // This function would update chart data when needed
      // In a real app, you would fetch actual data here
    }

    // Format date for display
    function formatDate(dateString) {
      if (!dateString) return '';
      const date = new Date(dateString);
      return date.toLocaleDateString('ar-EG');
    }

    // Print current page
    function printPage() {
      window.print();
    }

    // Print order
    function printOrder() {
      // In a real app, this would generate a printable order invoice
      alert('سيتم طباعة الفاتورة');
    }
  </script>
</body>
</html>
