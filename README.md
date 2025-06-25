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
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&amp;display=swap" rel="stylesheet"/>
  <style>
   :root {
    --color-bg-light: #f9fafb;
    --color-bg-dark: #121212;
    --color-primary-light: #2563eb;
    --color-primary-dark: #3b82f6;
    --color-text-light: #1f2937;
    --color-text-dark: #e5e7eb;
    --color-card-light: #ffffff;
    --color-card-dark: #1f2937;
    --color-glass-light: rgba(255 255 255 / 0.15);
    --color-glass-dark: rgba(0 0 0 / 0.3);
  }

  html,
  body {
    font-family: "Cairo", sans-serif;
    transition: background-color 0.3s, color 0.3s;
  }

  body.light {
    background-color: var(--color-bg-light);
    color: var(--color-text-light);
  }

  body.light header,
  body.light nav,
  body.light footer {
    background-color: var(--color-card-light);
  }

  body.light .card {
    background-color: var(--color-card-light);
    color: var(--color-text-light);
  }

  body.light .glass {
    background: var(--color-glass-light);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }

  body.dark {
    background-color: var(--color-bg-dark);
    color: var(--color-text-dark);
  }

  body.dark header,
  body.dark nav,
  body.dark footer {
    background-color: var(--color-card-dark);
  }

  body.dark .card {
    background-color: var(--color-card-dark);
    color: var(--color-text-dark);
  }

  body.dark .glass {
    background: var(--color-glass-dark);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
  }

  /* Scrollbar */
  ::-webkit-scrollbar {
    width: 8px;
    height: 8px;
  }
  ::-webkit-scrollbar-track {
    background: transparent;
  }
  ::-webkit-scrollbar-thumb {
    background-color: #6b7280;
    border-radius: 10px;
  }

  /* Glassmorphism input */
  input,
  button,
  textarea {
    transition: background-color 0.3s, color 0.3s, border-color 0.3s;
  }

  /* Hero image animation */
  @keyframes float {
    0% {
      transform: translatey(0px);
    }
    50% {
      transform: translatey(-15px);
    }
    100% {
      transform: translatey(0px);
    }
  }

  .animate-float {
    animation: float 4s ease-in-out infinite;
  }

  /* Button hover effect */
  .btn-primary {
    background-color: var(--color-primary-light);
    color: white;
    transition: background-color 0.3s;
  }
  body.dark .btn-primary {
    background-color: var(--color-primary-dark);
  }
  .btn-primary:hover {
    background-color: #1e40af;
  }
  body.dark .btn-primary:hover {
    background-color: #2563eb;
  }

  /* Glass button */
  .btn-glass {
    background: var(--color-glass-light);
    border: 1px solid rgba(255 255 255 / 0.3);
    color: var(--color-text-light);
  }
  body.dark .btn-glass {
    background: var(--color-glass-dark);
    border: 1px solid rgba(255 255 255 / 0.1);
    color: var(--color-text-dark);
  }
  .btn-glass:hover {
    background: rgba(255 255 255 / 0.25);
  }
  body.dark .btn-glass:hover {
    background: rgba(255 255 255 / 0.15);
  }

  /* Navbar link active */
  nav a.active {
    font-weight: 700;
    border-bottom: 2px solid var(--color-primary-light);
  }
  body.dark nav a.active {
    border-bottom: 2px solid var(--color-primary-dark);
  }

  /* Form input focus */
  input:focus,
  textarea:focus {
    outline: none;
    border-color: var(--color-primary-light);
    box-shadow: 0 0 5px var(--color-primary-light);
  }
  body.dark input:focus,
  body.dark textarea:focus {
    border-color: var(--color-primary-dark);
    box-shadow: 0 0 5px var(--color-primary-dark);
  }

  /* Disabled button */
  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  /* Responsive fixes */
  @media (max-width: 640px) {
    nav ul {
      flex-direction: column;
      gap: 0.5rem;
    }
  }
  </style>
 </head>
 <body class="light flex flex-col min-h-screen">
  <header class="sticky top-0 z-50 shadow-md">
   <nav aria-label="Primary Navigation" class="container mx-auto flex items-center justify-between p-4">
    <a aria-label="شعار نظام إدارة الصيدليات" class="flex items-center gap-2 text-xl font-extrabold text-primary-600 dark:text-primary-400" href="#home">
     <img alt="أيقونة صيدلية حديثة على شكل كبسولة دواء زرقاء وبيضاء مع صليب طبي" class="w-10 h-10" height="40" loading="lazy" src="https://storage.googleapis.com/a1aa/image/e95e2e6d-613e-426b-e809-8c0162d71e27.jpg" width="40"/>
     <span class="select-none">
      صيدليتك
     </span>
    </a>
    <ul class="hidden sm:flex gap-8 text-lg font-semibold text-gray-700 dark:text-gray-300" id="nav-links">
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400 active" href="#home">
       الرئيسية
      </a>
     </li>
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400" href="#about">
       عن النظام
      </a>
     </li>
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400" href="#services">
       الخدمات
      </a>
     </li>
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400" href="#login">
       تسجيل الدخول
      </a>
     </li>
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400" href="#dashboard">
       لوحة التحكم
      </a>
     </li>
     <li>
      <a class="hover:text-blue-600 dark:hover:text-blue-400" href="#contact">
       تواصل معنا
      </a>
     </li>
    </ul>
    <button aria-label="فتح القائمة" class="sm:hidden text-gray-700 dark:text-gray-300 focus:outline-none" id="nav-toggle">
     <i class="fas fa-bars fa-lg">
     </i>
    </button>
    <button aria-label="تبديل الوضع الليلي والنهاري" class="ml-4 p-2 rounded-full text-gray-700 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-700 transition" id="dark-mode-toggle" title="تبديل الوضع الليلي والنهاري">
     <i class="fas fa-moon">
     </i>
    </button>
   </nav>
   <div class="sm:hidden bg-white dark:bg-gray-800 shadow-md hidden flex-col gap-4 p-4" id="mobile-menu">
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700 active" href="#home">
     الرئيسية
    </a>
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700" href="#about">
     عن النظام
    </a>
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700" href="#services">
     الخدمات
    </a>
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700" href="#login">
     تسجيل الدخول
    </a>
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700" href="#dashboard">
     لوحة التحكم
    </a>
    <a class="block py-2 px-3 rounded hover:bg-gray-200 dark:hover:bg-gray-700" href="#contact">
     تواصل معنا
    </a>
   </div>
  </header>
  <main class="flex-grow container mx-auto px-4 py-8 space-y-20" id="main-content" tabindex="-1">
   <!-- Hero Section -->
   <section aria-label="قسم البطل" class="flex flex-col-reverse md:flex-row items-center justify-between gap-8" id="home">
    <div class="max-w-xl text-center md:text-right space-y-6">
     <h1 class="text-4xl sm:text-5xl font-extrabold leading-tight">
      نظام إدارة الصيدليات المتكامل
     </h1>
     <p class="text-gray-600 dark:text-gray-300 text-lg">
      الحل الأمثل لإدارة صيدليتك بكفاءة عالية، مع واجهة سهلة الاستخدام وتقارير دقيقة.
     </p>
     <button aria-label="جرب الآن" class="btn-primary px-8 py-3 rounded-lg font-semibold shadow-lg hover:shadow-xl transition" id="try-now-btn">
      جرب الآن
     </button>
    </div>
    <img alt="صورة متحركة ثلاثية الأبعاد لكبسولة دواء زرقاء وبيضاء مع تأثيرات حركة ناعمة وخلفية صيدلية حديثة" class="w-full max-w-md animate-float rounded-lg shadow-lg" height="300" loading="lazy" src="https://storage.googleapis.com/a1aa/image/0f680aeb-1d30-4506-7e0c-23e2ddc14122.jpg" width="400"/>
   </section>
   <!-- Features Section -->
   <section aria-label="مميزات النظام" class="space-y-12" id="services">
    <h2 class="text-3xl font-bold text-center mb-8">
     مميزات النظام
    </h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-8 text-center" role="list">
     <article aria-label="إدارة المخزون" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4 hover:shadow-2xl transition cursor-default" role="listitem">
      <img alt="أيقونة دواء على شكل علبة أدوية مع حبوب وصيدلي يرتدي معطف أبيض" class="w-20 h-20 animate-bounce rounded" height="80" loading="lazy" src="https://storage.googleapis.com/a1aa/image/2a2e16af-9f11-446c-7c37-831c695ae630.jpg" width="80"/>
      <h3 class="text-xl font-semibold">
       إدارة المخزون
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تتبع دقيق للمخزون مع تنبيهات تلقائية عند انخفاض الكميات.
      </p>
     </article>
     <article aria-label="تقارير المبيعات" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4 hover:shadow-2xl transition cursor-default" role="listitem">
      <img alt="أيقونة رسم بياني شريطي ملون يعبر عن تقارير المبيعات مع رمز دواء" class="w-20 h-20 animate-pulse rounded" height="80" loading="lazy" src="https://storage.googleapis.com/a1aa/image/5846df3e-13a5-4026-f910-805b8f1b24a3.jpg" width="80"/>
      <h3 class="text-xl font-semibold">
       تقارير المبيعات
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تقارير مفصلة تساعدك على اتخاذ قرارات ذكية وسريعة.
      </p>
     </article>
     <article aria-label="التنبيهات الدوائية" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4 hover:shadow-2xl transition cursor-default" role="listitem">
      <img alt="أيقونة جرس تنبيه مع رمز صليب طبي وأقراص دواء" class="w-20 h-20 animate-ping rounded" height="80" loading="lazy" src="https://storage.googleapis.com/a1aa/image/aeec6705-c5a8-474f-d736-77a7fd78371f.jpg" width="80"/>
      <h3 class="text-xl font-semibold">
       التنبيهات الدوائية
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       تنبيهات فورية للجرعات والتفاعلات الدوائية المهمة.
      </p>
     </article>
     <article aria-label="نظام نقاط البيع POS" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4 hover:shadow-2xl transition cursor-default" role="listitem">
      <img alt="أيقونة جهاز نقاط بيع حديث مع شاشة لمس وأيقونات دوائية" class="w-20 h-20 animate-bounce rounded" height="80" loading="lazy" src="https://storage.googleapis.com/a1aa/image/7d852746-a87e-4cde-a8f2-9c8c1a891c53.jpg" width="80"/>
      <h3 class="text-xl font-semibold">
       نظام نقاط البيع (POS)
      </h3>
      <p class="text-gray-600 dark:text-gray-300">
       نظام بيع سريع وآمن مع دعم لطرق الدفع المتعددة.
      </p>
     </article>
    </div>
   </section>
   <!-- About Section -->
   <section aria-label="عن النظام" class="max-w-4xl mx-auto space-y-6" id="about">
    <h2 class="text-3xl font-bold text-center mb-6">
     عن النظام
    </h2>
    <p class="text-center text-gray-700 dark:text-gray-300 text-lg leading-relaxed">
     نظام إدارة الصيدليات المتكامل هو الحل الأمثل الذي يجمع بين القوة، الأناقة، والتقنية الحديثة. تم تصميمه بعناية فائقة ليعكس خبرة 20 سنة في مجال الصيدلة، مع التركيز على التفاصيل الدقيقة التي تضمن لك تجربة استخدام سلسة وفعالة.
    </p>
    <img alt="واجهة نظام إدارة صيدلية حديثة تعرض لوحة تحكم مع إحصائيات وأيقونات تقنية دوائية" class="rounded-lg shadow-lg mx-auto" height="400" loading="lazy" src="https://storage.googleapis.com/a1aa/image/cf3048d2-0683-4e97-e310-d4d3fb07573e.jpg" width="800"/>
   </section>
   <!-- Login & Register Section -->
   <section aria-label="تسجيل الدخول وإنشاء حساب" class="max-w-md mx-auto space-y-8" id="login">
    <h2 class="text-3xl font-bold text-center mb-6">
     تسجيل الدخول / إنشاء حساب
    </h2>
    <div class="flex justify-center gap-4 mb-6">
     <button aria-controls="login-form" aria-selected="true" class="tab-btn btn-glass px-6 py-2 rounded-lg font-semibold" id="login-tab" role="tab" tabindex="0">
      تسجيل الدخول
     </button>
     <button aria-controls="register-form" aria-selected="false" class="tab-btn btn-glass px-6 py-2 rounded-lg font-semibold" id="register-tab" role="tab" tabindex="-1">
      إنشاء حساب
     </button>
    </div>
    <!-- Login Form -->
    <form aria-labelledby="login-tab" class="glass p-6 rounded-xl shadow-lg space-y-6" id="login-form" role="tabpanel">
     <label class="block font-semibold mb-1" for="login-username">
      اسم المستخدم
     </label>
     <input autocomplete="username" class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="login-username" name="login-username" placeholder="أدخل اسم المستخدم" required="" type="text"/>
     <label class="block font-semibold mb-1" for="login-password">
      كلمة المرور
     </label>
     <input autocomplete="current-password" class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="login-password" name="login-password" placeholder="أدخل كلمة المرور" required="" type="password"/>
     <button aria-label="تسجيل الدخول" class="btn-primary w-full py-3 rounded-lg font-semibold" type="submit">
      دخول
     </button>
     <p class="text-red-500 text-center hidden" id="login-error" role="alert">
     </p>
    </form>
    <!-- Register Form -->
    <form aria-labelledby="register-tab" class="glass p-6 rounded-xl shadow-lg space-y-6 hidden" id="register-form" role="tabpanel">
     <label class="block font-semibold mb-1" for="register-username">
      اسم المستخدم
     </label>
     <input autocomplete="username" class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="register-username" name="register-username" placeholder="اختر اسم مستخدم" required="" type="text"/>
     <label class="block font-semibold mb-1" for="register-password">
      كلمة المرور
     </label>
     <input autocomplete="new-password" class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="register-password" name="register-password" placeholder="اختر كلمة مرور" required="" type="password"/>
     <label class="block font-semibold mb-1" for="register-password-confirm">
      تأكيد كلمة المرور
     </label>
     <input autocomplete="new-password" class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="register-password-confirm" name="register-password-confirm" placeholder="أعد إدخال كلمة المرور" required="" type="password"/>
     <button aria-label="إنشاء حساب" class="btn-primary w-full py-3 rounded-lg font-semibold" type="submit">
      إنشاء حساب
     </button>
     <p class="text-red-500 text-center hidden" id="register-error" role="alert">
     </p>
     <p class="text-green-500 text-center hidden" id="register-success" role="alert">
     </p>
     <p class="text-sm text-gray-500 dark:text-gray-400 text-center">
      التسجيل يحتاج موافقة المسؤول (Oshdw).
     </p>
    </form>
   </section>
   <!-- Dashboard Section -->
   <section aria-label="لوحة التحكم" class="max-w-6xl mx-auto space-y-10 hidden" id="dashboard">
    <h2 class="text-3xl font-bold text-center mb-6">
     لوحة التحكم
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-8">
     <div aria-label="إحصائيات" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4">
      <i class="fas fa-chart-line fa-3x text-blue-600 dark:text-blue-400">
      </i>
      <h3 class="text-xl font-semibold">
       إحصائيات
      </h3>
      <p class="text-center text-gray-600 dark:text-gray-300">
       متابعة أداء الصيدلية بشكل لحظي.
      </p>
     </div>
     <div aria-label="إدارة الأدوية" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4">
      <i class="fas fa-capsules fa-3x text-green-600 dark:text-green-400">
      </i>
      <h3 class="text-xl font-semibold">
       إدارة الأدوية
      </h3>
      <p class="text-center text-gray-600 dark:text-gray-300">
       إضافة وتعديل وحذف الأدوية بسهولة.
      </p>
     </div>
     <div aria-label="تقارير" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4">
      <i class="fas fa-file-medical-alt fa-3x text-yellow-600 dark:text-yellow-400">
      </i>
      <h3 class="text-xl font-semibold">
       تقارير
      </h3>
      <p class="text-center text-gray-600 dark:text-gray-300">
       تقارير مفصلة عن المبيعات والمخزون.
      </p>
     </div>
     <div aria-label="إدارة الموظفين" class="card p-6 rounded-xl shadow-lg flex flex-col items-center space-y-4">
      <i class="fas fa-user-md fa-3x text-purple-600 dark:text-purple-400">
      </i>
      <h3 class="text-xl font-semibold">
       إدارة الموظفين
      </h3>
      <p class="text-center text-gray-600 dark:text-gray-300">
       إضافة وتعديل صلاحيات الموظفين.
      </p>
     </div>
    </div>
    <div class="text-center">
     <button aria-label="تسجيل الخروج" class="btn-primary px-8 py-3 rounded-lg font-semibold" id="logout-btn">
      تسجيل خروج
     </button>
    </div>
   </section>
   <!-- Contact Section -->
   <section aria-label="تواصل معنا" class="max-w-3xl mx-auto space-y-8" id="contact">
    <h2 class="text-3xl font-bold text-center mb-6">
     تواصل معنا
    </h2>
    <form aria-live="polite" class="glass p-6 rounded-xl shadow-lg space-y-6" id="contact-form">
     <label class="block font-semibold mb-1" for="contact-name">
      الاسم
     </label>
     <input class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="contact-name" name="contact-name" placeholder="أدخل اسمك" required="" type="text"/>
     <label class="block font-semibold mb-1" for="contact-email">
      البريد الإلكتروني
     </label>
     <input class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600" id="contact-email" name="contact-email" placeholder="example@mail.com" required="" type="email"/>
     <label class="block font-semibold mb-1" for="contact-message">
      الرسالة
     </label>
     <textarea class="w-full p-3 rounded-md bg-transparent border border-gray-300 dark:border-gray-600 resize-none" id="contact-message" name="contact-message" placeholder="اكتب رسالتك هنا" required="" rows="5"></textarea>
     <div class="flex justify-center gap-4">
      <button aria-label="إرسال الرسالة" class="btn-primary px-8 py-3 rounded-lg font-semibold" type="submit">
       إرسال
      </button>
      <button aria-label="إعادة تعيين النموذج" class="btn-glass px-8 py-3 rounded-lg font-semibold" type="reset">
       إعادة تعيين
      </button>
     </div>
     <p class="text-center text-green-600 dark:text-green-400 hidden" id="contact-feedback" role="alert">
     </p>
    </form>
    <div class="flex justify-center gap-8 text-3xl text-gray-600 dark:text-gray-400">
     <a aria-label="واتساب" class="hover:text-green-500 transition" href="https://wa.me/1234567890" rel="noopener noreferrer" target="_blank">
      <i class="fab fa-whatsapp">
      </i>
     </a>
     <a aria-label="فيسبوك" class="hover:text-blue-600 transition" href="https://facebook.com" rel="noopener noreferrer" target="_blank">
      <i class="fab fa-facebook">
      </i>
     </a>
     <a aria-label="البريد الإلكتروني" class="hover:text-red-600 transition" href="mailto:info@pharmacy.com">
      <i class="fas fa-envelope">
      </i>
     </a>
    </div>
   </section>
  </main>
  <footer class="bg-gray-200 dark:bg-gray-900 text-center py-6 mt-auto select-none">
   <p class="text-gray-700 dark:text-gray-400 font-semibold">
    تصميم بواسطة عمر بابكر (Omer Babiker - Omer Bk)
   </p>
  </footer>
  <script>
   // Dark mode toggle
  const darkModeToggle = document.getElementById("dark-mode-toggle");
  const body = document.body;

  // Load theme from localStorage or system preference
  function loadTheme() {
    const savedTheme = localStora
