<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Mastery: Giovanni's Challenge</title>
    <style>
        :root {
            --primary: #2c3e50;
            --accent: #3498db;
            --correct: #27ae60;
            --typo: #f1c40f;
            --wrong: #e74c3c;
            --bg: #f8f9fa;
        }

        body {
            font-family: 'Helvetica Neue', Arial, sans-serif;
            background-color: var(--bg);
            color: var(--primary);
            margin: 0;
            display: flex;
            justify-content: center;
            padding: 20px;
        }

        .app-container {
            max-width: 700px;
            width: 100%;
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.1);
        }

        .slide { display: none; }
        .active { display: block; animation: slideIn 0.4s ease-out; }

        @keyframes slideIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        h1, h2 { color: var(--accent); }
        p { line-height: 1.6; }

        .btn {
            background: var(--accent);
            color: white;
            border: none;
            padding: 12px 28px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s;
            margin-top: 20px;
        }

        .btn:hover { background: #2980b9; transform: scale(1.05); }

        .study-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin: 25px 0;
        }

        .word-card {
            background: #f1f3f5;
            padding: 15px;
            border-radius: 12px;
            cursor: pointer;
            text-align: center;
            border: 2px solid transparent;
        }

        .word-card:hover { border-color: var(--accent); }

        .exercise-block {
            text-align: left;
            margin-bottom: 25px;
            padding: 15px;
            border-left: 4px solid #eee;
        }

        input {
            border: 2px solid #ddd;
            padding: 8px 12px;
            border-radius: 8px;
            font-size: 16px;
            width: 180px;
            transition: 0.3s;
        }

        .explanation {
            font-size: 14px;
            color: #666;
            margin-top: 8px;
            background: #f9f9f9;
            padding: 10px;
            border-radius: 6px;
            display: none;
        }

        /* Status Colors */
        .correct { border-color: var(--correct); background: #eafaf1; }
        .typo { border-color: var(--typo); background: #fffde7; }
        .wrong { border-color: var(--wrong); background: #fdf2f2; }

        .scenario-box {
            background: #eef2f7;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            border: 1px solid #d1d9e6;
        }

    </style>
</head>
<body>

<div class="app-container">
    <div class="slide active" id="s1">
        <h1>Welcome back, Giovanni! 🚀</h1>
        <p>Dom mi ha detto che stai andando alla grande. Oggi alziamo il livello: <strong>niente aiuti!</strong></p>
        <p>Dovrai ricordare tu quando usare <em>-ing</em> o <em>to + infinito</em>.</p>
        <button class="btn" onclick="goTo(2)">Let's start</button>
    </div>

    <div class="slide" id="s2">
        <h2>Study List 🔊</h2>
        <p>Clicca sulle parole per ascoltare la pronuncia (Native Speaker Voice).</p>
        <div class="study-grid" id="study-list"></div>
        <button class="btn" onclick="goTo(3)">Challenge 1: Writing</button>
    </div>

    <div class="slide" id="s3">
        <h2>Fill in the Gaps ✍️</h2>
        <p>Completa le frasi usando il verbo tra parentesi. Niente suggerimenti!</p>
        <div id="gap-container"></div>
        <button class="btn" onclick="checkGaps()">Check Answers</button>
        <button class="btn" onclick="goTo(4)" style="background:#8e44ad">Next Game</button>
    </div>

    <div class="slide" id="s4">
        <h2>The Context Challenge 🧠</h2>
        <p>Scegli l'opzione corretta in base alla situazione descritta.</p>
        <div id="context-container"></div>
        <button class="btn" onclick="location.reload()">Reset Lessons</button>
    </div>
</div>

<script>
    const vocabulary = [
        { word: 'Avoid', sound: 'Avoid' }, { word: 'Decide', sound: 'Decide' },
        { word: 'Enjoy', sound: 'Enjoy' }, { word: 'Hope', sound: 'Hope' },
        { word: 'Manage', sound: 'Manage' }, { word: 'Suggest', sound: 'Suggest' },
        { word: 'Remember', sound: 'Remember' }, { word: 'Stop', sound: 'Stop' }
    ];

    const gapExercises = [
        { q: "I really enjoy _________ (travel) by train.", a: "travelling", why: "Dopo 'Enjoy' si usa sempre la forma -ing." },
        { q: "We decided _________ (move) to London.", a: "to move", why: "Il verbo 'Decide' vuole l'infinito per indicare una scelta futura." },
        { q: "_________ (exercise) is good for your mind.", a: "exercising", why: "Quando il verbo è il SOGGETTO della frase, usiamo -ing." },
        { q: "I'm looking forward to _________ (meet) you.", a: "meeting", why: "In questa espressione, 'to' è una preposizione, quindi segue -ing." },
        { q: "She managed _________ (finish) the report.", a: "to finish", why: "'Manage' (riuscire a) richiede sempre 'to + infinito'." },
        { q: "Stop _________ (make) so much noise!", a: "making", why: "Usa -ing quando vuoi dire di cessare un'attività che stai facendo." },
        { q: "I went home _________ (eat) something.", a: "to eat", why: "Usiamo 'to' per esprimere lo SCOPO (Perché sei andato a casa?)." },
        { q: "Avoid _________ (drink) too much coffee.", a: "drinking", why: "Dopo 'Avoid' usiamo sempre la forma -ing." },
        { q: "I hope _________ (see) you on Tuesday.", a: "to see", why: "'Hope' esprime un desiderio futuro, quindi vuole l'infinito." },
        { q: "He suggested _________ (go) to the cinema.", a: "going", why: "Dopo 'Suggest' la grammatica richiede la forma -ing." },
        { q: "Are you interested in _________ (learn) English?", a: "learning", why: "Dopo le preposizioni (in, of, at...) si usa sempre -ing." },
        { q: "Don't forget _________ (lock) the door.", a: "to lock", why: "'Forget/Remember' + to si usa per azioni future da non dimenticare." }
    ];

    const contextExercises = [
        {
            scenario: "Hai il vizio del fumo e decidi di smettere per sempre.",
            options: ["I stopped smoking", "I stopped to smoke"],
            correct: 0,
            why: "'Stop smoking' significa smettere l'azione. 'Stop to smoke' significa fermarsi per fumare."
        },
        {
            scenario: "Stavi camminando e ti sei fermato perché volevi guardare una vetrina.",
            options: ["I stopped to look", "I stopped looking"],
            correct: 0,
            why: "Qui usi 'to' perché indica lo scopo: ti sei fermato PER guardare."
        },
        {
            scenario: "Hai un ricordo di quando eri piccolo e giocavi al parco.",
            options: ["I remember to play", "I remember playing"],
            correct: 1,
            why: "Usa -ing con 'remember' quando ti riferisci a un ricordo del passato."
        }
    ];

    function goTo(n) {
        document.querySelectorAll('.slide').forEach(s => s.classList.remove('active'));
        document.getElementById('s' + n).classList.add('active');
    }

    function speak(text) {
        const msg = new SpeechSynthesisUtterance();
        msg.text = text;
        msg.lang = 'en-US';
        msg.rate = 0.8;
        window.speechSynthesis.speak(msg);
    }

    function levenshtein(a, b) {
        if(!a || !b) return 99;
        const m = [], al = a.length, bl = b.length;
        for (let i = 0; i <= al; i++) m[i] = [i];
        for (let j = 1; j <= bl; j++) m[0][j] = j;
        for (let i = 1; i <= al; i++)
            for (let j = 1; j <= bl; j++)
                m[i][j] = Math.min(m[i-1][j-1] + (a[i-1] !== b[j-1]), m[i][j-1] + 1, m[i-1][j] + 1);
        return m[al][bl];
    }

    // Initialize
    window.onload = () => {
        const list = document.getElementById('study-list');
        vocabulary.forEach(v => {
            const d = document.createElement('div');
            d.className = 'word-card';
            d.innerHTML = `<strong>${v.word}</strong> 🔊`;
            d.onclick = () => speak(v.word);
            list.appendChild(d);
        });

        const gapCont = document.getElementById('gap-container');
        gapExercises.forEach((ex, i) => {
            const d = document.createElement('div');
            d.className = 'exercise-block';
            d.innerHTML = `
                <p>${i+1}. ${ex.q}</p>
                <input type="text" id="in-${i}">
                <div id="exp-${i}" class="explanation"></div>
            `;
            gapCont.appendChild(d);
        });

        const ctxCont = document.getElementById('context-container');
        contextExercises.forEach((ex, i) => {
            const d = document.createElement('div');
            d.className = 'scenario-box';
            d.innerHTML = `
                <p><strong>Scenario:</strong> ${ex.scenario}</p>
                ${ex.options.map((opt, optIdx) => `
                    <button class="btn" style="background:#95a5a6; margin:5px" onclick="checkCtx(${i}, ${optIdx}, this)">
                        ${opt}
                    </button>
                `).join('')}
                <div id="ctx-exp-${i}" class="explanation"></div>
            `;
            ctxCont.appendChild(d);
        });
    };

    function checkGaps() {
        gapExercises.forEach((ex, i) => {
            const input = document.getElementById('in-' + i);
            const exp = document.getElementById('exp-' + i);
            const val = input.value.trim().toLowerCase();
            const ans = ex.a.toLowerCase();
            
            exp.style.display = 'block';
            exp.innerHTML = `<strong>Why?</strong> ${ex.why}`;

            if (val === ans) {
                input.className = 'correct';
            } else if (levenshtein(val, ans) <= 2) {
                input.className = 'typo';
                exp.innerHTML = `<span style="color:#d4ac0d">Typo! Correct spelling: <strong>${ex.a}</strong></span><br>` + exp.innerHTML;
            } else {
                input.className = 'wrong';
            }
        });
    }

    function checkCtx(qIdx, optIdx, btn) {
        const ex = contextExercises[qIdx];
        const exp = document.getElementById('ctx-exp-' + qIdx);
        exp.style.display = 'block';
        exp.innerHTML = `<strong>Explanation:</strong> ${ex.why}`;
        
        if (optIdx === ex.correct) {
            btn.style.background = 'var(--correct)';
        } else {
            btn.style.background = 'var(--wrong)';
        }
    }
</script>

</body>
</html>
