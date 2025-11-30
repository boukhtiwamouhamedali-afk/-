<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>🎯 لعبة تخمين الرقم — نسخة مطوّرة</title>
  <style>
    :root{
      --bg:#0f1724;
      --card:#0b1220;
      --accent:#60a5fa;
      --accent-2:#7c3aed;
      --muted:#94a3b8;
      --glass: rgba(255,255,255,0.04);
      --green:#10b981;
      --red:#ef4444;
      --gold:#f59e0b;
      font-family: "Segoe UI", Roboto, system-ui, "Noto Sans", "Helvetica Neue", Arial;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      background:linear-gradient(180deg,var(--bg),#071026 120%);
      color:#e6eef8;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
    }
    .container{
      width:100%;
      max-width:920px;
      background:linear-gradient(135deg,var(--card), rgba(12,18,30,0.9));
      border-radius:16px;
      padding:22px;
      box-shadow: 0 8px 30px rgba(2,6,23,0.7);
      display:grid;
      grid-template-columns: 1fr 360px;
      gap:20px;
      align-items:start;
    }

    header{
      grid-column: 1 / -1;
      display:flex;
      gap:12px;
      align-items:center;
      margin-bottom:6px;
    }
    .logo{
      width:56px;
      height:56px;
      border-radius:10px;
      background:linear-gradient(135deg,var(--accent),var(--accent-2));
      display:flex;
      align-items:center;
      justify-content:center;
      font-weight:700;
      box-shadow: 0 6px 18px rgba(96,165,250,0.12), inset 0 -6px 18px rgba(255,255,255,0.04);
      font-size:20px;
    }
    h1{margin:0;font-size:18px}
    p.lead{margin:0;color:var(--muted);font-size:13px}

    /* Main column */
    .play{
      background:var(--glass);
      padding:18px;
      border-radius:12px;
      min-height:320px;
      display:flex;
      flex-direction:column;
      gap:12px;
    }
    .panel{
      display:flex;
      gap:10px;
      align-items:center;
      justify-content:space-between;
      flex-wrap:wrap;
    }
    .controls {display:flex; gap:8px; align-items:center; flex-wrap:wrap}
    .btn{
      background:transparent;
      border:1px solid rgba(255,255,255,0.06);
      padding:10px 12px;
      border-radius:10px;
      color:inherit;
      cursor:pointer;
      transition:all .18s;
      font-weight:600;
    }
    .btn:hover{transform:translateY(-3px)}
    .btn.primary{
      background:linear-gradient(90deg,var(--accent),var(--accent-2));
      border:0;
      color:#021028;
      box-shadow: 0 10px 30px rgba(124,58,237,0.12);
    }
    .btn.ghost{background:transparent}
    .range{
      display:flex; gap:8px; align-items:center;
    }

    /* game area */
    .status{
      display:flex;
      gap:12px;
      align-items:center;
      justify-content:space-between;
      padding:10px;
      border-radius:10px;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
      border:1px solid rgba(255,255,255,0.02);
    }
    .big{
      font-size:34px;
      font-weight:800;
      letter-spacing:1px;
    }
    .muted{color:var(--muted); font-size:13px}
    .input-row{display:flex; gap:10px; align-items:center}
    input[type="number"]{
      -moz-appearance: textfield;
      appearance: textfield;
      padding:12px 14px;
      border-radius:10px;
      border:1px solid rgba(255,255,255,0.04);
      background:transparent;
      color:inherit;
      width:140px;
      font-size:16px;
      font-weight:700;
      text-align:center;
    }
    input[type="number"]::-webkit-outer-spin-button,
    input[type="number"]::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }

    .hints{font-size:14px;color:var(--muted);min-height:28px}

    /* sidebar */
    aside{
      background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);
      padding:16px;
      border-radius:12px;
      border:1px solid rgba(255,255,255,0.02);
      display:flex;
      flex-direction:column;
      gap:12px;
    }
    .card{background:transparent;padding:10px;border-radius:10px}
    .meta{display:flex;gap:8px;align-items:center;justify-content:space-between}
    ul.guesses{list-style:none;margin:0;padding:0;display:flex;flex-direction:column;gap:6px;max-height:220px;overflow:auto}
    li.guess{
      padding:8px 10px;border-radius:8px;background:rgba(255,255,255,0.02);
      display:flex;justify-content:space-between;align-items:center;font-weight:700;
    }
    .small{font-size:13px;color:var(--muted)}
    .stats{display:flex;gap:8px;flex-wrap:wrap}
    .badge{padding:6px 8px;border-radius:8px;background:rgba(255,255,255,0.02);font-weight:700}

    footer.help{grid-column:1/-1;margin-top:6px;color:var(--muted);font-size:13px;text-align:center}

    /* responsive */
    @media (max-width:900px){
      .container{grid-template-columns:1fr; padding:16px}
      aside{order:2}
    }

    /* small animations */
    .pulse{
      animation: pulse 1.2s infinite;
    }
    @keyframes pulse{
      0%{box-shadow:0 0 0 0 rgba(96,165,250,0.16)}
      70%{box-shadow:0 0 0 14px rgba(96,165,250,0)}
      100%{box-shadow:0 0 0 0 rgba(96,165,250,0)}
    }

    .result-win{color:var(--green); font-weight:800}
    .result-lose{color:var(--red); font-weight:800}

    /* accessibility focus */
    .btn:focus, input:focus { outline: 3px solid rgba(124,58,237,0.18); outline-offset:3px }
  </style>
</head>
<body>
  <main class="container" role="main" aria-labelledby="title">
    <header>
      <div class="logo" aria-hidden>🎯</div>
      <div>
        <h1 id="title">تخمين الرقم — نسخة مطوّرة</h1>
        <p class="lead">اختر مستوى، خمّن الرقم ضمن النطاق قبل انتهاء المحاولات — مع تخمينات سابقة وتلميحات ذكية وجداول أداء محفوظة.</p>
      </div>
    </header>

    <section class="play" aria-live="polite">
      <div class="panel">
        <div class="controls" role="region" aria-label="التحكم">
          <label for="level" class="small">المستوى</label>
          <select id="level" class="btn" aria-label="اختر مستوى">
            <option value="easy">سهل — 1 إلى 10 (10 محاولات)</option>
            <option value="medium">متوسّط — 1 إلى 100 (7 محاولات)</option>
            <option value="hard">صعب — 1 إلى 1000 (10 محاولات)</option>
          </select>

          <button id="newBtn" class="btn primary" title="ابدأ لعبة جديدة (N)">ابدأ لعبة جديدة</button>
          <button id="hintBtn" class="btn ghost" title="طلب تلميح (H)">تلميح</button>
          <button id="resetScore" class="btn" title="مسح أفضل نتيجة">مسح أفضل نتيجة</button>
        </div>

        <div class="meta small">
          <div>أفضل نتيجة: <span id="bestScore">—</span></div>
          <div class="small">اضغط <kbd>N</kbd> لبدء جديد — <kbd>H</kbd> تلميح — <kbd>Enter</kbd> لتخمين</div>
        </div>
      </div>

      <div class="status" role="status" aria-live="polite">
        <div>
          <div class="muted">النطاق الحالي</div>
          <div class="big" id="rangeDisplay">1 — 10</div>
          <div class="muted">محاولات متبقية: <strong id="attemptsLeft">10</strong></div>
        </div>

        <div style="min-width:220px;display:flex;flex-direction:column;gap:10px;align-items:flex-end">
          <div class="input-row" role="form" aria-label="نموذج التخمين">
            <input id="guessInput" type="number" inputmode="numeric" aria-label="أدخل رقمك" placeholder="رقم..." autocomplete="off" />
            <button id="submitBtn" class="btn primary">خمن</button>
          </div>
          <div class="hints" id="hintArea">ابدأ اللعب! اختر مستوى ثم اضغط "ابدأ لعبة جديدة".</div>
        </div>
      </div>

      <div class="panel" style="align-items:flex-start">
        <div style="flex:1">
          <div class="card">
            <div class="small">التخمينات السابقة</div>
            <ul id="guesses" class="guesses" aria-live="polite"></ul>
          </div>

          <div style="margin-top:10px" class="card small">
            <div>سجل الجولات (محفوظ محليًا)</div>
            <div id="history" class="small" style="margin-top:6px;max-height:120px;overflow:auto"></div>
          </div>
        </div>

        <aside aria-label="الإحصائيات">
          <div class="card meta">
            <div class="small">إحصائيات اللعب</div>
            <div class="small">جلسة</div>
          </div>

          <div class="card stats">
            <div class="badge">فازت: <strong id="wins">0</strong></div>
            <div class="badge">خسرت: <strong id="losses">0</strong></div>
            <div class="badge">أسرع فوز (محاولات): <strong id="fastest">—</strong></div>
          </div>

          <div class="card">
            <div class="small">نصائح</div>
            <ol class="small" style="margin:8px 0 0 0;padding-left:18px;color:var(--muted)">
              <li>جرّب مستويات مختلفة</li>
              <li>استخدم التلميحات بحكمة (تقلل المحاولات المتبقية أحيانًا)</li>
              <li>الهدف: الفوز بعدد محاولات قليل لحفظ أفضل نتيجة</li>
            </ol>
          </div>
        </aside>
      </div>

      <footer class="help">مُصممة للعرض على جهاز سطح مكتب أو هاتف — مدعومة من التخزين المحلي.</footer>
    </section>
  </main>

  <script>
    // ====== Game logic ======
    (() => {
      // DOM
      const newBtn = document.getElementById('newBtn');
      const submitBtn = document.getElementById('submitBtn');
      const hintBtn = document.getElementById('hintBtn');
      const levelSelect = document.getElementById('level');
      const guessInput = document.getElementById('guessInput');
      const guessesList = document.getElementById('guesses');
      const hintArea = document.getElementById('hintArea');
      const rangeDisplay = document.getElementById('rangeDisplay');
      const attemptsLeftEl = document.getElementById('attemptsLeft');
      const bestScoreEl = document.getElementById('bestScore');
      const historyEl = document.getElementById('history');
      const winsEl = document.getElementById('wins');
      const lossesEl = document.getElementById('losses');
      const fastestEl = document.getElementById('fastest');
      const resetScoreBtn = document.getElementById('resetScore');

      // State
      let secret = null;
      let min = 1, max = 10;
      let attemptsLeft = 10;
      let maxAttempts = 10;
      let guesses = [];
      let playing = false;
      let wins = 0, losses = 0, fastest = null;

      // Storage keys
      const STORAGE_KEY = 'guess-the-number-stats-v1';
      const HISTORY_KEY = 'guess-the-number-history-v1';

      // Sound simple
      function beep(freq=440, time=0.08, vol=0.07){
        try{
          const ctx = new (window.AudioContext || window.webkitAudioContext)();
          const o = ctx.createOscillator();
          const g = ctx.createGain();
          o.type = 'sine';
          o.frequency.value = freq;
          g.gain.value = vol;
          o.connect(g);
          g.connect(ctx.destination);
          o.start();
          setTimeout(()=>{ o.stop(); ctx.close(); }, time*1000);
        }catch(e){}
      }

      // Helpers
      function randInt(a,b){ return Math.floor(Math.random()*(b-a+1))+a }
      function saveStats(){
        const payload = {best: fastest, wins, losses};
        localStorage.setItem(STORAGE_KEY, JSON.stringify(payload));
      }
      function loadStats(){
        const raw = localStorage.getItem(STORAGE_KEY);
        if(raw) {
          try{
            const p = JSON.parse(raw);
            fastest = p.best ?? null;
            wins = p.wins ?? 0;
            losses = p.losses ?? 0;
          }catch(e){}
        }
        renderStats();
      }
      function saveHistory(entry){
        const raw = localStorage.getItem(HISTORY_KEY);
        let arr = [];
        try{ arr = raw ? JSON.parse(raw) : []; }catch(e){}
        arr.unshift(entry);
        arr = arr.slice(0,12);
        localStorage.setItem(HISTORY_KEY, JSON.stringify(arr));
        renderHistory();
      }
      function renderHistory(){
        const raw = localStorage.getItem(HISTORY_KEY);
        let arr = [];
        try{ arr = raw ? JSON.parse(raw) : []; }catch(e){}
        if(arr.length===0){ historyEl.innerHTML = '<div class="small" style="color:var(--muted)">لا توجد جولات سابقة</div>'; return }
        historyEl.innerHTML = arr.map(it=>`<div class="small">[${it.date}] ${it.level} — ${it.result} في ${it.attempts} محاولات</div>`).join('');
      }
      function renderStats(){
        winsEl.textContent = wins;
        lossesEl.textContent = losses;
        fastestEl.textContent = fastest === null ? '—' : fastest;
        bestScoreEl.textContent = fastest === null ? '—' : `${fastest} محاولات`;
      }

      function updateRange(){
        rangeDisplay.textContent = `${min} — ${max}`;
      }
      function updateAttempts(){
        attemptsLeftEl.textContent = attemptsLeft;
      }

      function startNewGame(){
        // Configure by level
        const level = levelSelect.value;
        if(level === 'easy'){ min=1; max=10; maxAttempts=10 }
        else if(level === 'medium'){ min=1; max=100; maxAttempts=7 }
        else if(level === 'hard'){ min=1; max=1000; maxAttempts=10 }

        secret = randInt(min,max);
        attemptsLeft = maxAttempts;
        guesses = [];
        playing = true;
        updateRange();
        updateAttempts();
        hintArea.textContent = `لعبة جديدة جاهزة — اختر رقم بين ${min} و ${max} واحاول!`;
        guessesList.innerHTML = '';
        guessInput.value = '';
        guessInput.placeholder = `أدخل رقم بين ${min} و ${max}`;
        guessInput.focus();
        // small cheer sound
        beep(880, 0.07, 0.06);
      }

      function endGame(win, info={}){
        playing = false;
        if(win){
          wins++;
          const attemptsUsed = info.attemptsUsed ?? (maxAttempts - attemptsLeft + 1);
          // update fastest
          if(fastest === null || attemptsUsed < fastest){
            fastest = attemptsUsed;
            hintArea.innerHTML = `<span class="result-win">ممتاز! فزت في ${attemptsUsed} محاولات — رقمك أفضل من سابقاً.</span>`;
          } else {
            hintArea.innerHTML = `<span class="result-win">فزت! استهلكت ${attemptsUsed} محاولات.</span>`;
          }
          // record
          saveHistory({
            date: new Date().toLocaleString(),
            level: levelSelect.options[levelSelect.selectedIndex].text,
            result: 'فوز',
            attempts: attemptsUsed
          });
          beep(660, 0.12, 0.08);
        } else {
          losses++;
          hintArea.innerHTML = `<span class="result-lose">انتهت المحاولات — الرقم كان ${secret}. جرب مجدداً!</span>`;
          saveHistory({
            date: new Date().toLocaleString(),
            level: levelSelect.options[levelSelect.selectedIndex].text,
            result: 'خسارة',
            attempts: maxAttempts
          });
          beep(220, 0.14, 0.06);
        }
        saveStats();
        renderStats();
      }

      function addGuessDisplay(value, note=''){
        const li = document.createElement('li');
        li.className = 'guess';
        li.innerHTML = `<span>${value}</span><span class="small">${note}</span>`;
        guessesList.prepend(li);
      }

      function makeHint(guess){
        // Smart hints:
        // - high/low
        // - "قريب جدًا" if within 2 for small ranges or within 3% for big ranges
        const diff = Math.abs(secret - guess);
        let msg = '';
        if(guess === secret){ msg = 'هذا هو الرقم تمامًا!'; }
        else if(guess < secret){ msg = 'أعلى قليلاً.'; }
        else { msg = 'أقل قليلاً.'; }

        // closeness
        const range = max - min + 1;
        const pct = (diff / range) * 100;
        if(diff === 0) { /* nothing */ }
        else if(range <= 20){
          if(diff <= 2) msg += ' 🔥 قريب جدًا!';
          else if(diff <= 5) msg += ' قريب.';
        } else {
          if(pct <= 1.5) msg += ' 🔥 قريب جدًا!';
          else if(pct <= 4) msg += ' قريب.';
        }
        return msg;
      }

      // Event handlers
      newBtn.addEventListener('click', ()=> startNewGame());
      resetScoreBtn.addEventListener('click', ()=>{
        if(confirm('هل تريد مسح أفضل نتيجة وسجل الإحصائيات؟')) {
          localStorage.removeItem(STORAGE_KEY);
          localStorage.removeItem(HISTORY_KEY);
          fastest = null; wins = 0; losses = 0;
          renderStats();
          renderHistory();
        }
      });

      submitBtn.addEventListener('click', onGuess);
      hintBtn.addEventListener('click', onRequestHint);

      function onGuess(e){
        if(!playing){ hintArea.textContent = 'اللعبة غير مفعّلة — اضغط "ابدأ لعبة جديدة".'; return }
        const raw = guessInput.value;
        if(raw === '') { hintArea.textContent = 'المرجو إدخال رقم.'; return }
        const g = Number(raw);
        if(Number.isNaN(g) || !Number.isInteger(g)) { hintArea.textContent = 'أدخل عددًا صحيحًا صالحًا.'; return }
        if(g < min || g > max){ hintArea.textContent = `الرقم خارج النطاق (${min} — ${max}).`; return }

        // process guess
        guesses.push(g);
        attemptsLeft--;
        updateAttempts();

        // create note
        let note = '';
        if(g === secret){
          addGuessDisplay(g, '✅ صحيح');
          endGame(true, { attemptsUsed: maxAttempts - attemptsLeft });
        } else {
          note = makeHint(g);
          addGuessDisplay(g, note);
          if(attemptsLeft <= 0){
            endGame(false);
          } else {
            hintArea.textContent = note + ` — محاولات متبقية: ${attemptsLeft}`;
            // small negative beep for wrong guess
            beep(240, 0.06, 0.04);
          }
        }
        guessInput.value = '';
        guessInput.focus();
      }

      function onRequestHint(){
        if(!playing){ hintArea.textContent = 'اللعبة غير مفعّلة — اضغط "ابدأ لعبة جديدة".'; return }
        // Hints cost one attempt (configurable)
        if(attemptsLeft <= 1){
          hintArea.textContent = 'لا يمكنك طلب تلميح الآن — المحاولات المتبقية قليلة.';
          return;
        }
        attemptsLeft--;
        updateAttempts();

        // Determine hint type (randomized)
        const type = randInt(1,3);
        let txt = '';
        if(type === 1){
          // parity hint
          txt = (secret % 2 === 0) ? 'الرقم زوجي.' : 'الرقم فردي.';
        } else if(type === 2){
          // divisible hint
          const divisors = [3,5,7,11].filter(d => secret % d === 0);
          txt = divisors.length ? `الرقم يقبل القسمة على ${divisors.join(', ')}.` : 'لا يقبل القسمة على 3 أو 5 أو 7 أو 11.';
        } else {
          // range shrink hint
          // give a subrange of size ~25% of full range that contains secret
          const size = Math.max(2, Math.floor((max - min + 1) * 0.25));
          let start = Math.max(min, secret - Math.floor(size/2));
          let end = Math.min(max, start + size - 1);
          // adjust start if near end
          start = Math.max(min, end - size + 1);
          txt = `الرقم بين ${start} و ${end}.`;
        }

        hintArea.textContent = `تلميح: ${txt} (تكلّف محاولة) — محاولات متبقية: ${attemptsLeft}`;
        beep(520, 0.08, 0.06);
      }

      // Keyboard shortcuts
      document.addEventListener('keydown', (e)=>{
        if(e.key === 'N' || e.key === 'n'){ newBtn.click(); e.preventDefault(); }
        if(e.key === 'H' || e.key === 'h'){ hintBtn.click(); e.preventDefault(); }
        if(e.key === 'Enter'){ submitBtn.click(); }
      });

      // Initialization
      loadStats();
      renderHistory();

      // auto-start first game for convenience
      startNewGame();

      // Accessibility: prevent scroll on number input with arrow keys (optional)
      guessInput.addEventListener('keydown', (e)=>{
        if(e.key === 'ArrowUp' || e.key === 'ArrowDown') e.preventDefault();
      });

      // small utility: show range when level changes (no restart)
      levelSelect.addEventListener('change', ()=>{
        const current = levelSelect.value;
        if(current === 'easy'){ min=1; max=10; }
        else if(current === 'medium'){ min=1; max=100; }
        else { min=1; max=1000; }
        updateRange();
        hintArea.textContent = `تم تغيير المستوى مؤقتًا (لم تبدأ جولة جديدة). اضغط "ابدأ لعبة جديدة" لبدء مستوى ${levelSelect.options[levelSelect.selectedIndex].text}.`;
      });

    })();
  </script>
</body>
</html>
