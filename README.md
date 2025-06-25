<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لوحة التحكم - الصيدلية</title>
  <style>
    :root {
      --primary: #3a86ff;
      --bg: #f7f9fc;
      --text: #222;
      --card: #fff;
    }body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: var(--bg);
  color: var(--text);
}

header {
  background: var(--primary);
  color: #fff;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 {
  margin: 0;
  font-size: 1.5rem;
}

.container {
  padding: 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}

.card {
  background: var(--card);
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.05);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.02);
}

.card h3 {
  margin-bottom: 0.5rem;
  font-size: 1.25rem;
  color: var(--primary);
}

.logout {
  background: #fff;
  color: var(--primary);
  padding: 0.5rem 1rem;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-weight: bold;
}

  </style>
</head>
<body>
  <header>
    <h1>لوحة التحكم - الصيدلية</h1>
    <button class="logout" onclick="logout()">تسجيل الخروج</button>
  </header>  <main class="container">
    <div class="card">
      <h3>عدد الأدوية</h3>
      <p id="medicineCount">0</p>
    </div>
    <div class="card">
      <h3>أدوية منتهية الصلاحية</h3>
      <p id="expiredCount">0</p>
    </div>
    <div class="card">
      <h3>تنبيهات المخزون</h3>
      <p id="alerts">0</p>
    </div>
    <div class="card">
      <h3>إجمالي الأصناف</h3>
      <p id="totalItems">0</p>
    </div>
  </main>  <script>
    // حماية الصفحة من الدخول بدون تسجيل
    const user = localStorage.getItem("pharmacyUser");
    if (!user) window.location.href = "index.html";

    // بيانات وهمية
    const medicines = [
      { name: "باراسيتامول", expiry: "2024-12-01", quantity: 50 },
      { name: "أموكسيسيلين", expiry: "2023-10-01", quantity: 0 },
      { name: "إيبوبروفين", expiry: "2025-01-10", quantity: 12 },
      { name: "ميترونيدازول", expiry: "2022-11-30", quantity: 8 },
    ];

    document.getElementById("medicineCount").textContent = medicines.length;
    document.getElementById("expiredCount").textContent = medicines.filter(m => new Date(m.expiry) < new Date()).length;
    document.getElementById("alerts").textContent = medicines.filter(m => m.quantity < 10).length;
    document.getElementById("totalItems").textContent = medicines.reduce((sum, m) => sum + m.quantity, 0);

    function logout() {
      localStorage.removeItem("pharmacyUser");
      window.location.href = "index.html";
    }
  </script></body>
</html>
<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لوحة التحكم - الصيدلية</title>
  <style>
    :root {
      --primary: #3a86ff;
      --bg: #f7f9fc;
      --text: #222;
      --card: #fff;
    }body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: var(--bg);
  color: var(--text);
}

header {
  background: var(--primary);
  color: #fff;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 {
  margin: 0;
  font-size: 1.5rem;
}

.container {
  padding: 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
}

.card {
  background: var(--card);
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.05);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.02);
}

.card h3 {
  margin-bottom: 0.5rem;
  font-size: 1.25rem;
  color: var(--primary);
}

.logout {
  background: #fff;
  color: var(--primary);
  padding: 0.5rem 1rem;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-weight: bold;
}

  </style>
</head>
<body>
  <header>
    <h1>لوحة التحكم - الصيدلية</h1>
    <button class="logout" onclick="logout()">تسجيل الخروج</button>
  </header>  <main class="container">
    <div class="card">
      <h3>عدد الأدوية</h3>
      <p id="medicineCount">0</p>
    </div>
    <div class="card">
      <h3>أدوية منتهية الصلاحية</h3>
      <p id="expiredCount">0</p>
    </div>
    <div class="card">
      <h3>تنبيهات المخزون</h3>
      <p id="alerts">0</p>
    </div>
    <div class="card">
      <h3>إجمالي الأصناف</h3>
      <p id="totalItems">0</p>
    </div>
  </main>  <script>
    // حماية الصفحة من الدخول بدون تسجيل
    const user = localStorage.getItem("pharmacyUser");
    if (!user) window.location.href = "index.html";

    // بيانات وهمية
    const medicines = [
      { name: "باراسيتامول", expiry: "2024-12-01", quantity: 50 },
      { name: "أموكسيسيلين", expiry: "2023-10-01", quantity: 0 },
      { name: "إيبوبروفين", expiry: "2025-01-10", quantity: 12 },
      { name: "ميترونيدازول", expiry: "2022-11-30", quantity: 8 },
    ];

    document.getElementById("medicineCount").textContent = medicines.length;
    document.getElementById("expiredCount").textContent = medicines.filter(m => new Date(m.expiry) < new Date()).length;
    document.getElementById("alerts").textContent = medicines.filter(m => m.quantity < 10).length;
    document.getElementById("totalItems").textContent = medicines.reduce((sum, m) => sum + m.quantity, 0);

    function logout() {
      localStorage.removeItem("pharmacyUser");
      window.location.href = "index.html";
    }
  </script></body>
</html>
