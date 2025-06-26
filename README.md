<html lang="en"><head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لوحة تحكم الصيدلية</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }body {
  display: flex;
  min-height: 100vh;
  background: #f2f2f2;
  color: #333;
}

.sidebar {
  width: 250px;
  background-color: #2c3e50;
  color: #ecf0f1;
  padding-top: 20px;
  position: relative;
  transition: transform 0.3s ease;
}

.sidebar.hidden {
  transform: translateX(-100%);
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  z-index: 100;
}

.sidebar h2 {
  text-align: center;
  margin-bottom: 30px;
}

.sidebar ul {
  list-style: none;
}

.sidebar ul li {
  padding: 15px 25px;
  border-bottom: 1px solid #34495e;
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: background 0.3s ease;
}

.sidebar ul li:hover {
  background: #34495e;
}

.sidebar ul li i {
  margin-right: 10px;
}

.content {
  flex: 1;
  padding: 20px;
}

.toggle-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  background: #2c3e50;
  color: white;
  border: none;
  font-size: 20px;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  z-index: 101;
}

.section {
  display: none;
}

.section.active {
  display: block;
}

footer {
  text-align: center;
  padding: 15px;
  background: #2c3e50;
  color: white;
  position: fixed;
  bottom: 0;
  width: 100%;
}

  </style>
</head><body>
  <button class="toggle-btn" onclick="toggleSidebar()">
    <i class="fas fa-bars"></i>
  </button>  <div class="sidebar" id="sidebar">
    <h2>لوحة التحكم</h2>
    <ul>
      <li onclick="showSection('home')"><i class="fas fa-house"></i> الصفحة الرئيسية</li>
      <li onclick="showSection('medicines')"><i class="fas fa-capsules"></i> الأدوية</li>
      <li onclick="showSection('sales')"><i class="fas fa-money-bill-trend-up"></i> المبيعات</li>
      <li onclick="showSection('expenses')"><i class="fas fa-receipt"></i> المصروفات</li>
      <li onclick="showSection('reports')"><i class="fas fa-chart-pie"></i> التقارير</li>
      <li onclick="showSection('settings')"><i class="fas fa-gear"></i> الإعدادات</li>
    </ul>
  </div>  <div class="content">
    <div id="home" class="section active">
      <h1>الصفحة الرئيسية</h1>
      <p>مرحبًا بك في لوحة تحكم الصيدلية.</p>
    </div>
    <div id="medicines" class="section">
      <h1>الأدوية</h1>
      <p>إدارة الأدوية وإضافة مخزون جديد.</p>
    </div>
    <div id="sales" class="section">
      <h1>المبيعات</h1>
      <p>عرض تفاصيل المبيعات اليومية.</p>
    </div>
    <div id="expenses" class="section">
      <h1>المصروفات</h1>
      <p>إدارة الفواتير والمصروفات.</p>
    </div>
    <div id="reports" class="section">
      <h1>التقارير</h1>
      <p>عرض وتحليل التقارير الدورية.</p>
    </div>
    <div id="settings" class="section">
      <h1>الإعدادات</h1>
      <p>تحديث بيانات الصيدلية والمستخدمين.</p>
    </div>
  </div>  <footer>
    تصميم وتطوير: عمر بابكر (Omer Babiker - Omer Bk)
  </footer>  <script>
    function toggleSidebar() {
      const sidebar = document.getElementById('sidebar');
      sidebar.classList.toggle('hidden');
    }

    function showSection(id) {
      document.querySelectorAll('.section').forEach(section => {
        section.classList.remove('active');
      });
      document.getElementById(id).classList.add('active');
    }
  </script></body></html>
