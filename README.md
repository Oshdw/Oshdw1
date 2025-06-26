<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>نظام إدارة الصيدلية</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: var(--bg);
      color: var(--text);
      transition: background 0.3s, color 0.3s;
    }:root {
  --bg: #f4f6f8;
  --text: #222;
  --card: white;
  --primary: #3a86ff;
  --border: #ccc;
}

.dark-mode {
  --bg: #121212;
  --text: #f4f4f4;
  --card: #1e1e1e;
  --primary: #90caf9;
  --border: #444;
}

header {
  background: var(--primary);
  color: white;
  padding: 1rem;
  text-align: center;
}

main { padding: 2rem; }

#loginPage, #dashboardPage, #medicinesPage, #usersPage {
  display: none;
}

.active { display: block; }

#loginBox, .card, table {
  background: var(--card);
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0,0,0,0.1);
  padding: 1rem;
  margin: auto;
}

input, button {
  width: 100%; padding: 0.75rem;
  margin: 0.5rem 0;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: var(--bg);
  color: var(--text);
}

button { background: var(--primary); color: white; cursor: pointer; }

.cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
}

table {
  width: 100%; border-collapse: collapse;
  margin-top: 1rem;
}

th, td {
  border: 1px solid var(--border);
  padding: 0.75rem;
  text-align: center;
}

th { background: #e9f0fb; }
.dark-mode th { background: #333; }

.top-actions {
  display: flex; justify-content: space-between;
  margin-bottom: 1rem;
}

.logout, .toggle-dark {
  width: auto; padding: 0.5rem 1rem;
  position: absolute; top: 1rem;
}

.logout { left: 1rem; }
.toggle-dark { right: 1rem; }

  </style>
</head>
<body>
  <section id="loginPage" class="active">
    <div id="loginBox">
      <h2>تسجيل الدخول</h2>
      <input id="username" placeholder="اسم المستخدم">
      <input type="password" id="password" placeholder="كلمة المرور">
      <button onclick="login()">دخول</button>
    </div>
  </section>  <section id="dashboardPage">
    <header>
      <h1>لوحة التحكم</h1>
      <button class="logout" onclick="logout()">تسجيل الخروج</button>
      <button class="toggle-dark" onclick="toggleDarkMode()">🌓</button>
    </header>
    <main class="cards">
      <div class="card">
        <h3>عدد الأدوية</h3>
        <p id="medicineCount">0</p>
      </div>
      <div class="card">
        <h3>منتهية الصلاحية</h3>
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
    <div class="top-actions">
      <button onclick="showSection('medicinesPage')">إدارة الأدوية</button>
      <button onclick="showSection('usersPage')">المستخدمين</button>
    </div>
  </section>  <section id="medicinesPage">
    <header>
      <h1>إدارة الأدوية</h1>
      <button onclick="showSection('dashboardPage')">رجوع</button>
    </header>
    <main>
      <input type="text" id="search" placeholder="ابحث..." oninput="searchMedicine()">
      <button onclick="addMedicinePrompt()">+ إضافة دواء</button>
      <table>
        <thead>
          <tr><th>الاسم</th><th>الكمية</th><th>الصلاحية</th><th>خيارات</th></tr>
        </thead>
        <tbody id="medicineTable"></tbody>
      </table>
    </main>
  </section>  <section id="usersPage">
    <header>
      <h1>إدارة المستخدمين</h1>
      <button onclick="showSection('dashboardPage')">رجوع</button>
    </header>
    <main>
      <input type="text" id="newUser" placeholder="اسم المستخدم الجديد">
      <input type="password" id="newPass" placeholder="كلمة المرور">
      <button onclick="addUser()">إضافة مستخدم</button>
      <ul id="userList"></ul>
    </main>
  </section>  <script>
    const users = JSON.parse(localStorage.getItem('users')) || [{user:'admin', pass:'1234'}];
    const medicines = JSON.parse(localStorage.getItem('medicines')) || [];

    function login() {
      const u = document.getElementById('username').value;
      const p = document.getElementById('password').value;
      const found = users.find(x => x.user === u && x.pass === p);
      if (found) {
        localStorage.setItem('currentUser', u);
        showSection('dashboardPage');
        updateDashboardStats();
      } else alert('بيانات غير صحيحة');
    }

    function logout() {
      localStorage.removeItem('currentUser');
      showSection('loginPage');
    }

    function showSection(id) {
      document.querySelectorAll('section').forEach(sec => sec.classList.remove('active'));
      document.getElementById(id).classList.add('active');
      if (id === 'usersPage') renderUsers();
    }

    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
    }

    function addUser() {
      const user = document.getElementById('newUser').value;
      const pass = document.getElementById('newPass').value;
      if (user && pass) {
        users.push({ user, pass });
        localStorage.setItem('users', JSON.stringify(users));
        renderUsers();
      }
    }

    function renderUsers() {
      const list = document.getElementById('userList');
      list.innerHTML = '';
      users.forEach(u => {
        list.innerHTML += `<li>${u.user}</li>`;
      });
    }

    function addMedicinePrompt() {
      const name = prompt('اسم الدواء');
      const quantity = +prompt('الكمية');
      const expiry = prompt('تاريخ الانتهاء YYYY-MM-DD');
      if (name && quantity && expiry) {
        medicines.push({ name, quantity, expiry });
        saveAndRender();
      }
    }

    function searchMedicine() {
      const q = document.getElementById('search').value;
      const filtered = medicines.filter(m => m.name.includes(q));
      renderMedicines(filtered);
    }

    function renderMedicines(list = medicines) {
      const table = document.getElementById('medicineTable');
      table.innerHTML = '';
      list.forEach((m, i) => {
        table.innerHTML += `
          <tr>
            <td>${m.name}</td>
            <td>${m.quantity}</td>
            <td>${m.expiry}</td>
            <td>
              <button onclick="editMedicine(${i})">تعديل</button>
              <button onclick="deleteMedicine(${i})">حذف</button>
            </td>
          </tr>`;
      });
    }

    function editMedicine(i) {
      const m = medicines[i];
      const name = prompt('اسم الدواء', m.name);
      const quantity = +prompt('الكمية', m.quantity);
      const expiry = prompt('الانتهاء', m.expiry);
      if (name && quantity && expiry) {
        medicines[i] = { name, quantity, expiry };
        saveAndRender();
      }
    }

    function deleteMedicine(i) {
      if (confirm('هل تريد الحذف؟')) {
        medicines.splice(i, 1);
        saveAndRender();
      }
    }

    function saveAndRender() {
      localStorage.setItem('medicines', JSON.stringify(medicines));
      renderMedicines();
      updateDashboardStats();
    }

    function updateDashboardStats() {
      document.getElementById('medicineCount').textContent = medicines.length;
      document.getElementById('expiredCount').textContent = medicines.filter(m => new Date(m.expiry) < new Date()).length;
      document.getElementById('alerts').textContent = medicines.filter(m => m.quantity < 10).length;
      document.getElementById('totalItems').textContent = medicines.reduce((a, b) => a + b.quantity, 0);
    }

    // استئناف الجلسة
    if (localStorage.getItem('currentUser')) {
      showSection('dashboardPage');
      updateDashboardStats();
    }
  </script></body>
</html>
