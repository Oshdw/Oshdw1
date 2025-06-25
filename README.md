<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الصيدلية - تسجيل الدخول</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap');
        
        body {
            font-family: 'Tajawal', sans-serif;
            transition: all 0.3s ease;
        }
        
        .login-container {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        }
        
        .dark .login-container {
            background: linear-gradient(135deg, #2c3e50 0%, #4ca1af 100%);
        }
        
        .form-input {
            transition: all 0.3s ease;
        }
        
        .form-input:focus {
            box-shadow: 0 0 0 3px rgba(79, 209, 197, 0.3);
        }
        
        .btn-primary {
            background: linear-gradient(to right, #4facfe 0%, #00f2fe 100%);
        }
        
        .dark .btn-primary {
            background: linear-gradient(to right, #3a7bd5 0%, #00d2ff 100%);
        }
        
        .login-decoration {
            background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Cpath fill='%23e2e8f0' d='M50 0h50v50H50zm0 50h50v50H50zM0 50h50v50H0zm0-50h50v50H0z'/%3E%3C/svg%3E");
        }
        
        .dark .login-decoration {
            background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='100' height='100' viewBox='0 0 100 100'%3E%3Cpath fill='%232d3748' d='M50 0h50v50H50zm0 50h50v50H50zM0 50h50v50H0zm0-50h50v50H0z'/%3E%3C/svg%3E");
        }
        
        .pill-icon {
            animation: float 6s ease-in-out infinite;
        }
        
        @keyframes float {
            0% { transform: translateY(0px); }
            50% { transform: translateY(-15px); }
            100% { transform: translateY(0px); }
        }
    </style>
</head>
<body class="bg-gray-100 dark:bg-gray-900">
    <div class="min-h-screen flex items-center justify-center p-4 login-container">
        <!-- Dark Mode Toggle -->
        <button id="theme-toggle" class="absolute top-4 right-4 p-2 rounded-full bg-white dark:bg-gray-800 shadow-lg">
            <i id="theme-icon" class="fas fa-moon text-gray-800 dark:text-yellow-300"></i>
        </button>

        <div class="w-full max-w-4xl bg-white dark:bg-gray-800 rounded-2xl shadow-xl overflow-hidden grid grid-cols-1 md:grid-cols-2">
            <!-- Left Side - Decoration -->
            <div class="hidden md:flex items-center justify-center p-8 login-decoration relative">
                <div class="absolute inset-0 bg-black bg-opacity-10 dark:bg-opacity-20"></div>
                <div class="relative z-10 text-center">
                    <svg class="w-48 h-48 mx-auto pill-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M8 12H16M12 8V16M20 12C20 16.4183 16.4183 20 12 20C7.58172 20 4 16.4183 4 12C4 7.58172 7.58172 4 12 4C16.4183 4 20 7.58172 20 12Z" stroke="#4CA1AF" stroke-width="2" stroke-linecap="round"/>
                    </svg>
                    <h2 class="text-3xl font-bold text-gray-800 dark:text-white mt-6">نظام إدارة الصيدلية</h2>
                    <p class="text-gray-600 dark:text-gray-300 mt-2">إدارة كاملة لمخزون وأعمال صيدليتك</p>
                </div>
            </div>
            
            <!-- Right Side - Login Form -->
            <div class="p-8 md:p-12">
                <div class="text-center md:text-right">
                    <h2 class="text-3xl font-bold text-gray-800 dark:text-white">تسجيل الدخول</h2>
                    <p class="text-gray-600 dark:text-gray-300 mt-2">أدخل بياناتك للوصول إلى لوحة التحكم</p>
                </div>
                
                <form class="mt-8 space-y-6">
                    <div>
                        <label for="email" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">البريد الإلكتروني</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <i class="fas fa-envelope text-gray-400"></i>
                            </div>
                            <input id="email" name="email" type="email" required class="w-full pr-10 form-input rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 px-4 py-3 text-gray-700 dark:text-gray-300 focus:outline-none" placeholder="example@pharmacy.com">
                        </div>
                    </div>
                    
                    <div>
                        <label for="password" class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">كلمة المرور</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <i class="fas fa-lock text-gray-400"></i>
                            </div>
                            <input id="password" name="password" type="password" required class="w-full pr-10 form-input rounded-lg border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 px-4 py-3 text-gray-700 dark:text-gray-300 focus:outline-none" placeholder="••••••••">
                        </div>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <input id="remember-me" name="remember-me" type="checkbox" class="h-4 w-4 rounded border-gray-300 text-blue-600 focus:ring-blue-500">
                            <label for="remember-me" class="mr-2 block text-sm text-gray-700 dark:text-gray-300">تذكرني</label>
                        </div>
                        <div class="text-sm">
                            <a href="#" class="font-medium text-blue-600 dark:text-blue-400 hover:text-blue-500">نسيت كلمة المرور؟</a>
                        </div>
                    </div>
                    
                    <div>
                        <button type="submit" class="w-full btn-primary flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-sm text-sm font-medium text-white hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition duration-300">
                            تسجيل الدخول
                        </button>
                    </div>
                </form>
                
                <div class="mt-6 text-center">
                    <p class="text-sm text-gray-600 dark:text-gray-400">
                        ليس لديك حساب؟ 
                        <a href="#" class="font-medium text-blue-600 dark:text-blue-400 hover:text-blue-500">سجل الآن</a>
                    </p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Dark Mode Toggle
        const themeToggle = document.getElementById('theme-toggle');
        const themeIcon = document.getElementById('theme-icon');
        
        // Check for saved user preference or use system preference
        const prefersDarkScheme = window.matchMedia('(prefers-color-scheme: dark)');
        const currentTheme = localStorage.getItem('theme');
        
        if (currentTheme === 'dark' || (!currentTheme && prefersDarkScheme.matches)) {
            document.body.classList.add('dark');
            themeIcon.classList.replace('fa-moon', 'fa-sun');
        }
        
        themeToggle.addEventListener('click', function() {
            document.body.classList.toggle('dark');
            
            if (document.body.classList.contains('dark')) {
                themeIcon.classList.replace('fa-moon', 'fa-sun');
                localStorage.setItem('theme', 'dark');
            } else {
                themeIcon.classList.replace('fa-sun', 'fa-moon');
                localStorage.setItem('theme', 'light');
            }
        });
        
        // Form validation
        document.querySelector('form').addEventListener('submit', function(e) {
            e.preventDefault();
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            // Simple validation
            if (!email || !password) {
                alert('الرجاء إدخال البريد الإلكتروني وكلمة المرور');
                return;
            }
            
            // Here you would normally send data to server
            console.log('Login attempt with:', { email, password });
            
            // Simulate successful login redirect
            alert('تم تسجيل الدخول بنجاح! سيتم توجيهك إلى لوحة التحكم.');
            // window.location.href = 'dashboard.html';
        });
    </script>
</body>
</html>

