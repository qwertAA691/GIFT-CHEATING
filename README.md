<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Gift Script Store</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: black;
      color: white;
      text-align: center;
    }

    .screen {
      display: none;
      height: 100vh;
      width: 100vw;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }

    .active {
      display: flex;
    }

    button {
      padding: 15px 40px;
      margin: 10px;
      font-size: 20px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
    }

    button:hover {
      transform: scale(1.05);
    }

    /* Главное меню */
    #startBtn {
      background-color: white;
      color: black;
      font-size: 24px;
    }

    /* Кнопки меню */
    .menu-btn {
      width: 200px;
      background-color: #222;
      color: white;
    }

    /* Карточки скриптов */
    .scripts {
      display: flex;
      justify-content: center;
      gap: 30px;
      flex-wrap: wrap;
    }

    .card {
      background-color: #222;
      padding: 20px;
      border-radius: 15px;
      width: 250px;
      box-shadow: 0 0 10px rgba(255,255,255,0.2);
    }

    .premium {
      background: linear-gradient(145deg, #FFD700, #FF8C00);
      color: black;
      box-shadow: 0 0 15px gold;
    }

    .card h2 {
      margin-top: 0;
    }

    .price {
      font-size: 18px;
      margin: 15px 0;
    }

    .buy-btn {
      background-color: #008cff;
      color: white;
      font-size: 18px;
    }

    .buy-btn:hover {
      background-color: #0066cc;
    }
  </style>
</head>
<body>

  <!-- Экран 1: Главная -->
  <div id="screen1" class="screen active">
    <button id="startBtn">Начать</button>
  </div>

  <!-- Экран 2: Меню -->
  <div id="screen2" class="screen">
    <button class="menu-btn" onclick="showScreen(3)">Получить скрипт</button>
    <button class="menu-btn" onclick="window.location.href='https://t.me/juilly'">Поддержка</button>
  </div>

  <!-- Экран 3: Скрипты -->
  <div id="screen3" class="screen">
    <div class="scripts">
      <!-- Обычный -->
      <div class="card">
        <h2>Обычный скрипт</h2>
        <div class="price">4000 ⭐ / 5000₽ / $55</div>
        <button class="buy-btn" onclick="window.location.href='https://t.me/arh_h'">Купить</button>
      </div>
      <!-- Премиум -->
      <div class="card premium">
        <h2>Ручной (премиум)</h2>
        <div class="price">12900 ⭐ / 13500₽ / $140</div>
        <button class="buy-btn" onclick="window.location.href='https://t.me/arh_h'">Купить</button>
      </div>
    </div>
  </div>

  <script>
    function showScreen(num) {
      document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
      document.getElementById('screen' + num).classList.add('active');
    }

    document.getElementById('startBtn').addEventListener('click', () => {
      showScreen(2);
    });
  </script>
</body>
</html>
