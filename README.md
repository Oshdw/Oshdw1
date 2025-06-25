<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>نظام إدارة الصيدلية</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    * {box-sizing: border-box;}
    body {
      margin: 0; font-family: 'Segoe UI', sans-serif;
      background: #f4f6f8; color: #222;
    }

    header {
      background: #3a86ff; color: white;
      padding: 1rem; text-align: center;
    }

    main {padding: 2rem;}

    /* ---- صفحة تسجيل الدخول ---- */
    #loginPage {
      display: flex; flex-direction: column;
      align-items: center; justify-content: center;
      min-height: 100vh;
    }

    #loginBox {
      background: white; padding: 2rem;
      border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      width: 100%; max-width: 400px;
    }

    #loginBox h2 {margin-bottom: 1rem;}

    input {
      padding: 0.75rem;
      margin-bottom: 1rem;
      width: 100%;
      border: 1px solid #ccc;
      border-radius: 6px;
    }

    button {
      padding: 0.75rem 1.5rem;
      background: #3a86ff;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    /* ---- لوحة التحكم ---- */
    #dashboardPage .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 1rem;
    }

    .card {
      background: white;
      padding: 1rem;
      border-radius: 12px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.05);
      text-align: center;
    }

    .card h3 {color: #3a86ff;}

    .logout {
      position: absolute; top: 1rem; left: 1rem;
      background: white; color: #3a86ff;
    }

    /* ---- إدارة الأدوية ---- */
    #medicinesPage input[type=\"text\"] {
      padding: 0.5rem; width: 250px;
      margin-bottom: 1rem;
    }

    table {
      width: 100%; border-collapse: collapse;
      background: white;
    }

    th, td {
      padding: 0.75rem; border: 1px solid #ddd;
      text-align: center;
    }

    th {
      background: #e9f0fb;
    }

    .hidden {display: none;}
  </style>
</head>
<body>

  <!-- صفحة تسجيل الدخول -->
  <section id="loginPage">
    <div id="loginBox">
      <h2>تسجيل الدخول</h2>
      <input type="text" id="username" placeholder="اسم المستخدم">
      <input type="password" id="password" placeholder="كلمة المرور">
      <button onclick="login()">دخول</button>
    </div>
  </section>

  <!-- لوحة التحكم -->
  <section id="dashboardPage" class="hidden">
    <header>
      <h1>لوحة التحكم</h1>
      <button class="logout" onclick="logout()">تسجيل الخروج</button>
    </header>
    <main class="cards">
      <div class="card">
        <h3>عدد الأدوية</h3>
        <p id="medicineCount">0</p>
      </div>
      <div class="card">
        <h3>أدوية منتهية</h3>
        <p id="expiredCount">0</p>
      </div>
      <div class="card">
        <h3>تنبيهات</h3>
        <p id="alerts">0</p>
      </div>
      <div class="card">
        <h3>إجمالي الكمية</h3>
        <p id="totalItems">0</p>
      </div>
    </main>
    <div style="text-align:center; margin-top:2rem;">
      <button onclick=\"showMedicinesPage()\">إدارة الأدوية</button>
    </div>
  </section>

  <!-- إدارة الأدوية -->
  <section id="medicinesPage" class="hidden">
    <header>
      <h1>إدارة الأدوية</h1>
      <button class="logout" onclick="backToDashboard()">رجوع</button>
    </header>
    <main>
      <input type="text" id="search" placeholder="ابحث باسم الدواء..." oninput="searchMedicine()">
      <button onclick="addMedicinePrompt()">+ إضافة دواء</button>
      <table>
        <thead>
          <tr>
            <th>الاسم</th>
            <th>الكمية</th>
            <th>تاريخ الانتهاء</th>
            <th>الخيارات</th>
          </tr>
        </thead>
        <tbody id="medicineTable"></tbody>
      </table>
    </main>
  </section>

  <script>
    // صفحة تسجيل الدخول
    function login() {
      const user = document.getElementById('username').value;
      const pass = document.getElementById('password').value;
      if (user === 'admin' && pass === '1234') {
        localStorage.setItem('pharmacyUser', user);
        showDashboard();
      } else {
        alert('بيانات غير صحيحة');
      }
    }

    function logout() {
      localStorage.removeItem('pharmacyUser');
      location.reload();
    }

    // حماية الدخول
    const currentUser = localStorage.getItem('pharmacyUser');
    if (currentUser) showDashboard();

    function showDashboard() {
      document.getElementById('loginPage').classList.add('hidden');
      document.getElementById('dashboardPage').classList.remove('hidden');
      updateDashboardStats();
    }

    // بيانات الأدوية
    let medicines = JSON.parse(localStorage.getItem('medicines')) || [
      { name: \"باراسيتامول\", quantity: 20, expiry: \"2024-12-01\" },
      { name: \"أموكسيسيلين\", quantity: 0, expiry: \"2023-10-01\" }
    ];

    function updateDashboardStats() {
      document.getElementById('medicineCount').textContent = medicines.length;
      document.getElementById('expiredCount').textContent = medicines.filter(m => new Date(m.expiry) < new Date()).length;
      document.getElementById('alerts').textContent = medicines.filter(m => m.quantity < 10).length;
      document.getElementById('totalItems').textContent = medicines.reduce((sum, m) => sum + m.quantity, 0);
    }

    function showMedicinesPage() {
      document.getElementById('dashboardPage').classList.add('hidden');
      document.getElementById('medicinesPage').classList.remove('hidden');
      renderMedicines();
    }

    function backToDashboard() {
      document.getElementById('medicinesPage').classList.add('hidden');
      document.getElementById('dashboardPage').classList.remove('hidden');
      updateDashboardStats();
    }

    function renderMedicines(list = medicines) {
      const table = document.getElementById('medicineTable');
      table.innerHTML = '';
      list.forEach((med, i) => {
        table.innerHTML += `
          <tr>
            <td>${med.name}</td>
            <td>${med.quantity}</td>
            <td>${med.expiry}</td>
            <td>
              <button onclick=\"editMedicine(${i})\">تعديل</button>
              <button onclick=\"deleteMedicine(${i})\">حذف</button>
            </td>
          </tr>
        `;
      });
    }

    function searchMedicine() {
      const q = document.getElementById('search').value;
      const filtered = medicines.filter(m => m.name.includes(q));
      renderMedicines(filtered);
    }

    function addMedicinePrompt() {
      const name = prompt('اسم الدواء:');
      const quantity = +prompt('الكمية:');
      const expiry = prompt('تاريخ الانتهاء (YYYY-MM-DD):');
      if (name && quantity && expiry) {
        medicines.push({ name, quantity, expiry });
        saveAndRender();
      }
    }

    function editMedicine(i) {
      const med = medicines[i];
      const name = prompt('اسم الدواء:', med.name);
      const quantity = +prompt('الكمية:', med.quantity);
      const expiry = prompt('تاريخ الانتهاء:', med.expiry);
      if (name && quantity && expiry) {
        medicines[i] = { name, quantity, expiry };
        saveAndRender();
      }
    }

    function deleteMedicine(i) {
      if (confirm('هل تريد حذف هذا الدواء؟')) {
        medicines.splice(i, 1);
        saveAndRender();
      }
    }

    function saveAndRender() {
      localStorage.setItem('medicines', JSON.stringify(medicines));
      renderMedicines();
      updateDashboardStats();
    }
  </script>
</body>
</html>
