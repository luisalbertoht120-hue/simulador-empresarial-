<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simulador Empresarial - Juego de Estrategia</title>

  <!-- Estilos básicos -->
  <style>
    body, html {
      margin: 0; padding: 0; height: 100%;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #121722;
      color: #eee;
      overflow: hidden;
      user-select: none;
    }

    /* --- Pantalla de inicio --- */
    .start-screen {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      background: #202a40;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      color: #eee;
      z-index: 10000;
      text-align: center;
      padding: 20px;
    }
    .start-screen h1 {
      font-size: 3em;
      margin-bottom: 30px;
      text-shadow: 0 0 8px #42a5f5;
    }
    .start-screen button {
      padding: 15px 50px;
      font-size: 1.6em;
      background-color: #42a5f5;
      border: none;
      border-radius: 10px;
      color: white;
      cursor: pointer;
      transition: background-color 0.3s ease;
      box-shadow: 0 0 10px #42a5f5;
    }
    .start-screen button:hover {
      background-color: #1e88e5;
      box-shadow: 0 0 20px #1e88e5;
    }

    /* --- Contenedor del juego --- */
    #gameContainer {
      position: relative;
      height: 100vh;
      width: 100vw;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    /* --- Fondo y partículas --- */
    #particles {
      position: fixed;
      top: 0; left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 0;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
    }
    .particle {
      position: absolute;
      border-radius: 50%;
      background-color: #42a5f5;
      opacity: 0.25;
      pointer-events: none;
      will-change: transform;
    }

    /* --- Header --- */
    header {
      padding: 15px 30px;
      background: #1e2a47;
      box-shadow: 0 2px 5px rgba(0,0,0,0.7);
      z-index: 10;
      color: #a1c4fd;
      font-weight: bold;
      font-size: 1.3em;
      text-align: center;
    }

    /* --- Estado juego --- */
    #status {
      position: fixed;
      top: 80px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(66, 165, 245, 0.9);
      color: #fff;
      padding: 8px 15px;
      border-radius: 25px;
      font-weight: 600;
      z-index: 20;
      opacity: 0;
      transition: opacity 0.3s ease;
      pointer-events: none;
    }
    #status.pop {
      opacity: 1;
      animation: pop 0.7s ease forwards;
    }
    @keyframes pop {
      0% { transform: translate(-50%, -10px) scale(0.6); opacity: 0; }
      50% { transform: translate(-50%, 0) scale(1.1); opacity: 1; }
      100% { transform: translate(-50%, -5px) scale(1); opacity: 0; }
    }

    /* --- Contenido principal --- */
    main {
      flex-grow: 1;
      display: flex;
      padding: 20px;
      gap: 20px;
      color: #ddd;
      z-index: 10;
      overflow: auto;
    }

    /* --- Panel izquierdo con info y botones --- */
    #infoPanel {
      flex: 1 1 350px;
      background: #1a273f;
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 0 15px #42a5f5aa;
      display: flex;
      flex-direction: column;
      gap: 15px;
    }
    #infoPanel h2 {
      margin-top: 0;
      color: #42a5f5;
      text-shadow: 0 0 6px #42a5f5;
    }
    .stat {
      font-size: 1.2em;
      font-weight: 600;
      margin-bottom: 5px;
    }
    .bar {
      height: 18px;
      background: #1e2a47;
      border-radius: 9px;
      overflow: hidden;
      margin-bottom: 15px;
    }
    .bar-inner {
      height: 100%;
      background: linear-gradient(90deg, #42a5f5, #80d0ff);
      transition: width 0.5s ease;
    }
    .btn-group {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }
    button {
      flex: 1 1 45%;
      padding: 12px 10px;
      background-color: #42a5f5;
      border: none;
      border-radius: 8px;
      font-weight: 600;
      color: #eee;
      cursor: pointer;
      box-shadow: 0 0 6px #42a5f5;
      transition: background-color 0.3s ease;
    }
    button:hover:enabled {
      background-color: #1e88e5;
    }
    button:disabled {
      background-color: #4a5a7a;
      cursor: not-allowed;
      box-shadow: none;
    }

    /* --- Panel derecho con gráficos --- */
    #chartsPanel {
      flex: 1 1 600px;
      background: #1a273f;
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 0 15px #42a5f5aa;
      display: flex;
      flex-direction: column;
      gap: 30px;
    }
    canvas {
      background: #0d1626;
      border-radius: 10px;
      box-shadow: inset 0 0 10px #42a5f5;
      width: 100% !important;
      height: 250px !important;
    }

    /* --- Música --- */
    #musicControl {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 48px;
      height: 48px;
      background-color: #42a5f5;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      box-shadow: 0 0 10px #42a5f5;
      z-index: 100;
      user-select: none;
    }
    #musicControl svg {
      width: 24px;
      height: 24px;
      fill: white;
      pointer-events: none;
    }
    #musicControl:hover {
      background-color: #1e88e5;
      box-shadow: 0 0 20px #1e88e5;
    }
  </style>
</head>
<body>

<!-- Pantalla de inicio -->
<div id="startScreen" class="start-screen" role="dialog" aria-modal="true" aria-labelledby="welcomeTitle">
  <h1 id="welcomeTitle">Bienvenido al Simulador Empresarial</h1>
  <button id="playButton" aria-label="Comenzar a jugar">Jugar</button>
</div>

<!-- Contenedor principal del juego (inicialmente oculto) -->
<div id="gameContainer" style="display:none;">

  <!-- Fondo de partículas -->
  <div id="particles" aria-hidden="true"></div>

  <header>Simulador Empresarial - Gestión Estratégica</header>

  <div id="status" role="alert" aria-live="assertive"></div>

  <main>
    <!-- Panel izquierdo -->
    <section id="infoPanel" aria-label="Información y controles de juego">
      <h2>Estado Actual</h2>
      <div class="stat">Dinero: $<span id="money">0</span></div>
      <div class="stat">Producción: <span id="income">0</span> unidades</div>
      <div class="bar" aria-label="Barra de producción">
        <div id="incomeBar" class="bar-inner" style="width: 0%;"></div>
      </div>

      <div class="stat">Reputación: <span id="reputation">0</span></div>
      <div class="bar" aria-label="Barra de reputación">
        <div id="reputationBar" class="bar-inner" style="width: 0%;"></div>
      </div>

      <div class="stat">Empleados: <span id="employees">0</span></div>
      <div class="stat">Mejoras: <span id="upgrades">0</span></div>
      <div class="stat">Marketing: <span id="marketing">0</span></div>
      <div class="stat">Recursos Humanos: <span id="hr">0</span></div>
      <div class="stat">I+D: <span id="rnd">0</span></div>
      <div class="stat">Expansión Mercado: <span id="marketExpansion">0</span></div>
      <div class="stat">Eficiencia: <span id="efficiency">1.00</span></div>
      <div class="stat">Seguro: <span id="insurance">No</span></div>
      <div class="stat">Capacitación: <span id="training">0</span></div>
      <div class="stat">Préstamo: $<span id="loan">0</span></div>

      <div class="btn-group" role="group" aria-label="Controles del juego">
        <button id="hire" aria-label="Contratar empleado">Contratar Empleado ($200)</button>
        <button id="upgrade" aria-label="Mejorar producción">Mejorar Producción ($500)</button>
        <button id="marketingBtn" aria-label="Campaña de marketing">Marketing ($300)</button>
        <button id="hrBtn" aria-label="Mejorar recursos humanos">Recursos Humanos ($400)</button>
        <button id="rndBtn" aria-label="Invertir en investigación y desarrollo">I+D ($700)</button>
        <button id="marketBtn" aria-label="Expandir mercado">Expansión Mercado ($600)</button>
        <button id="loanBtn" aria-label="Solicitar préstamo">Pedir Préstamo ($1000)</button>
        <button id="payLoanBtn" aria-label="Pagar préstamo" disabled>Pagar Préstamo</button>
        <button id="insuranceBtn" aria-label="Contratar seguro">Contratar Seguro ($350)</button>
        <button id="trainingBtn" aria-label="Capacitar empleados">Capacitación ($400)</button>
        <button id="resetBtn" aria-label="Reiniciar juego">Reiniciar Juego</button>
      </div>
    </section>

    <!-- Panel derecho con gráficos -->
    <section id="chartsPanel" aria-label="Gráficos de producción y reputación">
      <canvas id="incomeChart" aria-label="Gráfico de producción"></canvas>
      <canvas id="repChart" aria-label="Gráfico de reputación"></canvas>
    </section>
  </main>

  <!-- Control música -->
  <div id="musicControl" role="button" aria-pressed="false" tabindex="0" aria-label="Reproducir o pausar música">
    <svg id="musicIcon" viewBox="0 0 24 24"><path d="M3 9v6h4l5 5V4L7 9H3z"/></svg>
  </div>

</div>

<!-- Sonidos y scripts -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  // Variables de juego
  let money, income, employees, upgrades, marketing, hr, reputation, level, rnd, marketExpansion, loan;
  let efficiency = 1.0;
  let maintenanceLevel = 0;
  let insurance = 0;
  let training = 0;
  let eventCooldown = 0;
  const loanInterest = 0.03;
  const baseSalary = 10;

  let incomeHistory = [];
  let reputationHistory = [];
  const maxHistory = 50;

  let statusMessage = "";

  // Elementos UI
  const moneyEl = document.getElementById('money');
  const incomeEl = document.getElementById('income');
  const reputationEl = document.getElementById('reputation');
  const employeesEl = document.getElementById('employees');
  const upgradesEl = document.getElementById('upgrades');
  const marketingEl = document.getElementById('marketing');
  const hrEl = document.getElementById('hr');
  const rndEl = document.getElementById('rnd');
  const marketExpansionEl = document.getElementById('marketExpansion');
  const efficiencyEl = document.getElementById('efficiency');
  const insuranceEl = document.getElementById('insurance');
  const trainingEl = document.getElementById('training');
  const loanEl = document.getElementById('loan');

  const incomeBar = document.getElementById('incomeBar');
  const reputationBar = document.getElementById('reputationBar');

  // Botones
  const buttons = {
    hire: document.getElementById('hire'),
    upgrade: document.getElementById('upgrade'),
    marketing: document.getElementById('marketingBtn'),
    hr: document.getElementById('hrBtn'),
    rnd: document.getElementById('rndBtn'),
    market: document.getElementById('marketBtn'),
    loan: document.getElementById('loanBtn'),
    payLoan: document.getElementById('payLoanBtn'),
    insurance: document.getElementById('insuranceBtn'),
    training: document.getElementById('trainingBtn'),
    reset: document.getElementById('resetBtn')
  };

  const statusElement = document.getElementById('status');

  // Sonidos
  const clickSound = new Audio('https://actions.google.com/sounds/v1/cartoon/wood_plank_flicks.ogg');
  clickSound.volume = 0.15;

  // Música ambiental
  const music = new Audio('https://cdn.pixabay.com/download/audio/2022/03/29/audio_6c5a3010e1.mp3?filename=ambient-11164.mp3');
  music.loop = true;
  music.volume = 0.05;
  let musicPlaying = false;
  const musicControl = document.getElementById('musicControl');
  const musicIcon = document.getElementById('musicIcon');

  musicControl.addEventListener('click', () => {
    if(musicPlaying) {
      music.pause();
      musicPlaying = false;
      musicControl.setAttribute('aria-pressed', 'false');
      musicIcon.innerHTML = '<path d="M3 9v6h4l5 5V4L7 9H3z"/>'; // play icon
    } else {
      music.play();
      musicPlaying = true;
      musicControl.setAttribute('aria-pressed', 'true');
      musicIcon.innerHTML = '<path d="M6 19h4.5V5H6v14zm7-14v14h4.5V5H13z"/>'; // pause icon
    }
  });

  function playClickSound() {
    clickSound.currentTime = 0;
    clickSound.play();
  }

  // Actualizar UI con datos actuales
  function updateUI() {
    moneyEl.textContent = money.toFixed(2);
    incomeEl.textContent = income.toFixed(2);
    reputationEl.textContent = reputation.toFixed(0);
    employeesEl.textContent = employees;
    upgradesEl.textContent = upgrades;
    marketingEl.textContent = marketing;
    hrEl.textContent = hr;
    rndEl.textContent = rnd;
    marketExpansionEl.textContent = marketExpansion;
    efficiencyEl.textContent = efficiency.toFixed(2);
    insuranceEl.textContent = insurance > 0 ? "Sí" : "No";
    trainingEl.textContent = training;
    loanEl.textContent = loan.toFixed(2);

    incomeBar.style.width = Math.min(100, (income / 2000) * 100) + '%';
    reputationBar.style.width = reputation + '%';

    // Habilitar o deshabilitar botones según dinero y estado
    buttons.hire.disabled = money < 200;
    buttons.upgrade.disabled = money < 500;
    buttons.marketing.disabled = money < 300;
    buttons.hr.disabled = money < 400;
    buttons.rnd.disabled = money < 700;
    buttons.market.disabled = money < 600;
    buttons.insurance.disabled = money < 350;
    buttons.training.disabled = money < 400;

    buttons.loan.disabled = loan > 0;
    buttons.payLoan.disabled = loan === 0 || money < loan;
  }

  // Mostrar mensajes de estado con animación
  function showStatus(msg) {
    statusElement.textContent = msg;
    statusElement.classList.add('pop');
    setTimeout(() => statusElement.classList.remove('pop'), 700);
  }

  // Funciones de acciones
  function hireEmployee() {
    if (money >= 200) {
      money -= 200;
      employees++;
      showStatus("Empleado contratado");
      playClickSound();
      updateUI();
    }
  }
  function upgradeProduction() {
    if (money >= 500) {
      money -= 500;
      upgrades++;
      efficiency += 0.1;
      showStatus("Producción mejorada");
      playClickSound();
      updateUI();
    }
  }
  function marketingCampaign() {
    if (money >= 300) {
      money -= 300;
      marketing++;
      reputation += 10;
      if (reputation > 100) reputation = 100;
      showStatus("Campaña de marketing exitosa");
      playClickSound();
      updateUI();
    }
  }
  function improveHR() {
    if (money >= 400) {
      money -= 400;
      hr++;
      efficiency += 0.05;
      showStatus("Recursos Humanos mejorados");
      playClickSound();
      updateUI();
    }
  }
  function investRnd() {
    if (money >= 700) {
      money -= 700;
      rnd++;
      efficiency += 0.07;
      reputation += 5;
      if (reputation > 100) reputation = 100;
      showStatus("I+D incrementado");
      playClickSound();
      updateUI();
    }
  }
  function expandMarket() {
    if (money >= 600) {
      money -= 600;
      marketExpansion++;
      income += 50;
      showStatus("Mercado expandido");
      playClickSound();
      updateUI();
    }
  }
  function hireInsurance() {
    if (money >= 350 && insurance === 0) {
      money -= 350;
      insurance = 1;
      showStatus("Seguro contratado");
      playClickSound();
      updateUI();
    }
  }
  function trainEmployees() {
    if (money >= 400) {
      money -= 400;
      training++;
      efficiency += 0.1;
      showStatus("Empleados capacitados");
      playClickSound();
      updateUI();
    }
  }
  function requestLoan() {
    if (loan === 0) {
      loan = 1000;
      money += loan;
      showStatus("Préstamo concedido");
      playClickSound();
      updateUI();
    }
  }
  function payLoan() {
    if (loan > 0 && money >= loan) {
      money -= loan;
      loan = 0;
      showStatus("Préstamo pagado");
      playClickSound();
      updateUI();
    }
  }

  // Reiniciar juego
  function resetGame() {
    money = 1000;
    income = 100;
    employees = 5;
    upgrades = 0;
    marketing = 0;
    hr = 0;
    reputation = 50;
    level = 1;
    rnd = 0;
    marketExpansion = 0;
    loan = 0;
    efficiency = 1.0;
    maintenanceLevel = 0;
    insurance = 0;
    training = 0;
    incomeHistory = [];
    reputationHistory = [];
    updateUI();
    showStatus("Juego iniciado");
  }

  // Actualizar producción y finanzas
  function gameTick() {
    // Calcular ingresos básicos
    let baseIncome = employees * 10 * efficiency;

    // Añadir mejoras
    baseIncome += upgrades * 20;
    baseIncome += marketing * 5;
    baseIncome += hr * 8;
    baseIncome += rnd * 12;
    baseIncome += marketExpansion * 30;
    baseIncome *= (1 + training * 0.02);

    // Descontar mantenimiento o costos
    const salaryCosts = employees * baseSalary;
    const insuranceCost = insurance > 0 ? 5 : 0;
    const loanInterestCost = loan > 0 ? loan * loanInterest : 0;
    const maintenanceCost = maintenanceLevel * 2;

    // Actualizar dinero y reputación
    money += baseIncome - salaryCosts - insuranceCost - loanInterestCost - maintenanceCost;
    reputation += (marketing * 0.1 + hr * 0.05 + rnd * 0.02) - (maintenanceLevel * 0.1);
    if (reputation > 100) reputation = 100;
    if (reputation < 0) reputation = 0;

    income = baseIncome;

    // Historia para gráficos
    if (incomeHistory.length >= maxHistory) incomeHistory.shift();
    if (reputationHistory.length >= maxHistory) reputationHistory.shift();

    incomeHistory.push(income);
    reputationHistory.push(reputation);

    updateUI();

    // Eventos aleatorios cada 20 ticks aprox.
    if (eventCooldown <= 0) {
      if (Math.random() < 0.15) {
        triggerRandomEvent();
        eventCooldown = 20;
      }
    } else {
      eventCooldown--;
    }
  }

  // Eventos aleatorios con efectos
  function triggerRandomEvent() {
    const event = Math.floor(Math.random() * 3);
    switch(event) {
      case 0:
        // Inspección sorpresa: pierde reputación si no hay seguro
        if (insurance === 0) {
          reputation -= 15;
          showStatus("Inspección sorpresa: reputación disminuida");
        } else {
          showStatus("Inspección sorpresa: seguro protege la empresa");
        }
        break;
      case 1:
        // Campaña viral mejora marketing
        marketing++;
        showStatus("Campaña viral aumenta marketing");
        break;
      case 2:
        // Fuga de empleados: pierdes algunos empleados
        if (employees > 1) {
          employees = Math.max(1, employees - 2);
          showStatus("Fuga de empleados: reducción de plantilla");
        } else {
          showStatus("Fuga de empleados evitada");
        }
        break;
    }
    updateUI();
  }

  // Configurar eventos botones
  buttons.hire.addEventListener('click', () => { hireEmployee(); });
  buttons.upgrade.addEventListener('click', () => { upgradeProduction(); });
  buttons.marketing.addEventListener('click', () => { marketingCampaign(); });
  buttons.hr.addEventListener('click', () => { improveHR(); });
  buttons.rnd.addEventListener('click', () => { investRnd(); });
  buttons.market.addEventListener('click', () => { expandMarket(); });
  buttons.loan.addEventListener('click', () => { requestLoan(); });
  buttons.payLoan.addEventListener('click', () => { payLoan(); });
  buttons.insurance.addEventListener('click', () => { hireInsurance(); });
  buttons.training.addEventListener('click', () => { trainEmployees(); });
  buttons.reset.addEventListener('click', () => { resetGame(); });

  // Gráficos con Chart.js
  const ctxIncome = document.getElementById('incomeChart').getContext('2d');
  const ctxRep = document.getElementById('repChart').getContext('2d');

  const incomeChart = new Chart(ctxIncome, {
    type: 'line',
    data: {
      labels: [],
      datasets: [{
        label: 'Producción',
        data: [],
        borderColor: '#42a5f5',
        backgroundColor: 'rgba(66, 165, 245, 0.3)',
        fill: true,
        tension: 0.3,
        pointRadius: 0
      }]
    },
    options: {
      responsive: true,
      animation: { duration: 300 },
      scales: {
        y: { min: 0 }
      },
      plugins: {
        legend: { labels: { color: '#42a5f5' } }
      }
    }
  });

  const repChart = new Chart(ctxRep, {
    type: 'line',
    data: {
      labels: [],
      datasets: [{
        label: 'Reputación',
        data: [],
        borderColor: '#90caf9',
        backgroundColor: 'rgba(144, 202, 249, 0.3)',
        fill: true,
        tension: 0.3,
        pointRadius: 0
      }]
    },
    options: {
      responsive: true,
      animation: { duration: 300 },
      scales: {
        y: { min: 0, max: 100 }
      },
      plugins: {
        legend: { labels: { color: '#90caf9' } }
      }
    }
  });

  // Actualizar gráficos
  function updateCharts() {
    const labels = incomeHistory.map((_, i) => i + 1);
    incomeChart.data.labels = labels;
    incomeChart.data.datasets[0].data = incomeHistory;
    incomeChart.update();

    repChart.data.labels = labels;
    repChart.data.datasets[0].data = reputationHistory;
    repChart.update();
  }

  // Partículas de fondo
  const particlesContainer = document.getElementById('particles');
  const particleCount = 60;
  let particles = [];

  function createParticles() {
    particlesContainer.innerHTML = '';
    particles = [];
    for(let i = 0; i < particleCount; i++) {
      const p = document.createElement('div');
      p.classList.add('particle');
      const size = 5 + Math.random() * 10;
      p.style.width = size + 'px';
      p.style.height = size + 'px';
      p.style.top = (Math.random() * 100) + 'vh';
      p.style.left = (Math.random() * 100) + 'vw';
      p.style.opacity = 0.15 + Math.random() * 0.2;
      particlesContainer.appendChild(p);
      particles.push({
        el: p,
        size,
        x: parseFloat(p.style.left),
        y: parseFloat(p.style.top),
        speedX: (Math.random() - 0.5) * 0.05,
        speedY: (Math.random() - 0.5) * 0.02
      });
    }
  }
  function moveParticles() {
    particles.forEach(p => {
      p.x += p.speedX;
      p.y += p.speedY;
      if (p.x > 100) p.x = 0;
      else if (p.x < 0) p.x = 100;
      if (p.y > 100) p.y = 0;
      else if (p.y < 0) p.y = 100;
      p.el.style.left = p.x + 'vw';
      p.el.style.top = p.y + 'vh';
    });
  }

  // Loop principal del juego
  let gameInterval;
  function startGameLoop() {
    gameInterval = setInterval(() => {
      gameTick();
      updateCharts();
      moveParticles();
    }, 1000);
  }

  // Control pantalla de inicio y juego
  const startScreen = document.getElementById('startScreen');
  const gameContainer = document.getElementById('gameContainer');
  const playButton = document.getElementById('playButton');

  playButton.addEventListener('click', () => {
    startScreen.style.display = 'none';
    gameContainer.style.display = 'flex';
    resetGame();
    createParticles();
    startGameLoop();
  });

  // Para accesibilidad: permitir activar el botón con Enter/Space
  playButton.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      playButton.click();
    }
  });
</script>

</body>
</html>
