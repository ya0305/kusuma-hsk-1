<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Ujian Online</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .container {
            background: white;
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            max-width: 500px;
            width: 90%;
            text-align: center;
        }

        .logo {
            font-size: 2.5em;
            color: #667eea;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .subtitle {
            color: #666;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .form-group {
            margin-bottom: 25px;
            text-align: left;
        }

        label {
            display: block;
            margin-bottom: 15px;
            color: #333;
            font-weight: 600;
            font-size: 1.1em;
        }

        .duration-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-top: 10px;
        }

        .duration-option {
            background: #f8f9fa;
            border: 3px solid #e0e0e0;
            border-radius: 15px;
            padding: 20px 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
            font-size: 1em;
            color: #333;
        }

        .duration-option:hover {
            background: #e3f2fd;
            border-color: #667eea;
            transform: translateY(-3px);
        }

        .duration-option.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
            transform: scale(1.05);
        }

        .start-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 20px 40px;
            font-size: 20px;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 30px;
            width: 100%;
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .start-btn:hover:not(:disabled) {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.6);
        }

        .start-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }

        .exam-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: white;
            z-index: 9999;
        }

        .exam-header {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
            position: relative;
            z-index: 10000;
        }

        .exam-title {
            font-size: 1.3em;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .timer {
            font-size: 1.8em;
            font-weight: bold;
            background: rgba(255,255,255,0.2);
            padding: 10px 20px;
            border-radius: 30px;
            border: 2px solid rgba(255,255,255,0.3);
            backdrop-filter: blur(10px);
        }

        .exam-frame {
            width: 100%;
            height: calc(100vh - 80px);
            border: none;
            background: white;
        }

        .warning {
            background: linear-gradient(45deg, #fff3cd, #ffeaa7);
            border: 2px solid #ffc107;
            color: #856404;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 25px;
            font-size: 14px;
            line-height: 1.6;
            box-shadow: 0 4px 15px rgba(255, 193, 7, 0.2);
        }

        .warning strong {
            display: block;
            margin-bottom: 10px;
            font-size: 16px;
            color: #dc3545;
        }

        .time-warning {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(45deg, #dc3545, #c82333);
            color: white;
            padding: 25px 35px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 1.3em;
            z-index: 99999;
            display: none;
            text-align: center;
            box-shadow: 0 10px 40px rgba(220, 53, 69, 0.4);
            border: 3px solid rgba(255,255,255,0.3);
        }

        .lock-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(45deg, rgba(0,0,0,0.95), rgba(220,53,69,0.9));
            color: white;
            display: none;
            align-items: center;
            justify-content: center;
            text-align: center;
            z-index: 999999;
            font-size: 1.8em;
            backdrop-filter: blur(20px);
        }

        .lock-content {
            background: rgba(0,0,0,0.8);
            padding: 50px;
            border-radius: 20px;
            border: 3px solid #dc3545;
            max-width: 600px;
        }

        .lock-icon {
            font-size: 4em;
            margin-bottom: 20px;
            animation: shake 0.5s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(-5deg); }
            75% { transform: rotate(5deg); }
        }

        @keyframes pulse {
            0% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.8; transform: scale(1.05); }
            100% { opacity: 1; transform: scale(1); }
        }

        .pulse {
            animation: pulse 1s infinite;
        }
    </style>
</head>
<body>
    <div class="container" id="setupContainer">
        <div class="logo">📝 UJIAN ONLINE</div>
        <div class="subtitle">Sistem Ujian dengan Timer dan Lock Screen</div>
        
        <div class="warning">
            <strong>⚠️ PERHATIAN:</strong>
            • Setelah ujian dimulai, Anda tidak dapat keluar dari layar ujian<br>
            • Pastikan koneksi internet stabil sebelum memulai<br>
            • Pilih durasi ujian dengan hati-hati<br>
            • Kerjakan dengan tenang dan fokus
        </div>

        <div class="form-group">
            <label>⏱️ Pilih Durasi Ujian:</label>
            <div class="duration-grid">
                <div class="duration-option" data-minutes="40">40<br>Menit</div>
                <div class="duration-option" data-minutes="60">60<br>Menit</div>
                <div class="duration-option" data-minutes="90">90<br>Menit</div>
                <div class="duration-option" data-minutes="120">120<br>Menit</div>
                <div class="duration-option" data-minutes="150">150<br>Menit</div>
                <div class="duration-option" data-minutes="180">180<br>Menit</div>
            </div>
        </div>

        <button class="start-btn" id="startExam" disabled>🚀 MULAI UJIAN</button>
    </div>

    <div class="exam-container" id="examContainer">
        <div class="exam-header">
            <div class="exam-title">
                📝 UJIAN BERLANGSUNG
            </div>
            <div class="timer" id="timer">00:00:00</div>
        </div>
        <iframe class="exam-frame" id="examFrame"></iframe>
    </div>

    <div class="time-warning" id="timeWarning">
        <div class="pulse">
            ⚠️ WAKTU TERSISA 5 MENIT!<br>
            SEGERA SELESAIKAN UJIAN!
        </div>
    </div>

    <div class="lock-screen" id="lockScreen">
        <div class="lock-content">
            <div class="lock-icon">🔒</div>
            <h2>UJIAN BERLANGSUNG</h2>
            <p>Mohon fokus pada ujian Anda.<br>
            Sistem telah mendeteksi aktivitas yang tidak diizinkan.<br>
            <strong>Silakan lanjutkan mengerjakan soal.</strong></p>
        </div>
    </div>

    <script>
        // HARDCODED GOOGLE FORM URL - GANTI DENGAN URL FORM ANDA
        const GOOGLE_FORM_URL = "https://docs.google.com/forms/d/e/1FAIpQLSf_CONTOH_URL_FORM_ANDA/viewform";
        
        let selectedDuration = 0;
        let timerInterval;
        let remainingTime = 0;
        let examStarted = false;
        let preventExit = false;
        let lockScreenTimeout;
        let isSetupPhase = true;

        // Pilih durasi ujian
        document.querySelectorAll('.duration-option').forEach(option => {
            option.addEventListener('click', () => {
                document.querySelectorAll('.duration-option').forEach(opt => 
                    opt.classList.remove('selected'));
                option.classList.add('selected');
                selectedDuration = parseInt(option.dataset.minutes);
                document.getElementById('startExam').disabled = false;
            });
        });

        // Mulai ujian
        document.getElementById('startExam').addEventListener('click', () => {
            const confirmMessage = `Anda akan memulai ujian selama ${selectedDuration} menit.\nSetelah dimulai, sistem akan mengaktifkan mode pengawasan.\n\nLanjutkan?`;
            
            if (confirm(confirmMessage)) {
                startExam();
            }
        });

        function startExam() {
            isSetupPhase = false;
            examStarted = true;
            remainingTime = selectedDuration * 60;
            
            // Sembunyikan setup dan tampilkan ujian
            document.getElementById('setupContainer').style.display = 'none';
            document.getElementById('examContainer').style.display = 'block';
            
            // Load Google Form
            document.getElementById('examFrame').src = [GOOGLE_FORM_URL](https://docs.google.com/forms/d/e/1FAIpQLSfUbsIWpYyWydQ2DLIrey7nzau05c-EXBWzAlDcgU1bd-OLTg/viewform?usp=header);
            
            // Mulai timer
            startTimer();
            
            // Tunggu 3 detik sebelum mengaktifkan sistem keamanan
            setTimeout(() => {
                preventExit = true;
                initializeSecuritySystems();
                enterFullscreen();
            }, 3000);
        }

        function initializeSecuritySystems() {
            // Monitor key combinations yang berbahaya
            document.addEventListener('keydown', handleKeyDown, true);
            
            // Monitor visibility changes
            document.addEventListener('visibilitychange', handleVisibilityChange);
            
            // Prevent browser navigation
            window.addEventListener('beforeunload', handleBeforeUnload);
            
            // Disable context menu
            document.addEventListener('contextmenu', e => {
                if (preventExit) {
                    e.preventDefault();
                    showLockScreen("Context menu diblokir selama ujian");
                    return false;
                }
            });
            
            // Monitor window focus
            window.addEventListener('blur', handleWindowBlur);
            
            // Prevent back button
            window.history.pushState(null, null, window.location.href);
            window.addEventListener('popstate', handlePopState);
            
            // Monitor fullscreen
            ['fullscreenchange', 'webkitfullscreenchange', 'mozfullscreenchange'].forEach(event => {
                document.addEventListener(event, handleFullscreenChange);
            });
        }

        function handleKeyDown(e) {
            if (!preventExit) return;
            
            const key = e.key.toLowerCase();
            
            // Hanya blokir kombinasi yang benar-benar berbahaya
            const dangerousCombinations = [
                // Function keys
                ['f5'], ['f11'], ['f12'],
                // Alt combinations
                ['alt', 'tab'], ['alt', 'f4'],
                // Ctrl combinations yang berbahaya
                ['ctrl', 'w'], ['ctrl', 'n'], ['ctrl', 't'], ['ctrl', 'r'], 
                ['ctrl', 'shift', 'i'], ['ctrl', 'shift', 'j'], ['ctrl', 'u']
            ];
            
            let isDangerous = false;
            
            // Check F5 dan F11
            if (key === 'f5' || key === 'f11' || key === 'f12') {
                isDangerous = true;
            }
            
            // Check Alt+Tab
            if (e.altKey && key === 'tab') {
                isDangerous = true;
            }
            
            // Check Ctrl combinations
            if (e.ctrlKey) {
                const ctrlKeys = ['w', 'n', 't', 'r', 'u'];
                if (ctrlKeys.includes(key)) {
                    isDangerous = true;
                }
                if (e.shiftKey && (key === 'i' || key === 'j')) {
                    isDangerous = true;
                }
            }
            
            if (isDangerous) {
                e.preventDefault();
                e.stopPropagation();
                showLockScreen("Shortcut tidak diizinkan selama ujian");
                return false;
            }
        }

        function handleVisibilityChange() {
            if (document.hidden && preventExit) {
                showLockScreen("Jangan beralih tab selama ujian");
            }
        }

        function handleWindowBlur() {
            if (preventExit) {
                setTimeout(() => {
                    window.focus();
                    showLockScreen("Tetap fokus pada ujian");
                }, 100);
            }
        }

        function handleBeforeUnload(e) {
            if (preventExit) {
                e.preventDefault();
                e.returnValue = 'Ujian sedang berlangsung!';
                return 'Ujian sedang berlangsung!';
            }
        }

        function handlePopState() {
            if (preventExit) {
                window.history.pushState(null, null, window.location.href);
                showLockScreen("Tombol back tidak berfungsi selama ujian");
            }
        }

        function handleFullscreenChange() {
            if (!isFullscreen() && preventExit) {
                setTimeout(() => {
                    enterFullscreen();
                    showLockScreen("Mode fullscreen diperlukan");
                }, 500);
            }
        }

        function startTimer() {
            updateTimerDisplay();
            
            timerInterval = setInterval(() => {
                remainingTime--;
                updateTimerDisplay();
                
                // Peringatan 5 menit tersisa
                if (remainingTime === 300) {
                    document.getElementById('timeWarning').style.display = 'block';
                    setTimeout(() => {
                        document.getElementById('timeWarning').style.display = 'none';
                    }, 10000);
                }
                
                // Waktu habis
                if (remainingTime <= 0) {
                    endExam();
                }
            }, 1000);
        }

        function updateTimerDisplay() {
            const hours = Math.floor(remainingTime / 3600);
            const minutes = Math.floor((remainingTime % 3600) / 60);
            const seconds = remainingTime % 60;
            
            const display = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            document.getElementById('timer').textContent = display;
            
            // Ubah warna timer jika waktu hampir habis
            const timerElement = document.getElementById('timer');
            if (remainingTime <= 300) { // 5 menit
                timerElement.style.background = 'rgba(220, 53, 69, 0.8)';
                timerElement.classList.add('pulse');
            }
        }

        function endExam() {
            clearInterval(timerInterval);
            preventExit = false;
            examStarted = false;
            
            alert('⏰ WAKTU UJIAN TELAH HABIS!\n\nSilakan submit jawaban Anda jika belum!');
            
            // Beri waktu 30 detik untuk submit
            setTimeout(() => {
                if (confirm('Ujian selesai. Keluar dari sistem?')) {
                    exitFullscreen();
                    window.location.href = 'about:blank';
                }
            }, 30000);
        }

        function enterFullscreen() {
            const elem = document.documentElement;
            if (elem.requestFullscreen) {
                elem.requestFullscreen().catch(() => {});
            } else if (elem.webkitRequestFullscreen) {
                elem.webkitRequestFullscreen();
            } else if (elem.msRequestFullscreen) {
                elem.msRequestFullscreen();
            }
        }

        function exitFullscreen() {
            if (document.exitFullscreen) {
                document.exitFullscreen().catch(() => {});
            } else if (document.webkitExitFullscreen) {
                document.webkitExitFullscreen();
            } else if (document.msExitFullscreen) {
                document.msExitFullscreen();
            }
        }

        function isFullscreen() {
            return !!(document.fullscreenElement || document.webkitFullscreenElement || 
                     document.msFullscreenElement || document.mozFullScreenElement);
        }

        function showLockScreen(message = "Mohon fokus pada ujian") {
            if (!preventExit) return; // Jangan tampilkan jika ujian belum mulai
            
            const lockScreen = document.getElementById('lockScreen');
            lockScreen.querySelector('p').innerHTML = `${message}<br><strong>Silakan lanjutkan mengerjakan soal.</strong>`;
            lockScreen.style.display = 'flex';
            
            // Clear existing timeout
            if (lockScreenTimeout) {
                clearTimeout(lockScreenTimeout);
            }
            
            // Hide after 2 seconds
            lockScreenTimeout = setTimeout(() => {
                lockScreen.style.display = 'none';
                if (preventExit) {
                    window.focus();
                }
            }, 2000);
        }

        // Tambahan untuk mencegah select text (opsional)
        document.addEventListener('selectstart', e => {
            if (preventExit) {
                e.preventDefault();
            }
        });
    </script>
</body>
</html>
