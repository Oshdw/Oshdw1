<!DOCTYPE html><html lang="ar" dir="rtl"><head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>نظام إدارة الصيدلية</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 0;
      transition: background-color 0.3s, color 0.3s;
    }.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 30px;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

input[type="text"],
input[type="number"] {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0056b3;
}

.dark-mode {
  background-color: #121212;
  color: #ffffff;
}

.dark-mode input,
.dark-mode button {
  background-color: #333;
  color: #fff;
  border: 1px solid #555;
}

.message {
  margin-top: 15px;
  padding: 10px;
  border-radius: 5px;
  color: #fff;
}

.success {
  background-color: #4caf50;
}

.calculator {
  margin-top: 30px;
  padding: 15px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.calculator input,
.calculator button {
  margin-top: 5px;
}

  </style>
</head><body>
  <div class="container">
    <h1>نظام إدارة الصيدلية</h1>
    <button onclick="toggleDarkMode()">تبديل الوضع</button><form id="medicineForm">
  <input type="text" id="name" placeholder="اسم الدواء" required>
  <input type="text" id="category" placeholder="الفئة" required>
  <input type="number" id="price" placeholder="السعر" required>
  <input type="number" id="quantity" placeholder="الكمية" required>
  <button type="submit">إضافة الدواء</button>
</form>

<div id="message" class="message" style="display: none;"></div>

<div class="calculator">
  <h3>آلة حاسبة</h3>
  <input type="number" id="num1" placeholder="العدد الأول">
  <input type="number" id="num2" placeholder="العدد الثاني">
  <button onclick="calculate()">احسب</button>
  <div id="result"></div>
</div>

  </div>  <script>
    const form = document.getElementById('medicineForm');
    const message = document.getElementById('message');
    const body = document.body;

    form.addEventListener('submit', function (e) {
      e.preventDefault();
      const name = document.getElementById('name').value;
      const category = document.getElementById('category').value;
      const price = document.getElementById('price').value;
      const quantity = document.getElementById('quantity').value;

      // مثال بسيط للتخزين المحلي (يمكن تعديله لاحقاً لتخزين فعلي)
      console.log(`تمت إضافة الدواء: ${name}, ${category}, ${price}, ${quantity}`);

      message.textContent = 'تمت إضافة الدواء بنجاح';
      message.className = 'message success';
      message.style.display = 'block';

      setTimeout(() => {
        message.style.display = 'none';
      }, 3000);

      form.reset();
    });

    function toggleDarkMode() {
      body.classList.toggle('dark-mode');
    }

    function calculate() {
      const num1 = parseFloat(document.getElementById('num1').value);
      const num2 = parseFloat(document.getElementById('num2').value);
      const resultDiv = document.getElementById('result');

      if (!isNaN(num1) && !isNaN(num2)) {
        resultDiv.textContent = `الناتج: ${num1 + num2}`;
      } else {
        resultDiv.textContent = 'يرجى إدخال عددين صالحين';
      }
    }
  </script></body></html>
