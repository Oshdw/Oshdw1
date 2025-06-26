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
    <button onclick="navigate('dashboard')">
      <span class="material-icons">dashboard</span>
      لوحة التحكم
    </button>
    <button onclick="navigate('medicines')">
      <span class="material-icons">medication</span>
      إدارة الأدوية
    </button>
    <button onclick="navigate('suppliers')">
      <span class="material-icons">local_shipping</span>
      الموردون
    </button>
    <button onclick="navigate('customers')">
      <span class="material-icons">people</span>
      العملاء
    </button>
    <button onclick="navigate('orders')">
      <span class="material-icons">receipt</span>
      الفواتير
    </button>
    <button onclick="navigate('prescriptions')">
      <span class="material-icons">description</span>
      الوصفات الطبية
    </button>
    <button onclick="navigate('calculator')">
      <span class="material-icons">calculate</span>
      الآلة الحاسبة
    </button>
    <button onclick="navigate('statistics')">
      <span class="material-icons">bar_chart</span>
      الإحصائيات
    </button>
    <button onclick="navigate('settings')">
      <span class="material-icons">settings</span>
      الإعدادات
    </button>
  </div>
  <div class="main" id="main">
    <div class="topbar">
      <h2 id="page-title">لوحة التحكم</h2>
      <div class="topbar-actions">
        <span class="material-icons toggle-sidebar" onclick="toggleSidebar()">menu</span>
        <span class="material-icons toggle-theme" onclick="toggleTheme()">dark_mode</span>
        <span class="material-icons" onclick="printPage()">print</span>
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
          <button onclick="addMedicine()">إضافة دواء</button>
          <button class="danger" onclick="clearMedicineForm()">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الأدوية</h3>
        <div class="search-container">
          <input type="text" id="med-search" placeholder="ابحث عن دواء..." oninput="searchMedicine(this.value)">
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
          <button onclick="addSupplier()">إضافة مورد</button>
          <button class="danger" onclick="clearSupplierForm()">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة الموردين</h3>
        <div class="search-container">
          <input type="text" id="supplier-search" placeholder="ابحث عن مورد..." oninput="searchSupplier(this.value)">
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
          <button onclick="addCustomer()">إضافة عميل</button>
          <button class="danger" onclick="clearCustomerForm()">مسح النموذج</button>
        </div>
      </div>

      <div class="card">
        <h3>قائمة العملاء</h3>
        <div class="search-container">
          <input type="text" id="customer-search" placeholder="ابحث عن عميل..." oninput="searchCustomer(this.value)">
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
        
        <button onclick="addMedicineToOrder()">إضافة إلى الفاتورة</button>
        
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
          <button onclick="createOrder()">إنشاء الفاتورة</button>
          <button class="danger" onclick="clearOrderForm()">إلغاء</button>
          <button class="no-print" onclick="printOrder()">طباعة الفاتورة</button>
        </div>
      </div>

      <div class="card">
        <h3>سجل الفواتير</h3>
        <div class="search-container">
          <input type="text" id="order-search" placeholder="ابحث عن فاتورة..." oninput="searchOrder(this.value)">
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
        <h3>إضافة وصفة طبية</
        <p><strong>العميل:</strong> ${customer ? customer.name : 'غير معروف'}</p>
        <p><strong>الحالة:</strong> ${order.status}</p>
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
        const medicine = medicines.find(m => m.id === item.medicineId);
        details += `
          <tr>
            <td>${medicine ? medicine.name : 'غير معروف'}</td>
            <td>${item.qty}</td>
            <td>${item.price.toFixed(2)} ${settings.currency}</td>
            <td>${(item.qty * item.price).toFixed(2)} ${settings.currency}</td>
          </tr>
        `;
      });
      
      details += `
          </tbody>
          <tfoot>
            <tr>
              <td colspan="3" style="text-align: left;"><strong>المجموع:</strong></td>
              <td><strong>${order.total.toFixed(2)} ${settings.currency}</strong></td>
            </tr>
          </tfoot>
        </table>
      `;
      
      // Show in a dialog
      alert(details);
    }

    // Delete order
    function deleteOrder(orderId) {
      if (confirm('هل أنت متأكد من حذف هذه الفاتورة؟ لا يمكن التراجع عن هذا الإجراء.')) {
        orders = orders.filter(o => o.id !== orderId);
        updateOrderTable();
        updateDashboardStats();
        showMessage('order-message', 'تم حذف الفاتورة بنجاح', 'success');
      }
    }

    // Search orders
    function searchOrder(query) {
      const rows = document.querySelectorAll('#orderTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const id = row.cells[0].textContent.toLowerCase();
        const date = row.cells[1].textContent.toLowerCase();
        const customer = row.cells[2].textContent.toLowerCase();
        const total = row.cells[3].textContent.toLowerCase();
        const status = row.cells[4].textContent.toLowerCase();
        
        if (id.includes(lowerQuery) || date.includes(lowerQuery) || 
            customer.includes(lowerQuery) || total.includes(lowerQuery) || 
            status.includes(lowerQuery)) {
          row.style.display = '';
        } else {
          row.style.display = 'none';
        }
      });
    }

    // Print order
    function printOrder() {
      const customerId = parseInt(document.getElementById('orderCustomer').value);
      const rows = document.querySelectorAll('#orderItems tbody tr');
      
      if (isNaN(customerId) {
        showMessage('order-message', 'يرجى اختيار عميل', 'error');
        return;
      }
      
      if (rows.length === 0) {
        showMessage('order-message', 'يرجى إضافة أدوية إلى الفاتورة', 'error');
        return;
      }
      
      const customer = customers.find(c => c.id === customerId);
      const today = new Date().toISOString().split('T')[0];
      
      let printContent = `
        <div style="text-align: center; margin-bottom: 20px;">
          <h2>${settings.pharmacyName}</h2>
          <p>${settings.pharmacyAddress}</p>
          <p>هاتف: ${settings.pharmacyPhone}</p>
        </div>
        <h3 style="text-align: center;">فاتورة بيع</h3>
        <div style="display: flex; justify-content: space-between; margin-bottom: 20px;">
          <div>
            <p><strong>رقم الفاتورة:</strong> ${currentOrderId}</p>
            <p><strong>التاريخ:</strong> ${formatDate(today)}</p>
          </div>
          <div>
            <p><strong>العميل:</strong> ${customer ? customer.name : 'غير معروف'}</p>
            <p><strong>هاتف العميل:</strong> ${customer ? customer.phone : '-'}</p>
          </div>
        </div>
        <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
          <thead>
            <tr style="background-color: #f5f5f5;">
              <th style="padding: 8px; border: 1px solid #ddd;">الدواء</th>
              <th style="padding: 8px; border: 1px solid #ddd;">الكمية</th>
              <th style="padding: 8px; border: 1px solid #ddd;">السعر</th>
              <th style="padding: 8px; border: 1px solid #ddd;">الإجمالي</th>
            </tr>
          </thead>
          <tbody>
      `;
      
      let total = 0;
      
      rows.forEach(row => {
        const medicineId = parseInt(row.dataset.medicineId);
        const medicine = medicines.find(m => m.id === medicineId);
        const qty = parseInt(row.cells[1].textContent);
        const price = parseFloat(row.cells[2].textContent);
        const itemTotal = qty * price;
        total += itemTotal;
        
        printContent += `
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd;">${medicine ? medicine.name : 'غير معروف'}</td>
            <td style="padding: 8px; border: 1px solid #ddd;">${qty}</td>
            <td style="padding: 8px; border: 1px solid #ddd;">${price.toFixed(2)} ${settings.currency}</td>
            <td style="padding: 8px; border: 1px solid #ddd;">${itemTotal.toFixed(2)} ${settings.currency}</td>
          </tr>
        `;
      });
      
      printContent += `
          </tbody>
          <tfoot>
            <tr>
              <td colspan="3" style="text-align: left; padding: 8px; border: 1px solid #ddd;"><strong>المجموع:</strong></td>
              <td style="padding: 8px; border: 1px solid #ddd;"><strong>${total.toFixed(2)} ${settings.currency}</strong></td>
            </tr>
          </tfoot>
        </table>
        <div style="margin-top: 30px; text-align: center;">
          <p>شكراً لثقتكم بنا</p>
          <p>${settings.pharmacyName}</p>
        </div>
      `;
      
      // Open print window
      const printWindow = window.open('', '_blank');
      printWindow.document.write(`
        <html>
          <head>
            <title>فاتورة #${currentOrderId}</title>
            <style>
              body { font-family: Arial, sans-serif; margin: 20px; }
              table { width: 100%; border-collapse: collapse; }
              th, td { padding: 8px; text-align: right; border: 1px solid #ddd; }
              h2, h3 { color: #1976d2; }
            </style>
          </head>
          <body>
            ${printContent}
            <script>
              window.onload = function() {
                window.print();
              };
            </script>
          </body>
        </html>
      `);
      printWindow.document.close();
    }

    // Add medicine to prescription
    function addMedicineToPrescription() {
      const medicineId = parseInt(document.getElementById('prescriptionMedicine').value);
      const qty = parseInt(document.getElementById('prescriptionMedicineQty').value);
      const dosage = document.getElementById('prescriptionDosage').value.trim();
      
      if (isNaN(medicineId) {
        showMessage('prescription-message', 'يرجى اختيار دواء', 'error');
        return;
      }
      
      if (isNaN(qty) {
        showMessage('prescription-message', 'يرجى إدخال كمية صحيحة', 'error');
        return;
      }
      
      if (!dosage) {
        showMessage('prescription-message', 'يرجى إدخال الجرعة', 'error');
        return;
      }
      
      const medicine = medicines.find(m => m.id === medicineId);
      if (!medicine) return;
      
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
      
      // Reset medicine selection
      document.getElementById('prescriptionMedicine').value = '';
      document.getElementById('prescriptionMedicineQty').value = '1';
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
      const rows = document.querySelectorAll('#prescriptionItems tbody tr');
      
      if (isNaN(customerId)) {
        showMessage('prescription-message', 'يرجى اختيار عميل', 'error');
        return;
      }
      
      if (!doctor) {
        showMessage('prescription-message', 'يرجى إدخال اسم الطبيب', 'error');
        return;
      }
      
      if (rows.length === 0) {
        showMessage('prescription-message', 'يرجى إضافة أدوية إلى الوصفة', 'error');
        return;
      }
      
      // Get prescription items
      const items = Array.from(rows).map(row => {
        const medicineId = parseInt(row.dataset.medicineId);
        const qty = parseInt(row.cells[1].textContent);
        const dosage = row.cells[2].textContent;
        
        return { medicineId, qty, dosage };
      });
      
      // Create new prescription
      const newPrescription = {
        id: currentPrescriptionId++,
        customerId,
        doctor,
        date,
        notes,
        items
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
      document.getElementById('prescriptionMedicineQty').value = '1';
      document.getElementById('prescriptionDosage').value = '';
    }

    // Update prescriptions table
    function updatePrescriptionTable() {
      const tbody = document.querySelector('#prescriptionTable tbody');
      tbody.innerHTML = '';
      
      prescriptions.forEach(prescription => {
        const customer = customers.find(c => c.id === prescription.customerId);
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${prescription.id}</td>
          <td>${formatDate(prescription.date)}</td>
          <td>${customer ? customer.name : 'غير معروف'}</td>
          <td>${prescription.doctor}</td>
          <td class="actions-cell">
            <button class="no-print" onclick="viewPrescriptionDetails(${prescription.id})">عرض</button>
            <button class="no-print danger" onclick="deletePrescription(${prescription.id})">حذف</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }

    // View prescription details
    function viewPrescriptionDetails(prescriptionId) {
      const prescription = prescriptions.find(p => p.id === prescriptionId);
      if (!prescription) return;
      
      const customer = customers.find(c => c.id === prescription.customerId);
      
      let details = `
        <h3>تفاصيل الوصفة الطبية #${prescription.id}</h3>
        <p><strong>التاريخ:</strong> ${formatDate(prescription.date)}</p>
        <p><strong>العميل:</strong> ${customer ? customer.name : 'غير معروف'}</p>
        <p><strong>الطبيب:</strong> ${prescription.doctor}</p>
        <p><strong>ملاحظات:</strong> ${prescription.notes || 'لا توجد'}</p>
        <h4>الأدوية:</h4>
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
        const medicine = medicines.find(m => m.id === item.medicineId);
        details += `
          <tr>
            <td>${medicine ? medicine.name : 'غير معروف'}</td>
            <td>${item.qty}</td>
            <td>${item.dosage}</td>
          </tr>
        `;
      });
      
      details += `
          </tbody>
        </table>
      `;
      
      // Show in a dialog
      alert(details);
    }

    // Delete prescription
    function deletePrescription(prescriptionId) {
      if (confirm('هل أنت متأكد من حذف هذه الوصفة الطبية؟ لا يمكن التراجع عن هذا الإجراء.')) {
        prescriptions = prescriptions.filter(p => p.id !== prescriptionId);
        updatePrescriptionTable();
        showMessage('prescription-message', 'تم حذف الوصفة الطبية بن��اح', 'success');
      }
    }

    // Search prescriptions
    function searchPrescription(query) {
      const rows = document.querySelectorAll('#prescriptionTable tbody tr');
      const lowerQuery = query.toLowerCase();
      
      rows.forEach(row => {
        const id = row.cells[0].textContent.toLowerCase();
        const date = row.cells[1].textContent.toLowerCase();
        const customer = row.cells[2].textContent.toLowerCase();
        const doctor = row.cells[3].textContent.toLowerCase();
        
        if (id.includes(lowerQuery) || date.includes(lowerQuery) || 
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
      try {
        const display = document.getElementById('calc-display');
        display.value = eval(display.value);
      } catch (e) {
        alert('تعبير رياضي غير صحيح');
        clearCalc();
      }
    }

    // Update dashboard statistics
    function updateDashboardStats() {
      // Total medicines
      document.getElementById('med-count').textContent = medicines.length;
      
      // Expiring soon and expired medicines
      const today = new Date();
      const expiringSoonDate = new Date();
      expiringSoonDate.setDate(today.getDate() + settings.daysBeforeExpiry);
      
      let expiringSoon = 0;
      let expired = 0;
      
      medicines.forEach(med => {
        const expDate = new Date(med.exp);
        if (expDate < today) {
          expired++;
        } else if (expDate <= expiringSoonDate) {
          expiringSoon++;
        }
      });
      
      document.getElementById('expiring-soon').textContent = expiringSoon;
      document.getElementById('expired').textContent = expired;
      
      // Total sales
      const totalSales = orders.reduce((sum, order) => sum + order.total, 0);
      document.getElementById('sales').textContent = totalSales.toFixed(2);
      
      // Recent orders
      const recentOrdersTbody = document.getElementById('recent-orders');
      recentOrdersTbody.innerHTML = '';
      
      const sortedOrders = [...orders].sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 5);
      
      sortedOrders.forEach(order => {
        const customer = customers.find(c => c.id === order.customerId);
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${order.id}</td>
          <td>${formatDate(order.date)}</td>
          <td>${customer ? customer.name : 'غير معروف'}</td>
          <td>${order.total.toFixed(2)} ${settings.currency}</td>
          <td>${order.status}</td>
        `;
        recentOrdersTbody.appendChild(tr);
      });
    }

    // Update expiry report
    function updateExpiryReport() {
      const tbody = document.querySelector('#expiryReportTable tbody');
      tbody.innerHTML = '';
      
      const today = new Date();
      const expiringSoonDate = new Date();
      expiringSoonDate.setDate(today.getDate() + settings.daysBeforeExpiry);
      
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
      // This would be more sophisticated in a real app
      // For now, we'll just simulate some data changes
      const salesChart = Chart.getChart("salesChart");
      if (salesChart) {
        salesChart.data.datasets[0].data = salesChart.data.datasets[0].data.map(
          () => Math.floor(Math.random() * 25000) + 5000
        );
        salesChart.update();
      }
      
      const medicinesChart = Chart.getChart("medicinesChart");
      if (medicinesChart) {
        medicinesChart.data.datasets[0].data = medicinesChart.data.datasets[0].data.map(
          () => Math.floor(Math.random() * 40) + 5
        );
        medicinesChart.update();
      }
      
      const expiryChart = Chart.getChart("expiryChart");
      if (expiryChart) {
        expiryChart.data.datasets[0].data = expiryChart.data.datasets[0].data.map(
          () => Math.floor(Math.random() * 20) + 1
        );
        expiryChart.data.datasets[1].data = expiryChart.data.datasets[1].data.map(
          () => Math.floor(Math.random() * 15) + 1
        );
        expiryChart.update();
      }
    }

    // Format date
    function formatDate(dateString) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' };
      return new Date(dateString).toLocaleDateString('ar-EG', options);
    }

    // Print current page
    function printPage() {
      window.print();
    }
  </script>
</body>
</html>
