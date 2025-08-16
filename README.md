<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Gift Script Store</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root{
      --bg:#000; --fg:#fff; --muted:#c9c9c9;
      --card:#121212; --card2:#1a1a1a;
      --accent:linear-gradient(135deg,#9b5cff,#6ac8ff);
      --accent2:linear-gradient(135deg,#ff6a88,#ffb86c);
      --gold:linear-gradient(145deg,#ffe58a,#ffbf00);
      --blue:linear-gradient(135deg,#4facfe,#00f2fe);
      --shadow:0 0 22px rgba(255,255,255,.12);
      --elite:0 0 24px rgba(255,215,0,.35);
      --radius:18px; --t:.32s cubic-bezier(.2,.7,.2,1);
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, "Noto Sans", sans-serif;
      background: radial-gradient(1200px 1200px at 50% 10%, rgba(40,40,40,.35), transparent), var(--bg);
      color:var(--fg);
      -webkit-font-smoothing:antialiased; text-rendering:optimizeLegibility;
    }

    /* Screens */
    .screen{
      display:none; min-height:100dvh;
      padding:32px 20px 40px;
      align-items:center; justify-content:center; flex-direction:column;
      animation: fade .45s ease;
    }
    .active{display:flex}
    @keyframes fade{from{opacity:0; transform:translateY(6px)} to{opacity:1; transform:translateY(0)}}

    /* Buttons / inputs */
    .btn{
      border:none; color:#fff; padding:16px 32px; border-radius:16px; font-size:20px; cursor:pointer;
      transition: transform var(--t), filter var(--t), box-shadow var(--t); box-shadow: var(--shadow); user-select:none
    }
    .btn:active{transform:translateY(1px) scale(.99)}
    .btn-accent{background:var(--accent)}
    .btn-accent2{background:var(--accent2)}
    .btn-blue{background:var(--blue)}
    .btn-gold{background:var(--gold); color:#1b1200; box-shadow:var(--elite)}
    .btn-ghost{background:transparent; border:1px solid #2a2a2a; color:#d0d0d0}
    .btn:hover{filter:brightness(1.06); box-shadow:0 10px 30px rgba(0,0,0,.35)}

    .row{display:flex; gap:14px; flex-wrap:wrap; justify-content:center}
    .container{width:min(1100px,94vw); margin:0 auto}

    .field{
      background:var(--card2); border:1px solid #2a2a2a; color:#fff;
      border-radius:14px; padding:14px 14px; width:min(92vw,360px); font-size:16px;
      box-shadow: inset 0 -40px 80px rgba(255,255,255,.02); outline:none; transition:border var(--t), box-shadow var(--t)
    }
    .field:focus{border-color:#6ac8ff; box-shadow:0 0 0 6px rgba(106,200,255,.08)}

    .grid{display:grid; grid-template-columns:repeat(auto-fit,minmax(280px,1fr)); gap:22px; width:min(1100px,94vw)}
    .card{
      background:linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.008)) , var(--card);
      border:1px solid #2a2a2a; border-radius:24px; padding:22px; text-align:left; position:relative; overflow:hidden; box-shadow:var(--shadow)
    }
    .card h3{margin:0 0 6px}
    .price{margin:12px 0 8px; color:#cfd6ff}
    .note{font-size:13px; color:#bfbfbf}
    .card.premium{
      background: linear-gradient(180deg, rgba(255,255,255,.06), rgba(255,255,255,.02)), #191204;
      border:1px solid rgba(255,215,0,.45); box-shadow:var(--elite)
    }
    .badge{
      position:absolute; top:12px; right:12px; padding:6px 10px; border-radius:999px; font-size:12px;
      background:rgba(255,255,255,.08); border:1px solid #3a3a3a
    }
    .badge.gold{background:rgba(255,215,0,.18); border-color:rgba(255,215,0,.45); color:#ffe8a1}

    .info{
      background: linear-gradient(180deg, rgba(255,255,255,.06), rgba(255,255,255,.02));
      border:1px solid #2a2a2a; border-radius:18px; padding:18px; line-height:1.7; box-shadow:var(--shadow)
    }

    /* Tiny stuff */
    .sp-12{height:12px} .sp-16{height:16px} .sp-24{height:24px} .sp-32{height:32px}
    .center{text-align:center} .sub{opacity:.8; font-size:14px}
    .small{font-size:12px; opacity:.75}
    .link{color:#9bd3ff; text-decoration:underline; cursor:pointer}

    /* Info modal close (bottom-right) */
    .close-fab{
      position:fixed; right:18px; bottom:18px; border-radius:999px; padding:10px 12px; font-weight:700;
      background:#2a2a2a; border:1px solid #3a3a3a; color:#fff; cursor:pointer; opacity:.9; transition:.2s
    }
    .close-fab:hover{opacity:1; transform:translateY(-1px)}
  </style>
</head>
<body>

  <!-- START: только кнопка "Начать" -->
  <section id="screen-start" class="screen active">
    <button class="btn btn-accent2" id="btnStart" style="padding:20px 60px; font-size:26px; border-radius:22px">Начать</button>
  </section>

  <!-- AUTH (показываем только при первом запуске после "Начать") -->
  <section id="screen-auth" class="screen">
    <div class="container center">
      <h2>Вход</h2>
      <div class="sp-16"></div>
      <input class="field" id="lg-usr" placeholder="Логин">
      <input class="field" id="lg-pw1" type="password" placeholder="Пароль">
      <input class="field" id="lg-pw2" type="password" placeholder="Повторите пароль">
      <div class="sp-16"></div>
      <div class="row">
        <button class="btn btn-accent" id="btnAuthGo">Войти</button>
        <button class="btn btn-ghost" id="btnAuthCancel">Отмена</button>
      </div>
      <div class="sp-12"></div>
      <div class="small">Запоминаем вход на этом устройстве. На другом устройстве — введите те же логин и пароль.</div>
    </div>
  </section>

  <!-- MENU: три кнопки + Admin -->
  <section id="screen-menu" class="screen">
    <div class="container center">
      <div class="row">
        <button class="btn btn-blue" style="min-width:240px" id="btnGetScript">Получить скрипт</button>
        <button class="btn btn-accent" style="min-width:240px" id="btnHow">Как это работает</button>
        <button class="btn btn-ghost" style="min-width:240px" id="btnSupport">Поддержка</button>
        <button class="btn btn-accent2" style="min-width:240px" id="btnAdmin">Admin</button>
      </div>
      <div class="sp-16"></div>
      <div class="small">Авторизован: <b id="who"></b> • <span class="link" id="logout">Выйти</span></div>
    </div>
  </section>

  <!-- SCRIPTS: карточки (без упоминаний об оплате до шага оплаты) -->
  <section id="screen-scripts" class="screen">
    <div class="container">
      <h2 class="center">Выбор скрипта</h2>
      <div class="sp-16"></div>
      <div class="grid">
        <article class="card">
          <div class="badge">BASIC</div>
          <h3>Обычный скрипт</h3>
          <div class="price">4000 ⭐ / 5000₽ / $55</div>
          <p>Интервал проверки: <b>10–10000</b> секунд.</p>
          <p class="note">Каждые X секунд скрипт опрашивает магазин подарков и проверяет наличие новых лотов.</p>
          <div class="sp-12"></div>
          <button class="btn btn-blue" onclick="selectPlan('basic')">Выбрать</button>
        </article>

        <article class="card premium">
          <div class="badge gold">PREMIUM</div>
          <h3>Ручной (премиум)</h3>
          <div class="price">12900 ⭐ / 13500₽ / $140</div>
          <p>Интервал проверки: <b>0.1–10000</b> секунд.</p>
          <p style="color:#2e1a00">Максимальная скорость, тонкая настройка, элитные приоритеты.</p>
          <div class="sp-12"></div>
          <button class="btn btn-gold" onclick="selectPlan('premium')">Выбрать</button>
        </article>
      </div>
      <div class="sp-24"></div>
      <div class="center"><button class="btn btn-ghost" onclick="go('screen-menu')">Назад</button></div>
    </div>
  </section>

  <!-- PAYMENT: выбор метода → открываем контакт владельца, затем на активацию -->
  <section id="screen-payment" class="screen">
    <div class="container center">
      <h2>Выберите способ оплаты</h2>
      <div class="sp-16"></div>
      <div class="row">
        <button class="btn btn-accent2" onclick="pay()">₽ Рубли</button>
        <button class="btn btn-blue" onclick="pay()">⭐ Telegram Stars</button>
        <button class="btn btn-accent" onclick="pay()">$ Доллары</button>
      </div>
      <div class="sp-16"></div>
      <div class="small">После выбора будет открыт диалог с владельцем. Затем введите ваш код на следующем шаге.</div>
      <div class="sp-24"></div>
      <button class="btn btn-ghost" onclick="go('screen-scripts')">Назад</button>
    </div>
  </section>

  <!-- ACTIVATE: код сгорает -->
  <section id="screen-activate" class="screen">
    <div class="container center">
      <h2>Ввод кода доступа</h2>
      <div class="sp-16"></div>
      <input class="field" id="code-input" placeholder="Введите код (регистрозависимо)">
      <div class="sp-16"></div>
      <div class="row">
        <button class="btn btn-accent" id="btnActivate">Активировать</button>
        <button class="btn btn-ghost" onclick="go('screen-payment')">Назад</button>
      </div>
      <div class="sp-12"></div>
      <div id="code-result" class="sub"></div>
    </div>
  </section>

  <!-- HOW: длинное объяснение + крестик снизу справа -->
  <section id="screen-how" class="screen">
    <div class="container">
      <h2 class="center">Как это работает</h2>
      <div class="sp-16"></div>
      <div class="info">
        <p><b>“Секунды”</b> — это частота проверок. Если выставить, например, 1 секунду, скрипт <i>каждую секунду</i> опрашивает источник подарков и ловит новые лоты моментально.</p>
        <p><b>Обычный скрипт:</b> проверки от <b>10</b> до <b>10000</b> секунд.</p>
        <p><b>Ручной (премиум):</b> проверки от <b>0.1</b> до <b>10000</b> секунд — максимальная скорость и гибкость.</p>
        <p><b>Почему мы лучшие:</b> молниеносные проверки без лагов, устойчивость к лимитам и аккуратные задержки, стильный интерфейс.</p>
        <p><b>После оплаты</b> вы получаете <b>уникальный код</b>. На странице активации вводите его — и он <b>сгорает</b>, становясь недействительным для всех устройств.</p>
      </div>
      <div class="sp-24"></div>
      <div class="center"><button class="btn btn-ghost" onclick="go('screen-menu')">Назад</button></div>
    </div>
    <button class="close-fab" onclick="go('screen-menu')">✕</button>
  </section>

  <!-- ADMIN: добавление/удаление кодов -->
  <section id="screen-admin" class="screen">
    <div class="container">
      <h2 class="center">Админ-панель</h2>
      <p class="sub center">Управление кодами (добавить / удалить). Доступ по паролю.</p>
      <div class="sp-16"></div>

      <div class="info">
        <h3>Добавить код</h3>
        <div class="sp-12"></div>
        <input class="field" id="newcode" placeholder="Новый код (регистр важен)" style="width:min(92vw,520px)">
        <div class="sp-12"></div>
        <button class="btn btn-accent" id="btnAddCode">Добавить</button>
      </div>

      <div class="sp-24"></div>
      <div class="info">
        <h3>Текущие коды</h3>
        <div class="sp-12"></div>
        <div id="codes-list" class="small"></div>
      </div>

      <div class="sp-24"></div>
      <div class="center">
        <button class="btn btn-ghost" onclick="go('screen-menu')">В меню</button>
      </div>
    </div>
  </section>

  <script>
    // ===== UTIL / STORAGE =====
    const LS_USER = "gss_user";
    const LS_CODES = "gss_codes";
    const LS_ACTIVATED = "gss_activated"; // лог активаций
    const ADMIN_PASS = "Artur!vip";

    function show(id){ document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active')); document.getElementById(id).classList.add('active'); window.scrollTo({top:0, behavior:'smooth'}); }
    function go(id){ show(id); }

    function isAuthed(){ return !!localStorage.getItem(LS_USER); }
    function currentUser(){ return localStorage.getItem(LS_USER) || ""; }
    function logout(){ localStorage.removeItem(LS_USER); show('screen-start'); }
    function initCodes(){
      if(localStorage.getItem(LS_CODES)) return;
      const DEFAULT = [
        "N7q$Dx8Wm2Jk5zVh","K4p!Tg1Yv9Rb6QfX","Z9u@Bn3Lc7Hq2SwE","R8y#Mw5Pa1Tz4VkC",
        "H2v%Qj7Sd6Xn0LpF","Q6f&Uz2Kr8Yc3JoD","M1x*Ta9Ve5Pb7LiW","B5g^Wc4Zn0Ry2HvJ",
        "P3j!Fm7Qx6Ud1KaN","L8n@Hd2Sv5Rb4XqG","C0r#Ky6Tp3Vw9JeM","S7t$Vf1Qn8Lm2DzH",
        "J2w%Pg9Zs4Tb6RxC","E9h&Lm5Cx7Qv1UaK","X4d*Ny3Vq8Hr0JpF","V6k^Ta2Wm5Ys9QeD",
        "G1z!Qb7Xv4Hp6RfM","T5m@Yn3Ld8Kq1VwC","U0p#Rs6Fv2Xy9JeN","Y7s$Wq1Kb5Tn4DxH",
        "A9v%Hp4Qm7Zs2LtG","D3x&Jc8Vr5Wn1KyP","F6n*Ta0Lb2Qy9VeR","W8q^Xh5Sm3Vp7JdN",
        "R1k!Zp6Vw4Qn8TxM","H4m@Lc2Yr7Sb5QvE","N0t#Qy9Kd3Vx6WpF","K7y$Vh1Rm5Tp2DzJ",
        "Z5u%Sn4Qv8Lb1KyW","M2d&Xp9Tr6Vw0JhC","B8g*Yq3Vs7Qn5LpF","P6j^Wm1Rb4Tx9VeD",
        "L0n!Qd7Xv2Hp5RsM","C9r@Ky5Tn8Vw1JxE","S3t#Vf6Qm4Lb7YnH","J5w$Pg2Zs9Tb0RxC",
        "E1h%Lm7Cx5Qv8UaK","X6d&Ny4Vq1Hr9JpF","V8k*Ta3Wm7Ys0QeD","G2z^Qb5Xv9Hp4RfM",
        "T0m!Yn6Ld2Kq8VwC","U4p@Rs1Fv6Xy3JeN","Y9s#Wq5Kb0Tn7DxH","A2v$Hp8Qm1Zs6LtG",
        "D7x%Jc0Vr2Wn9KyP","F9n&Ta4Lb6Qy1VeR","W1q*Xh8Sm5Vp0JdN","R3k^Zp2Vw7Qn5TxM",
        "H6m!Lc9Yr1Sb8QvE","N8t@Qy3Kd5Vx0WpF"
      ];
      localStorage.setItem(LS_CODES, JSON.stringify(DEFAULT));
    }
    function getCodes(){ try{ return JSON.parse(localStorage.getItem(LS_CODES) || "[]"); }catch{ return []; } }
    function setCodes(list){ localStorage.setItem(LS_CODES, JSON.stringify(list)); }

    function burnCode(code){
      const list = getCodes();
      const idx = list.indexOf(code);
      if(idx>=0){
        list.splice(idx,1);
        setCodes(list);
        const used = JSON.parse(localStorage.getItem(LS_ACTIVATED) || "{}");
        used[code] = currentUser() || true;
        localStorage.setItem(LS_ACTIVATED, JSON.stringify(used));
        return true;
      }
      return false;
    }

    // ===== UI HOOKS =====
    const $ = (id)=>document.getElementById(id);

    // Start button
    $('btnStart').onclick = ()=>{
      if(isAuthed()){
        $('who').textContent = currentUser();
        go('screen-menu');
      }else{
        go('screen-auth');
      }
    };

    // Auth actions
    $('btnAuthCancel').onclick = ()=> go('screen-start');
    $('btnAuthGo').onclick = ()=>{
      const u = $('lg-usr').value.trim();
      const p1 = $('lg-pw1').value;
      const p2 = $('lg-pw2').value;
      if(!u || !p1 || !p2) return alert('Заполните все поля.');
      if(p1 !== p2) return alert('Пароли не совпадают.');
      localStorage.setItem(LS_USER, u);
      $('who').textContent = u;
      go('screen-menu');
    };

    // Menu buttons
    $('btnGetScript').onclick = ()=> go('screen-scripts');
    $('btnHow').onclick = ()=> go('screen-how');
    $('btnSupport').onclick = ()=> window.open('https://t.me/juilly','_blank');
    $('logout').onclick = logout;

    $('btnAdmin').onclick = ()=>{
      const p = prompt('Введите пароль администратора:');
      if(p === ADMIN_PASS){
        sessionStorage.setItem('gss_admin_ok','1');
        renderCodes(); // подготовить список
        go('screen-admin');
      }else if(p !== null){
        alert('Неверный пароль');
      }
    };

    // Scripts -> Payment
    function selectPlan(kind){
      sessionStorage.setItem('gss_plan', kind);
      go('screen-payment');
    }
    window.selectPlan = selectPlan;

    // Payment -> open owner chat -> Activate
    function pay(){
      window.open('https://t.me/arh_h','_blank');
      go('screen-activate');
    }
    window.pay = pay;

    // Activation
    $('btnActivate').onclick = ()=>{
      const code = ($('code-input').value || '').trim();
      if(!code){ $('code-result').textContent = 'Введите код.'; return; }
      const list = getCodes();
      if(list.includes(code)){
        burnCode(code);
        $('code-result').innerHTML = '✅ Код принят. Доступ открыт.';
      }else{
        $('code-result').innerHTML = '❌ Неверный или уже использованный код.';
      }
    };

    // Admin panel
    function requireAdmin(){ return sessionStorage.getItem('gss_admin_ok') === '1'; }
    function renderCodes(){
      if(!requireAdmin()) return;
      const list = getCodes();
      const box = $('codes-list');
      if(!list.length){ box.innerHTML = '<i>Кодов нет</i>'; return; }
      box.innerHTML = list.map(c =>
        `<div style="display:flex;align-items:center;gap:10px;margin:6px 0;">
          <code style="background:#1f1f1f;border:1px solid #2a2a2a;padding:6px 8px;border-radius:10px">${c}</code>
          <button class="btn btn-ghost" onclick="delCode('${c.replace(/"/g,'&quot;')}')">Удалить</button>
        </div>`
      ).join('');
    }
    function addCode(){
      if(!requireAdmin()) return;
      const v = ($('newcode').value || '').trim();
      if(!v){ alert('Введите код'); return; }
      const list = getCodes();
      if(list.includes(v)){ alert('Такой код уже существует'); return; }
      list.push(v); setCodes(list); $('newcode').value=''; renderCodes();
    }
    function delCode(code){
      if(!requireAdmin()) return;
      const list = getCodes(); const i = list.indexOf(code);
      if(i>=0){ list.splice(i,1); setCodes(list); renderCodes(); }
    }
    window.addCode = addCode; window.delCode = delCode;

    // Boot
    (function boot(){
      initCodes();
      if(isAuthed()) $('who').textContent = currentUser();
    })();
  </script>
</body>
</html>
