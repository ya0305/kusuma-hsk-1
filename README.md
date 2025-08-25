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

        /* Prevent text selection */
        * {
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }

        /* Allow scrolling */
    </style>
</head>
<body>
    <div class="container" id="setupContainer">
        <div class="logo">🔒 UJIAN TERKUNCI</div>
        <div class="subtitle">Sistem Ujian Online dengan Keamanan Maksimal</div>
        
        <div class="warning">
            <strong>⚠️ PERINGATAN KERAS:</strong>
            • Setelah ujian dimulai, Anda TIDAK DAPAT keluar dengan cara APAPUN<br>
            • DILARANG menutup browser, tab, atau keluar dari fullscreen<br>
            • DILARANG menggunakan shortcut keyboard apapun<br>
            • Sistem akan memaksa Anda tetap dalam mode ujian<br>
            • Pastikan Anda siap 100% sebelum memulai!
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

        <button class="start-btn" id="startExam" disabled>🚀 MULAI UJIAN SEKARANG</button>
    </div>

    <div class="exam-container" id="examContainer">
        <div class="exam-header">
            <div class="exam-title">
                🔒 UJIAN BERLANGSUNG - MODE TERKUNCI
            </div>
            <div class="timer" id="timer">00:00:00</div>
        </div>
        <iframe class="exam-frame" id="examFrame"></iframe>
    </div>

    <div class="time-warning" id="timeWarning">
        <div class="pulse">
            ⚠️ PERINGATAN!<br>
            WAKTU TERSISA 5 MENIT!<br>
            SEGERA SELESAIKAN UJIAN ANDA!
        </div>
    </div>

    <div class="lock-screen" id="lockScreen">
        <div class="lock-content">
            <div class="lock-icon">🔒</div>
            <h2>AKSES DITOLAK!</h2>
            <p>Anda tidak dapat keluar dari mode ujian.<br>
            Sistem telah mendeteksi percobaan keluar.<br>
            <strong>LANJUTKAN MENGERJAKAN UJIAN ANDA!</strong></p>
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
        let forceFullscreenInterval;

        // Disable drag and drop
        document.addEventListener('dragstart', e => e.preventDefault());
        document.addEventListener('drop', e => e.preventDefault());
        document.addEventListener('dragover', e => e.preventDefault());

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
            const confirmMessage = `PERINGATAN TERAKHIR!\n\nAnda akan memulai ujian selama ${selectedDuration} menit.\nSetelah dimulai, Anda TIDAK DAPAT keluar dari sistem dengan cara APAPUN!\n\nApakah Anda benar-benar yakin ingin melanjutkan?`;
            
            if (confirm(confirmMessage)) {
                if (confirm('Ini adalah konfirmasi terakhir. Setelah ini Anda akan TERKUNCI dalam sistem ujian. Lanjutkan?')) {
                    startExam();
                }
            }
        });

        function startExam() {
            examStarted = true;
            preventExit = true;
            remainingTime = selectedDuration * 60;
            
            // Sembunyikan setup dan tampilkan ujian
            document.getElementById('setupContainer').style.display = 'none';
            document.getElementById('examContainer').style.display = 'block';
            
            // Load Google Form dengan URL yang sudah di-hardcode
            document.getElementById('examFrame').src = GOOGLE_FORM_URL;
            
            // Mulai semua sistem keamanan
            initializeSecuritySystems();
            
            // Mulai timer
            startTimer();
            
            // Force fullscreen
            enterFullscreen();
            
            // Monitor fullscreen setiap 100ms
            forceFullscreenInterval = setInterval(() => {
                if (!isFullscreen() && examStarted) {
                    enterFullscreen();
                    showLockScreen("Mencoba keluar dari fullscreen!");
                }
            }, 100);
        }

        function initializeSecuritySystems() {
            // Disable semua event yang bisa digunakan untuk keluar
            document.addEventListener('keydown', handleKeyDown, true);
            document.addEventListener('keyup', handleKeyUp, true);
            document.addEventListener('keypress', handleKeyPress, true);
            
            // Monitor visibility
            document.addEventListener('visibilitychange', handleVisibilityChange, true);
            
            // Prevent browser navigation
            window.addEventListener('beforeunload', handleBeforeUnload, true);
            window.addEventListener('unload', handleUnload, true);
            
            // Disable context menu
            document.addEventListener('contextmenu', e => {
                e.preventDefault();
                e.stopPropagation();
                showLockScreen("Mencoba membuka context menu!");
                return false;
            }, true);
            
            // Monitor window blur/focus
            window.addEventListener('blur', () => {
                if (examStarted && preventExit) {
                    showLockScreen("Mencoba beralih window!");
                    window.focus();
                }
            }, true);
            
            // Prevent back button
            window.history.pushState(null, null, window.location.href);
            window.addEventListener('popstate', () => {
                if (preventExit) {
                    window.history.pushState(null, null, window.location.href);
                    showLockScreen("Mencoba menggunakan tombol back!");
                }
            }, true);
            
            // Monitor fullscreen changes
            ['fullscreenchange', 'webkitfullscreenchange', 'mozfullscreenchange', 'MSFullscreenChange'].forEach(event => {
                document.addEventListener(event, () => {
                    if (!isFullscreen() && examStarted && preventExit) {
                        setTimeout(() => {
                            enterFullscreen();
                            showLockScreen("Mencoba keluar dari fullscreen!");
                        }, 50);
                    }
                }, true);
            });
            
            // Override console methods
            overrideConsole();
            
            // Start developer tools detection
            startDevToolsDetection();
        }

        function handleKeyDown(e) {
            if (!examStarted) return;
            
            const key = e.key.toLowerCase();
            const code = e.code.toLowerCase();
            
            // List semua key yang dilarang
            const forbiddenKeys = [
                'f1', 'f2', 'f3', 'f4', 'f5', 'f6', 'f7', 'f8', 'f9', 'f10', 'f11', 'f12',
                'escape', 'tab', 'printscreen', 'insert', 'delete', 'home', 'end', 'pageup', 'pagedown'
            ];
            
            // Block function keys dan keys khusus
            if (forbiddenKeys.includes(key) || forbiddenKeys.includes(code)) {
                e.preventDefault();
                e.stopPropagation();
                e.stopImmediatePropagation();
                showLockScreen(`Mencoba menggunakan tombol ${key.toUpperCase()}!`);
                return false;
            }
            
            // Block semua kombinasi dengan Ctrl
            if (e.ctrlKey) {
                e.preventDefault();
                e.stopPropagation();
                e.stopImmediatePropagation();
                showLockScreen("Mencoba menggunakan shortcut Ctrl!");
                return false;
            }
            
            // Block semua kombinasi dengan Alt
            if (e.altKey) {
                e.preventDefault();
                e.stopPropagation();
                e.stopImmediatePropagation();
                showLockScreen("Mencoba menggunakan shortcut Alt!");
                return false;
            }
            
            // Block Windows/Cmd key
            if (e.metaKey) {
                e.preventDefault();
                e.stopPropagation();
                e.stopImmediatePropagation();
                showLockScreen("Mencoba menggunakan Windows/Cmd key!");
                return false;
            }
            
            return false;
        }

        function handleKeyUp(e) {
            if (examStarted) {
                e.preventDefault();
                e.stopPropagation();
                return false;
            }
        }

        function handleKeyPress(e) {
            if (examStarted) {
                // Allow normal typing for form input
                if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') {
                    return true;
                }
            }
        }

        function handleVisibilityChange() {
            if (document.hidden && examStarted && preventExit) {
                showLockScreen("Mencoba beralih tab/window!");
                // Force focus back
                setTimeout(() => {
                    window.focus();
                }, 100);
            }
        }

        function handleBeforeUnload(e) {
            if (preventExit) {
                showLockScreen("Mencoba menutup halaman!");
                e.preventDefault();
                e.returnValue = 'Ujian sedang berlangsung!';
                return 'Ujian sedang berlangsung!';
            }
        }

        function handleUnload(e) {
            if (preventExit) {
                e.preventDefault();
                return false;
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
                    }, 15000);
                }
                
                // Peringatan 1 menit tersisa
                if (remainingTime === 60) {
                    document.getElementById('timeWarning').innerHTML = `
                        <div class="pulse">
                            🚨 WAKTU HAMPIR HABIS!<br>
                            TERSISA 1 MENIT!<br>
                            SUBMIT SEKARANG!
                        </div>
                    `;
                    document.getElementById('timeWarning').style.display = 'block';
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
            clearInterval(forceFullscreenInterval);
            
            document.getElementById('timeWarning').style.display = 'none';
            
            // Show final warning
            alert('⏰ WAKTU UJIAN TELAH HABIS!\n\nSilakan submit jawaban Anda SEKARANG jika belum!\nSetelah itu sistem akan otomatis keluar.');
            
            // Give 30 seconds for submission
            setTimeout(() => {
                preventExit = false;
                examStarted = false;
                
                if (confirm('Ujian telah selesai. Klik OK untuk keluar dari sistem ujian.')) {
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
            } else if (elem.mozRequestFullScreen) {
                elem.mozRequestFullScreen();
            }
        }

        function exitFullscreen() {
            if (document.exitFullscreen) {
                document.exitFullscreen().catch(() => {});
            } else if (document.webkitExitFullscreen) {
                document.webkitExitFullscreen();
            } else if (document.msExitFullscreen) {
                document.msExitFullscreen();
            } else if (document.mozCancelFullScreen) {
                document.mozCancelFullScreen();
            }
        }

        function isFullscreen() {
            return !!(document.fullscreenElement || document.webkitFullscreenElement || 
                     document.msFullscreenElement || document.mozFullScreenElement);
        }

        function showLockScreen(reason = "Percobaan keluar terdeteksi!") {
            const lockScreen = document.getElementById('lockScreen');
            lockScreen.style.display = 'flex';
            
            // Update reason
            lockScreen.querySelector('h2').textContent = "🚨 " + reason;
            
            // Clear existing timeout
            if (lockScreenTimeout) {
                clearTimeout(lockScreenTimeout);
            }
            
            // Hide after 3 seconds
            lockScreenTimeout = setTimeout(() => {
                lockScreen.style.display = 'none';
                if (examStarted) {
                    enterFullscreen();
                    window.focus();
                }
            }, 3000);
            
            // Force focus and fullscreen
            window.focus();
            enterFullscreen();
        }

        function overrideConsole() {
            const methods = ['log', 'warn', 'error', 'info', 'debug', 'trace'];
            methods.forEach(method => {
                console[method] = function() {
                    if (examStarted) {
                        showLockScreen("Mencoba membuka Developer Tools!");
                    }
                };
            });
        }

        function startDevToolsDetection() {
            // Basic developer tools detection
            setInterval(() => {
                if (examStarted) {
                    const threshold = 200;
                    if (window.outerHeight - window.innerHeight > threshold || 
                        window.outerWidth - window.innerWidth > threshold) {
                        showLockScreen("Developer Tools terdeteksi!");
                    }
                }
            }, 1000);
            
            // Console clear spam
            setInterval(() => {
                if (examStarted) {
                    console.clear();
                    console.log('%c🔒 UJIAN BERLANGSUNG - AKSES DITOLAK', 'color: red; font-size: 50px; font-weight: bold;');
                }
            }, 100);
        }

        // Disable right click on iframe load
        document.getElementById('examFrame').addEventListener('load', function() {
            try {
                this.contentDocument.addEventListener('contextmenu', function(e) {
                    e.preventDefault();
                    showLockScreen("Context menu diblokir!");
                    return false;
                });
            } catch(e) {
                // Cross-origin restrictions
            }
        });

        // Additional security measures
        document.addEventListener('selectstart', e => e.preventDefault());
        document.addEventListener('mousedown', e => {
            if (e.button === 2) { // Right click
                e.preventDefault();
                showLockScreen("Right click diblokir!");
            }
        });

        // Prevent zoom only, allow normal scroll
        document.addEventListener('wheel', e => {
            if (examStarted && e.ctrlKey) {
                e.preventDefault();
                showLockScreen("Zoom diblokir!");
            }
            // Normal scrolling is allowed
        }, { passive: false });
    </script>
</body>
</html>
