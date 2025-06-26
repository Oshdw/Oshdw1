<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>نظام إدارة الصيدلية المتكامل</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="نظام متكامل لإدارة مخزون الصيدلية مع لوحة تحكم تفاعلية">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    :root {
      --primary-color: #4361ee;
      --primary-hover: #3a56d4;
      --secondary-color: #3f37c9;
      --danger-color: #f72585;
      --success-color: #4cc9f0;
      --warning-color: #f8961e;
      --light-bg: #f8f9fa;
      --dark-bg: #212529;
      --text-light: #f8f9fa;
      --text-dark: #212529;
      --card-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      --transition: all 0.3s ease;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Tajawal', 'Segoe UI', sans-serif;
    }

    body {
      transition: var(--transition);
      min-height: 100vh;
    }

    body.light {
      background-color: var(--light-bg);
      color: var(--text-dark);
    }

    body.dark {
      background-color: var(--dark-bg);
      color: var(--text-light);
    }

    /* Header Styles */
    header {
      background-color: var(--primary-color);
      color: white;
      padding: 1rem;
      text-align: center;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      position: relative;
    }

    body.dark header {
      background-color: #121212;
      border-bottom: 1px solid #333;
    }

    /* Navigation */
    nav {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 2rem;
      background-color: var(--primary-color);
    }

    .nav-links {
      display: flex;
      list-style: none;
    }

    .nav-links li {
      margin-left: 1.5rem;
    }

    .nav-links a {
      color: white;
      text-decoration: none;
      font-weight: 500;
      transition: var(--transition);
      padding: 0.5rem 1rem;
      border-radius: 4px;
    }

    .nav-links a:hover {
      background-color: rgba(255, 255, 255, 0.1);
    }

    /* Main Content */
    main {
      padding: 2rem;
      max-width: 1400px;
      margin: 0 auto;
    }

    /* Theme Toggle */
    #themeToggle {
      position: fixed;
      bottom: 2rem;
      left: 2rem;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      background-color: var(--primary-color);
      color: white;
      border: none;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.2rem;
      z-index: 100;
      box-shadow: var(--card-shadow);
      transition: var(--transition);
    }

    #themeToggle:hover {
      transform: scale(1.1);
    }

    /* Login Page */
    #loginPage {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    }

    body.dark #loginPage {
      background: linear-gradient(135deg, #2c3e50 0%, #1a1a2e 100%);
    }

    #loginBox {
      background: white;
      padding: 2.5rem;
      border-radius: 12px;
      box-shadow: var(--card-shadow);
      width: 100%;
      max-width: 450px;
      text-align: center;
      transition: var(--transition);
    }

    body.dark #loginBox {
      background: #2a2a2a;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    #loginBox h2 {
      margin-bottom: 1.5rem;
      color: var(--primary-color);
    }

    .input-group {
      margin-bottom: 1.5rem;
      text-align: right;
    }

    .input-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
    }

    input {
      padding: 0.75rem 1rem;
      margin-bottom: 1rem;
      width: 100%;
      border: 1px solid #ddd;
      border-radius: 6px;
      font-size: 1rem;
      transition: var(--transition);
    }

    body.dark input {
      background-color: #333;
      color: white;
      border-color: #444;
    }

    input:focus {
      outline: none;
      border-color: var(--primary-color);
      box-shadow: 0 0 0 2px rgba(67, 97, 238, 0.2);
    }

    /* Buttons */
    button {
      padding: 0.75rem 1.5rem;
      background-color: var(--primary-color);
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 500;
      transition: var(--transition);
    }

    button:hover {
      background-color: var(--primary-hover);
      transform: translateY(-2px);
    }

    button:active {
      transform: translateY(0);
    }

    button.secondary {
      background-color: #6c757d;
    }

    button.danger {
      background-color: var(--danger-color);
    }

    button.success {
      background-color: var(--success-color);
    }

    button.warning {
      background-color: var(--warning-color);
    }

    /* Dashboard */
    #dashboardPage .cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1.5rem;
      margin-top: 2rem;
    }

    .card {
      background: white;
      padding: 1.5rem;
      border-radius: 12px;
      box-shadow: var(--card-shadow);
      text-align: center;
      transition: var(--transition);
    }

    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
    }

    body.dark .card {
      background: #2a2a2a;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    }

    .card h3 {
      color: var(--primary-color);
      margin-bottom: 1rem;
    }

    .card .value {
      font-size: 2rem;
      font-weight: bold;
      margin: 0.5rem 0;
    }

    .card .description {
      color: #6c757d;
      font-size: 0.9rem;
    }

    body.dark .card .description {
      color: #aaa;
    }

    /* Quick Actions */
    .quick-actions {
      display: flex;
      justify-content: center;
      gap: 1rem;
      margin: 2rem 0;
      flex-wrap: wrap;
    }

    /* Medicines Page */
    #medicinesPage .search-container {
      display: flex;
      gap: 1rem;
      margin-bottom: 1.5rem;
      flex-wrap: wrap;
    }

    #medicinesPage input[type="text"] {
      flex: 1;
      min-width: 250px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      box-shadow: var(--card-shadow);
      border-radius: 8px;
      overflow: hidden;
    }

    body.dark table {
      background: #2a2a2a;
    }

    th, td {
      padding: 1rem;
      border: 1px solid #ddd;
      text-align: center;
    }

    body.dark th, body.dark td {
      border-color: #444;
    }

    th {
      background-color: var(--primary-color);
      color: white;
      font-weight: 500;
    }

    body.dark th {
      background-color: #1a1a2e;
    }

    tr:nth-child(even) {
      background-color: #f8f9fa;
    }

    body.dark tr:nth-child(even) {
      background-color: #333;
    }

    tr:hover {
      background-color: #e9ecef;
    }

    body.dark tr:hover {
      background-color: #3a3a3a;
    }

    .action-buttons {
      display: flex;
      gap: 0.5rem;
      justify-content: center;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      z-index: 1000;
      justify-content: center;
      align-items: center;
    }

    .modal-content {
      background-color: white;
      padding: 2rem;
      border-radius: 8px;
      width: 90%;
      max-width: 500px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
    }

    body.dark .modal-content {
      background-color: #2a2a2a;
    }

    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1.5rem;
    }

    .modal-header h3 {
      color: var(--primary-color);
    }

    .close-modal {
      background: none;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
      color: #6c757d;
    }

    .form-group {
      margin-bottom: 1.5rem;
    }

    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
    }

    .modal-footer {
      display: flex;
      justify-content: flex-end;
      gap: 1rem;
      margin-top: 2rem;
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      nav {
        flex-direction: column;
        padding: 1rem;
      }

      .nav-links {
        margin-top: 1rem;
        width: 100%;
        justify-content: space-around;
      }

      .nav-links li {
        margin-left: 0;
      }

      main {
        padding: 1rem;
      }

      table {
        display: block;
        overflow-x: auto;
      }
    }

    /* Animations */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .fade-in {
      animation: fadeIn 0.5s ease forwards;
    }

    /* Utility Classes */
    .hidden {
      display: none !important;
    }

    .text-center {
      text-align: center;
    }

    .mt-1 { margin-top: 0.5rem; }
    .mt-2 { margin-top: 1rem; }
    .mt-3 { margin-top: 1.5rem; }
    .mt-4 { margin-top: 2rem; }
    .mt-5 { margin-top: 3rem; }

    .mb-1 { margin-bottom: 0.5rem; }
    .mb-2 { margin-bottom: 1rem; }
    .mb-3 { margin-bottom: 1.5rem; }
    .mb-4 { margin-bottom: 2rem; }
    .mb-5 { margin-bottom: 3rem; }

    .flex {
      display: flex;
    }

    .justify-between {
      justify-content: space-between;
    }

    .items-center {
      align-items: center;
    }

    /* Notifications */
    .notification {
      position: fixed;
      top: 1rem;
      right: 1rem;
      padding: 1rem 1.5rem;
      border-radius: 8px;
      color: white;
      box-shadow: var(--card-shadow);
      z-index: 1000;
      transform: translateX(200%);
      transition: transform 0.3s ease;
    }

    .notification.show {
      transform: translateX(0);
    }

    .notification.success {
      background-color: var(--success-color);
    }

    .notification.error {
      background-color: var(--danger-color);
    }

    .notification.warning {
      background-color: var(--warning-color);
    }

    /* Loading Spinner */
    .spinner {
      display: inline-block;
      width: 20px;
      height: 20px;
      border: 3px solid rgba(255,255,255,.3);
      border-radius: 50%;
      border-top-color: white;
      animation: spin 1s ease-in-out infinite;
    }

    @keyframes spin {
      to { transform: rotate(360deg);
