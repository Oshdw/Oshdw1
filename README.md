<html dir="rtl" lang="ar">
 <head>
  <meta charset="utf-8"/>
  <meta content="width=device-width, initial-scale=1" name="viewport"/>
  <title>
   نظام إدارة الصيدليات المتكامل
  </title>
  <script src="https://cdn.tailwindcss.com">
  </script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" rel="stylesheet"/>
  <link href="https://fonts.googleapis.com/css2?family=Cairo&amp;display=swap" rel="stylesheet"/>
  <style>
   :root {
    --color-bg: #f9fafb;
    --color-text: #1f2937;
    --color-primary: #2563eb;
    --color-primary-hover: #1d4ed8;
    --color-card-bg: #ffffff;
    --color-glass-bg: rgba(255 255 255 / 0.15);
    --color-glass-border: rgba(255 255 255 / 0.3);
    --color-input-bg: #f3f4f6;
    --color-input-border: #d1d5db;
    --color-icon: #2563eb;
  }
  [data-theme="dark"] {
    --color-bg: #1f2937;
    --color-text: #f3f4f6;
    --color-primary: #3b82f6;
    --color-primary-hover: #60a5fa;
    --color-card-bg: #374151;
    --color-glass-bg: rgba(31 41 55 / 0.4);
    --color-glass-border: rgba(255 255 255 / 0.1);
    --color-input-bg: #4b5563;
    --color-input-border: #6b7280;
    --color-icon: #60a5fa;
  }
  body {
    font-family: 'Cairo', sans-serif;
    background-color: var(--color-bg);
    color: var(--color-text);
    transition: background-color 0.3s, color 0.3s;
  }
  a {
    color: var(--color-primary);
    transition: color 0.3s;
  }
  a:hover {
    color: var(--color-primary-hover);
  }
  .glass {
    background: var(--color-glass-bg);
    border: 1px solid var(--color-glass-border);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }
  input, select, textarea {
    background-color: var(--color-input-bg);
    border: 1px solid var(--color-input-border);
    color: var(--color-text);
    transition: background-color 0.3s, border-color 0.3s, color 0.3s;
  }
  input::placeholder, textarea::placeholder {
    color: var(--color-text);
    opacity: 0.6;
  }
  input:focus, textarea:focus {
    outline: none;
    border-color: var(--color-primary);
    box-shadow: 0 0 0 3px rgb(37 99 235 / 0.3);
  }
  /* Scrollbar for dark mode */
  [data-theme="dark"] ::-webkit-scrollbar {
    width: 8px;
  }
  [data-theme="dark"] ::-webkit-scrollbar-track {
    background: #374151;
  }
  [data-theme="dark"] ::-webkit-scrollbar-thumb {
    background-color: #60a5fa;
    border-radius: 10px;
  }
  /* Animations for icons */
  .icon-spin {
    animation: spin 4s linear infinite;
  }
  .icon-bounce {
    animation: bounce 2s infinite;
  }
  @keyframes spin {
    0% {transform: rotate(0deg);}
    100% {transform: rotate(360deg);}
  }
  @keyframes bounce {
    0%, 100% {transform: translateY(0);}
    50% {transform: translateY(-10%);}
  }
  </style>
 </head>
 <body class="min-h-screen flex flex-col" data-theme="light">
  <!-- Navbar -->
  <header class="bg-card-bg shadow-md sticky top-0 z-50">
   <nav class="container mx-auto flex items-center justify-between p-4">
    <a class="flex items-center space-x-2 rtl:space-x-reverse text-primary font-extrabold text-xl" href="#">
     <img alt="شعار أيقونة صيدلية حديثة على شكل حبة دواء زرقاء مع لمسة تقنية" class="w-10 h-10" height="40" src="https://storage.googleapis.com/a1aa/image/92fc7adb-fcb1-4267-de48-05d1558eb81a.jpg" width="40"/>
     <span>
      صيدليتك
     </span>
    </a>
    <ul class="hidden md:flex space-x-6 rtl:space-x-reverse text-base font-semibold text-text">
     <li>
      <a class="hover:text-primary transition" href="#home">
       الرئيسية
      </a>
     </li>
     <li>
      <a class="hover:text-primary transition" href="#about">
       عن النظام
      </a>
     </li>
     <li>
      <a class="hover:text-primary transition" href="#services">
       الخدمات
      </a>
     </li>
     <li>
      <a class="hover:text-primary transition" href="#login">
       تسجيل الدخول
      </a>
     </li>
     <li>
      <a class="hover:text-primary transition" href="#dashboard">
       لوحة التحكم
      </a>
     </li>
     <li>
      <a class="hover:text-primary transition" href="#contact">
       تواصل معنا
      </a>
     </li>
    </ul>
    <div class="flex items-center space-x-4 rtl:space-x-reverse">
     <button aria-label="تبديل الوضع الليلي والنهاري" class="text-primary hover:text-primary-hover focus:outline-none text-2xl" id="darkModeToggle">
      <i class="fas fa-moon">
      </i>
     </button>
     <button aria-label="قائمة التنقل للهواتف" class="md:hidden text-primary hover:text-primary-hover focus:outline-none text-2xl" id="mobileMenuBtn">
      <i class="fas fa-bars">
      </i>
     </button>
    </div>
   </nav>
   <div class="hidden md:hidden bg-card-bg border-t border-gray-300 dark:border-gray-700" id="mobileMenu">
    <ul class="flex flex-col p-4 space-y-3 text-right font-semibold text-text">
     <li>
      <a class="block hover:text-primary transition" href="#home">
       الرئيسية
      </a>
     </li>
     <li>
      <a class="block hover:text-primary transition" href="#about">
       عن النظام
      </a>
     </li>
     <li>
      <a class="block hover:text-primary transition" href="#services">
       الخدمات
      </a>
     </li>
     <li>
      <a class="block hover:text-primary transition" href="#login">
       تسجيل الدخول
      </a>
     </li>
     <li>
      <a class="block hover:text-primary transition" href="#dashboard">
       لوحة التحكم
      </a>
     </li>
     <li>
      <a class="block hover:text-primary transition" href="#contact">
       تواصل معنا
      </a>
     </li>
    </ul>
   </div>
  </header>
  <!-- Hero Section -->
  <section class="flex flex-col md:flex-row items-center justify-between container mx-auto px-6 py-16 gap-10" id="home">
   <div class="md:w-1/2 text-center md:text-right space-y-6">
    <h1 class="text-4xl md:text-5xl font-extrabold leading-tight">
     نظام إدارة الصيدليات المتكامل
    </h1>
    <p class="text-lg text-gray-600 dark:text-gray-300">
     حلول تقنية حديثة لإدارة صيدليتك بكفاءة وسهولة.
    </p>
    <button class="bg-primary hover:bg-primary-hover text-white font-bold py-3 px-8 rounded-lg shadow-lg transition focus:outline-none focus:ring-4 focus:ring-primary/50" id="tryNowBtn">
     جرب الآن
    </button>
   </div>
   <div class="md:w-1/2 flex justify-center">
    <img alt="صورة ثلاثية الأبعاد متحركة لصيدلية حديثة مع أدوية وأجهزة طبية توضح التقنية الحديثة" class="w-full max-w-md rounded-lg shadow-lg" height="300" src="https://storage.googleapis.com/a1aa/image/8040dc5b-752c-4f3b-a2cb-9f8d4f718aa9.jpg" width="400"/>
   </div>
  </section>
  <!-- Features Section -->
  <section class="bg-card-bg py-16" id="services">
   <div class="container mx-auto px-6">
    <h2 class="text-3xl font-extrabold text-center mb-12">
     مزايا النظام
    </h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-10">
     <div class="flex flex-col items-center text-center p-6 bg-card-bg rounded-xl shadow-lg hover:shadow-xl transition cursor-default">
      <i class="fas fa-boxes fa-3x text-primary icon-bounce mb-4">
      </i>
      <h3 class="text-xl font-semibold mb-2">
       إدارة المخزون
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تنظيم وتتبع الأدوية والمستلزمات بدقة عالية.
      </p>
     </div>
     <div class="flex flex-col items-center text-center p-6 bg-card-bg rounded-xl shadow-lg hover:shadow-xl transition cursor-default">
      <i class="fas fa-chart-line fa-3x text-primary icon-spin mb-4">
      </i>
      <h3 class="text-xl font-semibold mb-2">
       تقارير المبيعات
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تحليل مفصل لأداء المبيعات لتحسين الأرباح.
      </p>
     </div>
     <div class="flex flex-col items-center text-center p-6 bg-card-bg rounded-xl shadow-lg hover:shadow-xl transition cursor-default">
      <i class="fas fa-exclamation-triangle fa-3x text-primary icon-bounce mb-4">
      </i>
      <h3 class="text-xl font-semibold mb-2">
       التنبيهات الدوائية
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تنبيهات فورية للجرعات والتفاعلات الدوائية.
      </p>
     </div>
     <div class="flex flex-col items-center text-center p-6 bg-card-bg rounded-xl shadow-lg hover:shadow-xl transition cursor-default">
      <i class="fas fa-cash-register fa-3x text-primary icon-spin mb-4">
      </i>
      <h3 class="text-xl font-semibold mb-2">
       نظام نقاط البيع (POS)
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       نظام بيع متكامل وسهل الاستخدام لتسريع العمليات.
      </p>
     </div>
    </div>
   </div>
  </section>
  <!-- About Section -->
  <section class="container mx-auto px-6 py-16" id="about">
   <h2 class="text-3xl font-extrabold text-center mb-8">
    عن النظام
   </h2>
   <div class="max-w-4xl mx-auto text-center text-gray-700 dark:text-gray-300 space-y-6 text-lg leading-relaxed">
    <p>
     نظام إدارة الصيدليات المتكامل هو الحل الأمثل لتسهيل عمليات الصيدلية اليومية، من إدارة المخزون إلى تقارير المبيعات، مع واجهة مستخدم عصرية وسهلة الاستخدام.
    </p>
    <p>
     تم تصميم النظام ليعكس خبرة 20 سنة في مجال الصيدلة والتقنية، مع التركيز على الأمان، الدقة، والسرعة في الأداء.
    </p>
    <p>
     يدعم النظام الوضعين النهاري والليلي لتوفير تجربة مريحة في جميع الأوقات.
    </p>
   </div>
  </section>
  <!-- Login Section -->
  <section class="container mx-auto px-6 py-16 max-w-md" id="login">
   <h2 class="text-3xl font-extrabold text-center mb-8">
    تسجيل الدخول
   </h2>
   <form autocomplete="off" class="glass p-8 rounded-xl shadow-lg flex flex-col space-y-6" id="loginForm" novalidate="">
    <label class="text-lg font-semibold" for="username">
     اسم المستخدم
    </label>
    <input autocomplete="username" class="rounded-md p-3" id="username" name="username" placeholder="أدخل اسم المستخدم" required="" type="text"/>
    <label class="text-lg font-semibold" for="password">
     كلمة المرور
    </label>
    <input autocomplete="current-password" class="rounded-md p-3" id="password" name="password" placeholder="أدخل كلمة المرور" required="" type="password"/>
    <button class="bg-primary hover:bg-primary-hover text-white font-bold py-3 rounded-lg shadow-md transition focus:outline-none focus:ring-4 focus:ring-primary/50" type="submit">
     دخول
    </button>
    <p class="text-red-500 text-center hidden" id="loginError">
     اسم المستخدم أو كلمة المرور غير صحيحة.
    </p>
   </form>
  </section>
  <!-- Dashboard Section -->
  <section class="container mx-auto px-6 py-16 max-w-7xl hidden" id="dashboard">
   <h2 class="text-3xl font-extrabold text-center mb-8">
    لوحة التحكم
   </h2>
   <div class="grid grid-cols-1 md:grid-cols-4 gap-8 mb-12">
    <div class="bg-card-bg rounded-xl shadow-lg p-6 flex flex-col items-center text-center">
     <i class="fas fa-chart-pie fa-4x text-primary mb-4">
     </i>
     <h3 class="text-xl font-semibold mb-2">
      إحصائيات
     </h3>
     <p class="text-gray-600 dark:text-gray-300">
      معلومات شاملة عن أداء الصيدلية.
     </p>
    </div>
    <div class="bg-card-bg rounded-xl shadow-lg p-6 flex flex-col items-center text-center">
     <i class="fas fa-pills fa-4x text-primary mb-4">
     </i>
     <h3 class="text-xl font-semibold mb-2">
      إدارة الأدوية
     </h3>
     <p class="text-gray-600 dark:text-gray-300">
      إضافة وتعديل وحذف الأدوية بسهولة.
     </p>
    </div>
    <div class="bg-card-bg rounded-xl shadow-lg p-6 flex flex-col items-center text-center">
     <i class="fas fa-file-alt fa-4x text-primary mb-4">
     </i>
     <h3 class="text-xl font-semibold mb-2">
      تقارير
     </h3>
     <p class="text-gray-600 dark:text-gray-300">
      تقارير مفصلة عن المبيعات والمخزون.
     </p>
    </div>
    <div class="bg-card-bg rounded-xl shadow-lg p-6 flex flex-col items-center text-center">
     <i class="fas fa-users fa-4x text-primary mb-4">
     </i>
     <h3 class="text-xl font-semibold mb-2">
      إدارة موظفين
     </h3>
     <p class="text-gray-600 dark:text-gray-300">
      إضافة وتعديل صلاحيات الموظفين.
     </p>
    </div>
   </div>
   <div class="text-center">
    <button class="bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-8 rounded-lg shadow-md transition focus:outline-none focus:ring-4 focus:ring-red-400/50" id="logoutBtn">
     تسجيل خروج
    </button>
   </div>
  </section>
  <!-- Contact Section -->
  <section class="bg-card-bg py-16" id="contact">
   <div class="container mx-auto px-6 max-w-3xl">
    <h2 class="text-3xl font-extrabold text-center mb-8">
     تواصل معنا
    </h2>
    <form autocomplete="off" class="flex flex-col space-y-6" id="contactForm" novalidate="">
     <input class="rounded-md p-3 border border-input-border bg-input-bg text-text" id="contactName" name="contactName" placeholder="الاسم الكامل" required="" type="text"/>
     <input class="rounded-md p-3 border border-input-border bg-input-bg text-text" id="contactEmail" name="contactEmail" placeholder="البريد الإلكتروني" required="" type="email"/>
     <textarea class="rounded-md p-3 border border-input-border bg-input-bg text-text" id="contactMessage" name="contactMessage" placeholder="رسالتك" required="" rows="5"></textarea>
     <button class="bg-primary hover:bg-primary-hover text-white font-bold py-3 rounded-lg shadow-md transition focus:outline-none focus:ring-4 focus:ring-primary/50" type="submit">
      إرسال
     </button>
    </form>
    <div class="mt-12 flex justify-center space-x-8 rtl:space-x-reverse text-primary text-3xl">
     <a aria-label="واتساب" href="https://wa.me/1234567890" rel="noopener" target="_blank">
      <i class="fab fa-whatsapp hover:text-green-500 transition">
      </i>
     </a>
     <a aria-label="فيسبوك" href="https://facebook.com" rel="noopener" target="_blank">
      <i class="fab fa-facebook-f hover:text-blue-600 transition">
      </i>
     </a>
     <a aria-label="إيميل" href="mailto:info@pharmacy.com">
      <i class="fas fa-envelope hover:text-red-600 transition">
      </i>
     </a>
    </div>
   </div>
  </section>
  <!-- Footer -->
  <footer class="bg-card-bg border-t border-gray-300 dark:border-gray-700 py-6 mt-auto">
   <div class="container mx-auto px-6 text-center text-gray-600 dark:text-gray-400 text-sm">
    تصميم بواسطة
    <a class="text-primary hover:text-primary-hover font-semibold" href="https://omerbabiker.com" rel="noopener" target="_blank">
     عمر بابكر (Omer Babiker - Omer Bk)
    </a>
   </div>
  </footer>
  <script>
   // Dark mode toggle
  const darkModeToggle = document.getElementById('darkModeToggle');
  const body = document.body;
  const icon = darkModeToggle.querySelector('i');
  const mobileMenuBtn = document.getElementById('mobileMenuBtn');
  const mobileMenu = document.getElementById('mobileMenu');
  const tryNowBtn = document.getElementById('tryNowBtn');
  const loginForm = document.getElementById('loginForm');
  const loginError = document.getElementById('loginError');
  const dashboardSection = document.getElementById('dashboard');
  const logoutBtn = document.getElementById('logoutBtn');
  const navLinks = document.querySelectorAll('nav ul li a, #mobileMenu ul li a');

  // Load theme from localStorage or default to light
  function loadTheme() {
    const theme = localStorage.getItem('theme') || 'light';
    body.setAttribute('data-theme', theme);
    if (theme === 'dark') {
      icon.classList.remove('fa-moon');
      icon.classList.add('fa-sun');
    } else {
      icon.classList.remove('fa-sun');
      icon.classList.add('fa-moon');
    }
  }
  loadTheme();

  darkModeToggle.addEventListener('click', () => {
    if (body.getAttribute('data-theme') === 'light') {
      body.setAttribute('data-theme', 'dark');
      icon.classList.remove('fa-moon');
      icon.classList.add('fa-sun');
      localStorage.setItem('theme', 'dark');
    } else {
      body.setAttribute('data-theme', 'light');
      icon.classList.remove('fa-sun');
      icon.classList.add('fa-moon');
      localStorage.setItem('theme', 'light');
    }
  });

  // Mobile menu toggle
  mobileMenuBtn.addEventListener('click', () => {
    if (mobileMenu.classList.contains('hidden')) {
      mobileMenu.classList.remove('hidden');
    } else {
      mobileMenu.classList.add('hidden');
    }
  });

  // Simple auth system using localStorage
  // For demo: username: admin, password: 123456
  function isLoggedIn() {
    return localStorage.getItem('loggedIn') === 'true';
  }
  function showDashboard() {
    dashboardSection.classList.remove('hidden');
    document.getElementById('login').classList.add('hidden');
    document.getElementById('home').scrollIntoView({behavior: 'smooth'});
  }
  function showLogin() {
    dashboardSection.classList.add('hidden');
    document.getElementById('login').classList.remove('hidden');
    document.getElementById('login').scrollIntoView({behavior: 'smooth'});
  }

  if (isLoggedIn()) {
    showDashboard();
  } else {
    showLogin();
  }

  loginForm.addEventListener('submit', e => {
    e.preventDefault();
    const username = loginForm.username.value.trim();
    const password = loginForm.password.value.trim();
    if (username === 'admin' && password === '123456') {
      localStorage.setItem('loggedIn', 'true');
      loginError.classList.add('hidden');
      loginForm.reset();
      showDashboard();
    } else {
      loginError.classList.remove('hidden');
    }
  });

  logoutBtn.addEventListener('click', () => {
    localStorage.removeItem('loggedIn');
    showLogin();
  });

  // Smooth scroll for nav links
  navLinks.forEach(link => {
    link.addEventListener('click', e => {
      e.preventDefault();
      const targetId = link.getAttribute('href').substring(1);
      const targetSection = document.getElementById(targetId);
      if (targetSection) {
        targetSection.scrollIntoView({behavior: 'smooth'});
        if (!mobileMenu.classList.contains('hidden')) {
          mobileMenu.classList.add('hidden');
        }
      }
    });
  });

  // Try Now button scrolls to login
  tryNowBtn.addEventListener('click', () => {
    if (isLoggedIn()) {
      showDashboard();
    } else {
      showLogin();
    }
  });

  // Contact form submission (dummy)
  const contactForm = document.getElementById('contactForm');
  contactForm.addEventListener('submit', e => {
    e.preventDefault();
    alert('تم إرسال رسالتك بنجاح، شكراً لتواصلك معنا!');
    contactForm.reset();
  });
  </script>
 </body>
</html>
