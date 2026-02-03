<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover" />
  <title>Teleporter Dashboard</title>
  <style>
    :root{
      --bg1:#070A12;
      --bg2:#0B1224;
      --panel: rgba(255,255,255,0.06);
      --panel2: rgba(255,255,255,0.09);
      --line: rgba(255,255,255,0.12);
      --text: rgba(255,255,255,0.92);
      --muted: rgba(255,255,255,0.68);
      --accent:#66E3FF;
      --accent2:#9B7CFF;
      --danger:#FF3B6A;
      --good:#34C759;

      /* Safe-area */
      --sat: env(safe-area-inset-top, 0px);
      --sab: env(safe-area-inset-bottom, 0px);
      --sal: env(safe-area-inset-left, 0px);
      --sar: env(safe-area-inset-right, 0px);
    }

    *{box-sizing:border-box}
    body{
      margin:0;
      min-height:100vh;
      color:var(--text);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Inter, Arial, sans-serif;
      background:
        radial-gradient(900px 500px at 70% 20%, rgba(102,227,255,0.16), transparent 60%),
        radial-gradient(700px 500px at 20% 85%, rgba(155,124,255,0.16), transparent 60%),
        linear-gradient(180deg, var(--bg1), var(--bg2));
      overflow-x:hidden;
      padding: 0 calc(12px + var(--sal)) calc(14px + var(--sab)) calc(12px + var(--sar));
    }

    /* App frame */
    .app{
      max-width: 520px;
      margin: 0 auto;
      padding-top: calc(12px + var(--sat));
    }

    /* Top status / header */
    .top{
      display:flex;
      align-items:flex-start;
      justify-content:space-between;
      gap:12px;
      padding: 10px 6px 12px;
    }
    .brand{
      display:flex;
      flex-direction:column;
      gap:6px;
    }
    .brand .title{
      font-size: 18px;
      font-weight: 800;
      letter-spacing: 0.8px;
    }
    .brand .subtitle{
      font-size: 12px;
      color: var(--muted);
      line-height: 1.35;
    }
    .chip{
      background: linear-gradient(135deg, rgba(102,227,255,0.14), rgba(155,124,255,0.14));
      border:1px solid var(--line);
      border-radius: 999px;
      padding: 8px 12px;
      font-size: 12px;
      color: var(--muted);
      white-space: nowrap;
    }

    /* Panels */
    .grid{
      display:grid;
      gap:12px;
    }
    @media (min-width: 520px){
      .grid{ gap:14px; }
    }

    .panel{
      background: linear-gradient(180deg, var(--panel), rgba(255,255,255,0.04));
      border:1px solid var(--line);
      border-radius: 18px;
      padding: 14px;
      box-shadow: 0 18px 50px rgba(0,0,0,0.35);
      position:relative;
      overflow:hidden;
    }
    .panel:before{
      content:"";
      position:absolute;
      inset:-2px;
      background:
        radial-gradient(280px 120px at 20% 0%, rgba(102,227,255,0.10), transparent 60%),
        radial-gradient(320px 160px at 85% 30%, rgba(155,124,255,0.10), transparent 60%);
      pointer-events:none;
      filter: blur(0.2px);
    }
    .panel > * { position:relative; }

    .section-title{
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 1.6px;
      color: var(--muted);
      margin: 0 0 10px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
    }
    .badge{
      font-size: 11px;
      padding: 5px 9px;
      border-radius: 999px;
      border:1px solid var(--line);
      background: rgba(255,255,255,0.05);
      color: var(--muted);
    }

    /* PIN gate */
    .gate-wrap{
      min-height: 72vh;
      display:flex;
      align-items:center;
      justify-content:center;
      padding-top: calc(12px + var(--sat));
    }
    .gate{
      width: min(520px, 100%);
      padding: 18px;
    }
    .gate h1{
      margin: 0 0 8px;
      font-size: 22px;
      letter-spacing: 0.6px;
    }
    .gate p{
      margin: 0 0 14px;
      color: var(--muted);
      line-height: 1.45;
      font-size: 13px;
    }
    .row{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
    }

    input, select{
      width: 100%;
      padding: 12px 12px;
      border-radius: 14px;
      border: 1px solid rgba(255,255,255,0.14);
      background: rgba(0,0,0,0.18);
      color: var(--text);
      outline: none;
      font-size: 15px;
    }
    input::placeholder{ color: rgba(255,255,255,0.45); }
    select{ appearance: none; }

    .btn{
      width: 100%;
      padding: 12px 14px;
      border: 1px solid rgba(255,255,255,0.16);
      border-radius: 14px;
      background: rgba(255,255,255,0.06);
      color: var(--text);
      font-size: 15px;
      font-weight: 700;
      letter-spacing: 0.4px;
      cursor: pointer;
      transition: transform 0.12s ease, background 0.2s ease, border-color 0.2s ease;
      user-select:none;
      -webkit-tap-highlight-color: transparent;
    }
    .btn:active{ transform: scale(0.985); }
    .btn.primary{
      border-color: rgba(102,227,255,0.35);
      background: linear-gradient(135deg, rgba(102,227,255,0.22), rgba(155,124,255,0.16));
    }
    .btn.danger{
      border-color: rgba(255,59,106,0.35);
      background: rgba(255,59,106,0.12);
    }
    .btn.small{
      font-size: 13px;
      padding: 10px 12px;
      border-radius: 12px;
    }

    .two{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap:10px;
    }
    @media (max-width: 360px){
      .two{ grid-template-columns: 1fr; }
    }

    /* Dashboard layout */
    .dash{
      display:none;
    }
    .dash.active{ display:block; }

    .stats{
      display:grid;
      grid-template-columns: 1fr 1fr;
      gap:10px;
    }
    .stat{
      background: rgba(255,255,255,0.05);
      border: 1px solid rgba(255,255,255,0.10);
      border-radius: 16px;
      padding: 12px;
    }
    .stat .k{
      font-size: 11px;
      color: var(--muted);
      letter-spacing: 1.2px;
      text-transform:uppercase;
    }
    .stat .v{
      margin-top: 8px;
      font-size: 18px;
      font-weight: 800;
    }

    .contacts{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
    }
    .contact{
      flex: 1;
      min-width: 140px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      padding: 12px;
      border-radius: 16px;
      border:1px solid rgba(255,255,255,0.12);
      background: rgba(255,255,255,0.05);
    }
    .contact .name{
      font-weight: 800;
    }
    .contact .tag{
      font-size: 12px;
      color: var(--muted);
    }

    .dot{
      width: 10px; height: 10px; border-radius: 999px;
      background: var(--good);
      box-shadow: 0 0 0 4px rgba(52,199,89,0.16);
      flex:0 0 auto;
    }

    /* Teleport sequence overlay */
    .overlay{
      position: fixed;
      inset: 0;
      display:none;
      align-items:center;
      justify-content:center;
      background: rgba(0,0,0,0.55);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
      z-index: 999;
      padding: 22px;
    }
    .overlay.active{ display:flex; }

    .sequence{
      width:min(520px, 100%);
      border-radius: 22px;
      border: 1px solid rgba(255,255,255,0.14);
      background: linear-gradient(180deg, rgba(255,255,255,0.10), rgba(255,255,255,0.06));
      padding: 18px;
      box-shadow: 0 24px 80px rgba(0,0,0,0.55);
      overflow:hidden;
      position:relative;
    }
    .sequence:before{
      content:"";
      position:absolute;
      inset:-2px;
      background:
        radial-gradient(360px 200px at 20% 10%, rgba(102,227,255,0.18), transparent 60%),
        radial-gradient(360px 200px at 85% 70%, rgba(155,124,255,0.18), transparent 60%);
      pointer-events:none;
      filter: blur(0.2px);
    }
    .sequence > * { position:relative; }

    .seq-title{
      font-size: 13px;
      color: var(--muted);
      letter-spacing: 1.4px;
      text-transform: uppercase;
      margin: 0 0 10px;
    }
    .impact{
      font-size: 22px;
      font-weight: 900;
      letter-spacing: 0.6px;
      margin: 0 0 10px;
    }
    .count{
      font-size: 56px;
      font-weight: 1000;
      line-height: 1;
      margin: 6px 0 12px;
      text-shadow: 0 0 18px rgba(102,227,255,0.25);
    }
    .bar{
      height: 10px;
      border-radius: 999px;
      background: rgba(255,255,255,0.10);
      border:1px solid rgba(255,255,255,0.12);
      overflow:hidden;
    }
    .bar > div{
      height:100%;
      width: 0%;
      background: linear-gradient(90deg, rgba(102,227,255,0.85), rgba(155,124,255,0.85));
      transition: width 1s linear;
    }
    .hint{
      margin-top: 10px;
      font-size: 12px;
      color: var(--muted);
      line-height: 1.4;
    }

    /* Tiny footer buttons row */
    .actions{
      display:grid;
      grid-template-columns: repeat(3, 1fr);
      gap:10px;
    }
    @media (max-width: 360px){
      .actions{ grid-template-columns: 1fr; }
    }

    /* Small utility */
    .shake{
      animation: shake 0.4s ease;
    }
    @keyframes shake{
      0%{transform:translateX(0)}
      20%{transform:translateX(-8px)}
      40%{transform:translateX(8px)}
      60%{transform:translateX(-6px)}
      80%{transform:translateX(6px)}
      100%{transform:translateX(0)}
    }
  </style>
</head>

<body>
  <div class="app">

    <!-- GATE -->
    <div id="gate" class="gate-wrap">
      <div class="panel gate">
        <h1>TELEPORTER ACCESS</h1>
        <p>Enter your authorization PIN to unlock the temporal dashboard.</p>

        <div class="row" style="margin-bottom:10px;">
          <input id="pinInput" inputmode="numeric" placeholder="Enter PIN (hint: 2025)" maxlength="8" />
        </div>

        <div class="row" style="margin-bottom:10px;">
          <button class="btn primary" id="unlockBtn">Unlock</button>
        </div>

        <div class="row">
          <button class="btn small" id="demoFillBtn">Auto-fill PIN</button>
        </div>

        <div id="gateMsg" style="margin-top:12px;color:var(--muted);font-size:13px;line-height:1.35;"></div>
      </div>
    </div>

    <!-- DASHBOARD -->
    <div id="dash" class="dash">
      <div class="top">
        <div class="brand">
          <div class="title">TELEPORTER // DASHBOARD</div>
          <div class="subtitle" id="whereText">Locked on: <span style="color:var(--text);font-weight:700;">—</span></div>
        </div>
        <div class="chip" id="clockChip">Syncing…</div>
      </div>

      <div class="grid">

        <!-- STATUS PANEL -->
        <div class="panel">
          <div class="section-title">
            <span>System Status</span>
            <span class="badge" id="stabilityBadge">STABILITY: 100%</span>
          </div>

          <div class="stats">
            <div class="stat">
              <div class="k">Current Year</div>
              <div class="v" id="curYear">—</div>
            </div>
            <div class="stat">
              <div class="k">Current Time</div>
              <div class="v" id="curTime">—</div>
            </div>
          </div>
        </div>

        <!-- DESTINATION PANEL -->
        <div class="panel">
          <div class="section-title">
            <span>Destination Controls</span>
            <span class="badge" id="destBadge">READY</span>
          </div>

          <div class="two" style="margin-bottom:10px;">
            <div>
              <div style="font-size:12px;color:var(--muted);margin:0 0 6px;">Universe / World</div>
              <select id="worldSelect">
                <option value="Prime-01">Prime-01</option>
                <option value="Neon-Arcadia">Neon-Arcadia</option>
                <option value="Dust-Frontier">Dust-Frontier</option>
                <option value="Abyss-Station">Abyss-Station</option>
                <option value="Echo-Timeline">Echo-Timeline</option>
              </select>
            </div>

            <div>
              <div style="font-size:12px;color:var(--muted);margin:0 0 6px;">Target Year</div>
              <input id="yearInput" inputmode="numeric" placeholder="e.g. 2049" maxlength="6" />
            </div>
          </div>

          <div class="two" style="margin-bottom:10px;">
            <button class="btn primary" id="teleportBtn">Teleport</button>
            <button class="btn" id="randomBtn">Randomize</button>
          </div>

          <div class="actions">
            <button class="btn small" id="scanBtn">Scan</button>
            <button class="btn small" id="calibrateBtn">Calibrate</button>
            <button class="btn small danger" id="lockBtn">Lock</button>
          </div>

          <div id="dashMsg" style="margin-top:12px;color:var(--muted);font-size:13px;line-height:1.35;"></div>
        </div>

        <!-- CONTACTS PANEL -->
        <div class="panel">
          <div class="section-title">
            <span>Anonymous Channels</span>
            <span class="badge">ENCRYPTED</span>
          </div>

          <div class="contacts">
            <div class="contact" role="button" tabindex="0" id="contactKevin">
              <div>
                <div class="name">Kevin</div>
                <div class="tag">Signal: Unknown</div>
              </div>
              <div class="dot" title="online"></div>
            </div>

            <div class="contact" role="button" tabindex="0" id="contactRocky">
              <div>
                <div class="name">Rocky</div>
                <div class="tag">Signal: Unknown</div>
              </div>
              <div class="dot" title="online"></div>
            </div>
          </div>

          <div style="margin-top:10px;color:var(--muted);font-size:12px;line-height:1.35;">
            Tip: Tap a channel to “ping” it (just a UI effect).
          </div>
        </div>

      </div>
    </div>

  </div>

  <!-- TELEPORT OVERLAY -->
  <div id="overlay" class="overlay" aria-hidden="true">
    <div class="sequence">
      <div class="seq-title">Temporal Jump Sequence</div>
      <div class="impact" id="impactText">Brace for impact…</div>
      <div class="count" id="countNum">5</div>
      <div class="bar"><div id="barFill"></div></div>
      <div class="hint" id="hintText">Hold steady. Recalibrating spacetime alignment…</div>
    </div>
  </div>

<script>
  // ---------- Helpers ----------
  function pad2(n){ return String(n).padStart(2,'0'); }
  function nowParts(){
    const d = new Date();
    const year = d.getFullYear();
    const time = `${pad2(d.getHours())}:${pad2(d.getMinutes())}:${pad2(d.getSeconds())}`;
    return { year, time, date: d };
  }

  // ---------- Elements ----------
  const gate = document.getElementById('gate');
  const dash = document.getElementById('dash');

  const pinInput = document.getElementById('pinInput');
  const unlockBtn = document.getElementById('unlockBtn');
  const demoFillBtn = document.getElementById('demoFillBtn');
  const gateMsg = document.getElementById('gateMsg');

  const clockChip = document.getElementById('clockChip');
  const whereText = document.getElementById('whereText');
  const curYear = document.getElementById('curYear');
  const curTime = document.getElementById('curTime');
  const stabilityBadge = document.getElementById('stabilityBadge');

  const worldSelect = document.getElementById('worldSelect');
  const yearInput = document.getElementById('yearInput');
  const teleportBtn = document.getElementById('teleportBtn');
  const randomBtn = document.getElementById('randomBtn');
  const scanBtn = document.getElementById('scanBtn');
  const calibrateBtn = document.getElementById('calibrateBtn');
  const lockBtn = document.getElementById('lockBtn');
  const dashMsg = document.getElementById('dashMsg');

  const overlay = document.getElementById('overlay');
  const countNum = document.getElementById('countNum');
  const barFill = document.getElementById('barFill');
  const impactText = document.getElementById('impactText');
  const hintText = document.getElementById('hintText');

  const contactKevin = document.getElementById('contactKevin');
  const contactRocky = document.getElementById('contactRocky');

  // ---------- State ----------
  const ACCESS_PIN = "2025";
  let teleporting = false;
  let lastWorld = "Prime-01";
  let lastYear = null;

  // ---------- Live clock ----------
  function updateClock(){
    const { year, time } = nowParts();
    clockChip.textContent = `Local ${time}`;
    if(!lastYear){
      curYear.textContent = "—";
      curTime.textContent = "—";
    }
  }
  setInterval(updateClock, 1000);
  updateClock();

  // ---------- Gate logic ----------
  function unlock(){
    const val = (pinInput.value || "").trim();
    if(val === ACCESS_PIN){
      gateMsg.textContent = "Access granted. Initializing temporal dashboard…";
      // quick "boot" delay
      setTimeout(()=>{
        gate.style.display = "none";
        dash.classList.add('active');
        dashMsg.textContent = "System online. Choose a universe and year, then teleport.";
        // set default "current" values once unlocked
        const { year, time } = nowParts();
        curYear.textContent = "—";
        curTime.textContent = "—";
        whereText.innerHTML = `Locked on: <span style="color:var(--text);font-weight:800;">No active timeline</span>`;
        stabilityBadge.textContent = "STABILITY: 100%";
      }, 500);
    } else {
      gateMsg.textContent = "Authorization failed. Incorrect PIN.";
      document.querySelector('.gate').classList.remove('shake');
      void document.querySelector('.gate').offsetWidth; // restart animation
      document.querySelector('.gate').classList.add('shake');
    }
  }

  unlockBtn.addEventListener('click', unlock);
  pinInput.addEventListener('keydown', (e)=>{
    if(e.key === "Enter") unlock();
  });
  demoFillBtn.addEventListener('click', ()=>{
    pinInput.value = ACCESS_PIN;
    gateMsg.textContent = "PIN filled. Tap Unlock.";
  });

  // ---------- Dashboard actions ----------
  randomBtn.addEventListener('click', ()=>{
    const worlds = ["Prime-01","Neon-Arcadia","Dust-Frontier","Abyss-Station","Echo-Timeline"];
    const w = worlds[Math.floor(Math.random()*worlds.length)];
    const y = 1800 + Math.floor(Math.random()*350); // 1800–2149
    worldSelect.value = w;
    yearInput.value = String(y);
    dashMsg.textContent = `Randomized destination: ${w} / ${y}.`;
  });

  scanBtn.addEventListener('click', ()=>{
    dashMsg.textContent = "Scanning timeline integrity… (simulated)";
    stabilityBadge.textContent = "STABILITY: 99%";
    setTimeout(()=>{
      stabilityBadge.textContent = "STABILITY: 100%";
      dashMsg.textContent = "Scan complete. No anomalies detected.";
    }, 900);
  });

  calibrateBtn.addEventListener('click', ()=>{
    dashMsg.textContent = "Calibrating coils… (simulated)";
    stabilityBadge.textContent = "STABILITY: 98%";
    setTimeout(()=>{
      stabilityBadge.textContent = "STABILITY: 100%";
      dashMsg.textContent = "Calibration complete. Controls optimized.";
    }, 900);
  });

  lockBtn.addEventListener('click', ()=>{
    // lock app: show gate again
    dash.classList.remove('active');
    gate.style.display = "flex";
    pinInput.value = "";
    gateMsg.textContent = "Session locked.";
  });

  function pingContact(name){
    dashMsg.textContent = `Pinging ${name}… encrypted channel handshake (simulated).`;
    const old = stabilityBadge.textContent;
    stabilityBadge.textContent = "STABILITY: 97%";
    setTimeout(()=>{
      stabilityBadge.textContent = old;
      dashMsg.textContent = `${name} channel acknowledged.`;
    }, 700);
  }
  contactKevin.addEventListener('click', ()=>pingContact("Kevin"));
  contactRocky.addEventListener('click', ()=>pingContact("Rocky"));

  // ---------- Teleport sequence ----------
  function validateDestination(){
    const world = worldSelect.value;
    const yearStr = (yearInput.value || "").trim();
    const year = parseInt(yearStr, 10);

    if(!world){
      dashMsg.textContent = "Select a universe/world first.";
      return null;
    }
    if(!yearStr || Number.isNaN(year) || year < 1 || year > 99999){
      dashMsg.textContent = "Enter a valid year (1–99999).";
      return null;
    }
    return { world, year };
  }

  teleportBtn.addEventListener('click', ()=>{
    if(teleporting) return;
    const dest = validateDestination();
    if(!dest) return;

    teleporting = true;
    lastWorld = dest.world;
    lastYear = dest.year;

    dashMsg.textContent = `Destination locked: ${dest.world} / ${dest.year}.`;
    startCountdown(dest.world, dest.year);
  });

  function startCountdown(world, year){
    overlay.classList.add('active');
    overlay.setAttribute('aria-hidden', 'false');

    impactText.textContent = "Brace for impact…";
    hintText.textContent = `Aligning to ${world} / Year ${year}…`;
    barFill.style.width = "0%";
    countNum.textContent = "5";

    let n = 5;
    // Kick bar on next frame so transition animates
    requestAnimationFrame(()=> { barFill.style.width = "100%"; });

    const timer = setInterval(()=>{
      n--;
      countNum.textContent = String(n);

      if(n <= 0){
        clearInterval(timer);
        completeTeleport(world, year);
      }
    }, 1000);
  }

  function completeTeleport(world, year){
    // show a tiny "arrival" moment
    impactText.textContent = "Jump complete.";
    countNum.textContent = "✓";
    hintText.textContent = "Stabilizing timeline…";

    setTimeout(()=>{
      overlay.classList.remove('active');
      overlay.setAttribute('aria-hidden', 'true');

      const { time } = nowParts();
      curYear.textContent = String(year);
      curTime.textContent = time;

      whereText.innerHTML =
        `You are in <span style="color:var(--text);font-weight:900;">${year}</span> at <span style="color:var(--text);font-weight:900;">${time}</span>`;

      stabilityBadge.textContent = "STABILITY: 100%";
      dashMsg.textContent = `Arrived in ${world} / ${year}.`;
      teleporting = false;
    }, 650);
  }
</script>

</body>
</html>
