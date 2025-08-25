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
            overflow: hidden;
        }

        .exam-container {
            width: 100%;
            height: 100vh;
            display: flex;
            flex-direction: column;
            position: relative;
        }

        .header {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        .exam-title {
            color: white;
            font-size: 1.5rem;
            font-weight: 600;
        }

        .timer-container {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .timer {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            padding: 10px 20px;
            border-radius: 25px;
            font-size: 1.1rem;
            font-weight: bold;
            border: 2px solid rgba(255, 255, 255, 0.3);
            min-width: 120px;
            text-align: center;
        }

        .timer.warning {
            background: rgba(255, 193, 7, 0.3);
            border-color: #ffc107;
            animation: pulse 1s infinite;
        }

        .timer.danger {
            background: rgba(220, 53, 69, 0.3);
            border-color: #dc3545;
            animation: pulse 0.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .status-indicator {
            display: flex;
            align-items: center;
            gap: 10px;
            color: white;
        }

        .status-dot {
            width: 12px;
            height: 12px;
            background: #28a745;
            border-radius: 50%;
            animation: blink 2s infinite;
        }

        @keyframes blink {
            0%, 50% { opacity: 1; }
            51%, 100% { opacity: 0.3; }
        }

        .form-container {
            flex: 1;
            padding: 0;
            overflow: hidden;
        }

        .google-form {
            width: 100%;
            height: 100%;
            border: none;
            background: white;
        }

        .warning-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(220, 53, 69, 0.9);
            backdrop-filter: blur(5px);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            color: white;
            text-align: center;
        }

        .warning-content {
            background: rgba(255, 255, 255, 0.1);
            padding: 40px;
            border-radius: 15px;
            border: 2px solid rgba(255, 255, 255, 0.3);
            max-width: 500px;
        }

        .warning-content h2 {
            font-size: 2rem;
            margin-bottom: 20px;
        }

        .warning-content p {
            font-size: 1.2rem;
            line-height: 1.6;
        }

        .setup-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10000;
        }

        .setup-content {
            background: white;
            padding: 40px;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            max-width: 500px;
            width: 90%;
            text-align: center;
        }

        .setup-content h2 {
            color: #333;
            margin-bottom: 30px;
            font-size: 1.8rem;
        }

        .form-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 500;
        }

        .form-group input, .form-group textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-group input:focus, .form-group textarea:focus {
            outline: none;
            border-color: #667eea;
        }

        .start-button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            font-size: 1.1rem;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .start-button:hover {
            transform: translateY(-2px);
        }

        .hidden {
            display: none !important;
        }
    </style>
</head>
<body>
    <!-- Setup Modal -->
    <div id="setupModal" class="setup-modal">
        <div class="setup-content">
            <h2>🎓 Setup Ujian Online</h2>
            <form id="examSetup">
                <div class="form-group">
                    <label for="examTitle">Judul Ujian:</label>
                    <input type="text" id="examTitle" value="Ujian Matematika - Kelas X" required>
                </div>
                
                <div class="form-group">
                    <label for="examDuration">Durasi Ujian (menit):</label>
                    <input type="number" id="examDuration" value="60" min="1" max="240" required>
                </div>
                
                <div class="form-group">
                    <label for="googleFormUrl">URL Google Form:</label>
                    <textarea id="googleFormUrl" rows="3" placeholder="Paste URL Google Form di sini..." required>https://docs.google.com/forms/d/e/1FAIpQLSe_example_form_id/viewform?embedded=true</textarea>
                </div>
                
                <button type="submit" class="start-button">🚀 Mulai Ujian</button>
            </form>
        </div>
    </div>

    <!-- Warning Overlay -->
    <div id="warningOverlay" class="warning-overlay">
        <div class="warning-content">
            <h2>⚠️ PERINGATAN!</h2>
            <p>Anda mencoba keluar dari halaman ujian!<br>
            Tindakan ini akan dicatat dan dilaporkan ke pengawas.<br><br>
            <strong>Kembali ke ujian sekarang!</strong></p>
        </div>
    </div>

    <!-- Main Exam Container -->
    <div class="exam-container">
        <div class="header">
            <div class="exam-title" id="examTitleDisplay">Sistem Ujian Online</div>
            <div class="timer-container">
                <div class="status-indicator">
                    <div class="status-dot"></div>
                    <span>Ujian Aktif</span>
                </div>
                <div id="timer" class="timer">00:00:00</div>
            </div>
        </div>
        
        <div class="form-container">
            <iframe id="googleForm" class="google-form" src="" frameborder="0" marginheight="0" marginwidth="0">
                Loading...
            </iframe>
        </div>
    </div>

    <script>
        class ExamSystem {
            constructor() {
                this.examDuration = 0;
                this.startTime = 0;
                this.timerInterval = null;
                this.warningCount = 0;
                this.fullscreenExitCount = 0;
                this.isExamActive = false;
                this.visibilityWarningShown = false;
                
                this.initializeEventListeners();
            }

            initializeEventListeners() {
                // Setup form submission
                document.getElementById('examSetup').addEventListener('submit', (e) => {
                    e.preventDefault();
                    this.startExam();
                });

                // Prevent context menu (right-click)
                document.addEventListener('contextmenu', (e) => {
                    if (this.isExamActive) {
                        e.preventDefault();
                        this.showWarning();
                    }
                });

                // Prevent F12, Ctrl+Shift+I, etc.
                document.addEventListener('keydown', (e) => {
                    if (this.isExamActive) {
                        // F12, F11, Ctrl+Shift+I, Ctrl+U, Ctrl+S, etc.
                        if (e.key === 'F12' || 
                            e.key === 'F11' ||
                            (e.ctrlKey && e.shiftKey && e.key === 'I') ||
                            (e.ctrlKey && e.key === 'u') ||
                            (e.ctrlKey && e.key === 's') ||
                            (e.ctrlKey && e.shiftKey && e.key === 'C') ||
                            (e.ctrlKey && e.key === 'r') ||
                            e.key === 'F5') {
                            e.preventDefault();
                            this.showWarning();
                        }
                    }
                });

                // Detect page visibility change (tab switching)
                document.addEventListener('visibilitychange', () => {
                    if (this.isExamActive && document.hidden) {
                        this.handleTabSwitch();
                    }
                });

                // Detect window focus loss
                window.addEventListener('blur', () => {
                    if (this.isExamActive) {
                        this.handleTabSwitch();
                    }
                });

                // Detect fullscreen exit
                document.addEventListener('fullscreenchange', () => {
                    if (this.isExamActive && !document.fullscreenElement) {
                        this.handleFullscreenExit();
                    }
                });

                // Prevent page unload/refresh
                window.addEventListener('beforeunload', (e) => {
                    if (this.isExamActive) {
                        e.preventDefault();
                        e.returnValue = 'Anda yakin ingin keluar dari ujian? Progres akan hilang!';
                        return 'Anda yakin ingin keluar dari ujian? Progres akan hilang!';
                    }
                });

                // Handle page unload (when really leaving)
                window.addEventListener('unload', () => {
                    if (this.isExamActive) {
                        this.logViolation('Siswa keluar dari halaman ujian');
                    }
                });
            }

            startExam() {
                // Get form values
                const title = document.getElementById('examTitle').value;
                const duration = parseInt(document.getElementById('examDuration').value);
                const formUrl = document.getElementById('googleFormUrl').value.trim();

                // Validate Google Form URL
                if (!this.isValidGoogleFormUrl(formUrl)) {
                    alert('URL Google Form tidak valid! Pastikan URL sudah benar.');
                    return;
                }

                // Set exam parameters
                this.examDuration = duration * 60; // Convert to seconds
                this.startTime = Date.now();
                this.isExamActive = true;

                // Update UI
                document.getElementById('examTitleDisplay').textContent = title;
                document.getElementById('googleForm').src = formUrl;
                document.getElementById('setupModal').classList.add('hidden');

                // Start timer
                this.startTimer();

                // Enable full screen and lock features
                this.enableExamMode();

                console.log('🎓 Ujian dimulai:', { title, duration, formUrl });
            }

            isValidGoogleFormUrl(url) {
                return url.includes('docs.google.com/forms') && url.includes('/viewform');
            }

            enableExamMode() {
                // Request fullscreen
                if (document.documentElement.requestFullscreen) {
                    document.documentElement.requestFullscreen().catch(() => {
                        console.log('Fullscreen tidak didukung browser');
                    });
                }

                // Disable text selection
                document.body.style.userSelect = 'none';
                document.body.style.webkitUserSelect = 'none';
                document.body.style.mozUserSelect = 'none';
                
                // Hide cursor when idle (optional)
                let idleTimer;
                document.addEventListener('mousemove', () => {
                    document.body.style.cursor = 'auto';
                    clearTimeout(idleTimer);
                    idleTimer = setTimeout(() => {
                        document.body.style.cursor = 'none';
                    }, 3000);
                });
            }

            startTimer() {
                this.timerInterval = setInterval(() => {
                    const elapsed = Math.floor((Date.now() - this.startTime) / 1000);
                    const remaining = Math.max(0, this.examDuration - elapsed);
                    
                    this.updateTimerDisplay(remaining);
                    
                    if (remaining <= 0) {
                        this.endExam();
                    }
                }, 1000);
            }

            updateTimerDisplay(seconds) {
                const hours = Math.floor(seconds / 3600);
                const minutes = Math.floor((seconds % 3600) / 60);
                const secs = seconds % 60;
                
                const display = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
                
                const timerElement = document.getElementById('timer');
                timerElement.textContent = display;
                
                // Change color based on remaining time
                if (seconds <= 300) { // 5 minutes
                    timerElement.className = 'timer danger';
                } else if (seconds <= 600) { // 10 minutes  
                    timerElement.className = 'timer warning';
                } else {
                    timerElement.className = 'timer';
                }
            }

            handleFullscreenExit() {
                this.fullscreenExitCount++;
                this.logViolation(`Siswa keluar dari fullscreen (Pelanggaran ke-${this.fullscreenExitCount})`);
                
                if (this.fullscreenExitCount <= 3) {
                    // Show warning and force back to fullscreen
                    this.showFullscreenWarning();
                    
                    // Force back to fullscreen after 2 seconds
                    setTimeout(() => {
                        if (document.documentElement.requestFullscreen) {
                            document.documentElement.requestFullscreen().catch(() => {
                                console.log('Tidak bisa kembali ke fullscreen');
                            });
                        }
                    }, 2000);
                } else {
                    // More than 3 attempts - logout and restart
                    alert('🚨 TERLALU BANYAK PELANGGARAN!\n\nAnda telah keluar dari fullscreen lebih dari 3 kali.\nUjian akan direset dan harus dimulai dari awal.');
                    this.resetExam();
                }
            }

            showFullscreenWarning() {
                const warningMsg = `⚠️ PERINGATAN ${this.fullscreenExitCount}/3\n\nAnda telah keluar dari mode fullscreen!\nSistem akan mengembalikan ke fullscreen dalam 2 detik...\n\nPeringatan tersisa: ${3 - this.fullscreenExitCount}`;
                
                // Update warning overlay content
                const warningOverlay = document.getElementById('warningOverlay');
                const warningContent = warningOverlay.querySelector('.warning-content');
                warningContent.innerHTML = `
                    <h2>⚠️ PERINGATAN ${this.fullscreenExitCount}/3</h2>
                    <p>Anda telah keluar dari mode fullscreen!<br>
                    Sistem akan mengembalikan ke fullscreen dalam 2 detik...<br><br>
                    <strong>Peringatan tersisa: ${3 - this.fullscreenExitCount}</strong></p>
                `;
                
                warningOverlay.style.display = 'flex';
                
                // Hide after 3 seconds
                setTimeout(() => {
                    warningOverlay.style.display = 'none';
                    // Restore original warning content
                    warningContent.innerHTML = `
                        <h2>⚠️ PERINGATAN!</h2>
                        <p>Anda mencoba keluar dari halaman ujian!<br>
                        Tindakan ini akan dicatat dan dilaporkan ke pengawas.<br><br>
                        <strong>Kembali ke ujian sekarang!</strong></p>
                    `;
                }, 3000);
            }

            resetExam() {
                // Stop exam
                this.isExamActive = false;
                this.warningCount = 0;
                this.fullscreenExitCount = 0;
                
                if (this.timerInterval) {
                    clearInterval(this.timerInterval);
                }

                // Exit fullscreen
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }

                // Re-enable text selection
                document.body.style.userSelect = 'auto';
                document.body.style.cursor = 'auto';

                // Reset UI
                document.getElementById('timer').textContent = '00:00:00';
                document.getElementById('timer').className = 'timer';
                document.getElementById('googleForm').src = '';
                
                // Show setup modal again
                document.getElementById('setupModal').classList.remove('hidden');
                
                // Reset form values
                document.getElementById('examTitle').value = 'Ujian Matematika - Kelas X';
                document.getElementById('examDuration').value = '60';
                document.getElementById('googleFormUrl').value = 'https://docs.google.com/forms/d/e/1FAIpQLSe_example_form_id/viewform?embedded=true';

                console.log('🔄 Ujian direset - harus dimulai dari awal');
            }

            handleTabSwitch() {
                this.warningCount++;
                this.logViolation(`Siswa berganti tab/window (Pelanggaran ke-${this.warningCount})`);
                
                if (!this.visibilityWarningShown) {
                    this.showWarning();
                    this.visibilityWarningShown = true;
                    
                    // Reset flag after 3 seconds
                    setTimeout(() => {
                        this.visibilityWarningShown = false;
                    }, 3000);
                }

                // Auto-end exam after 3 violations
                if (this.warningCount >= 3) {
                    alert('Terlalu banyak pelanggaran! Ujian akan diakhiri.');
                    this.endExam();
                }
            }

            showWarning() {
                const overlay = document.getElementById('warningOverlay');
                overlay.style.display = 'flex';
                
                // Auto hide after 3 seconds
                setTimeout(() => {
                    overlay.style.display = 'none';
                }, 3000);
            }

            logViolation(message) {
                const timestamp = new Date().toLocaleString('id-ID');
                console.warn(`🚨 [${timestamp}] ${message}`);
                
                // In a real implementation, you would send this to a server
                // Example: sendViolationLog({ message, timestamp, studentId, examId });
            }

            endExam() {
                this.isExamActive = false;
                
                if (this.timerInterval) {
                    clearInterval(this.timerInterval);
                }

                // Exit fullscreen
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                }

                // Re-enable text selection
                document.body.style.userSelect = 'auto';
                document.body.style.cursor = 'auto';

                // Show completion message
                alert('⏰ Waktu ujian habis! Ujian telah berakhir.\n\nPastikan Anda telah submit jawaban di Google Form.');
                
                // Optional: Redirect to completion page
                // window.location.href = 'completion.html';
                
                console.log('✅ Ujian selesai. Total pelanggaran tab/window:', this.warningCount, '| Pelanggaran fullscreen:', this.fullscreenExitCount);
            }
        }

        // Initialize exam system when page loads
        document.addEventListener('DOMContentLoaded', () => {
            new ExamSystem();
        });

        // Disable drag and drop
        document.addEventListener('dragstart', (e) => e.preventDefault());
        document.addEventListener('drop', (e) => e.preventDefault());

        // Disable print
        window.addEventListener('beforeprint', (e) => {
            e.preventDefault();
            alert('Print tidak diizinkan selama ujian!');
        });
    </script>
</body>
</html>
