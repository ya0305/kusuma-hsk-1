
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
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
        }

        input[type="url"], select {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="url"]:focus, select:focus {
            outline: none;
            border-color: #667eea;
        }

        .duration-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .duration-option {
            background: #f8f9fa;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            padding: 15px 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .duration-option:hover {
            background: #e3f2fd;
            border-color: #667eea;
        }

        .duration-option.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .start-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 18px 40px;
            font-size: 18px;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            transition: transform 0.2s;
            margin-top: 20px;
            width: 100%;
        }

        .start-btn:hover {
            transform: translateY(-2px);
        }

        .start-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
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
            background: #667eea;
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .timer {
            font-size: 1.5em;
            font-weight: bold;
            background: rgba(255,255,255,0.2);
            padding: 8px 15px;
            border-radius: 25px;
        }

        .exam-frame {
            width: 100%;
            height: calc(100vh - 70px);
            border: none;
        }

        .warning {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            color: #856404;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 20px;
            font-size: 14px;
            line-height: 1.4;
        }

        .warning strong {
            display: block;
            margin-bottom: 5px;
        }

        .time-warning {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #dc3545;
            color: white;
            padding: 15px 20px;
            border-radius: 10px;
            font-weight: bold;
            z-index: 10000;
            display: none;
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }

        .fullscreen-lock {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(0,0,0,0.95);
            color: white;
            display: none;
            align-items: center;
            justify-content: center;
            text-align: center;
            z-index: 99999;
            font-size: 1.5em;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="logo">📝 UJIAN ONLINE</div>
        <div class="subtitle">Sistem Ujian Terkunci dengan Timer</div>
        
        <div class="warning">
            <strong>⚠️ PERINGATAN PENTING:</strong>
            • Setelah ujian dimulai, Anda TIDAK DAPAT keluar dari layar ujian<br>
            • Jangan tutup browser atau tab selama ujian berlangsung<br>
            • Pastikan koneksi internet stabil<br>
            • Siapkan diri Anda sebelum memulai ujian
        </div>

        <div class="form-group">
            <label for="formUrl">🔗 Link Google Form:</label>
            <input type="url" id="formUrl" placeholder="https://docs.google.com/forms/d/e/..." required>
        </div>

        <div class="form-group">
            <label>⏱️ Durasi Ujian:</label>
            <div class="duration-grid">
                <div class="duration-option" data-minutes="40">40 Menit</div>
                <div class="duration-option" data-minutes="60">60 Menit</div>
                <div class="duration-option" data-minutes="90">90 Menit</div>
                <div class="duration-option" data-minutes="120">120 Menit</div>
                <div class="duration-option" data-minutes="150">150 Menit</div>
                <div class="duration-option" data-minutes="180">180 Menit</div>
            </div>
        </div>

        <button class="start-btn" id="startExam" disabled>🚀 MULAI UJIAN</button>
    </div>

    <div class="exam-container" id="examContainer">
        <div class="exam-header">
            <div style="font-weight: bold; font-size: 1.2em;">📝 UJIAN BERLANGSUNG</div>
            <div class="timer" id="timer">00:00:00</div>
        </div>
        <iframe class="exam-frame" id="examFrame"></iframe>
    </div>

    <div class="time-warning" id="timeWarning">
        ⚠️ WAKTU TERSISA 5 MENIT!
    </div>

    <div class="fullscreen-lock" id="fullscreenLock">
        <div>
            <h2>🔒 UJIAN TERKUNCI</h2>
            <p>Anda tidak dapat keluar dari mode ujian.<br>Lanjutkan mengerjakan soal Anda.</p>
        </div>
    </div>

    <script>
        let selectedDuration = 0;
        let timerInterval;
        let remainingTime = 0;
        let examStarted = false;
        let preventExit = false;

        // Pilih durasi ujian
        document.querySelectorAll('.duration-option').forEach(option => {
            option.addEventListener('click', () => {
                document.querySelectorAll('.duration-option').forEach(opt => 
                    opt.classList.remove('selected'));
                option.classList.add('selected');
                selectedDuration = parseInt(option.dataset.minutes);
                checkFormValid();
            });
        });

        // Validasi form
        document.getElementById('formUrl').addEventListener('input', checkFormValid);

        function checkFormValid() {
            const url = document.getElementById('formUrl').value;
            const isValidGoogleForm = url.includes('docs.google.com/forms') || url.includes('forms.gle');
            const startBtn = document.getElementById('startExam');
            
            if (isValidGoogleForm && selectedDuration > 0) {
                startBtn.disabled = false;
            } else {
                startBtn.disabled = true;
            }
        }

        // Mulai ujian
        document.getElementById('startExam').addEventListener('click', () => {
            if (confirm('Apakah Anda yakin ingin memulai ujian? Setelah dimulai, Anda tidak dapat keluar dari layar ujian.')) {
                startExam();
            }
        });

        function startExam() {
            examStarted = true;
            preventExit = true;
            
            const formUrl = document.getElementById('formUrl').value;
            remainingTime = selectedDuration * 60; // Convert to seconds
            
            // Sembunyikan form dan tampilkan ujian
            document.querySelector('.container').style.display = 'none';
            document.getElementById('examContainer').style.display = 'block';
            
            // Load Google Form
            document.getElementById('examFrame').src = formUrl;
            
            // Mulai timer
            startTimer();
            
            // Aktifkan mode fullscreen
            enterFullscreen();
            
            // Aktifkan pencegahan keluar
            enableExitPrevention();
            
            // Disable browser shortcuts
            disableBrowserShortcuts();
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
                timerElement.style.animation = 'pulse 1s infinite';
            }
        }

        function endExam() {
            clearInterval(timerInterval);
            preventExit = false;
            examStarted = false;
            
            alert('Waktu ujian telah habis! Silakan submit jawaban Anda jika belum.');
            
            // Biarkan siswa submit form terlebih dahulu
            setTimeout(() => {
                if (confirm('Ujian telah selesai. Klik OK untuk keluar dari mode ujian.')) {
                    exitFullscreen();
                    location.reload();
                }
            }, 3000);
        }

        function enterFullscreen() {
            if (document.documentElement.requestFullscreen) {
                document.documentElement.requestFullscreen();
            } else if (document.documentElement.webkitRequestFullscreen) {
                document.documentElement.webkitRequestFullscreen();
            } else if (document.documentElement.msRequestFullscreen) {
                document.documentElement.msRequestFullscreen();
            }
        }

        function exitFullscreen() {
            if (document.exitFullscreen) {
                document.exitFullscreen();
            } else if (document.webkitExitFullscreen) {
                document.webkitExitFullscreen();
            } else if (document.msExitFullscreen) {
                document.msExitFullscreen();
            }
        }

        function enableExitPrevention() {
            // Prevent tab switching
            document.addEventListener('visibilitychange', handleVisibilityChange);
            
            // Prevent browser back button
            window.history.pushState(null, null, window.location.href);
            window.addEventListener('popstate', preventBack);
            
            // Prevent page close/refresh
            window.addEventListener('beforeunload', preventClose);
            
            // Prevent right click and context menu
            document.addEventListener('contextmenu', preventAction);
            
            // Monitor fullscreen exit
            document.addEventListener('fullscreenchange', handleFullscreenChange);
            document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
            document.addEventListener('msfullscreenchange', handleFullscreenChange);
        }

        function disableBrowserShortcuts() {
            document.addEventListener('keydown', function(e) {
                if (!examStarted) return;
                
                // Disable F11, F5, Ctrl+R, Ctrl+W, Ctrl+T, Alt+Tab, dll
                const forbiddenKeys = [
                    'F1', 'F2', 'F3', 'F4', 'F5', 'F6', 'F7', 'F8', 'F9', 'F10', 'F11', 'F12'
                ];
                
                if (forbiddenKeys.includes(e.key)) {
                    e.preventDefault();
                    e.stopPropagation();
                    showLockScreen();
                    return false;
                }
                
                // Disable Ctrl combinations
                if (e.ctrlKey && ['r', 'R', 'w', 'W', 't', 'T', 'n', 'N', 's', 'S', 'a', 'A', 'c', 'C', 'v', 'V', 'x', 'X', 'z', 'Z', 'y', 'Y'].includes(e.key)) {
                    e.preventDefault();
                    e.stopPropagation();
                    showLockScreen();
                    return false;
                }
                
                // Disable Alt+Tab
                if (e.altKey && e.key === 'Tab') {
                    e.preventDefault();
                    e.stopPropagation();
                    showLockScreen();
                    return false;
                }
                
                // Disable Windows key
                if (e.key === 'Meta') {
                    e.preventDefault();
                    e.stopPropagation();
                    showLockScreen();
                    return false;
                }
            });
        }

        function handleVisibilityChange() {
            if (document.hidden && examStarted && preventExit) {
                showLockScreen();
            }
        }

        function handleFullscreenChange() {
            if (!document.fullscreenElement && !document.webkitFullscreenElement && 
                !document.msFullscreenElement && examStarted && preventExit) {
                enterFullscreen();
                showLockScreen();
            }
        }

        function preventBack(e) {
            if (preventExit) {
                window.history.pushState(null, null, window.location.href);
                showLockScreen();
            }
        }

        function preventClose(e) {
            if (preventExit) {
                e.preventDefault();
                e.returnValue = '';
                showLockScreen();
                return '';
            }
        }

        function preventAction(e) {
            if (examStarted) {
                e.preventDefault();
                return false;
            }
        }

        function showLockScreen() {
            document.getElementById('fullscreenLock').style.display = 'flex';
            setTimeout(() => {
                document.getElementById('fullscreenLock').style.display = 'none';
                if (examStarted) {
                    enterFullscreen();
                }
            }, 2000);
        }

        // Override console dan developer tools
        setInterval(() => {
            if (examStarted) {
                console.clear();
                console.log('%cUJIAN BERLANGSUNG - DEVELOPER TOOLS TIDAK DIIZINKAN', 'color: red; font-size: 30px; font-weight: bold;');
            }
        }, 100);

        // Deteksi developer tools (basic)
        let devtools = {open: false, orientation: null};
        setInterval(() => {
            if (examStarted && (window.outerHeight - window.innerHeight > 200 || window.outerWidth - window.innerWidth > 200)) {
                showLockScreen();
                console.clear();
            }
        }, 500);
    </script>
</body>
</html>
