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
            transition: background-color 0.5s, color 0.5s;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px;
            background-color: #f8f9fa;
        }

        nav ul {
            list-style: none;
            display: flex;
        }

        nav ul li {
            margin: 0 15px;
        }

        nav ul li a {
            text-decoration: none;
            color: #333;
        }

        #hero {
            text-align: center;
            padding: 50px 0;
        }

        .feature-box {
            display: inline-block;
            width: 200px;
            height: 100px;
            margin: 20px;
            background-color: #e9ecef;
            text-align: center;
            line-height: 100px;
            border-radius: 10px;
        }

        footer {
            text-align: center;
            padding: 20px;
            background-color: #f8f9fa;
        }

        body.dark-mode {
            background-color: #343a40;
            color: #ffffff;
        }

        header.dark-mode {
            background-color: #495057;
        }

        nav ul li a.dark-mode {
            color: #ffffff;
        }

        .feature-box.dark-mode {
            background-color: #6c757d;
        }

        input {
            display: block;
            margin: 10px auto;
            padding: 10px;
            width: 200px;
        }

        button {
            padding: 10px 20px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">صيدلية حديثة HD</div>
        <nav>
            <ul>
                <li><a href="#home">الرئيسية</a></li>
                <li><a href="#services">الخدمات</a></li>
                <li><a href="#login">تسجيل الدخول</a></li>
                <li><a href="#contact">تواصل معنا</a></li>
            </ul>
        </nav>
        <button id="toggle-mode">🌓</button>
    </header>

    <section id="hero">
        <h1>نظام إدارة الصيدليات</h1>
        <button onclick="startNow()">ابدأ الاستخدام</button>
    </section>

    <section id="features">
        <div class="feature-box">إدارة المخزون</div>
        <div class="feature-box">تقارير المبيعات</div>
        <div class="feature-box">التنبيهات الدوائية</div>
        <div class="feature-box">نظام نقاط البيع (POS)</div>
    </section>

    <section id="login">
        <h2>تسجيل الدخول</h2>
        <form onsubmit="return login(event)">
            <input type="text" placeholder="اسم المستخدم" id="username" required>
            <input type="password" placeholder="كلمة المرور" id="password" required>
            <button type="submit">دخول</button>
        </form>
    </section>

    <footer>
        <p>تصميم بواسطة د. عمر بابكر (Omer Bk)</p>
    </footer>

    <script>
        function startNow() {
            alert("ابدأ الاستخدام الآن!");
        }

        document.getElementById('toggle-mode').addEventListener('click', function() {
            document.body.classList.toggle('dark-mode');
            const links = document.querySelectorAll('nav ul li a');
            links.forEach(link => link.classList.toggle('dark-mode'));
            const featureBoxes = document.querySelectorAll('.feature-box');
            featureBoxes.forEach(box => box.classList.toggle('dark-mode'));
        });

        function login(event) {
            event.preventDefault();
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;

            if (username === 'oshdw' && password === 'omer bk') {
                alert("تم تسجيل الدخول بنجاح!");
                // هنا يمكنك إضافة كود لتوجيه المستخدم إلى لوحة التحكم
            } else {
                alert("اسم المستخدم أو كلمة المرور غير صحيحة.");
            }
        }
    </script>
</body>
</html>
