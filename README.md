<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>نظام إدارة الصيدلية الاحترافي</title>
<style>
  /* ========== Reset & Base Styles ========== */
  * {
    box-sizing: border-box;
  }
  body, html {
    margin: 0; padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f5f9ff;
    color: #003366;
    height: 100vh;
    overflow: hidden;
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark {
    background-color: #001f4d;
    color: #cce0ff;
  }
  a {
    text-decoration: none;
    color: inherit;
  }

  /* ========== Container ========== */
  #container {
    display: flex;
    height: 100vh;
  }

  /* ========== Sidebar ========== */
  #sidebar {
    background-color: #004080;
    width: 260px;
    display: flex;
    flex-direction: column;
    padding: 1rem 0;
    color: white;
    transition: width 0.3s;
  }
  #sidebar.collapsed {
    width: 70px;
  }
  #sidebar h2 {
    margin: 0 0 1.5rem 0;
    text-align: center;
    font-weight: 700;
    letter-spacing: 3px;
  }
  #sidebar.collapsed h2 {
    display: none;
  }
  #sidebar nav {
    flex-grow: 1;
  }
  #sidebar ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  #sidebar ul li {
    cursor: pointer;
    padding: 1rem 1.5rem;
    display: flex;
    align-items: center;
    gap: 14px;
    font-size: 1.1rem;
    border-left: 4px solid transparent;
    transition: background-color 0.3s, border-color 0.3s;
    white-space: nowrap;
  }
  #sidebar ul li.active, #sidebar ul li:hover {
    background-color: #3366cc;
    border-left-color: #ffcc00;
  }
  #sidebar.collapsed ul li {
    justify-content: center;
    font-size: 1.6rem;
  }
  #sidebar ul li span.icon {
    width: 28px;
    text-align: center;
  }

  #toggleSidebar {
    background: none;
    border: none;
    color: white;
    font-size: 2rem;
    cursor: pointer;
    padding: 0.5rem 1rem;
    margin: 0 1rem 1rem 1rem;
    align-self: flex-end;
    transition: transform 0.3s;
  }
  #toggleSidebar.collapsed {
    transform: rotate(180deg);
  }

  #darkModeToggle {
    background-color: transparent;
    border: 2px solid white;
    color: white;
    border-radius: 20px;
    margin: 1rem auto 0 auto;
    padding: 0.3rem 1.2rem;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.3s, color 0.3s;
  }
  #darkModeToggle:hover {
    background-color: white;
    color: #004080;
  }
  #sidebar.collapsed #darkModeToggle {
    margin: 1rem auto;
  }

  #logoutBtn {
    background: transparent;
    border: 2px solid white;
    margin: 1rem auto 0 auto;
    width: 80%;
    padding: 0.5rem 0;
    font-size: 1rem;
    border-radius: 6px;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s;
    user-select: none;
  }
  #logoutBtn:hover {
    background-color: white;
    color: #004080;
  }

  /* ========== Main Content ========== */
  #mainContent {
    flex-grow: 1;
    background-color: white;
    overflow-y: auto;
    padding: 2rem 3rem;
    transition: background-color 0.3s, color 0.3s;
  }
  body.dark #mainContent {
    background-color: #003366;
  }
  h1, h2 {
    margin-top: 0;
  }

  /* ========== Buttons ========== */
  button {
    cursor: pointer;
    background-color: #004080;
    border: none;
    border-radius: 6px;
    color: white;
    padding: 0.6rem 1.3rem;
    font-size: 1rem;
    transition: background-color 0.3s;
  }
  button:hover {
    background-color: #003060;
  }
  button:disabled {
    background-color: #999;
    cursor: not-allowed;
  }

  /* ========== Inputs ========== */
  input[type="text"], input[type="number"], input[type="date"], input[type="password"] {
    padding: 0.6rem;
    font-size: 1rem;
    border-radius: 6px;
    border: 1px solid #004080;
    width: 100%;
    margin-bottom: 1rem;
    transition: border-color 0.3s;
  }
  input:focus {
    border-color: #ffcc00;
    outline: none;
  }

  /* ========== Tables ========== */
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 1rem;
  }
  th, td {
    border: 1px solid #004080;
    padding: 0.8rem;
    text-align: center;
    transition: background-color 0.3s, color 0.3s;
  }
  th {
    background-color: #cce0ff;
  }
  body.dark th {
    background-color: #002244;
    color: #cce0ff;
  }
  body.dark td {
    background-color: #001833;
    color: #aaddff;
  }
  .table-btn {
    margin: 0 3px;
    padding: 0.3rem 0.7rem;
    font-size: 0.9rem;
    border-radius: 5px;
    border: none;
    color: white;
    transition: background-color 0.3s;
  }
  .edit-btn {
    background-color: #ffcc00;
    color: #003366;
  }
  .edit-btn:hover {
    background-color: #e6b800;
  }
  .delete-btn {
    background-color: #cc0000;
  }
  .delete-btn:hover {
    background-color: #990000;
  }

  /* ========== Pages ========== */
  .page {
    display: none;
  }
  .page.active {
    display: block;
  }

  /* ========== Alerts ========== */
  .alert {
    background-color: #cc0000;
    color: white;
    padding: 0.7rem 1.2rem;
    border-radius: 8px;
    margin-bottom: 1rem;
    font-weight: 700;
    animation: pulse 1.5s infinite alternate;
  }
  @keyframes pulse {
    0% {opacity: 1;}
    100% {opacity: 0.7;}
  }

  /* ========== Calculator ========== */
  #calculator {
    max-width: 360px;
    margin: 2rem auto;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 6px 16px rgba(0,0,0,0.12);
    background: #004080;
    color: white;
  }
  #calcDisplay {
    background: rgba(0,0,0,0.2);
    font-size: 2rem;
    padding: 1.2rem;
    text-align: right;
    letter-spacing: 1.2px;
    user-select: none;
    height: 60px;
    line-height: 60px;
    overflow-x: auto;
    white-space: nowrap;
  }
  #calcButtons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 0.4rem;
    padding: 1rem;
    background: #002b66;
  }
  #calcButtons button {
    background: #003f99;
    border-radius: 8px;
    border: none;
    font-size: 1.5rem;
    color: white;
    transition: background-color 0.3s;
  }
  #calcButtons button:hover {
    background: #002a66;
  }
  #calcButtons button.operator {
    background: #ffcc00;
    color: #003366;
  }
  #calcButtons button.operator:hover {
    background: #e6b800;
  }
  #calcButtons button.zero {
    grid-column: span 2;
  }

  /* ========== Responsive ========== */
  @media (max-width: 768px) {
    #container {
      flex-direction: column;
    }
    #sidebar {
      width: 100%;
      height: auto;
      flex-direction: row;
      overflow-x: auto;
    }
    #sidebar.collapsed {
      width: 100%;
    }
    #sidebar nav ul {
      display: flex;
      gap: 1rem;
      justify-content: center;
    }
