<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scoreboard Hadangan - Rekap Total</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --red-team: #ef4444;
            --blue-team: #22c55e;
            --accent: #f59e0b;
            --white: #f8fafc;
        }

        body {
            font-family: 'Segoe UI', Roboto, sans-serif;
            background-color: var(--bg-color);
            color: var(--white);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        .container {
            width: 90%;
            max-width: 900px;
            background: var(--card-bg);
            padding: 30px;
            border-radius: 24px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            text-align: center;
        }

        /* Header & Timer */
        .header { margin-bottom: 20px; }
        .timer-box {
            font-size: 4rem;
            font-weight: 800;
            color: var(--accent);
            background: #000;
            padding: 10px 40px;
            border-radius: 15px;
            display: inline-block;
            box-shadow: inset 0 0 10px rgba(245, 158, 11, 0.3);
            margin-bottom: 15px;
        }

        .timer-controls button {
            background: #334155;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 6px;
            cursor: pointer;
            margin: 0 4px;
        }

        /* Scoreboard Layout */
        .scoreboard {
            display: flex;
            gap: 20px;
            margin-top: 30px;
        }

        .team-card {
            flex: 1;
            background: rgba(0,0,0,0.2);
            padding: 20px;
            border-radius: 20px;
            border: 1px solid rgba(255,255,255,0.1);
        }

        .red-border { border-top: 8px solid var(--red-team); }
        .blue-border { border-top: 8px solid var(--blue-team); }

        .team-name {
            font-size: 1.8rem;
            font-weight: bold;
            color: white;
            background: transparent;
            border: none;
            border-bottom: 2px solid rgba(255,255,255,0.1);
            text-align: center;
            width: 90%;
            margin-bottom: 15px;
        }

        /* Poin Per Baris */
        .point-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255,255,255,0.03);
            padding: 12px;
            border-radius: 12px;
            margin-bottom: 10px;
        }

        .point-label { font-size: 0.9rem; font-weight: 600; color: #94a3b8; }
        .point-val { font-size: 1.5rem; font-weight: bold; color: var(--accent); }

        .btn-group button {
            width: 35px;
            height: 35px;
            border-radius: 8px;
            border: none;
            font-weight: bold;
            cursor: pointer;
            margin-left: 5px;
        }

        .btn-plus { background: #10b981; color: white; }
        .btn-minus { background: #f43f5e; color: white; }

        /* KOLOM TOTAL POINT DI BAWAH */
        .total-summary {
            margin-top: 30px;
            padding: 20px;
            background: #000;
            border-radius: 20px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            border: 2px solid var(--accent);
        }

        .summary-item { text-align: center; }
        .summary-label { font-size: 1rem; color: #94a3b8; text-transform: uppercase; letter-spacing: 1px; }
        .summary-score { font-size: 4.5rem; font-weight: 900; }
        
        .score-red { color: var(--red-team); text-shadow: 0 0 15px rgba(239, 68, 68, 0.4); }
        .score-blue { color: var(--blue-team); text-shadow: 0 0 15px rgba(34, 197, 94, 0.4); }
        
        .vs-circle {
            background: var(--accent);
            color: black;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            font-weight: 900;
            font-size: 1.2rem;
        }

        .reset-btn {
            margin-top: 25px;
            background: transparent;
            color: #64748b;
            border: 1px solid #475569;
            padding: 8px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.8rem;
        }
        
        button:active { transform: scale(0.9); }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div class="timer-box" id="display-timer">10:00</div>
        <div class="timer-controls">
            <button onclick="startTimer()">START</button>
            <button onclick="pauseTimer()">PAUSE</button>
            <button onclick="resetTimer()">RESET TIME</button>
        </div>
    </div>

    <div class="scoreboard">
        <div class="team-card red-border">
            <input type="text" class="team-name" value="TIM MERAH">
            
            <div class="point-row">
                <div class="point-label">GARIS DEPAN</div>
                <div class="point-val" id="valA_depan">0</div>
                <div class="btn-group">
                    <button class="btn-plus" onclick="change('A', 'depan', 1)">+</button>
                    <button class="btn-minus" onclick="change('A', 'depan', -1)">-</button>
                </div>
            </div>

            <div class="point-row">
                <div class="point-label">GARIS BELAKANG</div>
                <div class="point-val" id="valA_belakang">0</div>
                <div class="btn-group">
                    <button class="btn-plus" onclick="change('A', 'belakang', 1)">+</button>
                    <button class="btn-minus" onclick="change('A', 'belakang', -1)">-</button>
                </div>
            </div>
        </div>

        <div class="team-card blue-border">
            <input type="text" class="team-name" value="TIM BIRU">
            
            <div class="point-row">
                <div class="point-label">GARIS DEPAN</div>
                <div class="point-val" id="valB_depan">0</div>
                <div class="btn-group">
                    <button class="btn-plus" onclick="change('B', 'depan', 1)">+</button>
                    <button class="btn-minus" onclick="change('B', 'depan', -1)">-</button>
                </div>
            </div>

            <div class="point-row">
                <div class="point-label">GARIS BELAKANG</div>
                <div class="point-val" id="valB_belakang">0</div>
                <div class="btn-group">
                    <button class="btn-plus" onclick="change('B', 'belakang', 1)">+</button>
                    <button class="btn-minus" onclick="change('B', 'belakang', -1)">-</button>
                </div>
            </div>
        </div>
    </div>

    <div class="total-summary">
        <div class="summary-item">
            <div class="summary-label">Total Skor</div>
            <div class="summary-score score-red" id="totalA">0</div>
        </div>
        
        <div class="vs-circle">VS</div>

        <div class="summary-item">
            <div class="summary-label">Total Skor</div>
            <div class="summary-score score-blue" id="totalB">0</div>
        </div>
    </div>

    <button class="reset-btn" onclick="resetAll()">RESET SEMUA DATA</button>
</div>

<script>
    let scores = {
        A: { depan: 0, belakang: 0 },
        B: { depan: 0, belakang: 0 }
    };

    function change(team, pos, val) {
        scores[team][pos] += val;
        if (scores[team][pos] < 0) scores[team][pos] = 0;
        
        // Update angka di baris masing-masing
        document.getElementById(`val${team}_${pos}`).innerText = scores[team][pos];
        
        // Update Total Akumulasi (Garis depan 1pt, Garis belakang 1pt)
        let total = scores[team].depan + scores[team].belakang;
        document.getElementById(`total${team}`).innerText = total;
    }

    function resetAll() {
        if(confirm("Hapus semua skor?")) {
            scores = { A: { depan: 0, belakang: 0 }, B: { depan: 0, belakang: 0 } };
            document.getElementById('valA_depan').innerText = 0;
            document.getElementById('valA_belakang').innerText = 0;
            document.getElementById('valB_depan').innerText = 0;
            document.getElementById('valB_belakang').innerText = 0;
            document.getElementById('totalA').innerText = 0;
            document.getElementById('totalB').innerText = 0;
        }
    }

    // Timer Logic
    let timer;
    let time = 600;
    function startTimer() {
        clearInterval(timer);
        timer = setInterval(() => {
            if(time > 0) { time--; updateDisplay(); }
        }, 1000);
    }
    function pauseTimer() { clearInterval(timer); }
    function resetTimer() { pauseTimer(); time = 600; updateDisplay(); }
    function updateDisplay() {
        let m = Math.floor(time/60);
        let s = time%60;
        document.getElementById('display-timer').innerText = `${m}:${s < 10 ? '0'+s : s}`;
    }
</script>

</body>
</html>
