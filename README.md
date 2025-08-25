<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ujian Online - Anti Cheating</title>
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
        }

        body.exam-mode {
            overflow: hidden;
        }

        /* Setup Screen */
        .setup-container {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            padding: 20px;
        }

        .setup-card {
            background: white;
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            max-width: 500px;
            width: 100%;
            text-align: center;
        }

        .logo {
            font-size: 3rem;
            margin-bottom: 10px;
        }

        .title {
            font-size: 2rem;
            color: #667eea;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .subtitle {
            color: #666;
            margin-bottom: 30px;
        }

        .form-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
        }

        .form-group input, .form-group textarea {
            width: 100%;
            padding: 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        .form-group input:focus, .form-group textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .form-group textarea {
            resize: vertical;
            min-height: 80px;
        }

        .duration-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 10px;
        }

        .duration-option {
            padding: 15px;
            border: 2px solid #e2e8f0;
            border-radius: 10px;
            cursor: pointer;
            text-align: center;
            transition: all 0.3s;
            font-weight: 600;
        }

        .duration-option:hover {
            border-color: #667eea;
            background: #f8faff;
        }

        .duration-option.selected {
            border-color: #667eea;
            background: linear-gradient(135deg, #667eea20, #764ba220);
            color: #667eea;
        }

        .btn {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 25px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
            width: 100%;
            margin-top: 20px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        /* Exam Screen */
        .exam-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            z-index: 9999;
        }

        .exam-header {
            background: rgba(255,255,255,0.95);
            backdrop-filter: blur(10px);
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .student-info {
            font-size: 18px;
            font-weight: 600;
            color: #333;
        }

        .timer {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            padding: 15px 25px;
            border-radius: 25px;
            font-size: 24px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(238, 90, 36, 0.4);
            animation: pulse 2s infinite;
            min-width: 150px;
            text-align: center;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .timer.warning {
            background: linear-gradient(135deg, #ff9ff3, #f368e0);
            animation: fastPulse 1s infinite;
        }

        .timer.danger {
            background: linear-gradient(135deg, #ff3b3b, #c44569);
            animation: fastPulse 0.5s infinite;
        }

        @keyframes fastPulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .exam-content {
            height: calc(100vh - 90px);
            padding: 20px;
            overflow-y: auto;
        }

        .exam-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            margin: 0 auto;
            max-width: 1000px;
            min-height: calc(100vh - 130px);
            display: flex;
            flex-direction: column;
        }

        .form-title {
            font-size: 24px;
            color: #667eea;
            margin-bottom: 20px;
            text-align: center;
            font-weight: bold;
        }

        .form-instructions {
            background: #f8faff;
            border-left: 4px solid #667eea;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 5px;
            font-size: 14px;
        }

        .google-form-container {
            flex: 1;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            min-height: 500px;
        }

        .google-form-frame {
            width: 100%;
            height: 100%;
            border: none;
            min-height: 600px;
        }

        /* Anti-Cheating Overlay */
        .anti-cheat-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(220, 38, 38, 0.95);
            backdrop-filter: blur(10px);
            z-index: 99999;
            color: white;
            text-align: center;
            align-items: center;
            justify-content: center;
            flex-direction: column;
        }

        .warning-icon {
            font-size: 8rem;
            margin-bottom: 30px;
            animation: shake 0.5s infinite;
        }

        @keyframes shake {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(-5deg); }
            75% { transform: rotate(5deg); }
        }

        .warning-title {
            font-size: 3rem;
            font-weight: bold;
            margin-bottom: 20px;
        }

        .warning-message {
            font-size: 1.5rem;
            margin-bottom: 30px;
            max-width: 600px;
        }

        .warning-counter {
            font-size: 2rem;
            font-weight: bold;
            color: #fbbf24;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(0,0,0,0.8);
            z-index: 100000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            max-width: 500px;
            margin: 20px;
        }

        .modal-icon {
            font-size: 4rem;
            margin-bottom: 20px;
        }

        .success-icon { color: #10b981; }
        .warning-icon-modal { color: #f59e0b; }
        .danger-icon { color: #ef4444; }

        /* Scrollbar untuk exam content */
        .exam-content::-webkit-scrollbar {
            width: 8px;
        }

        .exam-content::-webkit-scrollbar-track {
            background: rgba(255,255,255,0.1);
            border-radius: 4px;
        }

        .exam-content::-webkit-scrollbar-thumb {
            background: rgba(255,255,255,0.3);
            border-radius: 4px;
        }

        .exam-content::-webkit-scrollbar-thumb:hover {
            background: rgba(255,255,255,0.5);
        }

        /* Animation untuk notifikasi */
        @keyframes slideInRight {
            from { 
                transform: translateX(100%); 
                opacity: 0; 
            }
            to { 
                transform: translateX(0); 
                opacity: 1; 
            }
        }

        .notification {
            position: fixed;
            top: 100px;
            right: 30px;
            background: linear-gradient(135deg, #ef4444, #dc2626);
            color: white;
            padding: 20px 30px;
            border-radius: 15px;
            font-size: 18px;
            font-weight: bold;
            z-index: 10001;
            box-shadow: 0 10px 30px rgba(239, 68, 68, 0.4);
            animation: slideInRight 0.5s ease-out;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .setup-card {
                padding: 20px;
                margin: 10px;
            }
            
            .duration-grid {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .exam-header {
                padding: 10px 15px;
                flex-direction: column;
                gap: 10px;
            }
            
            .student-info {
                font-size: 14px;
            }
            
            .timer {
                font-size: 18px;
                padding: 10px 15px;
            }
        }
    </style>
</head>
<body>
    <!-- Setup Screen -->
    <div class="setup-container" id="setup-screen">
        <div class="setup-card">
            <div class="logo">🔒</div>
            <div class="title">Ujian Online</div>
            <div class="subtitle">Anti-Cheating System</div>
            
            <form id="setup-form">
                <div class="form-group">
                    <label>Nama Lengkap:</label>
                    <input type="text" id="student-name" placeholder="Masukkan nama lengkap" required>
                </div>
                
                <div class="form-group">
                    <label>Link Google Form:</label>
                    <textarea id="google-form-url" placeholder="https://docs.google.com/forms/d/e/..." required></textarea>
                    <small style="color: #666;">Paste link Google Form yang akan digunakan untuk ujian</small>
                </div>
                
                <div class="form-group">
                    <label>Durasi Ujian:</label>
                    <div class="duration-grid">
                        <div class="duration-option" data-duration="30">
                            <div style="font-size: 20px;">30</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                        <div class="duration-option" data-duration="60">
                            <div style="font-size: 20px;">60</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                        <div class="duration-option" data-duration="90">
                            <div style="font-size: 20px;">90</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                        <div class="duration-option" data-duration="120">
                            <div style="font-size: 20px;">120</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                        <div class="duration-option" data-duration="150">
                            <div style="font-size: 20px;">150</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                        <div class="duration-option" data-duration="180">
                            <div style="font-size: 20px;">180</div>
                            <div style="font-size: 12px;">menit</div>
                        </div>
                    </div>
                </div>
                
                <button type="submit" class="btn">🚀 Mulai Ujian</button>
            </form>
            
            <div style="margin-top: 20px; font-size: 14px; color: #666;">
                ⚠️ <strong>Peringatan:</strong> Setelah mulai ujian, Anda tidak bisa keluar dari halaman ini atau membuka tab/aplikasi lain sampai ujian selesai.
            </div>
        </div>
    </div>

    <!-- Exam Screen -->
    <div class="exam-container" id="exam-screen">
        <div class="exam-header">
            <div class="student-info">
                <span id="exam-student-name">Nama Siswa</span> | 
                <span>Durasi: <span id="exam-duration-display">60</span> menit</span>
            </div>
            <div class="timer" id="timer">
                <span id="timer-display">01:00:00</span>
            </div>
        </div>
        
        <div class="exam-content">
            <div class="exam-card">
                <div class="form-title">📝 Soal Ujian</div>
                
                <div class="form-instructions">
                    <strong>Petunjuk Ujian:</strong><br>
                    • Timer akan berjalan terus, tidak bisa dihentikan<br>
                    • Jangan coba keluar dari fullscreen atau ganti tab<br>
                    • Ujian akan otomatis berakhir ketika waktu habis<br>
                    • Pastikan submit jawaban sebelum waktu habis
                </div>
                
                <div class="google-form-container">
                    <iframe id="google-form-frame" class="google-form-frame" src="about:blank"></iframe>
                </div>
            </div>
        </div>
    </div>

    <!-- Anti-Cheat Warning Overlay -->
    <div class="anti-cheat-overlay" id="cheat-warning">
        <div class="warning-icon">⚠️</div>
        <div class="warning-title">PERINGATAN!</div>
        <div class="warning-message">
            Terdeteksi aktivitas mencurigakan!<br>
            Anda mencoba keluar dari mode ujian.<br>
            Pelanggaran: <span id="violation-count">1</span>/3
        </div>
        <div class="warning-counter">
            Kembali ke ujian dalam: <span id="warning-countdown">10</span> detik
        </div>
    </div>

    <!-- Finish Modal -->
    <div class="modal" id="finish-modal">
        <div class="modal-content">
            <div class="modal-icon success-icon">✅</div>
            <h2>Ujian Selesai!</h2>
            <p>Waktu ujian telah berakhir atau Anda telah menyelesaikan ujian.</p>
            <button class="btn" onclick="exitExam()" style="margin-top: 20px;">Keluar dari Ujian</button>
        </div>
    </div>

    <!-- Disqualified Modal -->
    <div class="modal" id="disqualified-modal">
        <div class="modal-content">
            <div class="modal-icon danger-icon">❌</div>
            <h2>Ujian Dibatalkan!</h2>
            <p>Anda telah melakukan terlalu banyak pelanggaran.<br>Ujian dibatalkan secara otomatis.</p>
            <button class="btn" onclick="exitExam()" style="margin-top: 20px; background: #ef4444;">Keluar</button>
        </div>
    </div>

    <script>
        let examState = {
            duration: 0,
            timeLeft: 0,
            studentName: '',
            googleFormUrl: '',
            isActive: false,
            violations: 0,
            maxViolations: 3,
            timer: null,
            warningTimer: null,
            isFullscreen: false
        };

        // Initialize duration selection
        document.addEventListener('DOMContentLoaded', function() {
            const durationOptions = document.querySelectorAll('.duration-option');
            durationOptions.forEach(option => {
                option.addEventListener('click', function() {
                    // Remove selected from all options
                    durationOptions.forEach(opt => opt.classList.remove('selected'));
                    // Add selected to clicked option
                    this.classList.add('selected');
                    // Set duration
                    examState.duration = parseInt(this.dataset.duration);
                });
            });
        });

        // Setup form submit
        document.getElementById('setup-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const studentName = document.getElementById('student-name').value.trim();
            const googleFormUrl = document.getElementById('google-form-url').value.trim();
            
            if (!studentName) {
                alert('Silakan masukkan nama lengkap!');
                return;
            }
            
            if (!googleFormUrl) {
                alert('Silakan masukkan link Google Form!');
                return;
            }
            
            if (!examState.duration) {
                alert('Silakan pilih durasi ujian!');
                return;
            }
            
            // Validate Google Form URL
            if (!googleFormUrl.includes('docs.google.com/forms')) {
                alert('Link Google Form tidak valid!');
                return;
            }
            
            examState.studentName = studentName;
            examState.googleFormUrl = googleFormUrl;
            examState.timeLeft = examState.duration * 60; // Convert to seconds
            
            startExam();
        });

        // Start exam
        function startExam() {
            try {
                // Hide setup screen
                document.getElementById('setup-screen').style.display = 'none';
                
                // Show exam screen
                document.getElementById('exam-screen').style.display = 'block';
                
                // Set exam info
                document.getElementById('exam-student-name').textContent = examState.studentName;
                document.getElementById('exam-duration-display').textContent = examState.duration;
                
                // Load Google Form
                const iframe = document.getElementById('google-form-frame');
                iframe.src = examState.googleFormUrl;
                
                // Enter fullscreen and lock
                enterFullscreenLock();
                
                // Start timer
                examState.isActive = true;
                updateTimerDisplay();
                startTimer();
                
                // Enable anti-cheat protection after a delay
                setTimeout(() => {
                    enableAntiCheat();
                }, 1000);
                
                console.log('Ujian dimulai successfully');
                
            } catch (error) {
                console.error('Error starting exam:', error);
                alert('Terjadi kesalahan saat memulai ujian. Silakan refresh halaman dan coba lagi.');
            }
        }

        // Enter fullscreen and lock
        function enterFullscreenLock() {
            try {
                // Request fullscreen
                const docElm = document.documentElement;
                
                const requestFullscreen = docElm.requestFullscreen || 
                                        docElm.webkitRequestFullScreen || 
                                        docElm.mozRequestFullScreen || 
                                        docElm.msRequestFullscreen;
                
                if (requestFullscreen) {
                    requestFullscreen.call(docElm).then(() => {
                        examState.isFullscreen = true;
                        console.log('Fullscreen activated');
                    }).catch(err => {
                        console.warn('Fullscreen failed:', err);
                        // Continue without fullscreen
                    });
                } else {
                    console.warn('Fullscreen not supported');
                }
                
                // Add exam mode class
                document.body.classList.add('exam-mode');
                
            } catch (error) {
                console.error('Error entering fullscreen:', error);
            }
        }

        // Start timer
        function startTimer() {
            if (examState.timer) {
                clearInterval(examState.timer);
            }
            
            examState.timer = setInterval(() => {
                if (!examState.isActive) return;
                
                examState.timeLeft--;
                updateTimerDisplay();
                
                // Time warnings
                if (examState.timeLeft === 300) { // 5 minutes
                    showTimeWarning('⚠️ Waktu tersisa 5 menit!');
                } else if (examState.timeLeft === 60) { // 1 minute
                    showTimeWarning('🚨 Waktu tersisa 1 menit!');
                }
                
                // Time up
                if (examState.timeLeft <= 0) {
                    finishExam();
                }
            }, 1000);
        }

        // Update timer display
        function updateTimerDisplay() {
            const hours = Math.floor(examState.timeLeft / 3600);
            const minutes = Math.floor((examState.timeLeft % 3600) / 60);
            const seconds = examState.timeLeft % 60;
            
            const display = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
            const timerDisplay = document.getElementById('timer-display');
            if (timerDisplay) {
                timerDisplay.textContent = display;
            }
            
            const timerElement = document.getElementById('timer');
            if (timerElement) {
                if (examState.timeLeft <= 300) { // 5 minutes
                    timerElement.className = 'timer danger';
                } else if (examState.timeLeft <= 600) { // 10 minutes
                    timerElement.className = 'timer warning';
                } else {
                    timerElement.className = 'timer';
                }
            }
        }

        // Show time warning
        function showTimeWarning(message) {
            try {
                // Create notification
                const notification = document.createElement('div');
                notification.className = 'notification';
                notification.innerHTML = message;
                
                document.body.appendChild(notification);
                
                setTimeout(() => {
                    if (document.body.contains(notification)) {
                        document.body.removeChild(notification);
                    }
                }, 5000);
                
            } catch (error) {
                console.error('Error showing warning:', error);
            }
        }

        // Enable anti-cheat protection
        function enableAntiCheat() {
            if (!examState.isActive) return;
            
            try {
                // Prevent context menu
                document.addEventListener('contextmenu', handleContextMenu);
                
                // Prevent keyboard shortcuts
                document.addEventListener('keydown', handleKeyDown);
                
                // Detect tab switch/window blur
                document.addEventListener('visibilitychange', handleVisibilityChange);
                window.addEventListener('blur', handleWindowBlur);
                
                // Detect fullscreen exit
                document.addEventListener('fullscreenchange', handleFullscreenChange);
                document.addEventListener('webkitfullscreenchange', handleFullscreenChange);
                document.addEventListener('mozfullscreenchange', handleFullscreenChange);
                document.addEventListener('MSFullscreenChange', handleFullscreenChange);
                
                // Prevent page refresh/navigation
                window.addEventListener('beforeunload', handleBeforeUnload);
                
                console.log('Anti-cheat protection enabled');
                
            } catch (error) {
                console.error('Error enabling anti-cheat:', error);
            }
        }

        // Event handlers
        function handleContextMenu(e) {
            e.preventDefault();
            triggerViolation('Mencoba membuka context menu');
        }

        function handleKeyDown(e) {
            if (!examState.isActive) return;
            
            // Prevent dangerous key combinations
            if (
                e.key === 'F12' ||
                (e.ctrlKey && e.shiftKey && ['I', 'C', 'J'].includes(e.key.toUpperCase())) ||
                (e.ctrlKey && ['U', 'S', 'R'].includes(e.key.toUpperCase())) ||
                (e.altKey && e.key === 'Tab') ||
                (e.ctrlKey && e.key === 'Tab') ||
                e.key === 'F5'
            ) {
                e.preventDefault();
                triggerViolation('Mencoba menggunakan shortcut keyboard terlarang');
            }
        }

        function handleVisibilityChange() {
            if (examState.isActive && document.hidden) {
                triggerViolation('Mencoba beralih tab/window');
            }
        }

        function handleWindowBlur() {
            if (examState.isActive) {
                triggerViolation('Kehilangan focus dari window');
            }
        }

        function handleFullscreenChange() {
            const isFullscreen = !!(document.fullscreenElement || 
                                   document.webkitFullscreenElement || 
                                   document.mozFullScreenElement || 
                                   document.msFullscreenElement);
            
            if (examState.isActive && examState.isFullscreen && !isFullscreen) {
                triggerViolation('Mencoba keluar dari fullscreen');
                // Try to re-enter fullscreen
                setTimeout(enterFullscreenLock, 100);
            }
            
            examState.isFullscreen = isFullscreen;
        }

        function handleBeforeUnload(e) {
            if (examState.isActive) {
                e.preventDefault();
                e.returnValue = 'Ujian sedang berlangsung!';
                triggerViolation('Mencoba keluar dari halaman');
                return 'Ujian sedang berlangsung!';
            }
        }

        // Trigger violation
        function triggerViolation(reason) {
            if (!examState.isActive) return;
            
            examState.violations++;
            console.log(`Violation ${examState.violations}: ${reason}`);
            
            if (examState.violations >= examState.maxViolations) {
                disqualifyStudent();
            } else {
                showViolationWarning();
            }
        }

        // Show violation warning
        function showViolationWarning() {
            try {
                const violationElement = document.getElementById('violation-count');
                if (violationElement) {
                    violationElement.textContent = examState.violations;
                }
                
                const warningOverlay = document.getElementById('cheat-warning');
                if (warningOverlay) {
                    warningOverlay.style.display = 'flex';
                }
                
                let countdown = 10;
                const countdownElement = document.getElementById('warning-countdown');
                if (countdownElement) {
                    countdownElement.textContent = countdown;
                }
                
                if (examState.warningTimer) {
                    clearInterval(examState.warningTimer);
                }
                
                examState.warningTimer = setInterval(() => {
                    countdown--;
                    if (countdownElement) {
                        countdownElement.textContent = countdown;
                    }
                    
                    if (countdown <= 0) {
                        clearInterval(examState.warningTimer);
                        if (warningOverlay) {
                            warningOverlay.style.display = 'none';
                        }
                    }
                }, 1000);
                
            } catch (error) {
                console.error('Error showing violation warning:', error);
            }
        }

        // Disqualify student
        function disqualifyStudent() {
            try {
                if (examState.timer) {
                    clearInterval(examState.timer);
                }
                examState.isActive = false;
                
                const modal = document.getElementById('disqualified-modal');
                if (modal) {
                    modal.style.display = 'flex';
                }
                
            } catch (error) {
                console.error('Error disqualifying student:', error);
            }
        }

        // Finish exam
        function finishExam() {
            try {
                if (examState.timer) {
                    clearInterval(examState.timer);
                }
                examState.isActive = false;
                
                const modal = document.getElementById('finish-modal');
                if (modal) {
                    modal.style.display = 'flex';
                }
                
            } catch (error) {
                console.error('Error finishing exam:', error);
            }
        }

        // Exit exam
        function exitExam() {
            try {
                // Clear all timers
                if (examState.timer) {
                    clearInterval(examState.timer);
                }
                if (examState.warningTimer) {
                    clearInterval(examState.warningTimer);
                }
                
                examState.isActive = false;
                
                // Exit fullscreen
                const exitFullscreen = document.exitFullscreen || 
                                      document.webkitExitFullscreen || 
                                      document.mozCancelFullScreen || 
                                      document.msExitFullscreen;
                
                if (exitFullscreen) {
                    exitFullscreen.call(document).catch(err => {
                        console.warn('Exit fullscreen failed:', err);
                    });
                }
                
                // Remove exam mode
                document.body.classList.remove('exam-mode');
                
                // Remove event listeners
                removeEventListeners();
                
                // Reset and show setup screen
                setTimeout(() => {
                    location.reload();
                }, 100);
                
            } catch (error) {
                console.error('Error exiting exam:', error);
                location.reload();
            }
        }

        // Remove event listeners
        function removeEventListeners() {
            try {
                document.removeEventListener('contextmenu', handleContextMenu);
                document.removeEventListener('keydown', handleKeyDown);
                document.removeEventListener('visibilitychange', handleVisibilityChange);
                window.removeEventListener('blur', handleWindowBlur);
                document.removeEventListener('fullscreenchange', handleFullscreenChange);
                document.removeEventListener('webkitfullscreenchange', handleFullscreenChange);
                document.removeEventListener('mozfullscreenchange', handleFullscreenChange);
                document.removeEventListener('MSFullscreenChange', handleFullscreenChange);
                window.removeEventListener('beforeunload', handleBeforeUnload);
            } catch (error) {
                console.error('Error removing event listeners:', error);
            }
        }

        // Developer tools detection (improved)
        let devtools = {
            open: false,
            orientation: null,
            checkInterval: null
        };

        function startDevToolsDetection() {
            if (!examState.isActive) return;
            
            devtools.checkInterval = setInterval(() => {
                if (!examState.isActive) {
                    clearInterval(devtools.checkInterval);
                    return;
                }
                
                const heightThreshold = window.outerHeight - window.innerHeight > 200;
                const widthThreshold = window.outerWidth - window.innerWidth > 300;
                
                if (heightThreshold || widthThreshold) {
                    if (!devtools.open) {
                        devtools.open = true;
                        triggerViolation('Mencoba membuka Developer Tools');
                    }
                } else {
                    devtools.open = false;
                }
            }, 2000);
        }

        // Initialize when page loads
        document.addEventListener('DOMContentLoaded', function() {
            console.log('Sistem ujian online dimuat');
            
            // Test Google Form URL format
            document.getElementById('google-form-url').addEventListener('input', function() {
                const url = this.value.trim();
                if (url && !url.includes('docs.google.com/forms')) {
                    this.style.borderColor = '#ef4444';
                } else {
                    this.style.borderColor = '#e2e8f0';
                }
            });
        });

        // Enhanced iframe error handling
        document.addEventListener('DOMContentLoaded', function() {
            const iframe = document.getElementById('google-form-frame');
            
            iframe.addEventListener('load', function() {
                console.log('Google Form loaded successfully');
            });
            
            iframe.addEventListener('error', function() {
                console.error('Error loading Google Form');
                showTimeWarning('❌ Error memuat Google Form');
            });
        });

        // Prevent common bypass attempts
        (function() {
            // Override console methods during exam
            let originalConsole = {};
            
            function disableConsole() {
                if (examState.isActive) {
                    Object.keys(console).forEach(key => {
                        if (typeof console[key] === 'function') {
                            originalConsole[key] = console[key];
                            console[key] = function() {
                                triggerViolation('Mencoba menggunakan console');
                            };
                        }
                    });
                }
            }
            
            function enableConsole() {
                Object.keys(originalConsole).forEach(key => {
                    console[key] = originalConsole[key];
                });
            }
            
            // Monitor for exam state changes
            let lastActiveState = false;
            setInterval(() => {
                if (examState.isActive !== lastActiveState) {
                    if (examState.isActive) {
                        disableConsole();
                        startDevToolsDetection();
                    } else {
                        enableConsole();
                        if (devtools.checkInterval) {
                            clearInterval(devtools.checkInterval);
                        }
                    }
                    lastActiveState = examState.isActive;
                }
            }, 1000);
        })();

        // Network monitoring (basic)
        function monitorNetwork() {
            if (!navigator.onLine) {
                showTimeWarning('⚠️ Koneksi internet terputus!');
            }
        }

        window.addEventListener('online', () => {
            if (examState.isActive) {
                showTimeWarning('✅ Koneksi internet pulih');
            }
        });

        window.addEventListener('offline', () => {
            if (examState.isActive) {
                showTimeWarning('❌ Koneksi internet terputus!');
            }
        });

        // Final security check
        setInterval(() => {
            if (examState.isActive) {
                // Check if page is still focused
                if (!document.hasFocus()) {
                    console.warn('Page lost focus');
                }
                
                // Check for suspicious window properties
                if (window.outerWidth === 0 || window.outerHeight === 0) {
                    triggerViolation('Window tersembunyi atau diminimize');
                }
            }
        }, 3000);
    </script>
</body>
</html>
