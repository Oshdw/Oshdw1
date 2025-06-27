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
      margin: 1rem 0;
      padding-bottom: 1rem;
      border-bottom: 1px solid rgba(255, 255, 255, 0.2);
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

    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
      padding-bottom: 1rem;
      border-bottom: 1px solid var(--border-light);
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
    </div>

    <div id="medicines" class="page">
      <div class="card">
        <h3>إضافة دواء جديد</h3>
        <div class="form-group">
          <label for="medName">اسم الدواء</label>
          <input type="text" id="medName" class="form-control" placeholder="أدخل اسم الدواء">
        </div>
        <div class="form-group">
          <label for="medPrice">السعر</label>
          <input type="number" id="medPrice" class="form-control" placeholder="أدخل السعر">
        </div>
        <button id="addMedicineBtn" class="btn btn-success">
          <span class="material-icons">save</span>
          إضافة دواء
        </button>
      </div>

      <div class="card">
        <h3>قائمة الأدوية</h3>
        <div class="table-responsive">
          <table id="medicinesTable">
            <thead>
              <tr>
                <th>اسم الدواء</th>
                <th>السعر</th>
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

    <!-- باقي الصفحات بنفس الهيكل -->

    <div id="suppliers" class="page">
      <div class="card">
        <h3>إدارة الموردين</h3>
        <!-- نموذج إضافة مورد -->
      </div>
    </div>

    <div id="customers" class="page">
      <div class="card">
        <h3>إدارة العملاء</h3>
        <!-- نموذج إضافة عميل -->
      </div>
    </div>

    <footer style="text-align: center; margin-top: 2rem; padding: 1rem; border-top: 1px solid var(--border-light);">
      <p>نظام إدارة الصيدلية &copy; <span id="currentYear"></span></p>
    </footer>
  </div>

  <script>
    // بيانات التطبيق
    let medicines = [];
    let suppliers = [];
    let customers = [];
    let orders = [];
    let prescriptions = [];

    // تهيئة التطبيق
    document.addEventListener('DOMContentLoaded', function() {
      // تعيين السنة الحالية
      document.getElementById('currentYear').textContent = new Date().getFullYear();
      
      // تعيين معالج الأحداث للقائمة الجانبية
      document.getElementById('toggleSidebar').addEventListener('click', function() {
        document.getElementById('sidebar').classList.toggle('hidden');
        document.getElementById('main').classList.toggle('full');
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
        });
      });

      // معالج حدث إضافة دواء جديد
      document.getElementById('addMedicineBtn').addEventListener('click', function() {
        const name = document.getElementById('medName').value;
        const price = document.getElementById('medPrice').value;
        
        if (!name || !price) {
          showAlert('error', 'الرجاء إدخال جميع البيانات المطلوبة');
          return;
        }
        
        const newMedicine = {
          id: Date.now(),
          name: name,
          price: price
        };
        
        medicines.push(newMedicine);
        updateMedicinesTable();
        
        // إظهار رسالة نجاح
        showAlert('success', 'تم إضافة الدواء بنجاح');
        
        // مسح الحقول
        document.getElementById('medName').value = '';
        document.getElementById('medPrice').value = '';
      });

      // تحميل البيانات الأولية
      loadSampleData();
    });

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

    function updateMedicinesTable() {
      const tbody = document.querySelector('#medicinesTable tbody');
      tbody.innerHTML = '';
      
      medicines.forEach(medicine => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${medicine.name}</td>
          <td>${medicine.price} ج.م</td>
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

    function loadSampleData() {
      // بيانات عينة للأدوية
      medicines = [
        { id: 1, name: 'بانادول', price: '10.50' },
        { id: 2, name: 'أموكسيسيلين', price: '15.75' },
        { id: 3, name: 'فولتارين', price: '12.00' }
      ];
      
      updateMedicinesTable();
    }

    // وظائف التعديل والحذف
    window.editMedicine = function(id) {
      const medicine = medicines.find(m => m.id === id);
      if (!medicine) return;
      
      document.getElementById('medName').value = medicine.name;
      document.getElementById('medPrice').value = medicine.price;
      
      // قم بتغيير زر الإضافة إلى تحديث
      const addBtn = document.getElementById('addMedicineBtn');
      addBtn.innerHTML = '<span class="material-icons">save</span> تحديث الدواء';
      
      // احفظ معرف الدواء في الزر لتحديثه لاحقاً
      addBtn.dataset.editingId = id;
      
      // غير معالج الحدث للتحديث بدلاً من الإضافة
      addBtn.onclick = function() {
        const name = document.getElementById('medName').value;
        const price = document.getElementById('medPrice').value;
        
        if (!name || !price) {
          showAlert('error', 'الرجاء إدخال جميع البيانات المطلوبة');
          return;
        }
        
        const medicineIndex = medicines.findIndex(m => m.id === id);
        if (medicineIndex !== -1) {
          medicines[medicineIndex] = {
            id: id,
            name: name,
            price: price
          };
          
          updateMedicinesTable();
          showAlert('success', 'تم تحديث الدواء بنجاح');
          
          // استعادة الزر إلى حالته الأصلية
          addBtn.innerHTML = '<span class="material-icons">save</span> إضافة دواء';
          addBtn.onclick = document.getElementById('addMedicineBtn').onclick;
          delete addBtn.dataset.editingId;
          
          // مسح الحقول
          document.getElementById('medName').value = '';
          document.getElementById('medPrice').value = '';
        }
      };
    };

    window.deleteMedicine = function(id) {
      if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
        medicines = medicines.filter(medicine => medicine.id !== id);
        updateMedicinesTable();
        showAlert('success', 'تم حذف الدواء بنجاح');
      }
    };
  </script>
</body>
</html>
