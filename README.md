<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>نظام إدارة الصيدلية</title>
  <style>
    :root {
      --bg-color: #f4f4f4;
      --text-color: #222;
      --card-color: #fff;
      --accent: #3a86ff;
    }

    body.dark {
      --bg-color: #121212;
      --text-color: #f4f4f4;
      --card-color: #1f1f1f;
      --accent: #90e0ef;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-color);
      line-height: 1.6;
      transition: all 0.3s ease;
    }

    header {
      background: var(--accent);
      color: white;
      padding: 1rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
    }

    header h1 {
      font-size: 1.5rem;
    }

    nav {
      display: flex;
      gap: 1rem;
      align-items: center;
    }

    nav a {
      color: white;
      text-decoration: none;
      font-weight: bold;
    }

    #toggle-theme {
      background: none;
      border: none;
      font-size: 1.2rem;
      cursor: pointer;
      color: white;
    }

    .hero {
      padding: 2rem;
      text-align: center;
    }

    .cta {
      padding: 0.7rem 1.5rem;
      background: var(--accent);
      border: none;
      color: white;
      font-size: 1rem;
      margin-top: 1rem;
      border-radius: 8px;
      cursor: pointer;
    }

    .features {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 1rem;
      padding: 2rem;
    }

    .card {
      background: var(--card-color);
      padding: 1rem;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }

    .card:hover {
      transform: scale(1.03);
    }

    footer {
      text-align: center;
      padding: 1rem;
      background: #333;
      color: white;
    }
  </style>
</head>
<body>
  <header>
    <h1>💊 نظام إدارة الصيدلية</h1>
    <nav>
      <a href="#">الرئيسية</a>
      <a href="#">الأدوية</a>
      <a href="#">المخزون</a>
      <a href="#">التقارير</a>
      <a href="#">تسجيل الخروج</a>
      <button id="toggle-theme">🌓</button>
    </nav>
  </header>

  <main>
    <section class="hero">
      <h2>مرحباً بك في نظام صيدليتك</h2>
      <p>نساعدك على إدارة الأدوية والمخزون والتقارير بكل سهولة واحترافية.</p>
      <button class="cta">ابدأ الآن</button>
    </section>

    <section class="features">
      <div class="card">
        <h3>📦 إدارة المخزون</h3>
        <p>راقب الأدوية والكميات وتاريخ الانتهاء بكل دقة.</p>
      </div>
      <div class="card">
        <h3>💼 تقارير متقدمة</h3>
        <p>أنشئ تقارير يومية وشهرية وتحليلات الأداء.</p>
      </div>
      <div class="card">
        <h3>🔒 تسجيل دخول آمن</h3>
        <p>نظام دخول وخروج للمستخدمين مع حماية عالية.</p>
      </div>
    </section>
  </main>

  <footer>
    <p>جميع الحقوق محفوظة © <span id="year"></span> عمر بابكر (Omer Babiker - Omer Bk)</p>
  </footer>

  <script>
    // تفعيل الوضع الداكن والفاتح
    const toggleBtn = document.getElementById("toggle-theme");
    const body = document.body;

    toggleBtn.addEventListener("click", () => {
      body.classList.toggle("dark");
      toggleBtn.textContent = body.classList.contains("dark") ? "🌞" : "🌓";
    });

    // تحديث السنة تلقائياً في الفوتر
    document.getElementById("year").textContent = new Date().getFullYear();
  </script>
</body>
</html>
