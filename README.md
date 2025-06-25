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
            transition: background-color 0.3s ease, color 0.3s ease;
        }
        
        .login-container {
            background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
        }
        
        .dark .login-container {
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%);
        }
        
        .form-input {
            transition: all 0.3s ease;
        }
        
        .form-input:focus {
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.3);
        }
        
        .btn-primary {
            background: linear-gradient(to right, #4f46e5 0%, #7c3aed 100%);
            color: white;
        }
        
        .dark .btn-primary {
            background: linear-gradient(to right, #6366f1 0%, #8b5cf6 100%);
        }
        
        .login-decoration {
            background: rgba(241, 245, 249, 0.8);
        }
        
        .dark .login-decoration {
            background: rgba(15, 23, 42, 0.8);
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
<body class="bg-gray-50 dark:bg-slate-900">
    <div class="min-h-screen flex items-center justify-center p-4 login-container">
        <!-- Dark Mode Toggle -->
        <button id="theme-toggle" class="absolute top-4 right-4 p-2 rounded-full bg-white dark:bg-slate-800 shadow-lg hover:bg-gray-100 dark:hover:bg-slate-700 transition-colors">
            <i id="theme-icon" class="fas fa-moon text-indigo-600 dark:text-yellow-300"></i>
        </button>

        <div class="w-full max-w-4xl bg-white dark:bg-slate-800 rounded-2xl shadow-xl overflow-hidden grid grid-cols-1 md:grid-cols-2 border border-gray-200 dark:border-slate-700">
            <!-- Left Side - Decoration -->
            <div class="hidden md:flex items-center justify-center p-8 login-decoration">
                <div class="text-center">
                    <svg class="w-48 h-48 mx-auto pill-icon" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M8 12H16M12 8V16M20 12C20 16.4183 16.4183 20 12 20C7.58172 20 4 16.4183 4 12C4 7.58172 7.58172 4 12 4C16.4183 4 20 7.58172 20 12Z" 
                              stroke="#4f46e5" stroke-width="2" stroke-linecap="round" class="dark:stroke-indigo-400"/>
                    </svg>
                    <h2 class="text-3xl font-bold text-gray-800 dark:text-slate-100 mt-6">نظام إدارة الصيدلية</h2>
                    <p class="text-gray-600 dark:text-slate-300 mt-2">إدارة كاملة لمخزون وأعمال صيدليتك</p>
                </div>
            </div>
            
            <!-- Right Side - Login Form -->
            <div class="p-8 md:p-12">
                <div class="text-center md:text-right">
                    <h2 class="text-3xl font-bold text-gray-800 dark:text-slate-100">تسجيل الدخول</h2>
                    <p class="text-gray-600 dark:text-slate-300 mt-2">أدخل بياناتك للوصول إلى لوحة التحكم</p>
                </div>
                
                <form class="mt-8 space-y-6">
                    <div>
                        <label for="email" class="block text-sm font-medium text-gray-700 dark:text-slate-300 mb-1">البريد الإلكتروني</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <i class="fas fa-envelope text-gray-500 dark:text-slate-400"></i>
                            </div>
                            <input id="email" name="email" type="email" required 
                                   class="w-full pr-10 form-input rounded-lg border border-gray-300 dark:border-slate-600 bg-white dark:bg-slate-700 px-4 py-3 text-gray-700 dark:text-slate-200 focus:outline-none" 
                                   placeholder="example@pharmacy.com">
                        </div>
                    </div>
                    
                    <div>
                        <label for="password" class="block text-sm font-medium text-gray-700 dark:text-slate-300 mb-1">كلمة المرور</label>
                        <div class="relative">
                            <div class="absolute inset-y-0 right-0 flex items-center pr-3 pointer-events-none">
                                <i class="fas fa-lock text-gray-500 dark:text-slate-400"></i>
                            </div>
                            <input id="password" name="password" type="password" required 
                                   class="w-full pr-10 form-input rounded-lg border border-gray-300 dark:border-slate-600 bg-white dark:bg-slate-700 px-4 py-3 text-gray-700 dark:text-slate-200 focus:outline-none" 
                                   placeholder="••••••••">
                        </div>
                    </div>
                    
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <input id="remember-me" name="remember-me" type="checkbox" 
                                   class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500 dark:border-slate-600 dark:bg-slate-700 dark:checked:bg-indigo-500">
                            <label for="remember-me" class="mr-2 block text-sm text-gray-700 dark:text-slate-300">تذكرني</label>
                        </div>
                        <div class="text-sm">
                            <a href="#" class="font-medium text-indigo-600 dark:text-indigo-400 hover:text-indigo-500">نسيت كلمة المرور؟</a>
                        </div>
                    </div>
                    
                    <div>
                        <button type="submit" class="w-full btn-primary flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-sm text-sm font-medium hover:opacity-90 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 transition duration-300">
                            تسجيل الدخول
                            <i class="fas fa-sign-in-alt mr-2"></i>
                        </button>
                    </div>
                </form>
                
                <div class="mt-6 text-center">
                    <p class="text-sm text-gray-600 dark:text-slate-400">
                        ليس لديك حساب؟ 
                        <a href="#" class="font-medium text-indigo-600 dark:text-indigo-400 hover:text-indigo-500">سجل الآن</a>
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
            
            console.log('Login attempt with:', { email, password });
            alert('تم تسجيل الدخول بنجاح! سيتم توجيهك إلى لوحة التحكم.');
        });
    </script>
</body>
</html>
