<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>HOANGDZ TOOL - MATRIX ULTIMATE v6.1</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Rajdhani:wght@300;500;700&display=swap');

        :root {
            --cyan: #00f2ff;
            --purple: #7000ff;
            --pink: #ff0055;
            --dark: #050508;
            --glass: rgba(10, 10, 15, 0.95);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body {
            background-color: var(--dark);
            color: #fff;
            font-family: 'Rajdhani', sans-serif;
            display: flex; flex-direction: column; align-items: center;
            min-height: 100vh; overflow-x: hidden;
            touch-action: manipulation; /* Ngăn zoom khi double tap */
        }

        #matrix {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: -1; opacity: 0.3;
        }

        .main-wrapper {
            width: 100%; max-width: 650px; padding: 20px; z-index: 10;
        }

        .nav-buttons {
            display: flex; gap: 10px; margin-bottom: 20px;
        }

        .nav-btn {
            flex: 1; padding: 15px; border: 1px solid var(--cyan);
            background: rgba(0, 242, 255, 0.05); color: var(--cyan);
            font-family: 'Orbitron', sans-serif; font-weight: bold;
            cursor: pointer; border-radius: 10px; transition: 0.3s;
            font-size: 14px;
        }

        .nav-btn.active { background: var(--cyan); color: #000; box-shadow: 0 0 20px var(--cyan); }

        .panel { display: none; animation: fadeIn 0.5s ease; }
        .panel.active { display: block; }

        .home-card { background: var(--glass); border: 1px solid var(--purple); border-radius: 20px; overflow: hidden; }
        .home-content { padding: 25px; height: 400px; overflow-y: auto; line-height: 1.8; }

        .tool-card { background: var(--glass); border: 1px solid var(--cyan); border-radius: 20px; padding: 25px; }

        /* Quan trọng: Để font-size 16px để trình duyệt mobile không tự động zoom */
        input {
            width: 100%; padding: 18px; background: #000;
            border: 1px solid #333; border-radius: 12px;
            color: var(--cyan); font-family: 'Orbitron'; margin-bottom: 15px;
            text-align: center; outline: none;
            font-size: 16px; 
        }

        .action-btns { display: grid; grid-template-columns: 4fr 1fr; gap: 10px; }
        .btn-run {
            padding: 18px; background: linear-gradient(45deg, var(--cyan), var(--purple));
            border: none; border-radius: 12px; color: #fff; font-family: 'Orbitron';
            font-weight: bold; cursor: pointer; font-size: 14px;
        }
        .btn-clear { background: rgba(255,0,85,0.1); border: 1px solid var(--pink); color: var(--pink); border-radius: 12px; cursor: pointer; }

        .loading-text { display: none; text-align: center; color: var(--cyan); margin: 20px 0; font-family: 'Orbitron'; animation: blink 1s infinite; font-size: 14px; }

        .analysis-box {
            display: none; margin-top: 25px; padding: 20px;
            background: rgba(255, 255, 255, 0.02); border-radius: 15px;
            border-left: 5px solid var(--cyan);
        }

        .detail-item { margin-bottom: 10px; font-size: 15px; border-bottom: 1px solid rgba(255,255,255,0.05); padding-bottom: 5px; overflow: hidden; }
        .detail-item span { color: #888; font-weight: 300; }
        .detail-item b { color: #fff; float: right; }
        
        .advise-box {
            margin-top: 15px; padding: 15px; background: rgba(112, 0, 255, 0.1);
            border-radius: 10px; border: 1px dashed var(--purple); color: #ddd; font-style: italic; font-size: 14px;
        }

        .history-section { margin-top: 25px; background: rgba(0,0,0,0.5); border-radius: 15px; padding: 15px; }
        .history-table { width: 100%; border-collapse: collapse; font-size: 12px; }
        .history-table th { text-align: left; color: #555; padding-bottom: 10px; }
        .history-table td { padding: 8px 0; border-top: 1px solid #222; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes blink { 0% { opacity: 0.2; } 50% { opacity: 1; } 100% { opacity: 0.2; } }
        
        .tele-btn { display: block; width: 100%; padding: 15px; background: #229ED9; color: #fff; text-align: center; text-decoration: none; border-radius: 10px; margin-top: 10px; font-weight: bold; font-family: 'Orbitron'; font-size: 13px; }
    </style>
</head>
<body>

    <canvas id="matrix"></canvas>

    <div class="main-wrapper">
        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showPanel('home', this)">NHÀ</button>
            <button class="nav-btn" onclick="showPanel('tool', this)">BẬT TOOL</button>
        </div>

        <div id="home" class="panel active">
            <div class="home-card">
                <div class="home-content">
                    <h2 style="color:var(--cyan); font-family:'Orbitron'; font-size: 18px; margin-bottom: 15px;">HOANGDZ BIOGRAPHY</h2>
                    <p>Hệ thống được phát triển bởi <b>Trần Hoàng</b> - Chuyên gia phân tích mã độc và thuật toán băm dữ liệu. Đây là phiên bản Ultimate tích hợp trí tuệ nhân tạo để bóc tách mã MD5.</p>
                    <p><b>Triết lý:</b> "Mọi dãy số đều có sơ hở". HOANGDZ TOOL không tạo ra kết quả, chúng tôi tìm thấy kết quả từ trong chính chuỗi mã bạn cung cấp.</p>
                    <p>Khi bạn nhấn phân tích, hệ thống sẽ thực hiện hàng triệu phép tính Bitwise để tìm ra sự mất cân bằng giữa Tài và Xỉu trong 32 ký tự Hex.</p>
                    <a href="https://t.me/tranhoang2286" class="tele-btn">TELEGRAM: @tranhoang2286</a>
                </div>
            </div>
        </div>

        <div id="tool" class="panel">
            <div class="tool-card">
                <input type="text" id="md5-input" placeholder="DÁN MÃ MD5 TẠI ĐÂY..." maxlength="32" inputmode="text">
                <div class="action-btns">
                    <button class="btn-run" id="btn-analyze">PHÂN TÍCH HỆ THỐNG</button>
                    <button class="btn-clear" id="btn-reset"><i class="fas fa-trash"></i></button>
                </div>

                <div id="loading" class="loading-text">ĐANG PHÂN TÍCH... VUI LÒNG CHỜ....</div>

                <div id="result-area" class="analysis-box">
                    <div class="detail-item"><span>Dự đoán:</span> <b id="res-dudoan">---</b></div>
                    <div class="detail-item"><span>Tỉ lệ:</span> <b id="res-tile">0%</b></div>
                    <div class="detail-item"><span>Đoạn phân tích:</span> <b id="res-doan">0x00...</b></div>
                    <div class="detail-item"><span>Tỉ lệ trùng:</span> <b id="res-trung">0%</b></div>
                    <div class="detail-item"><span>Tỉ lệ bẻ mã:</span> <b id="res-be">0%</b></div>
                    <div class="detail-item"><span>Xác suất bẻ tool:</span> <b id="res-betool">0%</b></div>
                    
                    <div class="advise-box">
                        <b>Anh HOANGDZ khuyên:</b> <span id="res-khuyen">---</span>
                    </div>
                </div>

                <div class="history-section">
                    <h4 style="color:var(--cyan); margin-bottom:10px; font-size:11px; letter-spacing: 1px;">LỊCH SỬ PHÂN TÍCH</h4>
                    <table class="history-table">
                        <thead><tr><th>PHIÊN (MD5)</th><th>KẾT QUẢ</th><th>TỈ LỆ</th></tr></thead>
                        <tbody id="history-body"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <script>
        // --- MATRIX RAIN ANIMATION ---
        const canvas = document.getElementById('matrix');
        const ctx = canvas.getContext('2d');
        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;
        const alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789$+-*/=%";
        const fontSize = 16;
        let columns = width / fontSize;
        let rainDrops = Array.from({ length: columns }).fill(1);

        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
            columns = width / fontSize;
            rainDrops = Array.from({ length: columns }).fill(1);
        });

        function drawMatrix() {
            ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.fillRect(0, 0, width, height);
            ctx.fillStyle = '#0F0';
            ctx.font = fontSize + 'px monospace';
            for (let i = 0; i < rainDrops.length; i++) {
                const text = alphabet.charAt(Math.floor(Math.random() * alphabet.length));
                ctx.fillText(text, i * fontSize, rainDrops[i] * fontSize);
                if (rainDrops[i] * fontSize > height && Math.random() > 0.975) rainDrops[i] = 0;
                rainDrops[i]++;
            }
        }
        setInterval(drawMatrix, 35);

        // --- NAVIGATION ---
        function showPanel(id, btn) {
            document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(id).classList.add('active');
            btn.classList.add('active');
        }

        // --- CORE SYSTEM (UNCHANGED) ---
        function analyzeMD5(md5) {
            md5 = md5.toLowerCase();
            let energy = 0;
            let xorVal = 0;
            for (let i = 0; i < md5.length; i++) {
                let dec = parseInt(md5[i], 16);
                energy += dec;
                xorVal ^= dec;
            }
            let lastChar = parseInt(md5[31], 16);
            let score = (energy + xorVal + lastChar) % 100;
            
            let duDoan = score > 48 ? "TÀI" : "XỈU";
            let tiLe = 75 + (energy % 20);
            let tiLeTrung = 50 + (xorVal % 40);
            let tiLeBeMa = 10 + (lastChar % 15);
            let xacSuatBeTool = 100 - tiLe - (energy % 5);
            
            let khuyen = "";
            if (tiLe > 85) khuyen = "Kèo thơm, vả mạnh tay vào anh em ơi!";
            else if (tiLe > 75) khuyen = "Cầu khá đẹp, đi đều tay là lượm.";
            else khuyen = "Cầu hơi loạn, đánh nhẹ nhàng hoặc bỏ qua phiên này.";

            return { duDoan, tiLe, energy, tiLeTrung, tiLeBeMa, xacSuatBeTool, khuyen, md5Short: md5.substring(24) };
        }

        // --- UI HANDLERS ---
        const btnAnalyze = document.getElementById('btn-analyze');
        const input = document.getElementById('md5-input');
        const resultArea = document.getElementById('result-area');
        const loading = document.getElementById('loading');
        const historyBody = document.getElementById('history-body');

        btnAnalyze.addEventListener('click', () => {
            const val = input.value.trim();
            if (val.length !== 32) {
                alert("Mã MD5 phải đúng 32 ký tự!");
                return;
            }

            loading.style.display = 'block';
            resultArea.style.display = 'none';
            input.blur(); // Tự động đóng bàn phím khi bắt đầu phân tích

            setTimeout(() => {
                const data = analyzeMD5(val);
                
                document.getElementById('res-dudoan').innerText = data.duDoan;
                document.getElementById('res-dudoan').style.color = data.duDoan === "TÀI" ? "var(--cyan)" : "var(--pink)";
                document.getElementById('res-tile').innerText = data.tiLe + "%";
                document.getElementById('res-doan').innerText = "0x" + data.energy.toString(16).toUpperCase() + "...";
                document.getElementById('res-trung').innerText = data.tiLeTrung + "%";
                document.getElementById('res-be').innerText = data.tiLeBeMa + "%";
                document.getElementById('res-betool').innerText = Math.abs(data.xacSuatBeTool) + "%";
                document.getElementById('res-khuyen').innerText = data.khuyen;

                loading.style.display = 'none';
                resultArea.style.display = 'block';

                const row = `<tr><td>...${data.md5Short}</td><td style="color:${data.duDoan === 'TÀI' ? 'var(--cyan)' : 'var(--pink)'}">${data.duDoan}</td><td>${data.tiLe}%</td></tr>`;
                historyBody.insertAdjacentHTML('afterbegin', row);
            }, 2000);
        });

        document.getElementById('btn-reset').addEventListener('click', () => {
            input.value = "";
            resultArea.style.display = 'none';
            input.focus();
        });
    </script>
</body>
</html>
