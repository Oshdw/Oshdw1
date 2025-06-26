<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>نظام إدارة الصيدلية</title>

<!-- مكتبة أيقونات Material Icons -->
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

<style>
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
    position: fixed;
    width: 100%;
    top: 0;
    z-index: 10;
  }
  header h1 {
    margin: 0;
    font-weight: 700;
  }
  #toggleDarkMode, #toggleSidebar {
    cursor: pointer;
    background: transparent;
    border: none;
    color: white;
    font-size: 1.8rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  #toggleSidebar {
    margin-left: 1rem;
  }

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
    z-index: 9;
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
    padding: 1rem 1.5rem;
    text-align: right;
    font-size: 1rem;
    cursor: pointer;
    border-left: 4px solid transparent;
    display: flex;
    align-items: center;
    gap: 0.6rem;
    transition: background-color 0.2s, border-left-color 0.2s;
  }
  #sidebar button.active,
  #sidebar button:hover {
    background-color: rgba(255,255,255,0.2);
    border-left-color: #fff;
  }
  #sidebar button .material-icons {
    font-size: 1.4rem;
  }

  main {
    margin-top: 60px;
    margin-right: 220px;
    padding: 1.5rem;
    min-height: calc(100vh - 60px);
    transition: margin-right 0.3s ease;
  }
  #sidebar.hide + main {
    margin-right: 0;
  }

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
  <div style="display:flex; align-items:center;">
    <button id="toggleSidebar" title="إظهار/إخفاء القائمة الجانبية">
      <span class="material-icons">menu</span>
    </button>
    <h1>نظام إدارة الصيدلية</h1>
  </div>
  <button id="toggleDarkMode" title="تبديل الوضع الليلي">
    <span class="material-icons">dark_mode</span>
  </button>
</header>

<div id="sidebar" class="dark">
  <button data-page="dashboard" class="active"><span class="material-icons">dashboard</span> لوحة التحكم</button>
  <button data-page="medicines"><span class="material-icons">local_pharmacy</span> إدارة الأدوية</button>
  <button data-page="orders"><span class="material-icons">inventory_2</span> إدارة الطلبات</button>
  <button data-page="sales"><span class="material-icons">attach_money</span> تقارير المبيعات</button>
  <button data-page="expenses"><span class="material-icons">receipt_long</span> سجل المنصرفات</button>
  <button data-page="statistics"><span class="material-icons">bar_chart</span> الإحصائيات</button>
  <button data-page="calculator"><span class="material-icons">calculate</span> الآلة الحاسبة</button>
</div>

<main>
  <div id="message"></div>

  <!-- لوحة التحكم -->
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

  <!-- إدارة الأدوية -->
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

  <!-- إدارة الطلبات -->
<!-- إدارة الطلبات -->
<section id="orders" class="page" style="display:none;">
  <h2>إدارة الطلبات</h2>
  <!-- أضف محتوى إدارة الطلبات هنا -->
</section>
<script>
  // تبديل الوضع الليلي
  const toggleDarkModeBtn = document.getElementById('toggleDarkMode');
  toggleDarkModeBtn.addEventListener('click', () => {
    document.body.classList.toggle('dark');
    document.getElementById('sidebar').classList.toggle('dark');
  });

  // إظهار/إخفاء القائمة الجانبية
  const toggleSidebarBtn = document.getElementById('toggleSidebar');
  const sidebar = document.getElementById('sidebar');
  const main = document.querySelector('main');

  toggleSidebarBtn.addEventListener('click', () => {
    sidebar.classList.toggle('hide');
  });

  // التنقل بين الصفحات
  const pages = document.querySelectorAll('.page');
  const sidebarButtons = sidebar.querySelectorAll('button');

  sidebarButtons.forEach(button => {
    button.addEventListener('click', () => {
      // إزالة الحالة النشطة من جميع الأزرار
      sidebarButtons.forEach(btn => btn.classList.remove('active'));
      button.classList.add('active');

      // إخفاء كل الصفحات
      pages.forEach(page => page.style.display = 'none');

      // إظهار الصفحة المختارة
      const pageId = button.getAttribute('data-page');
      document.getElementById(pageId).style.display = 'block';
    });
  });

  // مثال بسيط على عرض إحصائيات وهمية في لوحة التحكم
  document.getElementById('statMedicines').textContent = '120';
  document.getElementById('statExpired').textContent = '5';
  document.getElementById('statPendingOrders').textContent = '7';
  document.getElementById('statTotalSales').textContent = '3450 ج.س';

  // إضافة/تحديث دواء
  const medicineForm = document.getElementById('medicineForm');
  const medTableBody = document.getElementById('medTableBody');
  const medSearch = document.getElementById('medSearch');
  const message = document.getElementById('message');

  let medicines = [];

  medicineForm.addEventListener('submit', (e) => {
    e.preventDefault();
    const name = document.getElementById('medName').value.trim();
    const quantity = parseInt(document.getElementById('medQuantity').value);
    const expiry = document.getElementById('medExpiry').value;

    if (!name || isNaN(quantity) || !expiry) {
      showMessage('يرجى تعبئة جميع الحقول بشكل صحيح', 'error');
      return;
    }

    const existing = medicines.find(med => med.name === name);
    if (existing) {
      existing.quantity = quantity;
      existing.expiry = expiry;
      showMessage('تم تحديث الدواء بنجاح', 'success');
    } else {
      medicines.push({ name, quantity, expiry });
      showMessage('تمت إضافة الدواء بنجاح', 'success');
    }

    medicineForm.reset();
    renderMedicines();
  });

  function renderMedicines(filter = '') {
    medTableBody.innerHTML = '';
    medicines
      .filter(med => med.name.includes(filter))
      .forEach((med, index) => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${med.name}</td>
          <td>${med.quantity}</td>
          <td>${med.expiry}</td>
          <td>
            <button class="btn" onclick="editMedicine(${index})">تعديل</button>
            <button class="btn danger" onclick="deleteMedicine(${index})">حذف</button>
          </td>
        `;
        medTableBody.appendChild(row);
      });
  }

  medSearch.addEventListener('input', (e) => {
    renderMedicines(e.target.value.trim());
  });

  window.editMedicine = function(index) {
    const med = medicines[index];
    document.getElementById('medName').value = med.name;
    document.getElementById('medQuantity').value = med.quantity;
    document.getElementById('medExpiry').value = med.expiry;
  };

  window.deleteMedicine = function(index) {
    if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
      medicines.splice(index, 1);
      renderMedicines();
      showMessage('تم حذف الدواء بنجاح', 'success');
    }
  };

  function showMessage(text, type) {
    message.textContent = text;
    message.className = '';
    message.classList.add(type === 'success' ? 'success' : 'error');
    message.style.display = 'block';
    setTimeout(() => message.style.display = 'none', 3000);
  }
</script>
