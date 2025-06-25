<html dir="rtl" lang="ar">
 <head>
  <meta charset="utf-8"/>
  <meta content="width=device-width, initial-scale=1" name="viewport"/>
  <title>
   نظام إدارة الصيدلية المتكامل
  </title>
  <script src="https://cdn.tailwindcss.com">
  </script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" rel="stylesheet"/>
  <link href="https://fonts.googleapis.com/css2?family=Cairo&amp;display=swap" rel="stylesheet"/>
  <style>
   body {
      font-family: 'Cairo', sans-serif;
    }
    /* Scrollbar for tables */
    .table-scroll {
      overflow-x: auto;
    }
  </style>
 </head>
 <body class="bg-gray-50 min-h-screen flex flex-col">
  <!-- Header -->
  <header class="bg-blue-700 text-white shadow-md">
   <div class="container mx-auto px-4 py-4 flex items-center justify-between">
    <div class="flex items-center space-x-3 rtl:space-x-reverse">
     <img alt="شعار الصيدلية عبارة عن رمز صيدلية مع صليب أخضر" class="rounded-full" height="50" src="https://storage.googleapis.com/a1aa/image/48714b55-0c34-466b-be30-4073640b6e38.jpg" width="50"/>
     <h1 class="text-2xl font-bold">
      نظام إدارة الصيدلية
     </h1>
    </div>
    <nav class="hidden md:flex space-x-6 rtl:space-x-reverse text-lg font-semibold">
     <a class="hover:text-yellow-300 transition" href="#dashboard">
      الرئيسية
     </a>
     <a class="hover:text-yellow-300 transition" href="#medicines">
      الأدوية
     </a>
     <a class="hover:text-yellow-300 transition" href="#sales">
      المبيعات
     </a>
     <a class="hover:text-yellow-300 transition" href="#suppliers">
      الموردون
     </a>
     <a class="hover:text-yellow-300 transition" href="#customers">
      العملاء
     </a>
     <a class="hover:text-yellow-300 transition" href="#reports">
      التقارير
     </a>
     <a class="hover:text-yellow-300 transition" href="#settings">
      الإعدادات
     </a>
    </nav>
    <button aria-label="فتح القائمة" class="md:hidden text-white text-2xl focus:outline-none" id="menu-btn">
     <i class="fas fa-bars">
     </i>
    </button>
   </div>
   <!-- Mobile menu -->
   <nav class="hidden bg-blue-600 text-white px-4 py-3 space-y-2 text-lg font-semibold md:hidden" id="mobile-menu">
    <a class="block hover:text-yellow-300 transition" href="#dashboard">
     الرئيسية
    </a>
    <a class="block hover:text-yellow-300 transition" href="#medicines">
     الأدوية
    </a>
    <a class="block hover:text-yellow-300 transition" href="#sales">
     المبيعات
    </a>
    <a class="block hover:text-yellow-300 transition" href="#suppliers">
     الموردون
    </a>
    <a class="block hover:text-yellow-300 transition" href="#customers">
     العملاء
    </a>
    <a class="block hover:text-yellow-300 transition" href="#reports">
     التقارير
    </a>
    <a class="block hover:text-yellow-300 transition" href="#settings">
     الإعدادات
    </a>
   </nav>
  </header>
  <main class="flex-grow container mx-auto px-4 py-6 space-y-10">
   <!-- Dashboard Section -->
   <section class="space-y-6" id="dashboard">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     لوحة التحكم
    </h2>
    <div class="grid grid-cols-1 md:grid-cols-4 gap-6">
     <div class="bg-white rounded-lg shadow p-5 flex items-center space-x-4 rtl:space-x-reverse">
      <div class="bg-green-100 text-green-700 p-3 rounded-full">
       <i class="fas fa-pills fa-2x">
       </i>
      </div>
      <div>
       <p class="text-gray-500">
        عدد الأدوية
       </p>
       <p class="text-2xl font-bold" id="total-medicines">
        0
       </p>
      </div>
     </div>
     <div class="bg-white rounded-lg shadow p-5 flex items-center space-x-4 rtl:space-x-reverse">
      <div class="bg-yellow-100 text-yellow-700 p-3 rounded-full">
       <i class="fas fa-shopping-cart fa-2x">
       </i>
      </div>
      <div>
       <p class="text-gray-500">
        عدد المبيعات اليوم
       </p>
       <p class="text-2xl font-bold" id="total-sales">
        0
       </p>
      </div>
     </div>
     <div class="bg-white rounded-lg shadow p-5 flex items-center space-x-4 rtl:space-x-reverse">
      <div class="bg-blue-100 text-blue-700 p-3 rounded-full">
       <i class="fas fa-truck fa-2x">
       </i>
      </div>
      <div>
       <p class="text-gray-500">
        عدد الموردين
       </p>
       <p class="text-2xl font-bold" id="total-suppliers">
        0
       </p>
      </div>
     </div>
     <div class="bg-white rounded-lg shadow p-5 flex items-center space-x-4 rtl:space-x-reverse">
      <div class="bg-red-100 text-red-700 p-3 rounded-full">
       <i class="fas fa-users fa-2x">
       </i>
      </div>
      <div>
       <p class="text-gray-500">
        عدد العملاء
       </p>
       <p class="text-2xl font-bold" id="total-customers">
        0
       </p>
      </div>
     </div>
    </div>
   </section>
   <!-- Medicines Section -->
   <section class="space-y-6" id="medicines">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     إدارة الأدوية
    </h2>
    <div class="flex flex-col md:flex-row md:items-center md:justify-between space-y-4 md:space-y-0">
     <button class="bg-green-600 hover:bg-green-700 text-white px-5 py-2 rounded shadow transition" id="add-medicine-btn">
      <i class="fas fa-plus ml-2 rtl:ml-0 rtl:mr-2">
      </i>
      إضافة دواء جديد
     </button>
     <input class="border border-gray-300 rounded px-4 py-2 w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" id="search-medicine" placeholder="ابحث عن دواء..." type="text"/>
    </div>
    <div class="table-scroll">
     <table class="min-w-full bg-white rounded shadow overflow-hidden">
      <thead class="bg-blue-700 text-white text-right">
       <tr>
        <th class="py-3 px-4">
         اسم الدواء
        </th>
        <th class="py-3 px-4">
         الكمية
        </th>
        <th class="py-3 px-4">
         سعر الوحدة (ر.س)
        </th>
        <th class="py-3 px-4">
         تاريخ الانتهاء
        </th>
        <th class="py-3 px-4">
         المورد
        </th>
        <th class="py-3 px-4">
         الإجراءات
        </th>
       </tr>
      </thead>
      <tbody class="text-right" id="medicines-table-body">
       <!-- Medicines rows will be inserted here dynamically -->
      </tbody>
     </table>
    </div>
   </section>
   <!-- Sales Section -->
   <section class="space-y-6" id="sales">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     إدارة المبيعات
    </h2>
    <div class="flex flex-col md:flex-row md:items-center md:justify-between space-y-4 md:space-y-0">
     <button class="bg-yellow-600 hover:bg-yellow-700 text-white px-5 py-2 rounded shadow transition" id="add-sale-btn">
      <i class="fas fa-plus ml-2 rtl:ml-0 rtl:mr-2">
      </i>
      إضافة بيع جديد
     </button>
     <input class="border border-gray-300 rounded px-4 py-2 w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" id="search-sale" placeholder="ابحث عن بيع..." type="text"/>
    </div>
    <div class="table-scroll">
     <table class="min-w-full bg-white rounded shadow overflow-hidden">
      <thead class="bg-yellow-700 text-white text-right">
       <tr>
        <th class="py-3 px-4">
         رقم البيع
        </th>
        <th class="py-3 px-4">
         اسم الدواء
        </th>
        <th class="py-3 px-4">
         الكمية
        </th>
        <th class="py-3 px-4">
         السعر الإجمالي (ر.س)
        </th>
        <th class="py-3 px-4">
         تاريخ البيع
        </th>
        <th class="py-3 px-4">
         الإجراءات
        </th>
       </tr>
      </thead>
      <tbody class="text-right" id="sales-table-body">
       <!-- Sales rows will be inserted here dynamically -->
      </tbody>
     </table>
    </div>
   </section>
   <!-- Suppliers Section -->
   <section class="space-y-6" id="suppliers">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     إدارة الموردين
    </h2>
    <div class="flex flex-col md:flex-row md:items-center md:justify-between space-y-4 md:space-y-0">
     <button class="bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded shadow transition" id="add-supplier-btn">
      <i class="fas fa-plus ml-2 rtl:ml-0 rtl:mr-2">
      </i>
      إضافة مورد جديد
     </button>
     <input class="border border-gray-300 rounded px-4 py-2 w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" id="search-supplier" placeholder="ابحث عن مورد..." type="text"/>
    </div>
    <div class="table-scroll">
     <table class="min-w-full bg-white rounded shadow overflow-hidden">
      <thead class="bg-blue-900 text-white text-right">
       <tr>
        <th class="py-3 px-4">
         اسم المورد
        </th>
        <th class="py-3 px-4">
         رقم الهاتف
        </th>
        <th class="py-3 px-4">
         البريد الإلكتروني
        </th>
        <th class="py-3 px-4">
         العنوان
        </th>
        <th class="py-3 px-4">
         الإجراءات
        </th>
       </tr>
      </thead>
      <tbody class="text-right" id="suppliers-table-body">
       <!-- Suppliers rows will be inserted here dynamically -->
      </tbody>
     </table>
    </div>
   </section>
   <!-- Customers Section -->
   <section class="space-y-6" id="customers">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     إدارة العملاء
    </h2>
    <div class="flex flex-col md:flex-row md:items-center md:justify-between space-y-4 md:space-y-0">
     <button class="bg-red-600 hover:bg-red-700 text-white px-5 py-2 rounded shadow transition" id="add-customer-btn">
      <i class="fas fa-plus ml-2 rtl:ml-0 rtl:mr-2">
      </i>
      إضافة عميل جديد
     </button>
     <input class="border border-gray-300 rounded px-4 py-2 w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" id="search-customer" placeholder="ابحث عن عميل..." type="text"/>
    </div>
    <div class="table-scroll">
     <table class="min-w-full bg-white rounded shadow overflow-hidden">
      <thead class="bg-red-700 text-white text-right">
       <tr>
        <th class="py-3 px-4">
         اسم العميل
        </th>
        <th class="py-3 px-4">
         رقم الهاتف
        </th>
        <th class="py-3 px-4">
         البريد الإلكتروني
        </th>
        <th class="py-3 px-4">
         العنوان
        </th>
        <th class="py-3 px-4">
         الإجراءات
        </th>
       </tr>
      </thead>
      <tbody class="text-right" id="customers-table-body">
       <!-- Customers rows will be inserted here dynamically -->
      </tbody>
     </table>
    </div>
   </section>
   <!-- Reports Section -->
   <section class="space-y-6" id="reports">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     التقارير
    </h2>
    <div class="bg-white rounded shadow p-6 space-y-6">
     <div class="flex flex-col md:flex-row md:items-center md:space-x-6 rtl:space-x-reverse">
      <label class="font-semibold text-lg mb-2 md:mb-0" for="report-type">
       نوع التقرير:
      </label>
      <select class="border border-gray-300 rounded px-4 py-2 w-full md:w-64 focus:outline-none focus:ring-2 focus:ring-blue-500" id="report-type">
       <option value="sales">
        تقرير المبيعات
       </option>
       <option value="inventory">
        تقرير المخزون
       </option>
       <option value="suppliers">
        تقرير الموردين
       </option>
       <option value="customers">
        تقرير العملاء
       </option>
      </select>
      <button class="bg-blue-700 hover:bg-blue-800 text-white px-5 py-2 rounded shadow mt-4 md:mt-0" id="generate-report-btn">
       عرض التقرير
      </button>
     </div>
     <div class="overflow-x-auto text-right" id="report-output">
     </div>
    </div>
   </section>
   <!-- Settings Section -->
   <section class="space-y-6" id="settings">
    <h2 class="text-3xl font-bold text-blue-700 mb-4">
     الإعدادات
    </h2>
    <div class="bg-white rounded shadow p-6 space-y-6 max-w-3xl">
     <form class="space-y-6" id="settings-form">
      <div>
       <label class="block font-semibold mb-1" for="pharmacy-name">
        اسم الصيدلية
       </label>
       <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="pharmacy-name" placeholder="أدخل اسم الصيدلية" required="" type="text"/>
      </div>
      <div>
       <label class="block font-semibold mb-1" for="pharmacy-address">
        عنوان الصيدلية
       </label>
       <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="pharmacy-address" placeholder="أدخل عنوان الصيدلية" required="" type="text"/>
      </div>
      <div>
       <label class="block font-semibold mb-1" for="pharmacy-phone">
        رقم الهاتف
       </label>
       <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="pharmacy-phone" placeholder="أدخل رقم الهاتف" required="" type="tel"/>
      </div>
      <div>
       <label class="block font-semibold mb-1" for="pharmacy-email">
        البريد الإلكتروني
       </label>
       <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="pharmacy-email" placeholder="أدخل البريد الإلكتروني" required="" type="email"/>
      </div>
      <button class="bg-blue-700 hover:bg-blue-800 text-white px-6 py-2 rounded shadow transition" type="submit">
       حفظ الإعدادات
      </button>
     </form>
    </div>
   </section>
  </main>
  <!-- Footer -->
  <footer class="bg-blue-700 text-white text-center py-4">
   <p>
    © 2024 نظام إدارة الصيدلية. جميع الحقوق محفوظة.
   </p>
  </footer>
  <!-- Modals -->
  <!-- Medicine Modal -->
  <div aria-hidden="true" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 hidden z-50" id="medicine-modal">
   <div class="bg-white rounded-lg shadow-lg max-w-lg w-full p-6 relative">
    <button aria-label="إغلاق" class="absolute top-3 left-3 text-gray-600 hover:text-gray-900 focus:outline-none" id="close-medicine-modal">
     <i class="fas fa-times fa-lg">
     </i>
    </button>
    <h3 class="text-xl font-bold mb-4 text-right">
     إضافة / تعديل دواء
    </h3>
    <form class="space-y-4 text-right" id="medicine-form" novalidate="">
     <div>
      <label class="block font-semibold mb-1" for="medicine-name">
       اسم الدواء
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="medicine-name" required="" type="text"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="medicine-quantity">
       الكمية
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="medicine-quantity" min="0" required="" type="number"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="medicine-price">
       سعر الوحدة (ر.س)
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="medicine-price" min="0" required="" step="0.01" type="number"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="medicine-expiry">
       تاريخ الانتهاء
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="medicine-expiry" required="" type="date"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="medicine-supplier">
       المورد
      </label>
      <select class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500" id="medicine-supplier" required="">
       <!-- Suppliers options will be inserted dynamically -->
      </select>
     </div>
     <div class="flex justify-between">
      <button class="bg-green-600 hover:bg-green-700 text-white px-5 py-2 rounded shadow transition" type="submit">
       حفظ
      </button>
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-5 py-2 rounded shadow transition" id="cancel-medicine-btn" type="button">
       إلغاء
      </button>
     </div>
    </form>
   </div>
  </div>
  <!-- Sale Modal -->
  <div aria-hidden="true" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 hidden z-50" id="sale-modal">
   <div class="bg-white rounded-lg shadow-lg max-w-lg w-full p-6 relative">
    <button aria-label="إغلاق" class="absolute top-3 left-3 text-gray-600 hover:text-gray-900 focus:outline-none" id="close-sale-modal">
     <i class="fas fa-times fa-lg">
     </i>
    </button>
    <h3 class="text-xl font-bold mb-4 text-right">
     إضافة / تعديل بيع
    </h3>
    <form class="space-y-4 text-right" id="sale-form" novalidate="">
     <div>
      <label class="block font-semibold mb-1" for="sale-medicine">
       اسم الدواء
      </label>
      <select class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-500" id="sale-medicine" required="">
       <!-- Medicines options will be inserted dynamically -->
      </select>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="sale-quantity">
       الكمية
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-500" id="sale-quantity" min="1" required="" type="number"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="sale-date">
       تاريخ البيع
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-500" id="sale-date" required="" type="date"/>
     </div>
     <div class="flex justify-between">
      <button class="bg-yellow-600 hover:bg-yellow-700 text-white px-5 py-2 rounded shadow transition" type="submit">
       حفظ
      </button>
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-5 py-2 rounded shadow transition" id="cancel-sale-btn" type="button">
       إلغاء
      </button>
     </div>
    </form>
   </div>
  </div>
  <!-- Supplier Modal -->
  <div aria-hidden="true" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 hidden z-50" id="supplier-modal">
   <div class="bg-white rounded-lg shadow-lg max-w-lg w-full p-6 relative">
    <button aria-label="إغلاق" class="absolute top-3 left-3 text-gray-600 hover:text-gray-900 focus:outline-none" id="close-supplier-modal">
     <i class="fas fa-times fa-lg">
     </i>
    </button>
    <h3 class="text-xl font-bold mb-4 text-right">
     إضافة / تعديل مورد
    </h3>
    <form class="space-y-4 text-right" id="supplier-form" novalidate="">
     <div>
      <label class="block font-semibold mb-1" for="supplier-name">
       اسم المورد
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-700" id="supplier-name" required="" type="text"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="supplier-phone">
       رقم الهاتف
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-700" id="supplier-phone" required="" type="tel"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="supplier-email">
       البريد الإلكتروني
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-700" id="supplier-email" type="email"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="supplier-address">
       العنوان
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-700" id="supplier-address" type="text"/>
     </div>
     <div class="flex justify-between">
      <button class="bg-blue-600 hover:bg-blue-700 text-white px-5 py-2 rounded shadow transition" type="submit">
       حفظ
      </button>
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-5 py-2 rounded shadow transition" id="cancel-supplier-btn" type="button">
       إلغاء
      </button>
     </div>
    </form>
   </div>
  </div>
  <!-- Customer Modal -->
  <div aria-hidden="true" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 hidden z-50" id="customer-modal">
   <div class="bg-white rounded-lg shadow-lg max-w-lg w-full p-6 relative">
    <button aria-label="إغلاق" class="absolute top-3 left-3 text-gray-600 hover:text-gray-900 focus:outline-none" id="close-customer-modal">
     <i class="fas fa-times fa-lg">
     </i>
    </button>
    <h3 class="text-xl font-bold mb-4 text-right">
     إضافة / تعديل عميل
    </h3>
    <form class="space-y-4 text-right" id="customer-form" novalidate="">
     <div>
      <label class="block font-semibold mb-1" for="customer-name">
       اسم العميل
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-red-700" id="customer-name" required="" type="text"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="customer-phone">
       رقم الهاتف
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-red-700" id="customer-phone" required="" type="tel"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="customer-email">
       البريد الإلكتروني
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-red-700" id="customer-email" type="email"/>
     </div>
     <div>
      <label class="block font-semibold mb-1" for="customer-address">
       العنوان
      </label>
      <input class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:ring-2 focus:ring-red-700" id="customer-address" type="text"/>
     </div>
     <div class="flex justify-between">
      <button class="bg-red-600 hover:bg-red-700 text-white px-5 py-2 rounded shadow transition" type="submit">
       حفظ
      </button>
      <button class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-5 py-2 rounded shadow transition" id="cancel-customer-btn" type="button">
       إلغاء
      </button>
     </div>
    </form>
   </div>
  </div>
  <script>
   // Mobile menu toggle
    const menuBtn = document.getElementById('menu-btn');
    const mobileMenu = document.getElementById('mobile-menu');
    menuBtn.addEventListener('click', () => {
      mobileMenu.classList.toggle('hidden');
    });

    // Data storage (simulate database with localStorage)
    const STORAGE_KEYS = {
      medicines: 'pharmacy_medicines',
      sales: 'pharmacy_sales',
      suppliers: 'pharmacy_suppliers',
      customers: 'pharmacy_customers',
      settings: 'pharmacy_settings',
    };

    // Utility functions
    function saveData(key, data) {
      localStorage.setItem(key, JSON.stringify(data));
    }
    function loadData(key) {
      const data = localStorage.getItem(key);
      return data ? JSON.parse(data) : [];
    }
    function loadSettings() {
      const data = localStorage.getItem(STORAGE_KEYS.settings);
      return data ? JSON.parse(data) : {
        pharmacyName: '',
        pharmacyAddress: '',
        pharmacyPhone: '',
        pharmacyEmail: '',
      };
    }
    function saveSettings(settings) {
      localStorage.setItem(STORAGE_KEYS.settings, JSON.stringify(settings));
    }

    // Initialize data arrays
    let medicines = loadData(STORAGE_KEYS.medicines);
    let sales = loadData(STORAGE_KEYS.sales);
    let suppliers = loadData(STORAGE_KEYS.suppliers);
    let customers = loadData(STORAGE_KEYS.customers);
    let settings = loadSettings();

    // Elements references
    const totalMedicinesEl = document.getElementById('total-medicines');
    const totalSalesEl = document.getElementById('total-sales');
    const totalSuppliersEl = document.getElementById('total-suppliers');
    const totalCustomersEl = document.getElementById('total-customers');

    // Tables bodies
    const medicinesTableBody = document.getElementById('medicines-table-body');
    const salesTableBody = document.getElementById('sales-table-body');
    const suppliersTableBody = document.getElementById('suppliers-table-body');
    const customersTableBody = document.getElementById('customers-table-body');

    // Search inputs
    const searchMedicineInput = document.getElementById('search-medicine');
    const searchSaleInput = document.getElementById('search-sale');
    const searchSupplierInput = document.getElementById('search-supplier');
    const searchCustomerInput = document.getElementById('search-customer');

    // Modals and forms
    const medicineModal = document.getElementById('medicine-modal');
    const medicineForm = document.getElementById('medicine-form');
    const medicineSupplierSelect = document.getElementById('medicine-supplier');
    const addMedicineBtn = document.getElementById('add-medicine-btn');
    const closeMedicineModalBtn = document.getElementById('close-medicine-modal');
    const cancelMedicineBtn = document.getElementById('cancel-medicine-btn');

    const saleModal = document.getElementById('sale-modal');
    const saleForm = document.getElementById('sale-form');
    const saleMedicineSelect = document.getElementById('sale-medicine');
    const addSaleBtn = document.getElementById('add-sale-btn');
    const closeSaleModalBtn = document.getElementById('close-sale-modal');
    const cancelSaleBtn = document.getElementById('cancel-sale-btn');

    const supplierModal = document.getElementById('supplier-modal');
    const supplierForm = document.getElementById('supplier-form');
    const addSupplierBtn = document.getElementById('add-supplier-btn');
    const closeSupplierModalBtn = document.getElementById('close-supplier-modal');
    const cancelSupplierBtn = document.getElementById('cancel-supplier-btn');

    const customerModal = document.getElementById('customer-modal');
    const customerForm = document.getElementById('customer-form');
    const addCustomerBtn = document.getElementById('add-customer-btn');
    const closeCustomerModalBtn = document.getElementById('close-customer-modal');
    const cancelCustomerBtn = document.getElementById('cancel-customer-btn');

    const settingsForm = document.getElementById('settings-form');
    const pharmacyNameInput = document.getElementById('pharmacy-name');
    const pharmacyAddressInput = document.getElementById('pharmacy-address');
    const pharmacyPhoneInput = document.getElementById('pharmacy-phone');
    const pharmacyEmailInput = document.getElementById('pharmacy-email');

    const reportTypeSelect = document.getElementById('report-type');
    const generateReportBtn = document.getElementById('generate-report-btn');
    const reportOutput = document.getElementById('report-output');

    // Current editing IDs
    let editingMedicineId = null;
    let editingSaleId = null;
    let editingSupplierId = null;
    let editingCustomerId = null;

    // Generate unique ID
    function generateId() {
      return '_' + Math.random().toString(36).substr(2, 9);
    }

    // Format date to yyyy-mm-dd
    function formatDate(date) {
      const d = new Date(date);
      const month = '' + (d.getMonth() + 1);
      const day = '' + d.getDate();
      const year = d.getFullYear();

      return [year, month.padStart(2, '0'), day.padStart(2, '0')].join('-');
    }

    // Format date to readable Arabic format
    function formatDateArabic(date) {
      const d = new Date(date);
      return d.toLocaleDateString('ar-EG', {
        year: 'numeric',
        month: 'long',
        day: 'numeric',
      });
    }

    // Update dashboard counts
    function updateDashboard() {
      totalMedicinesEl.textContent = medicines.length;
      totalSalesEl.textContent = sales.length;
      totalSuppliersEl.textContent = suppliers.length;
      totalCustomersEl.textContent = customers.length;
    }

    // Render suppliers options for selects
    function renderSuppliersOptions() {
      medicineSupplierSelect.innerHTML = '';
      suppliers.forEach((supplier) => {
        const option = document.createElement('option');
        option.value = supplier.id;
        option.textContent = supplier.name;
        medicineSupplierSelect.appendChild(option);
      });
    }

    // Render medicines options for sales select
    function renderMedicinesOptions() {
      saleMedicineSelect.innerHTML = '';
      medicines.forEach((medicine) => {
        const option = document.createElement('option');
        option.value = medicine.id;
        option.textContent = medicine.name;
        saleMedicineSelect.appendChild(option);
      });
    }

    // Render medicines table
    function renderMedicinesTable(filter = '') {
      medicinesTableBody.innerHTML = '';
      const filtered = medicines.filter((med) =>
        med.name.includes(filter)
      );
      filtered.forEach((med) => {
        const supplier = suppliers.find((s) => s.id === med.supplierId);
        const tr = document.createElement('tr');
        tr.classList.add('border-b', 'hover:bg-gray-100');
        tr.innerHTML = `
          <td class="py-2 px-4">${med.name}</td>
          <td class="py-2 px-4">${med.quantity}</td>
          <td class="py-2 px-4">${med.price.toFixed(2)}</td>
          <td class="py-2 px-4">${formatDateArabic(med.expiry)}</td>
          <td class="py-2 px-4">${supplier ? supplier.name : 'غير معروف'}</td>
          <td class="py-2 px-4 space-x-2 rtl:space-x-reverse">
            <button data-id="${med.id}" class="edit-medicine-btn text-blue-600 hover:text-blue-800" title="تعديل">
              <i class="fas fa-edit"></i>
            </button>
            <button data-id="${med.id}" class="delete-medicine-btn text-red-600 hover:text-red-800" title="حذف">
              <i class="fas fa-trash-alt"></i>
            </button>
          </td>
        `;
        medicinesTableBody.appendChild(tr);
      });
    }

    // Render sales table
    function renderSalesTable(filter = '') {
      salesTableBody.innerHTML = '';
      const filtered = sales.filter((sale) => {
        const medicine = medicines.find((m) => m.id === sale.medicineId);
        return medicine && medicine.name.includes(filter);
      });
      filtered.forEach((sale) => {
        const medicine = medicines.find((m) => m.id === sale.medicineId);
        const totalPrice = medicine ? medicine.price * sale.quantity : 0;
        const tr = document.createElement('tr');
        tr.classList.add('border-b', 'hover:bg-gray-100');
        tr.innerHTML = `
          <td class="py-2 px-4">${sale.id}</td>
          <td class="py-2 px-4">${medicine ? medicine.name : 'غير معروف'}</td>
          <td class="py-2 px-4">${sale.quantity}</td>
          <td class="py-2 px-4">${totalPrice.toFixed(2)}</td>
          <td class="py-2 px-4">${formatDateArabic(sale.date)}</td>
          <td class="py-2 px-4 space-x-2 rtl:space-x-reverse">
            <button data-id="${sale.id}" class="edit-sale-btn text-blue-600 hover:text-blue-800" title="تعديل">
              <i class="fas fa-edit"></i>
            </button>
            <button data-id="${sale.id}" class="delete-sale-btn text-red-600 hover:text-red-800" title="حذف">
              <i class="fas fa-trash-alt"></i>
            </button>
          </td>
        `;
        salesTableBody.appendChild(tr);
      });
    }

    // Render suppliers table
    function renderSuppliersTable(filter = '') {
      suppliersTableBody.innerHTML = '';
      const filtered = suppliers.filter((sup) =>
        sup.name.includes(filter)
      );
      filtered.forEach((sup) => {
        const tr = document.createElement('tr');
        tr.classList.add('border-b', 'hover:bg-gray-100');
        tr.innerHTML = `
          <td class="py-2 px-4">${sup.name}</td>
          <td class="py-2 px-4">${sup.phone}</td>
          <td class="py-2 px-4">${sup.email || '-'}</td>
          <td class="py-2 px-4">${sup.address || '-'}</td>
          <td class="py-2 px-4 space-x-2 rtl:space-x-reverse">
            <button data-id="${sup.id}" class="edit-supplier-btn text-blue-600 hover:text-blue-800" title="تعديل">
              <i class="fas fa-edit"></i>
            </button>
            <button data-id="${sup.id}" class="delete-supplier-btn text-red-600 hover:text-red-800" title="حذف">
              <i class="fas fa-trash-alt"></i>
            </button>
          </td>
        `;
        suppliersTableBody.appendChild(tr);
      });
    }

    // Render customers table
    function renderCustomersTable(filter = '') {
      customersTableBody.innerHTML = '';
      const filtered = customers.filter((cus) =>
        cus.name.includes(filter)
      );
      filtered.forEach((cus) => {
        const tr = document.createElement('tr');
        tr.classList.add('border-b', 'hover:bg-gray-100');
        tr.innerHTML = `
          <td class="py-2 px-4">${cus.name}</td>
          <td class="py-2 px-4">${cus.phone}</td>
          <td class="py-2 px-4">${cus.email || '-'}</td>
          <td class="py-2 px-4">${cus.address || '-'}</td>
          <td class="py-2 px-4 space-x-2 rtl:space-x-reverse">
            <button data-id="${cus.id}" class="edit-customer-btn text-blue-600 hover:text-blue-800" title="تعديل">
              <i class="fas fa-edit"></i>
            </button>
            <button data-id="${cus.id}" class="delete-customer-btn text-red-600 hover:text-red-800" title="حذف">
              <i class="fas fa-trash-alt"></i>
            </button>
          </td>
        `;
        customersTableBody.appendChild(tr);
      });
    }

    // Open modal helper
    function openModal(modal) {
      modal.classList.remove('hidden');
      modal.setAttribute('aria-hidden', 'false');
    }
    // Close modal helper
    function closeModal(modal) {
      modal.classList.add('hidden');
      modal.setAttribute('aria-hidden', 'true');
    }

    // Clear medicine form
    function clearMedicineForm() {
      medicineForm.reset();
      editingMedicineId = null;
    }
    // Clear sale form
    function clearSaleForm() {
      saleForm.reset();
      editingSaleId = null;
    }
    // Clear supplier form
    function clearSupplierForm() {
      supplierForm.reset();
      editingSupplierId = null;
    }
    // Clear customer form
    function clearCustomerForm() {
      customerForm.reset();
      editingCustomerId = null;
    }

    // Add or update medicine
    medicineForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const name = medicineForm['medicine-name'].value.trim();
      const quantity = parseInt(medicineForm['medicine-quantity'].value);
      const price = parseFloat(medicineForm['medicine-price'].value);
      const expiry = medicineForm['medicine-expiry'].value;
      const supplierId = medicineForm['medicine-supplier'].value;

      if (!name || isNaN(quantity) || isNaN(price) || !expiry || !supplierId) {
        alert('يرجى ملء جميع الحقول بشكل صحيح.');
        return;
      }

      if (editingMedicineId) {
        // Update existing
        const medIndex = medicines.findIndex((m) => m.id === editingMedicineId);
        if (medIndex !== -1) {
          medicines[medIndex] = {
            id: editingMedicineId,
            name,
            quantity,
            price,
            expiry,
            supplierId,
          };
        }
      } else {
        // Add new
        medicines.push({
          id: generateId(),
          name,
          quantity,
          price,
          expiry,
          supplierId,
        });
      }
      saveData(STORAGE_KEYS.medicines, medicines);
      updateDashboard();
      renderMedicinesTable(searchMedicineInput.value.trim());
      renderMedicinesOptions();
      closeModal(medicineModal);
      clearMedicineForm();
    });

    // Add or update sale
    saleForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const medicineId = saleForm['sale-medicine'].value;
      const quantity = parseInt(saleForm['sale-quantity'].value);
      const date = saleForm['sale-date'].value;

      if (!medicineId || isNaN(quantity) || quantity < 1 || !date) {
        alert('يرجى ملء جميع الحقول بشكل صحيح.');
        return;
      }

      // Check if enough quantity in stock
      const medicine = medicines.find((m) => m.id === medicineId);
      if (!medicine) {
        alert('الدواء غير موجود.');
        return;
      }
      if (quantity > medicine.quantity) {
        alert('الكمية المطلوبة أكبر من الكمية المتوفرة في المخزون.');
        return;
      }

      if (editingSaleId) {
        // Update existing sale
        const saleIndex = sales.findIndex((s) => s.id === editingSaleId);
        if (saleIndex !== -1) {
          // Restore old quantity to medicine stock first
          const oldSale = sales[saleIndex];
          const oldMedicine = medicines.find((m) => m.id === oldSale.medicineId);
          if (oldMedicine) {
            oldMedicine.quantity += oldSale.quantity;
          }
          // Deduct new quantity
          medicine.quantity -= quantity;
          sales[saleIndex] = {
            id: editingSaleId,
            medicineId,
            quantity,
            date,
          };
        }
      } else {
        // New sale
        medicine.quantity -= quantity;
        sales.push({
          id: generateId(),
          medicineId,
          quantity,
          date,
        });
      }
      saveData(STORAGE_KEYS.medicines, medicines);
      saveData(STORAGE_KEYS.sales, sales);
      updateDashboard();
      renderMedicinesTable(searchMedicineInput.value.trim());
      renderSalesTable(searchSaleInput.value.trim());
      renderMedicinesOptions();
      closeModal(saleModal);
      clearSaleForm();
    });

    // Add or update supplier
    supplierForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const name = supplierForm['supplier-name'].value.trim();
      const phone = supplierForm['supplier-phone'].value.trim();
      const email = supplierForm['supplier-email'].value.trim();
      const address = supplierForm['supplier-address'].value.trim();

      if (!name || !phone) {
        alert('يرجى ملء الحقول المطلوبة.');
        return;
      }

      if (editingSupplierId) {
        const supIndex = suppliers.findIndex((s) => s.id === editingSupplierId);
        if (supIndex !== -1) {
          suppliers[supIndex] = {
            id: editingSupplierId,
            name,
            phone,
            email,
            address,
          };
        }
      } else {
        suppliers.push({
          id: generateId(),
          name,
          phone,
          email,
          address,
        });
      }
      saveData(STORAGE_KEYS.suppliers, suppliers);
      updateDashboard();
      renderSuppliersTable(searchSupplierInput.value.trim());
      renderSuppliersOptions();
      closeModal(supplierModal);
      clearSupplierForm();
    });

    // Add or update customer
    customerForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const name = customerForm['customer-name'].value.trim();
      const phone = customerForm['customer-phone'].value.trim();
      const email = customerForm['customer-email'].value.trim();
      const address = customerForm['customer-address'].value.trim();

      if (!name || !phone) {
        alert('يرجى ملء الحقول المطلوبة.');
        return;
      }

      if (editingCustomerId) {
        const cusIndex = customers.findIndex((c) => c.id === editingCustomerId);
        if (cusIndex !== -1) {
          customers[cusIndex] = {
            id: editingCustomerId,
            name,
            phone,
            email,
            address,
          };
        }
      } else {
        customers.push({
          id: generateId(),
          name,
          phone,
          email,
          address,
        });
      }
      saveData(STORAGE_KEYS.customers, customers);
      updateDashboard();
      renderCustomersTable(searchCustomerInput.value.trim());
      closeModal(customerModal);
      clearCustomerForm();
    });

    // Settings form submit
    settingsForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const pharmacyName = pharmacyNameInput.value.trim();
      const pharmacyAddress = pharmacyAddressInput.value.trim();
      const pharmacyPhone = pharmacyPhoneInput.value.trim();
      const pharmacyEmail = pharmacyEmailInput.value.trim();

      if (!pharmacyName || !pharmacyAddress || !pharmacyPhone || !pharmacyEmail) {
        alert('يرجى ملء جميع الحقول.');
        return;
      }

      settings = {
        pharmacyName,
        pharmacyAddress,
        pharmacyPhone,
        pharmacyEmail,
      };
      saveSettings(settings);
      alert('تم حفظ الإعدادات بنجاح.');
    });

    // Render settings form values
    function renderSettings() {
      pharmacyNameInput.value = settings.pharmacyName || '';
      pharmacyAddressInput.value = settings.pharmacyAddress || '';
      pharmacyPhoneInput.value = settings.pharmacyPhone || '';
      pharmacyEmailInput.value = settings.pharmacyEmail || '';
    }

    // Event delegation for edit and delete buttons in tables
    medicinesTableBody.addEventListener('click', (e) => {
      if (e.target.closest('.edit-medicine-btn')) {
        const id = e.target.closest('.edit-medicine-btn').dataset.id;
        const med = medicines.find((m) => m.id === id);
        if (med) {
          editingMedicineId = id;
          medicineForm['medicine-name'].value = med.name;
          medicineForm['medicine-quantity'].value = med.quantity;
          medicineForm['medicine-price'].value = med.price;
          medicineForm['medicine-expiry'].value = med.expiry;
          medicineForm['medicine-supplier'].value = med.supplierId;
          openModal(medicineModal);
        }
      } else if (e.target.closest('.delete-medicine-btn')) {
        const id = e.target.closest('.delete-medicine-btn').dataset.id;
        if (confirm('هل أنت متأكد من حذف هذا الدواء؟')) {
          medicines = medicines.filter((m) => m.id !== id);
          saveData(STORAGE_KEYS.medicines, medicines);
          updateDashboard();
          renderMedicinesTable(searchMedicineInput.value.trim());
          renderMedicinesOptions();
        }
      }
    });

    salesTableBody.addEventListener('click', (e) => {
      if (e.target.closest('.edit-sale-btn')) {
        const id = e.target.closest('.edit-sale-btn').dataset.id;
        const sale = sales.find((s) => s.id === id);
        if (sale) {
          editingSaleId = id;
          saleForm['sale-medicine'].value = sale.medicineId;
          saleForm['sale-quantity'].value = sale.quantity;
          saleForm['sale-date'].value = sale.date;
          openModal(saleModal);
        }
      } else if (e.target.closest('.delete-sale-btn')) {
        const id = e.target.closest('.delete-sale-btn').dataset.id;
        if (confirm('هل أنت متأكد من حذف هذا البيع؟')) {
          const sale = sales.find((s) => s.id === id);
          if (sale) {
            // Restore quantity to medicine stock
            const medicine = medicines.find((m) => m.id === sale.medicineId);
            if (medicine) {
              medicine.quantity += sale.quantity;
            }
            sales = sales.filter((s) => s.id !== id);
            saveData(STORAGE_KEYS.sales, sales);
            saveData(STORAGE_KEYS.medicines, medicines);
            updateDashboard();
            renderSalesTable(searchSaleInput.value.trim());
            renderMedicinesTable(searchMedicineInput.value.trim());
            renderMedicinesOptions();
          }
        }
      }
    });

    suppliersTableBody.addEventListener('click', (e) => {
      if (e.target.closest('.edit-supplier-btn')) {
        const id = e.target.closest('.edit-supplier-btn').dataset.id;
        const sup = suppliers.find((s) => s.id === id);
        if (sup) {
          editingSupplierId = id;
          supplierForm['supplier-name'].value = sup.name;
          supplierForm['supplier-phone'].value = sup.phone;
          supplierForm['supplier-email'].value = sup.email || '';
          supplierForm['supplier-address'].value = sup.address || '';
          openModal(supplierModal);
        }
      } else if (e.target.closest('.delete-supplier-btn')) {
        const id = e.target.closest('.delete-supplier-btn').dataset.id;
        if (confirm('هل أنت متأكد من حذف هذا المورد؟')) {
          // Check if any medicine uses this supplier
          const used = medicines.some((m) => m.supplierId === id);
          if (used) {
            alert('لا يمكن حذف المورد لأنه مرتبط بأدوية في المخزون.');
            return;
          }
          suppliers = suppliers.filter((s) => s.id !== id);
          saveData(STORAGE_KEYS.suppliers, suppliers);
          updateDashboard();
          renderSuppliersTable(searchSupplierInput.value.trim());
          renderSuppliersOptions();
        }
      }
    });

    customersTableBody.addEventListener('click', (e) => {
      if (e.target.closest('.edit-customer-btn')) {
        const id = e.target.closest('.edit-customer-btn').dataset.id;
        const cus = customers.find((c) => c.id === id);
        if (cus) {
          editingCustomerId = id;
          customerForm['customer-name'].value = cus.name;
          customerForm['customer-phone'].value = cus.phone;
          customerForm['customer-email'].value = cus.email || '';
          customerForm['customer-address'].value = cus.address || '';
          openModal(customerModal);
        }
      } else if (e.target.closest('.delete-customer-btn')) {
        const id = e.target.closest('.delete-customer-btn').dataset.id;
        if (confirm('هل أنت متأكد من حذف هذا العميل؟')) {
          customers = customers.filter((c) => c.id !== id);
          saveData(STORAGE_KEYS.customers, customers);
          updateDashboard();
          renderCustomersTable(searchCustomerInput.value.trim());
        }
      }
    });

    // Search inputs event listeners
    searchMedicineInput.addEventListener('input', () => {
      renderMedicinesTable(searchMedicineInput.value.trim());
    });
    searchSaleInput.addEventListener('input', () => {
      renderSalesTable(searchSaleInput.value.trim());
    });
    searchSupplierInput.addEventListener('input', () => {
      renderSuppliersTable(searchSupplierInput.value.trim());
    });
    searchCustomerInput.addEventListener('input', () => {
      renderCustomersTable(searchCustomerInput.value.trim());
    });

    // Open modals buttons
    addMedicineBtn.addEventListener('click', () => {
      clearMedicineForm();
      renderSuppliersOptions();
      openModal(medicineModal);
    });
    addSaleBtn.addEventListener('click', () => {
      clearSaleForm();
      renderMedicinesOptions();
      openModal(saleModal);
    });
    addSupplierBtn.addEventListener('click', () => {
      clearSupplierForm();
      openModal(supplierModal);
    });
    addCustomerBtn.addEventListener('click', () => {
      clearCustomerForm();
      openModal(customerModal);
    });

    // Close modals buttons
    closeMedicineModalBtn.addEventListener('click', () => {
      closeModal(medicineModal);
      clearMedicineForm();
    });
    cancelMedicineBtn.addEventListener('click', () => {
      closeModal(medicineModal);
      clearMedicineForm();
    });

    closeSaleModalBtn.addEventListener('click', () => {
      closeModal(saleModal);
      clearSaleForm();
    });
    cancelSaleBtn.addEventListener('click', () => {
      closeModal(saleModal);
      clearSaleForm();
    });

    closeSupplierModalBtn.addEventListener('click', () => {
      closeModal(supplierModal);
      clearSupplierForm();
    });
    cancelSupplierBtn.addEventListener('click', () => {
      closeModal(supplierModal);
      clearSupplierForm();
    });

    closeCustomerModalBtn.addEventListener('click', () => {
      closeModal(customerModal);
      clearCustomerForm();
    });
    cancelCustomerBtn.addEventListener('click', () => {
      closeModal(customerModal);
      clearCustomerForm();
    });

    // Close modals on outside click
    [medicineModal, saleModal, supplierModal, customerModal].forEach((modal) => {
      modal.addEventListener('click', (e) => {
        if (e.target === modal) {
          closeModal(modal);
          if (modal === medicineModal) clearMedicineForm();
          else if (modal === saleModal) clearSaleForm();
          else if (modal === supplierModal) clearSupplierForm();
          else if (modal === customerModal) clearCustomerForm();
        }
      });
    });

    // Generate reports
    generateReportBtn.addEventListener('click', () => {
      const type = reportTypeSelect.value;
      reportOutput.innerHTML = '';
      if (type === 'sales') {
        if (sales.length === 0) {
          reportOutput.textContent = 'لا توجد مبيعات لعرضها.';
          return;
        }
        const table = document.createElement('table');
        table.className = 'min-w-full bg-white rounded shadow overflow-hidden text-right';
        const thead = document.createElement('thead');
        thead.className = 'bg-yellow-700 text-white';
        thead.innerHTML = `
          <tr>
            <th class="py-3 px-4">رقم البيع</th>
            <th class="py-3 px-4">اسم الدواء</th>
            <th class="py-3 px-4">الكمية</th>
            <th class="py-3 px-4">السعر الإجمالي (ر.س)</th>
            <th class="py-3 px-4">تاريخ البيع</th>
          </tr>
        `;
        table.appendChild(thead);
        const tbody = document.createElement('tbody');
        sales.forEach((sale) => {
          const medicine = medicines.find((m) => m.id === sale.medicineId);
          const totalPrice = medicine ? medicine.price * sale.quantity : 0;
          const tr = document.createElement('tr');
          tr.classList.add('border-b', 'hover:bg-gray-100');
          tr.innerHTML = `
            <td class="py-2 px-4">${sale.id}</td>
            <td class="py-2 px-4">${medicine ? medicine.name : 'غير معروف'}</td>
            <td class="py-2 px-4">${sale.quantity}</td>
            <td class="py-2 px-4">${totalPrice.toFixed(2)}</td>
            <td class="py-2 px-4">${formatDateArabic(sale.date)}</td>
          `;
          tbody.appendChild(tr);
        });
        table.appendChild(tbody);
        reportOutput.appendChild(table);
      } else if (type === 'inventory') {
        if (medicines.length === 0) {
          reportOutput.textContent = 'لا توجد أدوية في المخزون.';
          return;
        }
        const table = document.createElement('table');
        table.className = 'min-w-full bg-white rounded shadow overflow-hidden text-right';
        const thead = document.createElement('thead');
        thead.className = 'bg-green-700 text-white';
        thead.innerHTML = `
          <tr>
            <th class="py-3 px-4">اسم الدواء</th>
            <th class="py-3 px-4">الكمية المتوفرة</th>
            <th class="py-3 px-4">سعر الوحدة (ر.س)</th>
            <th class="py-3 px-4">تاريخ الانتهاء</th>
            <th class="py-3 px-4">المورد</th>
          </tr>
        `;
        table.appendChild(thead);
        const tbody = document.createElement('tbody');
        medicines.forEach((med) => {
          const supplier = suppliers.find((s) => s.id === med.supplierId);
          const tr = document.createElement('tr');
          tr.classList.add('border-b', 'hover:bg-gray-100');
          tr.innerHTML = `
            <td class="py-2 px-4">${med.name}</td>
            <td class="py-2 px-4">${med.quantity}</td>
            <td class="py-2 px-4">${med.price.toFixed(2)}</td>
            <td class="py-2 px-4">${formatDateArabic(med.expiry)}</td>
            <td class="py-2 px-4">${supplier ? supplier.name : 'غير معروف'}</td>
          `;
          tbody.appendChild(tr);
        });
        table.appendChild(tbody);
        reportOutput.appendChild(table);
      } else if (type === 'suppliers') {
        if (suppliers.length === 0) {
          reportOutput.textContent = 'لا توجد موردين لعرضهم.';
          return;
        }
        const table = document.createElement('table');
        table.className = 'min-w-full bg-white rounded shadow overflow-hidden text-right';
        const thead = document.createElement('thead');
        thead.className = 'bg-blue-900 text-white';
        thead.innerHTML = `
          <tr>
            <th class="py-3 px-4">اسم المورد</th>
            <th class="py-3 px-4">رقم الهاتف</th>
            <th class="py-3 px-4">البريد الإلكتروني</th>
            <th class="py-3 px-4">العنوان</th>
          </tr>
        `;
        table.appendChild(thead);
        const tbody = document.createElement('tbody');
        suppliers.forEach((sup) => {
          const tr = document.createElement('tr');
          tr.classList.add('border-b', 'hover:bg-gray-100');
          tr.innerHTML = `
            <td class="py-2 px-4">${sup.name}</td>
            <td class="py-2 px-4">${sup.phone}</td>
            <td class="py-2 px-4">${sup.email || '-'}</td>
            <td class="py-2 px-4">${sup.address || '-'}</td>
          `;
          tbody.appendChild(tr);
        });
        table.appendChild(tbody);
        reportOutput.appendChild(table);
      } else if (type === 'customers') {
        if (customers.length === 0) {
          reportOutput.textContent = 'لا توجد عملاء لعرضهم.';
          return;
        }
        const table = document.createElement('table');
        table.className = 'min-w-full bg-white rounded shadow overflow-hidden text-right';
        const thead = document.createElement('thead');
        thead.className = 'bg-red-700 text-white';
        thead.innerHTML = `
          <tr>
            <th class="py-3 px-4">اسم العميل</th>
            <th class="py-3 px-4">رقم الهاتف</th>
            <th class="py-3 px-4">البريد الإلكتروني</th>
            <th class="py-3 px-4">العنوان</th>
          </tr>
        `;
        table.appendChild(thead);
        const tbody = document.createElement('tbody');
        customers.forEach((cus) => {
          const tr = document.createElement('tr');
          tr.classList.add('border-b', 'hover:bg-gray-100');
          tr.innerHTML = `
            <td class="py-2 px-4">${cus.name}</td>
            <td class="py-2 px-4">${cus.phone}</td>
            <td class="py-2 px-4">${cus.email || '-'}</td>
            <td class="py-2 px-4">${cus.address || '-'}</td>
          `;
          tbody.appendChild(tr);
        });
        table.appendChild(tbody);
        reportOutput.appendChild(table);
      }
    });

    // Initial render
    updateDashboard();
    renderSuppliersOptions();
    renderMedicinesOptions();
    renderMedicinesTable();
    renderSalesTable();
    renderSuppliersTable();
    renderCustomersTable();
    renderSettings();
  </script>
 </body>
</html>
