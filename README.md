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
            transition: background-color 0.3s, color 0.3s;
        }

        header {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        nav ul {
            list-style: none;
            display: flex;
        }

        nav ul li {
            margin: 0 15px;
        }

        nav a {
            color: white;
            text-decoration: none;
        }

        #hero {
            background-image: url('hero-image.jpg'); /* استبدل هذا بالمسار الصحيح للصورة */
            height: 300px;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            text-align: center;
        }

        #features {
            display: flex;
            justify-content: space-around;
            padding: 20px;
        }

        .feature-box {
            background-color: #f4f4f4;
            padding: 20px;
            border-radius: 5px;
            text-align: center;
            transition: background-color 0.3s;
            width: 20%;
            cursor: pointer;
        }

        footer {
            text-align: center;
            padding: 10px;
            background-color: #333;
            color: white;
        }

        body.dark-mode {
            background-color: #121212;
            color: white;
        }

        header.dark-mode {
            background-color: #1F1F1F;
        }

        #hero.dark-mode {
            background-color: #1F1F1F;
        }

        .feature-box.dark-mode {
            background-color: #2C2C2C;
        }

        input[type="text"], input[type="password"], input[type="number"] {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        button {
            padding: 10px 15px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }

        #inventory, #reports {
            display: none;
            padding: 20px;
        }
    </style>
</head>
<body>
    <header>
        <div class="logo">شعار الصيدلية</div>
        <nav>
            <ul>
                <li><a href="#" onclick="showSection('home')">الرئيسية</a></li>
                <li><a href="#" onclick="showSection('about')">عن النظام</a></li>
                <li><a href="#" onclick="showSection('services')">الخدمات</a></li>
                <li><a href="#" onclick="showSection('login')">تسجيل الدخول</a></li>
                <li><a href="#" onclick="showSection('contact')">تواصل معنا</a></li>
            </ul>
        </nav>
        <button id="toggle-mode">تبديل الوضع</button>
    </header>

    <section id="home">
        <div id="hero">
            <h1>نظام إدارة الصيدليات</h1>
            <button onclick="showSection('login')">جرب الآن</button>
        </div>

        <section id="features">
            <div class="feature-box" onclick="showSection('inventory')">إدارة المخزون</div>
            <div class="feature-box" onclick="showSection('reports')">تقارير المبيعات</div>
            <div class="feature-box">التنبيهات الدوائية</div>
            <div class="feature-box">نظام نقاط البيع (POS)</div>
        </section>
    </section>

    <section id="login">
        <h2>تسجيل الدخول</h2>
        <form id="login-form">
            <input type="text" placeholder="اسم المستخدم" required>
            <input type="password" placeholder="كلمة المرور" required>
            <button type="submit">دخول</button>
        </form>
    </section>

    <section id="inventory">
        <h2>إدارة المخزون</h2>
        <form id="inventory-form">
            <input type="text" placeholder="اسم الدواء" required>
            <input type="number" placeholder="الكمية" required>
            <button type="submit">إضافة إلى المخزون</button>
        </form>
        <div id="inventory-list"></div>
    </section>

    <section id="reports">
        <h2>تقارير المبيعات</h2>
        <p>هنا يمكنك عرض تقارير المبيعات.</p>
    </section>

    <footer>
        <p>برمجة د. عمر بابكر (Dr.Omer Bk)</p>
    </footer>

    <script>
        document.getElementById('toggle-mode').addEventListener('click', function() {
            document.body.classList.toggle('dark-mode');
            document.querySelector('header').classList.toggle('dark-mode');
            document.querySelector('#hero').classList.toggle('dark-mode');
            document.querySelectorAll('.feature-box').forEach(box => {
                box.classList.toggle('dark-mode');
            });
        });

        const username = 'oshdw';
        const password = 'omer bk';

        document.getElementById('login-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const userInput = document.querySelector('input[type="text"]').value;
            const passInput = document.querySelector('input[type="password"]').value;

            if (userInput === username && passInput === password) {
                alert('دخول ناجح!');
                showSection('inventory'); // توجيه المستخدم إلى إدارة المخزون
            } else {
                alert('اسم المستخدم أو كلمة المرور غير صحيحة.');
            }
        });

        document.getElementById('inventory-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const medicineName = document.querySelector('input[type="text"]').value;
            const quantity = document.querySelector('input[type="number"]').value;

            const inventoryList = document.getElementById('inventory-list');
            const newEntry = document.createElement('div');
            newEntry.textContent = `اسم الدواء: ${medicineName}, الكمية: ${quantity}`;
            inventoryList.appendChild(newEntry);

            // إعادة تعيين النموذج
            document.getElementById('inventory-form').reset();
        });

        function showSection(sectionId) {
            const sections = ['home', 'login', 'inventory', 'reports'];
            sections.forEach(id => {
                document.getElementById(id).style.display = (id === sectionId) ? 'block' : 'none';
            });
        }

        // عرض الصفحة الرئيسية عند تحميل الصفحة
        showSection('home');
    </script>
</body>
</html>
