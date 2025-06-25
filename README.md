<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الصيدليات</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
            transition: background-color 0.3s, color 0.3s;
        }

        header {
            background: #007bff;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        nav ul {
            list-style: none;
            padding: 0;
        }

        nav ul li {
            display: inline;
            margin: 0 10px;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
        }

        .hero {
            text-align: center;
            padding: 50px 20px;
            background: #e9ecef;
        }

        .service-box {
            border: 1px solid #ccc;
            padding: 20px;
            margin: 10px;
            display: inline-block;
            width: 200px;
            text-align: center;
        }

        footer {
            text-align: center;
            padding: 20px;
            background: #007bff;
            color: white;
        }

        /* وضع داكن */
        body.dark-mode {
            background-color: #121212;
            color: #ffffff;
        }

        body.dark-mode header {
            background: #1f1f1f;
        }

        body.dark-mode .hero {
            background: #1e1e1e;
        }

        body.dark-mode .service-box {
            border: 1px solid #444;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">شعار الصيدلية</div>
        <nav>
            <ul>
                <li><a href="#home">الرئيسية</a></li>
                <li><a href="#about">عن النظام</a></li>
                <li><a href="#services">الخدمات</a></li>
                <li><a href="#login">تسجيل الدخول</a></li>
                <li><a href="#contact">تواصل معنا</a></li>
            </ul>
        </nav>
        <button id="toggle-darkmode">تبديل الوضع</button>
    </header>

    <main>
        <section id="home" class="hero">
            <h1>نظام إدارة الصيدليات</h1>
            <button>جرب الآن</button>
        </section>

        <section id="about">
            <h2>عن النظام</h2>
            <p>نظام متكامل لإدارة الصيدليات بكفاءة.</p>
        </section>

        <section id="services">
            <h2>الخدمات</h2>
            <div class="service-box">
                <h3>إدارة المخزون</h3>
                <p>تتبع المخزون بسهولة.</p>
            </div>
            <div class="service-box">
                <h3>تقارير المبيعات</h3>
                <p>تحليل المبيعات بشكل دوري.</p>
            </div>
            <div class="service-box">
                <h3>التنبيهات الدوائية</h3>
                <p>تنبيهات حول الأدوية المنتهية.</p>
            </div>
            <div class="service-box">
                <h3>نظام نقاط البيع (POS)</h3>
                <p>نظام سهل الاستخدام لعمليات البيع.</p>
            </div>
        </section>

        <section id="login">
            <h2>تسجيل الدخول</h2>
            <form id="login-form">
                <input type="text" placeholder="اسم المستخدم" required>
                <input type="password" placeholder="كلمة المرور" required>
                <button type="submit">دخول</button>
            </form>
        </section>

        <section id="contact">
            <h2>تواصل معنا</h2>
            <form>
                <input type="text" placeholder="اسمك" required>
                <input type="email" placeholder="بريدك الإلكتروني" required>
                <textarea placeholder="رسالتك" required></textarea>
                <button type="submit">إرسال</button>
            </form>
            <div class="social-icons">
                <a href="https://wa.me/+249119441527">واتساب</a>
                <a href="https://fb.com/oshdw">فيسبوك</a>
            </div>
        </section>
    </main>

    <footer>
        <p>برمجة د. عمر بابكر (Dr.Omer Bk)</p>
    </footer>

    <script>
        document.getElementById('toggle-darkmode').addEventListener('click', function() {
            document.body.classList.toggle('dark-mode');
        });
    </script>
</body>
</html>
