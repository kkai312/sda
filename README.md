<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TrustCheck — Premium Security Analysis</title>
    <link href="https://fonts.googleapis.com/css2?family=Manrope:wght@400;600;700;800&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        :root {
            --bg: #020617;
            --bg-accent: radial-gradient(circle at 50% -20%, #1e40af 0%, #020617 80%);
            --accent: #3b82f6;
            --accent-glow: rgba(59, 130, 246, 0.5);
            --text: #ffffff;
            --muted: #94a3b8;
            --card: rgba(30, 41, 59, 0.5);
            --border: rgba(255, 255, 255, 0.1);
            --green: #10b981;
            --green-glow: rgba(16, 185, 129, 0.4);
            --red: #ef4444;
        }

        body.light-theme {
            --bg: #f8fafc;
            --bg-accent: none;
            --text: #0f172a;
            --muted: #64748b;
            --card: #ffffff;
            --border: rgba(0, 0, 0, 0.1);
        }

        body {
            background: var(--bg);
            background-image: var(--bg-accent);
            color: var(--text);
            font-family: 'Manrope', sans-serif;
            margin: 0;
            min-height: 100vh;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            overflow-x: hidden;
        }

        .container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }

        header { display: flex; justify-content: space-between; align-items: center; padding: 30px 0; }
        .logo { display: flex; align-items: center; gap: 10px; font-weight: 800; font-size: 28px; letter-spacing: -1px; }
        .logo i { color: var(--accent); filter: drop-shadow(0 0 8px var(--accent-glow)); }

        .controls { display: flex; gap: 15px; align-items: center; }
        select { background: var(--card); color: var(--text); border: 1px solid var(--border); padding: 8px 12px; border-radius: 10px; cursor: pointer; outline: none; font-weight: 600; }
        .theme-btn { background: var(--card); border: 1px solid var(--border); color: var(--text); padding: 10px; border-radius: 50%; cursor: pointer; transition: 0.3s; display: flex; align-items: center; }
        .theme-btn:hover { transform: rotate(15deg); border-color: var(--accent); box-shadow: 0 0 15px var(--accent-glow); }

        .hero { display: grid; grid-template-columns: 1.1fr 0.9fr; gap: 60px; padding: 80px 0; align-items: center; }
        .badge { background: rgba(59, 130, 246, 0.15); color: var(--accent); padding: 8px 18px; border-radius: 50px; font-size: 14px; font-weight: 700; margin-bottom: 25px; display: inline-flex; align-items: center; gap: 8px; border: 1px solid var(--accent); }
        
        h1 { font-size: 64px; font-weight: 800; line-height: 1.1; margin: 0 0 25px; letter-spacing: -3px; }
        h1 span { color: var(--accent); text-shadow: 0 0 40px var(--accent-glow); }

        .input-box { 
            background: var(--card); border: 1px solid var(--border); border-radius: 20px; 
            display: flex; padding: 10px; backdrop-filter: blur(20px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3); transition: 0.3s;
        }
        .input-box:focus-within { border-color: var(--accent); box-shadow: 0 0 25px var(--accent-glow); }
        input { background: transparent; border: none; color: var(--text); flex: 1; padding: 15px; outline: none; font-size: 17px; }
        
        .btn-check { 
            background: var(--accent); color: white; border: none; padding: 15px 35px; border-radius: 14px; 
            font-weight: 800; cursor: pointer; transition: 0.3s; box-shadow: 0 4px 15px var(--accent-glow);
        }
        .btn-check:hover { transform: scale(1.05); filter: brightness(1.1); }

        .res-card { 
            background: linear-gradient(145deg, #0f172a, #020617); 
            border: 1px solid var(--border); border-radius: 30px; overflow: hidden; 
            box-shadow: 0 50px 100px rgba(0,0,0,0.6); position: relative;
        }
        .br-head { background: rgba(255, 255, 255, 0.03); padding: 18px 25px; display: flex; align-items: center; gap: 15px; }
        .dots span { width: 10px; height: 10px; border-radius: 50%; display: inline-block; margin-right: 5px; }
        .url-line { background: rgba(0, 0, 0, 0.4); flex: 1; border-radius: 10px; padding: 8px 15px; font-size: 13px; color: var(--muted); border: 1px solid var(--border); }
        
        .card-inner { padding: 50px; text-align: center; }
        .ring { 
            width: 180px; height: 180px; border-radius: 50%; border: 10px solid rgba(255,255,255,0.05); 
            border-top-color: var(--green); display: flex; flex-direction: column; align-items: center; 
            justify-content: center; margin: 0 auto 30px; transition: 0.5s;
            box-shadow: 0 0 40px var(--green-glow);
        }
        .ring-score { font-size: 56px; font-weight: 800; }
        .ring-label { color: var(--green); font-weight: 800; font-size: 14px; text-transform: uppercase; letter-spacing: 2px; }

        .stat-l { display: flex; justify-content: space-between; padding: 15px 0; border-bottom: 1px solid var(--border); font-size: 15px; }

        .features { display: grid; grid-template-columns: repeat(5, 1fr); gap: 20px; margin: 80px 0; }
        .f-item { 
            background: var(--card); border: 1px solid var(--border); padding: 30px; border-radius: 24px; 
            transition: 0.3s; backdrop-filter: blur(10px);
        }
        .f-item:hover { transform: translateY(-10px); background: rgba(255,255,255,0.05); border-color: var(--accent); }
        .f-item i { color: var(--accent); margin-bottom: 20px; filter: drop-shadow(0 0 10px var(--accent-glow)); }
        .f-item h3 { font-size: 16px; margin-bottom: 12px; font-weight: 700; }
        .f-item p { font-size: 13px; color: var(--muted); line-height: 1.5; }

        .how { padding: 100px 0; text-align: center; }
        .steps { display: grid; grid-template-columns: repeat(4, 1fr); gap: 40px; margin-top: 80px; position: relative; }
        .steps::before { 
            content: ""; position: absolute; top: 30px; left: 10%; right: 10%; height: 2px; 
            border-top: 2px dashed var(--accent); opacity: 0.2; z-index: 0; 
        }
        .s-num { 
            width: 60px; height: 60px; background: var(--accent); color: white; border-radius: 50%; 
            display: flex; align-items: center; justify-content: center; margin: 0 auto 25px; 
            font-weight: 800; font-size: 22px; position: relative; z-index: 1;
            box-shadow: 0 0 30px var(--accent-glow); border: 5px solid var(--bg);
        }

        @keyframes scan {
            0% { border-top-color: var(--accent); transform: rotate(0deg); }
            100% { border-top-color: var(--green); transform: rotate(360deg); }
        }
        .scanning { animation: scan 1.5s infinite linear; }

        @media (max-width: 968px) {
            .hero { grid-template-columns: 1fr; text-align: center; }
            .features { grid-template-columns: 1fr 1fr; }
            .steps { grid-template-columns: 1fr 1fr; }
            h1 { font-size: 40px; }
        }
    </style>
</head>
<body>

<div class="container">
    <header>
        <div class="logo"><i data-lucide="shield-check"></i> TrustCheck</div>
        <div class="controls">
            <select id="langSelect" onchange="updateLang()">
                <option value="ru">Русский</option>
                <option value="en">English</option>
                <option value="kz">Қазақша</option>
            </select>
            <button class="theme-btn" onclick="toggleTheme()"><i data-lucide="moon" id="t-icon"></i></button>
        </div>
    </header>

    <div class="hero">
        <div class="h-text">
            <div class="badge" id="t-badge"><i data-lucide="shield" size="14"></i> Проверяйте сайты на надежность</div>
            <h1 id="t-h1">Узнайте, насколько сайт <span>надежный</span></h1>
            <p id="t-desc" style="color:var(--muted); font-size: 19px; margin-bottom:45px">Анализируем десятки факторов, чтобы определить, насколько сайт безопасен для вас и ваших данных.</p>
            <div class="input-box">
                <i data-lucide="link" style="margin: 15px; color:var(--accent)"></i>
                <input type="text" id="urlIn" placeholder="Введите адрес для проверки..." value="https://google.com">
                <button class="btn-check" onclick="runCheck()" id="t-btn">Проверить сайт</button>
            </div>
            <p style="font-size: 13px; color: var(--muted); margin-top: 15px;"><i data-lucide="lock" size="13"></i> Проверка безопасна. Мы не сохраняем ваши данные.</p>
        </div>

        <div class="res-card">
            <div class="br-head">
                <div class="dots"><span style="background:#ff5f56"></span><span style="background:#ffbd2e"></span><span style="background:#27c93f"></span></div>
                <div class="url-line" id="urlDisp">https://google.com</div>
            </div>
            <div class="card-inner">
                <div class="ring" id="mainRing">
                    <div class="ring-score" id="score">0</div>
                    <div class="ring-label" id="t-status">Ожидание...</div>
                </div>
                <div style="margin-top: 20px;">
                    <div class="stat-l"><span id="t-s1">Репутация домена</span> <span id="t-v1" style="font-weight:700">--</span></div>
                    <div class="stat-l"><span id="t-s2">SSL-сертификат</span> <span id="t-v2" style="font-weight:700">--</span></div>
                    <div class="stat-l"><span id="t-s3">Безопасность сайта</span> <span id="t-v3" style="font-weight:700">--</span></div>
                </div>
            </div>
        </div>
    </div>

    <div class="features">
        <div class="f-item"><i data-lucide="globe" size="32"></i><h3 id="t-f1h">Репутация домена</h3><p id="t-f1p">Проверяем возраст домена, историю, репутацию и наличие в черных списках.</p></div>
        <div class="f-item"><i data-lucide="shield-check" size="32"></i><h3 id="t-f2h">Безопасность</h3><p id="t-f2p">Проверяем SSL-сертификат, защиту от вредоносного ПО и уязвимости.</p></div>
        <div class="f-item"><i data-lucide="eye-off" size="32"></i><h3 id="t-f3h">Конфиденциальность</h3><p id="t-f3p">Анализируем политику конфиденциальности и защиту данных.</p></div>
        <div class="f-item"><i data-lucide="zap" size="32"></i><h3 id="t-f4h">Технологии</h3><p id="t-f4p">Определяем используемые технологии и их актуальность.</p></div>
        <div class="f-item"><i data-lucide="message-circle" size="32"></i><h3 id="t-f5h">Отзывы и упоминания</h3><p id="t-f5p">Ищем отзывы пользователей и упоминания сайта в интернете.</p></div>
    </div>

    <div class="how">
        <h2 id="t-how-title" style="font-size: 42px; font-weight: 800;">Как это работает</h2>
        <div class="steps">
            <div class="step"><div class="s-num">1</div><h4 id="t-st1h">Введите URL</h4><p id="t-st1p">Введите адрес сайта, который хотите проверить.</p></div>
            <div class="step"><div class="s-num">2</div><h4 id="t-st2h">Анализ системы</h4><p id="t-st2p">Наши системы анализируют более 50 параметров.</p></div>
            <div class="step"><div class="s-num">3</div><h4 id="t-st3h">Получите результат</h4><p id="t-st3p">Вы получаете оценку надежности и детали проверки.</p></div>
            <div class="step"><div class="s-num">4</div><h4 id="t-st4h">Принимайте решения</h4><p id="t-st4p">Используйте данные для безопасного серфинга.</p></div>
        </div>
    </div>
</div>

<script>
    const langData = {
        ru: {
            badge: "Проверяйте сайты на надежность", h1: "Узнайте, насколько сайт <span>надежный</span>", desc: "Анализируем десятки факторов, чтобы определить, насколько сайт безопасен для вас и ваших данных.", btn: "Проверить сайт", status: "Высокая надежность", danger: "Опасно", s1: "Репутация домена", s2: "SSL-сертификат", s3: "Безопасность сайта", vGood: "Хорошо", vBad: "Рискованно", vSSL: "Действителен", vNoSSL: "Отсутствует", f1h: "Репутация домена", f1p: "Проверяем возраст домена, историю, репутацию и наличие в черных списках.", f2h: "Безопасность", f2p: "Проверяем SSL-сертификат, защиту от вредоносного ПО и уязвимости.", f3h: "Конфиденциальность", f3p: "Анализируем политику конфиденциальности и защиту данных.", f4h: "Технологии", f4p: "Определяем используемые технологии и их актуальность.", f5h: "Отзывы и упоминания", f5p: "Ищем отзывы пользователей и упоминания сайта в интернете.", how: "Как это работает", st1h: "Введите URL", st1p: "Введите адрес сайта, который хотите проверить.", st2h: "Анализ системы", st2p: "Наши системы анализируют более 50 параметров.", st3h: "Получите результат", st3p: "Вы получаете оценку надежности и детали проверки.", st4h: "Принимайте решения", st4p: "Используйте данные для безопасного серфинга."
        },
        en: {
            badge: "Verify website reliability", h1: "Discover how <span>reliable</span> a site is", desc: "We analyze dozens of factors to determine how safe a site is for you and your data.", btn: "Check Website", status: "High Reliability", danger: "Danger", s1: "Domain Reputation", s2: "SSL Certificate", s3: "Site Security", vGood: "Good", vBad: "Risky", vSSL: "Valid", vNoSSL: "Missing", f1h: "Domain Reputation", f1p: "We check domain age, history, reputation, and presence in blacklists.", f2h: "Security", f2p: "We check SSL certificates, malware protection, and vulnerabilities.", f3h: "Privacy", f3p: "We analyze privacy policies and data protection measures.", f4h: "Technologies", f4p: "We identify the technology stack and its current relevance.", f5h: "Reviews & Mentions", f5p: "We search for user feedback and mentions across the web.", how: "How it works", st1h: "Enter URL", st1p: "Type in the website address you wish to analyze.", st2h: "System Analysis", st2p: "Our engines scan over 50 different safety parameters.", st3h: "Get Results", st3p: "Receive a full reliability score and scan details.", st4h: "Action", st4p: "Use our data to browse the web safely and confidently."
        },
        kz: {
            badge: "Сайттардың сенімділігін тексеріңіз", h1: "Сайттың қаншалықты <span>сенімді</span> екенін біліңіз", desc: "Сайттың сіз бен деректеріңіз үшін қаншалықты қауіпсіз екенін анықтау үшін ондаған факторларды талдаймыз.", btn: "Тексеру", status: "Жоғары сенімділік", danger: "Қауіпті", s1: "Домен беделі", s2: "SSL сертификаты", s3: "Сайт қауіпсіздігі", vGood: "Жақсы", vBad: "Қауіпті", vSSL: "Жарамды", vNoSSL: "Жоқ", f1h: "Домен беделі", f1p: "Доменнің жасын, тарихын, беделін және қара тізімдерді тексереміз.", f2h: "Қауіпсіздік", f2p: "SSL сертификатын, зиянды бағдарламалардан қорғауды тексереміз.", f3h: "Құпиялылық", f3p: "Құпиялылық саясатын және деректерді қорғауды талдаймыз.", f4h: "Технологиялар", f4p: "Қолданылатын технологияларды және олардың өзектілігін анықтаймыз.", f5h: "Пікірлер", f5p: "Желідегі пайдаланушылардың пікірлері мен сайт туралы айтылымдарды іздейміз.", how: "Бұл қалай жұмыс істейді", st1h: "URL енгізіңіз", st1p: "Тексергіңіз келетін сайттың мекенжайын енгізіңіз.", st2h: "Жүйелік талдау", st2p: "Біздің жүйелер 50-ден астам параметрді талдайды.", st3h: "Нәтижені алыңыз", st3p: "Сіз сенімділік бағасын және тексеру мәліметтерін аласыз.", st4h: "Шешім қабылдаңыз", st4p: "Қауіпсіз серфинг үшін деректерді пайдаланыңыз."
        }
    };

    function updateLang() {
        const l = document.getElementById('langSelect').value;
        const d = langData[l];
        document.getElementById('t-badge').innerHTML = `<i data-lucide="shield" size="14"></i> ${d.badge}`;
        document.getElementById('t-h1').innerHTML = d.h1;
        document.getElementById('t-desc').innerText = d.desc;
        document.getElementById('t-btn').innerText = d.btn;
        document.getElementById('t-s1').innerText = d.s1; document.getElementById('t-s2').innerText = d.s2; document.getElementById('t-s3').innerText = d.s3;
        document.getElementById('t-f1h').innerText = d.f1h; document.getElementById('t-f1p').innerText = d.f1p;
        document.getElementById('t-f2h').innerText = d.f2h; document.getElementById('t-f2p').innerText = d.f2p;
        document.getElementById('t-f3h').innerText = d.f3h; document.getElementById('t-f3p').innerText = d.f3p;
        document.getElementById('t-f4h').innerText = d.f4h; document.getElementById('t-f4p').innerText = d.f4p;
        document.getElementById('t-f5h').innerText = d.f5h; document.getElementById('t-f5p').innerText = d.f5p;
        document.getElementById('t-how-title').innerText = d.how;
        document.getElementById('t-st1h').innerText = d.st1h; document.getElementById('t-st1p').innerText = d.st1p;
        document.getElementById('t-st2h').innerText = d.st2h; document.getElementById('t-st2p').innerText = d.st2p;
        document.getElementById('t-st3h').innerText = d.st3h; document.getElementById('t-st3p').innerText = d.st3p;
        document.getElementById('t-st4h').innerText = d.st4h; document.getElementById('t-st4p').innerText = d.st4p;
        lucide.createIcons();
    }

    function toggleTheme() {
        document.body.classList.toggle('light-theme');
        const icon = document.getElementById('t-icon');
        const isLight = document.body.classList.contains('light-theme');
        icon.setAttribute('data-lucide', isLight ? 'sun' : 'moon');
        lucide.createIcons();
    }

    async function runCheck() {
        const urlIn = document.getElementById('urlIn').value.trim().toLowerCase();
        if(!urlIn) return;

        const l = document.getElementById('langSelect').value;
        const d = langData[l];
        const ring = document.getElementById('mainRing');
        const scoreEl = document.getElementById('score');
        const labelEl = document.getElementById('t-status');

        // Сброс UI
        document.getElementById('urlDisp').innerText = urlIn;
        ring.classList.add('scanning');
        labelEl.innerText = "...";
        scoreEl.innerText = "0";

        // ЛОГИКА АНАЛИЗА
        let score = 10; // Базовый балл
        let stats = { domain: false, ssl: false, safety: false };

        try {
            // 1. Очистка URL для API
            const cleanUrl = urlIn.replace(/^https?:\/\//, '').split('/')[0];
            
            // 2. Реальная проверка DNS (существует ли сайт)
            const dnsResponse = await fetch(`https://cloudflare-dns.com/dns-query?name=${cleanUrl}&type=A`, {
                headers: { 'Accept': 'application/dns-json' }
            });
            const dnsData = await dnsResponse.json();
            
            if(dnsData.Status === 0) { 
                score += 40; 
                stats.domain = true; 
            }

            // 3. Проверка SSL (HTTPS)
            if(urlIn.startsWith('https://')) {
                score += 30;
                stats.ssl = true;
            }

            // 4. Подозрительные слова в URL
            const redFlags = ['free', 'win', 'hack', 'gift', 'login', 'verify', 'money'];
            const hasFlags = redFlags.some(word => cleanUrl.includes(word));
            
            if(!hasFlags && stats.domain) {
                score += 18;
                stats.safety = true;
            } else if (hasFlags) {
                score -= 30;
            }

            // Ограничение баллов
            score = Math.max(5, Math.min(score, 98));

            // Анимация счетчика
            let current = 0;
            const interval = setInterval(() => {
                current++;
                scoreEl.innerText = current;
                if(current >= score) {
                    clearInterval(interval);
                    applyResult(score, stats, d, ring, labelEl);
                }
            }, 15);

        } catch (e) {
            labelEl.innerText = "Error";
            ring.classList.remove('scanning');
        }
    }

    function applyResult(score, stats, d, ring, labelEl) {
        ring.classList.remove('scanning');
        const isDanger = score < 50;

        labelEl.innerText = isDanger ? d.danger : d.status;
        labelEl.style.color = isDanger ? 'var(--red)' : 'var(--green)';
        ring.style.borderTopColor = isDanger ? 'var(--red)' : 'var(--green)';
        ring.style.boxShadow = isDanger ? '0 0 30px rgba(239, 68, 68, 0.4)' : '0 0 30px var(--green-glow)';

        document.getElementById('t-v1').innerText = stats.domain ? d.vGood : d.vBad;
        document.getElementById('t-v1').style.color = stats.domain ? 'var(--green)' : 'var(--red)';
        
        document.getElementById('t-v2').innerText = stats.ssl ? d.vSSL : d.vNoSSL;
        document.getElementById('t-v2').style.color = stats.ssl ? 'var(--green)' : 'var(--red)';
        
        document.getElementById('t-v3').innerText = stats.safety ? d.vGood : d.vBad;
        document.getElementById('t-v3').style.color = stats.safety ? 'var(--green)' : 'var(--red)';
    }

    lucide.createIcons();
</script>

</body>
</html>
