<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ai Là Triệu Phú - Chế độ Giáo Dục</title>
    <!-- Tailwind CSS for layout and basic styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome for icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700;900&display=swap" rel="stylesheet">
    <!-- Confetti JS for winning effect -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- JSZip for parsing DOCX -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>

    <style>
        :root {
            --primary-blue: #0a1128;
            --secondary-blue: #1c3b7a;
            --highlight-blue: #3b82f6;
            --accent-gold: #fbbf24;
            --neon-purple: #8b5cf6;
            --correct-green: #22c55e;
            --wrong-red: #ef4444;
            --text-light: #f8fafc;
        }

        body {
            font-family: 'Roboto', sans-serif;
            background-color: var(--primary-blue);
            color: var(--text-light);
            margin: 0;
            padding: 0;
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            user-select: none;
            background-image: radial-gradient(circle at center, #1e3a8a 0%, #020617 100%);
            position: relative;
        }

        body::before {
            content: '';
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: 
                radial-gradient(circle at 20% 30%, rgba(139, 92, 246, 0.15) 0%, transparent 50%),
                radial-gradient(circle at 80% 70%, rgba(59, 130, 246, 0.15) 0%, transparent 50%);
            z-index: 0;
            pointer-events: none;
        }

        /* --- SCREEN VISIBILITY CLASSES --- */
        .screen { display: none; height: 100%; width: 100%; position: absolute; inset: 0; z-index: 10; }
        .screen.active { display: flex; }

        /* --- 1. MÀN HÌNH NHẬP THÔNG TIN --- */
        #screen-input {
            justify-content: center;
            align-items: center;
            background: radial-gradient(circle, #020617 0%, #000000 100%);
        }
        
        .neon-input-box {
            background: linear-gradient(180deg, rgba(2, 6, 23, 0.9) 0%, rgba(15, 23, 42, 0.95) 100%);
            border: 2px solid #00a8ff;
            border-radius: 20px;
            padding: 40px;
            width: 700px;
            box-shadow: 0 0 25px rgba(0, 168, 255, 0.6), inset 0 0 15px rgba(0, 168, 255, 0.4);
            display: flex;
            flex-direction: column;
            gap: 15px;
            transform: perspective(1000px) rotateX(2deg);
        }

        /* Dòng 1 & 2 theo yêu cầu */
        .neon-input-box .title-sm {
            color: #00a8ff; font-size: 1.4rem; text-align: center; text-shadow: 0 0 8px #00a8ff; letter-spacing: 2px;
            font-weight: bold;
        }
        .neon-input-box .title-lg {
            color: var(--accent-gold); font-size: 2.8rem; font-weight: 900; text-align: center; text-shadow: 0 0 20px var(--accent-gold); margin-bottom: 10px;
        }

        .input-row { display: flex; gap: 15px; align-items: center; }
        .input-row label { color: white; width: 90px; font-weight: bold; font-size: 1.1rem; }
        .input-row input[type="text"] {
            flex: 1; padding: 12px 15px; background: rgba(0,0,0,0.6); border: 1px solid #3b82f6; 
            border-radius: 8px; color: white; outline: none; font-size: 1.1rem;
        }
        .input-row input[type="text"]:focus { box-shadow: 0 0 12px #3b82f6; border-color: #00a8ff; }

        .file-upload-wrapper {
            background: rgba(139, 92, 246, 0.15); border: 2px dashed var(--neon-purple); border-radius: 8px; padding: 20px; text-align: center; margin-top: 10px; transition: 0.3s;
        }
        .file-upload-wrapper:hover { background: rgba(139, 92, 246, 0.25); box-shadow: 0 0 15px rgba(139, 92, 246, 0.4); }
        .file-status { margin-top: 12px; font-size: 1rem; color: #ef4444; min-height: 20px; font-weight: bold;}
        .file-status.success { color: var(--correct-green); text-shadow: 0 0 5px rgba(34, 197, 94, 0.5); }

        .btn-group { display: flex; justify-content: center; align-items: center; gap: 20px; margin-top: 25px; position: relative;}
        
        .btn-rules {
            background: transparent; color: var(--accent-gold); border: 2px solid var(--accent-gold); 
            padding: 12px 30px; border-radius: 10px; font-weight: bold; cursor: pointer; transition: 0.3s; font-size: 1.1rem;
        }
        .btn-rules:hover { background: var(--accent-gold); color: black; box-shadow: 0 0 15px var(--accent-gold); }
        
        .btn-start-game {
            background: linear-gradient(90deg, #1e3a8a 0%, #3b82f6 50%, #1e3a8a 100%); color: white; 
            border: 2px solid white; padding: 15px 50px; border-radius: 30px; font-weight: bold; font-size: 1.3rem; cursor: pointer; transition: 0.3s;
            text-transform: uppercase;
        }
        .btn-start-game:hover:not(:disabled) { box-shadow: 0 0 25px #3b82f6; transform: scale(1.05); }
        .btn-start-game:disabled { filter: grayscale(100%); cursor: not-allowed; opacity: 0.5; }

        /* --- 2. SCREEN VIDEO --- */
        #screen-video { background: black; justify-content: center; align-items: center; }
        #video-intro { width: 100%; height: 100%; object-fit: cover; }

        /* --- 3. SCREEN INTRO (ẢNH 2) --- */
        #screen-intro {
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: radial-gradient(circle, #1e3a8a 0%, #020617 100%);
        }

        /* --- 4. SCREEN GAME --- */
        #game-container {
            background-image: url('ghe.jpg');
            background-size: cover;
            background-position: center;
            flex-direction: column;
            position: relative;
            z-index: 10;
        }
        
        #game-container::before {
            content: ''; position: absolute; inset: 0; background: rgba(0,0,0,0.65); z-index: -1;
        }

        .main-stage { display: flex; flex: 1; padding: 20px; gap: 20px; }
        .left-panel { flex: 3; display: flex; flex-direction: column; align-items: center; justify-content: flex-end; position: relative; }
        
        .right-panel {
            flex: 1; min-width: 250px; max-width: 350px;
            background: linear-gradient(180deg, rgba(2, 6, 23, 0.9) 0%, rgba(30, 58, 138, 0.9) 100%);
            border-left: 2px solid var(--accent-gold); border-radius: 10px 0 0 10px;
            padding: 10px; display: flex; flex-direction: column-reverse; 
            box-shadow: -5px 0 25px rgba(0, 0, 0, 0.5); z-index: 20;
        }

        /* Logo Area */
        .logo-container {
            position: absolute; top: 15%; left: 50%; transform: translateX(-50%);
            width: 250px; height: 250px; border-radius: 50%; border: 4px solid var(--accent-gold);
            box-shadow: 0 0 30px var(--accent-gold), inset 0 0 20px var(--accent-gold);
            display: flex; justify-content: center; align-items: center;
            background: radial-gradient(circle, #1e3a8a 0%, #0f172a 80%);
            animation: pulse-glow 3s infinite alternate;
        }
        @keyframes pulse-glow {
            0% { box-shadow: 0 0 20px var(--accent-gold), inset 0 0 10px var(--accent-gold); }
            100% { box-shadow: 0 0 40px var(--accent-gold), inset 0 0 30px var(--neon-purple); }
        }

        /* QA Box */
        .qa-container { width: 100%; max-width: 950px; margin-bottom: 20px; }
        .question-box {
            background: linear-gradient(90deg, transparent 0%, rgba(28, 59, 122, 0.95) 15%, rgba(28, 59, 122, 0.95) 85%, transparent 100%);
            border-top: 2px solid white; border-bottom: 2px solid white;
            padding: 20px 40px; text-align: center; font-size: 1.5rem; font-weight: bold;
            margin-bottom: 20px; min-height: 100px; display: flex; align-items: center; justify-content: center;
            box-shadow: 0 0 15px rgba(59, 130, 246, 0.5);
        }

        .answers-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        .answer-box {
            background: linear-gradient(90deg, transparent 0%, rgba(15, 23, 42, 0.95) 10%, rgba(30, 58, 138, 0.95) 50%, rgba(15, 23, 42, 0.95) 90%, transparent 100%);
            border: 2px solid var(--highlight-blue); color: white; padding: 15px 30px; font-size: 1.25rem;
            cursor: pointer; transition: all 0.3s ease; position: relative; display: flex; align-items: center;
            clip-path: polygon(5% 0%, 95% 0%, 100% 50%, 95% 100%, 5% 100%, 0% 50%);
        }
        .answer-box:hover {
            background: linear-gradient(90deg, transparent 0%, rgba(30, 58, 138, 0.9) 10%, rgba(59, 130, 246, 0.9) 50%, rgba(30, 58, 138, 0.9) 90%, transparent 100%);
            border-color: white; box-shadow: 0 0 15px var(--highlight-blue); transform: scale(1.02);
        }
        .answer-letter { color: var(--accent-gold); font-weight: bold; margin-right: 15px; font-size: 1.4rem; }

        /* States */
        .answer-box.selected { background: linear-gradient(90deg, transparent 0%, #ca8a04 10%, #facc15 50%, #ca8a04 90%, transparent 100%); border-color: white; color: black; }
        .answer-box.selected .answer-letter { color: black; }
        .answer-box.correct { background: linear-gradient(90deg, transparent 0%, #166534 10%, #22c55e 50%, #166534 90%, transparent 100%); border-color: white; animation: blink-green 0.5s 6; }
        .answer-box.wrong { background: linear-gradient(90deg, transparent 0%, #991b1b 10%, #ef4444 50%, #991b1b 90%, transparent 100%); border-color: white; }
        @keyframes blink-green {
            0%, 100% { background: linear-gradient(90deg, transparent 0%, #166534 10%, #22c55e 50%, #166534 90%, transparent 100%); }
            50% { background: linear-gradient(90deg, transparent 0%, rgba(15, 23, 42, 0.9) 10%, rgba(30, 58, 138, 0.9) 50%, rgba(15, 23, 42, 0.9) 90%, transparent 100%); }
        }
        .answer-box.hidden-answer { visibility: hidden; pointer-events: none; }

        /* Money Tree */
        .money-item {
            display: flex; justify-content: space-between; padding: 8px 15px; margin: 2px 0;
            font-weight: bold; font-size: 1.1rem; border-radius: 5px; transition: all 0.3s;
        }
        .money-item .level { color: #cbd5e1; width: 30px;}
        .money-item .amount { color: var(--accent-gold); text-align: right; flex: 1;}
        .money-item.safe-haven { color: white; }
        .money-item.safe-haven .level, .money-item.safe-haven .amount { color: white; }
        .money-item.current {
            background-color: var(--accent-gold); color: black; box-shadow: 0 0 10px var(--accent-gold);
            transform: scale(1.05); z-index: 2; position: relative;
        }
        .money-item.current .level, .money-item.current .amount { color: black; }
        .money-item.passed { opacity: 0.5; }

        /* Lifelines */
        .lifelines-container {
            display: flex; gap: 15px; position: absolute; top: 20px; right: 20px; flex-direction: column;
        }
        .lifeline-btn {
            width: 65px; height: 45px; border-radius: 25px; border: 2px solid white;
            background: linear-gradient(180deg, #1e3a8a 0%, #0f172a 100%); color: white; font-size: 1.3rem;
            cursor: pointer; display: flex; justify-content: center; align-items: center;
            transition: all 0.2s; box-shadow: 0 0 10px rgba(59, 130, 246, 0.5);
        }
        .lifeline-btn:hover:not(:disabled) { transform: scale(1.1); box-shadow: 0 0 15px var(--accent-gold); border-color: var(--accent-gold); }
        .lifeline-btn:disabled { opacity: 0.3; cursor: not-allowed; filter: grayscale(100%); position: relative; }
        .lifeline-btn:disabled::after { content: 'X'; position: absolute; color: red; font-size: 2rem; font-weight: bold; opacity: 0.7; }

        /* Màn hình Intro (Ảnh 2) */
        .start-logo {
            width: 300px; height: 300px; border-radius: 50%; border: 5px solid var(--accent-gold);
            display: flex; justify-content: center; align-items: center; box-shadow: 0 0 50px var(--accent-gold);
            margin-bottom: 50px; background: radial-gradient(circle, #1e3a8a 0%, #0f172a 80%); animation: pulse-glow 2s infinite alternate;
        }
        .start-btn {
            padding: 15px 50px; font-size: 2rem; font-weight: bold; color: white;
            background: linear-gradient(90deg, #1e3a8a 0%, #3b82f6 50%, #1e3a8a 100%);
            border: 2px solid white; border-radius: 50px; cursor: pointer; box-shadow: 0 0 20px rgba(59, 130, 246, 0.8); transition: all 0.3s;
        }
        .start-btn:hover {
            transform: scale(1.1); box-shadow: 0 0 30px var(--accent-gold);
            background: linear-gradient(90deg, #ca8a04 0%, #facc15 50%, #ca8a04 100%); color: black; border-color: black;
        }

        /* Popups */
        .popup-overlay {
            position: fixed; inset: 0; background: rgba(0, 0, 0, 0.85); display: flex; justify-content: center; align-items: center;
            z-index: 200; opacity: 0; pointer-events: none; transition: opacity 0.3s;
        }
        .popup-overlay.active { opacity: 1; pointer-events: all; }
        .popup-content {
            background: linear-gradient(135deg, #1e3a8a 0%, #0f172a 100%); border: 3px solid var(--accent-gold);
            border-radius: 15px; padding: 30px; max-width: 600px; width: 90%; text-align: center;
            box-shadow: 0 0 30px rgba(251, 191, 36, 0.3); transform: translateY(-50px); transition: transform 0.3s;
        }
        .popup-overlay.active .popup-content { transform: translateY(0); }
        .popup-title { color: var(--accent-gold); font-size: 2rem; font-weight: 900; margin-bottom: 20px; border-bottom: 1px solid rgba(255,255,255,0.2); padding-bottom: 10px; }
        .popup-body { font-size: 1.2rem; line-height: 1.6; margin-bottom: 25px; text-align: justify; }
        .action-btn {
            background: var(--highlight-blue); color: white; border: none; padding: 12px 35px; font-size: 1.2rem; font-weight: bold;
            border-radius: 30px; cursor: pointer; transition: all 0.2s; border: 2px solid transparent;
        }
        .action-btn:hover { background: var(--accent-gold); color: black; border-color: white; }

        /* Nút giải thích */
        #explain-btn {
            display: none; position: absolute; bottom: 20px; right: 20px; background: #8b5cf6; color: white;
            border: 2px solid white; padding: 10px 20px; border-radius: 20px; font-weight: bold; cursor: pointer;
            z-index: 50; box-shadow: 0 0 15px rgba(139, 92, 246, 0.6); animation: bounce 2s infinite;
        }
        #explain-btn:hover { background: #7c3aed; }
        @keyframes bounce { 0%, 20%, 50%, 80%, 100% {transform: translateY(0);} 40% {transform: translateY(-10px);} 60% {transform: translateY(-5px);} }

        /* Audience Chart */
        .chart-container { display: flex; justify-content: space-around; align-items: flex-end; height: 160px; margin-top: 20px; }
        .bar-wrapper { display: flex; flex-direction: column; align-items: center; width: 45px; }
        .bar { width: 100%; background: var(--highlight-blue); transition: height 1s ease-out; height: 0; border-radius: 5px 5px 0 0; }
        .bar-label { margin-top: 5px; font-weight: bold; font-size: 1.1rem; }
        .bar-value { margin-bottom: 5px; font-size: 1rem; color: var(--accent-gold); font-weight: bold;}

        /* Rules Text formatting theo chuẩn yêu cầu */
        .rules-text { text-align: left; font-size: 1.1rem; line-height: 1.8; }
        .rules-text p { margin-bottom: 10px; }
        .rules-text ul { list-style: none; padding-left: 0; margin-top: 10px; margin-bottom: 15px; }
        .rules-text li { margin-bottom: 8px; padding-left: 20px; position: relative; }
        .rules-text li::before { content: '•'; position: absolute; left: 0; font-weight: bold;}

        /* Top Info Bar */
        .top-info { position: absolute; top: 15px; left: 20px; z-index: 50; background: rgba(0,0,0,0.6); padding: 10px 20px; border-radius: 10px; border: 1px solid var(--highlight-blue); }
    </style>
</head>
<body>

    <!-- Audio Elements (Theo yêu cầu, cùng thư mục) -->
    <audio id="audio-nhacch" src="nhacch.mp3" loop></audio>
    <audio id="audio-dung" src="dung.mp3"></audio>
    <audio id="audio-sai" src="sai.mp3"></audio>
    <audio id="audio-win" src="win.mp3"></audio>

    <!-- MÀN 1: NHẬP THÔNG TIN (Sửa cấu trúc theo 6 dòng, mặc định Điện trường, Vật lí, Lớp 11) -->
    <div id="screen-input" class="screen active">
        <div class="neon-input-box">
            <!-- Dòng 1 & 2 -->
            <div class="title-sm">CHƯƠNG TRÌNH GAMESHOW</div>
            <div class="title-lg">AI LÀ TRIỆU PHÚ</div>
            
            <!-- Dòng 3 -->
            <div class="input-row">
                <label>Chủ đề:</label>
                <input type="text" id="input-topic" placeholder="Ví dụ: Điện trường">
            </div>
            
            <!-- Dòng 4 -->
            <div class="input-row">
                <label>Môn học:</label>
                <input type="text" id="input-subject" placeholder="Ví dụ: Vật lí" style="flex: 2;">
                <label style="width: auto; margin-left: 10px;">Lớp:</label>
                <input type="text" id="input-grade" placeholder="Ví dụ: 11" style="flex: 1;">
            </div>

            <!-- Dòng 5 -->
            <div class="file-upload-wrapper">
                <label for="file-upload" style="cursor: pointer; display: block; color: var(--accent-gold); font-weight: bold; font-size: 1.2rem;">
                    <i class="fas fa-file-upload mr-2"></i> Chọn file câu hỏi (.doc, .docx, .pdf)
                </label>
                <input type="file" id="file-upload" accept=".docx, .doc, .pdf" style="display: none;">
                <div id="file-status" class="file-status">Vui lòng tải lên file chứa 15 câu hỏi.</div>
            </div>

            <!-- Dòng 6 -->
            <div class="btn-group">
                <button class="btn-rules" onclick="showRulesPopup()">LUẬT CHƠI</button>
                <button id="btn-start-flow" class="btn-start-game" disabled onclick="startFlow()">BẮT ĐẦU GAMESHOW</button>
            </div>
        </div>
    </div>

    <!-- MÀN 2: VIDEO INTRO -->
    <div id="screen-video" class="screen">
        <video id="video-intro" playsinline>
            <source src="intro.mp4" type="video/mp4">
            Trình duyệt không hỗ trợ video.
        </video>
    </div>

    <!-- MÀN 3: GIỚI THIỆU CHỦ ĐỀ (Ảnh 2) -->
    <div id="screen-intro" class="screen">
        <div class="start-logo">
            <div class="text-center">
                <h1 class="text-3xl font-black text-white" style="text-shadow: 0 0 10px #3b82f6;">AI LÀ</h1>
                <h1 class="text-5xl font-black text-yellow-400" style="text-shadow: 0 0 15px #fbbf24;">TRIỆU PHÚ</h1>
            </div>
        </div>
        
        <div class="text-center mb-8">
            <h2 class="text-3xl text-white mb-2">Chuyên đề: <span id="display-topic" class="font-bold text-yellow-400">TÊN CHUYÊN ĐỀ</span></h2>
            <h3 class="text-xl text-gray-300 mb-4" id="display-subject">Môn - Lớp</h3>
            <p class="text-gray-300 text-lg">Gồm 15 câu hỏi trắc nghiệm từ dễ đến khó. Bạn có 3 quyền trợ giúp.</p>
        </div>

        <button class="start-btn" onclick="startGame()">Bắt Đầu Chơi</button>
    </div>

    <!-- MÀN 4: SÂN KHẤU CHÍNH (Nền ghe.jpg) -->
    <div id="game-container" class="screen">
        
        <!-- Top Info -->
        <div class="top-info">
            <div class="text-lg font-bold">Câu hỏi: <span id="current-q-num" class="text-yellow-400">1/15</span></div>
            <div class="text-lg font-bold">Điểm hiện tại: <span id="current-score" class="text-green-400">0</span></div>
        </div>

        <div class="main-stage">
            <div class="left-panel">
                
                <!-- Logo Animation -->
                <div class="logo-container">
                    <div class="text-center">
                        <h2 class="text-3xl font-black text-white" style="text-shadow: 0 0 10px #3b82f6;">AI LÀ</h2>
                        <h2 class="text-4xl font-black text-yellow-400" style="text-shadow: 0 0 15px #fbbf24;">TRIỆU PHÚ</h2>
                    </div>
                </div>

                <!-- Lifelines (Chỉ 3 quyền theo yêu cầu mới) -->
                <div class="lifelines-container">
                    <button class="lifeline-btn" id="btn-5050" onclick="use5050()" title="50:50">
                        <span class="font-bold text-lg text-yellow-400">50:50</span>
                    </button>
                    <button class="lifeline-btn" id="btn-audience" onclick="useAudience()" title="Hỏi ý kiến khán giả">
                        <i class="fas fa-users text-yellow-400"></i>
                    </button>
                    <button class="lifeline-btn" id="btn-call" onclick="useCall()" title="Gọi điện thoại cho người thân">
                        <i class="fas fa-phone text-yellow-400"></i>
                    </button>
                </div>

                <!-- CÂU HỎI & ĐÁP ÁN -->
                <div class="qa-container">
                    <div class="question-box" id="question-text">
                        Đang tải câu hỏi...
                    </div>
                    <div class="answers-grid">
                        <div class="answer-box" id="ans-A" onclick="selectAnswer('A')">
                            <span class="answer-letter">A:</span> <span class="answer-text"></span>
                        </div>
                        <div class="answer-box" id="ans-B" onclick="selectAnswer('B')">
                            <span class="answer-letter">B:</span> <span class="answer-text"></span>
                        </div>
                        <div class="answer-box" id="ans-C" onclick="selectAnswer('C')">
                            <span class="answer-letter">C:</span> <span class="answer-text"></span>
                        </div>
                        <div class="answer-box" id="ans-D" onclick="selectAnswer('D')">
                            <span class="answer-letter">D:</span> <span class="answer-text"></span>
                        </div>
                    </div>
                </div>

                <!-- Button Giải thích -->
                <button id="explain-btn" onclick="showExplanation()"><i class="fas fa-graduation-cap mr-2"></i>Giải thích</button>

            </div>

            <!-- Money Tree Right Panel -->
            <div class="right-panel" id="money-tree"></div>
        </div>
    </div>

    <!-- POPUP THÔNG BÁO CHUNG -->
    <div id="custom-popup" class="popup-overlay">
        <div class="popup-content">
            <h2 id="popup-title" class="popup-title">Thông báo</h2>
            <div id="popup-body" class="popup-body">Nội dung</div>
            <button class="action-btn" onclick="closePopup()">Đóng</button>
            <button id="popup-action-btn" class="action-btn ml-4" style="display:none;" onclick="">Tiếp tục</button>
        </div>
    </div>

    <!-- POPUP LUẬT CHƠI (Nội dung chính xác theo yêu cầu) -->
    <div id="rules-popup" class="popup-overlay">
        <div class="popup-content" style="max-width: 750px;">
            <h2 class="popup-title">📋 LUẬT CHƠI</h2>
            <div class="rules-text text-white">
                <p>🔹 Trả lời đúng 15 câu hỏi về tính chất kim loại để trở thành triệu phú.</p>
                <p>🔹 Cột mốc an toàn: Câu 5 (1000 VNĐ) và Câu 10 (15,000,000 VNĐ). Vượt qua mốc nào, bạn chắc chắn nhận được số tiền đó dù trả lời sai ở các câu sau.</p>
                <p>🔹 3 Quyền trợ giúp (Mỗi quyền dùng 1 lần):</p>
                <ul>
                    <li><strong>50:50:</strong> Loại bỏ 2 phương án sai.</li>
                    <li><strong>👥 Hỏi khán giả:</strong> Xem tỷ lệ bình chọn ảo của khán giả.</li>
                    <li><strong>🧙‍♂️ Gọi điện người thân:</strong> Nhận gợi ý chuyên môn từ người thân có chuyên môn.</li>
                </ul>
                <div class="mt-4 p-4 bg-blue-900 border-l-4 border-yellow-400 rounded">
                    <p class="text-yellow-400 font-bold mb-1">💡 MẸO TỪ GỢI Ý TỪ CHƯƠNG TRÌNH:</p>
                    <p class="text-sm">Đọc kĩ đề, nhớ lại các tính chất vật lí và hóa học đặc trưng. Đừng vội vàng nhé!</p>
                </div>
            </div>
            <button class="action-btn mt-6" onclick="closePopup('rules-popup')">ĐÃ HIỂU</button>
        </div>
    </div>

    <script>
        // --- 1. DỮ LIỆU GAME & TIỀN THƯỞNG ---
        const moneyLevels = [
            "200 VNĐ", "400 VNĐ", "600 VNĐ", "800 VNĐ", "1000 VNĐ", // Mốc an toàn 1
            "2,000,000 VNĐ", "5,000,000 VNĐ", "8,000,000 VNĐ", "10,000,000 VNĐ", "15,000,000 VNĐ", // Mốc an toàn 2
            "30,000,000 VNĐ", "40,000,000 VNĐ", "60,000,000 VNĐ", "85,000,000 VNĐ", "150,000,000 VNĐ"
        ];

        let questionsData = [];

        // --- 2. XỬ LÝ ĐỌC FILE WORD (DOCX) NÂNG CAO ---
        const fileInput = document.getElementById('file-upload');
        const fileStatus = document.getElementById('file-status');
        const btnStartFlow = document.getElementById('btn-start-flow');

        fileInput.addEventListener('change', async function(e) {
            const file = e.target.files[0];
            if (!file) return;

            fileStatus.innerHTML = "⏳ Đang phân tích dữ liệu môn Vật lí, vui lòng đợi...";
            fileStatus.className = "file-status";
            btnStartFlow.disabled = true;

            try {
                if (file.name.endsWith('.docx')) {
                    await parseDocx(file);
                } else {
                    throw new Error("Vui lòng tải lên file định dạng .docx để hệ thống quét dữ liệu và kiểu chữ chuẩn xác.");
                }

                if (questionsData.length < 15) {
                    fileStatus.innerHTML = `❌ Lỗi: File hiện quét được ${questionsData.length} câu. Gameshow cần tối thiểu 15 câu để bắt đầu.`;
                } else {
                    questionsData = questionsData.slice(0, 15);
                    fileStatus.innerHTML = `✅ Đã nhập thành công 15 câu hỏi. Chương trình Gameshow được phép bắt đầu.`;
                    fileStatus.className = "file-status success";
                    btnStartFlow.disabled = false;
                }
            } catch (err) {
                console.error(err);
                fileStatus.innerHTML = `❌ Lỗi đọc file: ${err.message}`;
            }
        });

        // Hàm hỗ trợ render các ký tự đã cắt lát sang mã HTML chuẩn (giữ chỉ số trên/dưới)
        function renderCharSliceToHtml(slice) {
            let html = "";
            let inSub = false;
            let inSup = false;
            
            for (let i = 0; i < slice.length; i++) {
                const c = slice[i];
                
                if (c.isSub && !inSub) {
                    if (inSup) { html += "</sup>"; inSup = false; }
                    html += "<sub>";
                    inSub = true;
                } else if (!c.isSub && inSub) {
                    html += "</sub>";
                    inSub = false;
                }
                
                if (c.isSup && !inSup) {
                    if (inSub) { html += "</sub>"; inSub = false; }
                    html += "<sup>";
                    inSup = true;
                } else if (!c.isSup && inSup) {
                    html += "</sup>";
                    inSup = false;
                }
                
                let charStr = c.char;
                if (charStr === "<") charStr = "&lt;";
                else if (charStr === ">") charStr = "&gt;";
                else if (charStr === "&") charStr = "&amp;";
                
                html += charStr;
            }
            
            if (inSub) html += "</sub>";
            if (inSup) html += "</sup>";
            
            return html;
        }

        // PHÂN TÍCH XML DOCX NÂNG CAO - HỖ TRỢ ĐÁP ÁN NẰM TRÊN CÙNG MỘT DÒNG
        async function parseDocx(file) {
            const zip = await JSZip.loadAsync(file);
            const docXml = await zip.file("word/document.xml").async("string");
            
            const parser = new DOMParser();
            const xmlDoc = parser.parseFromString(docXml, "text/xml");
            const paragraphs = xmlDoc.getElementsByTagName("w:p");

            let currentQuestion = null;
            let currentOptions = [];
            questionsData = [];

            for (let i = 0; i < paragraphs.length; i++) {
                const p = paragraphs[i];
                
                // Thu thập thông tin ký tự kèm style định dạng
                let charList = [];
                const runs = p.getElementsByTagName("w:r");
                for (let j = 0; j < runs.length; j++) {
                    const r = runs[j];
                    const tNode = r.getElementsByTagName("w:t")[0];
                    if (!tNode) continue;
                    let text = tNode.textContent;

                    let isUnderlined = false;
                    let isColored = false;
                    let isSub = false;
                    let isSup = false;

                    const rPr = r.getElementsByTagName("w:rPr")[0];
                    if (rPr) {
                        // Kiểu 1: Gạch chân (Underline)
                        if (rPr.getElementsByTagName("w:u").length > 0) isUnderlined = true;
                        
                        // Highlight
                        if (rPr.getElementsByTagName("w:highlight").length > 0) isUnderlined = true; 
                        
                        // Kiểu 2: Tô màu chữ (Color)
                        const colorNode = rPr.getElementsByTagName("w:color")[0];
                        if (colorNode) {
                            const val = colorNode.getAttribute("w:val");
                            if (val && val !== "000000" && val !== "auto") isColored = true;
                        }

                        // Chỉ số trên/dưới trong Lý/Hóa/Toán
                        const vertAlign = rPr.getElementsByTagName("w:vertAlign")[0];
                        if (vertAlign) {
                            const alignVal = vertAlign.getAttribute("w:val");
                            if (alignVal === "subscript") isSub = true;
                            if (alignVal === "superscript") isSup = true;
                        }
                    }

                    for (let k = 0; k < text.length; k++) {
                        charList.push({
                            char: text[k],
                            isUnderlined: isUnderlined,
                            isColored: isColored,
                            isSub: isSub,
                            isSup: isSup
                        });
                    }
                }

                const rawText = charList.map(c => c.char).join("");
                const trimmedText = rawText.trim();
                if (!trimmedText) continue;

                // 1. Nhận diện đầu mục Câu hỏi
                if (trimmedText.toLowerCase().startsWith("câu")) {
                    if (currentQuestion && currentOptions.length >= 4) {
                        saveParsedQuestion(currentQuestion, currentOptions);
                    }
                    currentQuestion = renderCharSliceToHtml(charList);
                    currentOptions = [];
                } 
                // 2. Nhận diện Đáp án (Hỗ trợ cả inline đáp án trên 1 dòng)
                else {
                    const optionRegex = /(?:^|[\s\t])([A-D])[\.\:\s]/g;
                    let matches = [];
                    let match;
                    while ((match = optionRegex.exec(rawText)) !== null) {
                        matches.push({
                            letter: match[1],
                            index: match.index,
                            prefixLength: match[0].length
                        });
                    }

                    if (matches.length > 0) {
                        for (let m = 0; m < matches.length; m++) {
                            const currentMatch = matches[m];
                            // Xác định lát cắt ký tự của từng đáp án
                            const startLetterIdx = currentMatch.index + (currentMatch.prefixLength - 2);
                            const textStartIdx = currentMatch.index + currentMatch.prefixLength;
                            const textEndIdx = (m < matches.length - 1) ? matches[m + 1].index : charList.length;

                            const optionTextSlice = charList.slice(textStartIdx, textEndIdx);
                            const prefixSlice = charList.slice(startLetterIdx, textStartIdx);
                            
                            const optionHtml = renderCharSliceToHtml(optionTextSlice).trim();
                            
                            // Xác định đáp án đúng dựa vào Underline hoặc Color ở Prefix hoặc nội dung
                            const isCorrect = prefixSlice.some(c => c.isUnderlined || c.isColored) || 
                                              optionTextSlice.some(c => c.isUnderlined || c.isColored);

                            currentOptions.push({
                                letter: currentMatch.letter,
                                text: optionHtml,
                                isCorrect: isCorrect
                            });
                        }
                    }
                }
            }
            // Lưu câu hỏi cuối cùng
            if (currentQuestion && currentOptions.length >= 4) {
                saveParsedQuestion(currentQuestion, currentOptions);
            }
        }

        function saveParsedQuestion(qText, options) {
            let qObj = { 
                question: qText, 
                A: "", B: "", C: "", D: "", 
                correct: "A",
                explanation: "Đáp án và lời giải đã được hệ thống tự động bóc tách và phân tích từ file đề thi."
            };
            
            options.forEach(opt => {
                qObj[opt.letter] = opt.text;
                if(opt.isCorrect) qObj.correct = opt.letter;
            });
            questionsData.push(qObj);
        }

        // --- 3. QUẢN LÝ MÀN HÌNH ---
        function switchScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
        }

        function startFlow() {
            const topic = document.getElementById('input-topic').value.trim() || "ĐIỆN TRƯỜNG";
            const subject = document.getElementById('input-subject').value.trim() || "Vật lí";
            const grade = document.getElementById('input-grade').value.trim() || "11";

            document.getElementById('display-topic').innerText = topic.toUpperCase();
            document.getElementById('display-subject').innerText = `Môn: ${subject} - Lớp: ${grade}`;

            switchScreen('screen-video');
            const video = document.getElementById('video-intro');
            
            video.play().catch(e => {
                console.log("Trình duyệt chặn video chạy tự động, chuyển màn...");
                endVideo();
            });

            video.onended = endVideo;
        }

        function endVideo() {
            switchScreen('screen-intro');
        }

        function showRulesPopup() {
            document.getElementById('rules-popup').classList.add('active');
        }

        // --- 4. TRẠNG THÁI TRÒ CHƠI ---
        let currentQuestionIndex = 0;
        let isAnswering = false;
        let hasGuessed = false;
        let lifelines = { '5050': true, 'audience': true, 'call': true };

        // --- 5. AUDIO & FALLBACK ---
        const audioNhacch = document.getElementById('audio-nhacch');
        const audioDung = document.getElementById('audio-dung');
        const audioSai = document.getElementById('audio-sai');
        const audioWin = document.getElementById('audio-win');

        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        function playToneFallback(frequency, type, duration, vol=0.5) {
            if(audioCtx.state === 'suspended') audioCtx.resume();
            const oscillator = audioCtx.createOscillator();
            const gainNode = audioCtx.createGain();
            oscillator.type = type;
            oscillator.frequency.value = frequency;
            oscillator.connect(gainNode);
            gainNode.connect(audioCtx.destination);
            gainNode.gain.setValueAtTime(vol, audioCtx.currentTime);
            gainNode.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + duration);
            oscillator.start();
            oscillator.stop(audioCtx.currentTime + duration);
        }

        // --- 6. GIAO DIỆN GAME ---
        function initMoneyTree() {
            const tree = document.getElementById('money-tree');
            tree.innerHTML = '';
            for (let i = 0; i < 15; i++) {
                const div = document.createElement('div');
                div.className = 'money-item';
                div.id = `money-level-${i}`;
                
                if (i === 4 || i === 9 || i === 14) {
                    div.classList.add('safe-haven');
                }

                div.innerHTML = `
                    <span class="level">${i + 1}</span>
                    <span class="amount">${moneyLevels[i]}</span>
                    <span style="width:20px; text-align:right;">♦</span>
                `;
                tree.appendChild(div);
            }
        }

        function updateMoneyTree() {
            for (let i = 0; i < 15; i++) {
                const el = document.getElementById(`money-level-${i}`);
                el.classList.remove('current', 'passed');
                if (i < currentQuestionIndex) {
                    el.classList.add('passed');
                }
            }
            if (currentQuestionIndex < 15) {
                document.getElementById(`money-level-${currentQuestionIndex}`).classList.add('current');
            }
            
            document.getElementById('current-q-num').innerText = `${currentQuestionIndex + 1}/15`;
            document.getElementById('current-score').innerText = currentQuestionIndex > 0 ? moneyLevels[currentQuestionIndex - 1] : "0 VNĐ";
        }

        function loadQuestion() {
            if (currentQuestionIndex >= 15) {
                gameWon();
                return;
            }

            const q = questionsData[currentQuestionIndex];
            document.getElementById('question-text').innerHTML = q.question;
            
            const letters = ['A', 'B', 'C', 'D'];
            letters.forEach(l => {
                const box = document.getElementById(`ans-${l}`);
                box.className = 'answer-box'; 
                box.querySelector('.answer-text').innerHTML = q[l];
            });

            updateMoneyTree();
            document.getElementById('explain-btn').style.display = 'none';
            isAnswering = true;
            hasGuessed = false;
            
            audioNhacch.currentTime = 0;
            audioNhacch.play().catch(e => playToneFallback(150, 'sawtooth', 0.5));
        }

        // --- 7. XỬ LÝ ĐÁP ÁN ---
        function startGame() {
            switchScreen('game-container');
            initMoneyTree();
            setTimeout(() => { loadQuestion(); }, 1000);
        }

        function selectAnswer(selected) {
            if (!isAnswering || hasGuessed) return;
            hasGuessed = true;
            isAnswering = false;

            audioNhacch.pause();
            playToneFallback(440, 'square', 0.1, 0.1);

            const selectedBox = document.getElementById(`ans-${selected}`);
            selectedBox.classList.add('selected');

            setTimeout(() => { checkAnswer(selected); }, 2000);
        }

        function checkAnswer(selected) {
            const q = questionsData[currentQuestionIndex];
            const correctBox = document.getElementById(`ans-${q.correct}`);
            const selectedBox = document.getElementById(`ans-${selected}`);

            if (selected === q.correct) {
                correctBox.classList.remove('selected');
                correctBox.classList.add('correct');
                
                audioDung.currentTime = 0;
                audioDung.play().catch(e => { playToneFallback(523.25, 'sine', 0.2); setTimeout(() => playToneFallback(1046.50, 'sine', 0.8), 200); });
                
                document.getElementById('explain-btn').style.display = 'block';

                setTimeout(() => {
                    showPopup("CHÍNH XÁC!", `Bạn đã vượt qua câu hỏi số ${currentQuestionIndex + 1}.`, "Câu tiếp theo", () => {
                        closePopup();
                        currentQuestionIndex++;
                        loadQuestion();
                    });
                }, 3000);

            } else {
                selectedBox.classList.remove('selected');
                selectedBox.classList.add('wrong');
                correctBox.classList.add('correct'); 
                
                audioSai.currentTime = 0;
                audioSai.play().catch(e => playToneFallback(200, 'sawtooth', 1.5));

                document.getElementById('explain-btn').style.display = 'block';

                let reward = "0 VNĐ";
                if (currentQuestionIndex >= 10) reward = moneyLevels[9]; 
                else if (currentQuestionIndex >= 5) reward = moneyLevels[4]; 

                setTimeout(() => {
                    showPopup("RẤT TIẾC!", `Đáp án đúng là ${q.correct}.<br><br>Trò chơi kết thúc.<br>Điểm thưởng ra về: <strong class="text-yellow-400 text-2xl">${reward}</strong>.`, "Chơi lại", () => { location.reload(); });
                }, 3000);
            }
        }

        function gameWon() {
            audioNhacch.pause();
            audioWin.currentTime = 0;
            audioWin.play().catch(e => playToneFallback(523.25, 'sine', 2));
            
            triggerConfetti();
            document.getElementById('explain-btn').style.display = 'none';
            
            showPopup("🎉 CHÚC MỪNG 🎉", "BẠN ĐÃ TRỞ THÀNH TRIỆU PHÚ!<br>Hoàn thành xuất sắc 15 câu hỏi của bài học.", "Chơi lại", () => { location.reload(); });
        }

        // --- 8. TRỢ GIÚP ---
        function use5050() {
            if (!lifelines['5050'] || hasGuessed || !isAnswering) return;
            lifelines['5050'] = false;
            document.getElementById('btn-5050').disabled = true;
            playToneFallback(440, 'square', 0.1);

            const q = questionsData[currentQuestionIndex];
            const letters = ['A', 'B', 'C', 'D'];
            const wrongAnswers = letters.filter(l => l !== q.correct);
            
            wrongAnswers.sort(() => Math.random() - 0.5);
            const toHide = [wrongAnswers[0], wrongAnswers[1]];

            toHide.forEach(l => { document.getElementById(`ans-${l}`).classList.add('hidden-answer'); });
        }

        function useAudience() {
            if (!lifelines['audience'] || hasGuessed || !isAnswering) return;
            lifelines['audience'] = false;
            document.getElementById('btn-audience').disabled = true;
            playToneFallback(440, 'square', 0.1);

            const q = questionsData[currentQuestionIndex];
            const letters = ['A', 'B', 'C', 'D'];
            
            let percentages = {};
            let remaining = 100;
            
            let correctPct = Math.floor(Math.random() * 41) + 40; // 40-80%
            percentages[q.correct] = correctPct;
            remaining -= correctPct;

            let wrongLetters = letters.filter(l => l !== q.correct);
            for (let i = 0; i < wrongLetters.length; i++) {
                let l = wrongLetters[i];
                if (document.getElementById(`ans-${l}`).classList.contains('hidden-answer')) {
                    percentages[l] = 0;
                } else {
                    if (i === wrongLetters.length - 1) {
                        percentages[l] = remaining; 
                    } else {
                        let pct = Math.floor(Math.random() * (remaining + 1));
                        percentages[l] = pct;
                        remaining -= pct;
                    }
                }
            }

            let chartHtml = `<div class="chart-container">`;
            letters.forEach(l => {
                chartHtml += `
                    <div class="bar-wrapper">
                        <div class="bar-value">${percentages[l]}%</div>
                        <div class="bar" style="height: 0%;" data-height="${percentages[l]}%"></div>
                        <div class="bar-label">${l}</div>
                    </div>`;
            });
            chartHtml += `</div>`;

            showPopup("Khán giả bình chọn", chartHtml, "Cảm ơn khán giả");
            
            setTimeout(() => {
                const bars = document.querySelectorAll('.bar');
                bars.forEach(bar => { bar.style.height = bar.getAttribute('data-height'); });
            }, 100);
        }

        function useCall() {
            if (!lifelines['call'] || hasGuessed || !isAnswering) return;
            lifelines['call'] = false;
            document.getElementById('btn-call').disabled = true;
            playToneFallback(440, 'square', 0.1);

            const q = questionsData[currentQuestionIndex];
            const isCorrect = Math.random() < 0.8;
            let suggestion = q.correct;

            if (!isCorrect) {
                const letters = ['A', 'B', 'C', 'D'].filter(l => l !== q.correct && !document.getElementById(`ans-${l}`).classList.contains('hidden-answer'));
                suggestion = letters[Math.floor(Math.random() * letters.length)];
            }

            showPopup("Gọi điện thoại cho người thân", `📞 "Alo, theo mình nghiên cứu thì đáp án của câu hỏi này khả năng cao là <strong>${suggestion}</strong> nhé!"`, "Cảm ơn");
        }

        // --- 9. TIỆN ÍCH ---
        function showExplanation() {
            const q = questionsData[currentQuestionIndex];
            showPopup("Phân tích kiến thức giáo khoa", `<div class="text-left"><p class="mb-2 text-yellow-400 font-bold">Đáp án chính xác: ${q.correct}</p><p class="text-white bg-blue-900 p-4 rounded">${q.explanation}</p></div>`, "Đóng");
        }

        function showPopup(title, bodyHtml, btnText, btnCallback = null) {
            document.getElementById('popup-title').innerText = title;
            document.getElementById('popup-body').innerHTML = bodyHtml;
            
            const actionBtn = document.getElementById('popup-action-btn');
            if (btnCallback) {
                actionBtn.style.display = 'inline-block';
                actionBtn.innerText = btnText;
                actionBtn.onclick = btnCallback;
                actionBtn.previousElementSibling.style.display = 'none'; 
            } else {
                actionBtn.style.display = 'none';
                actionBtn.previousElementSibling.style.display = 'inline-block';
                actionBtn.previousElementSibling.innerText = btnText || "Đóng";
            }
            document.getElementById('custom-popup').classList.add('active');
        }

        function closePopup(id = 'custom-popup') { document.getElementById(id).classList.remove('active'); }

        function triggerConfetti() {
            var duration = 15 * 1000;
            var animationEnd = Date.now() + duration;
            var defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 300 };
            var interval = setInterval(function() {
                var timeLeft = animationEnd - Date.now();
                if (timeLeft <= 0) return clearInterval(interval);
                var particleCount = 50 * (timeLeft / duration);
                confetti(Object.assign({}, defaults, { particleCount, origin: { x: Math.random() * 0.2 + 0.1, y: Math.random() - 0.2 } }));
                confetti(Object.assign({}, defaults, { particleCount, origin: { x: Math.random() * 0.2 + 0.7, y: Math.random() - 0.2 } }));
            }, 250);
        }
    </script>
</body>
</html>
