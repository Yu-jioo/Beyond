<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0, viewport-fit=cover">
    <title>LiveTrack - Achievement Tracker</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --deep-abyss: #0a0a1a;
            --abyssal-blue: #1a1a4a;
            --neon-cyan: #00f3ff;
            --accent-coral: #ff4d6d;
            --glass-bg-dark: rgba(10, 10, 30, 0.97);
            --text-primary: #e0e0ff;
            --space-gradient: linear-gradient(135deg, #0a0a2a 0%, #1a1a4a 100%);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', sans-serif;
        }

        body {
            background: var(--space-gradient);
            min-height: 100vh;
            color: var(--text-primary);
            line-height: 1.6;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem 1rem 100px;
        }

        /* Enhanced Header */
        .header {
            text-align: center;
            margin-bottom: 3rem;
            padding: 3rem 2rem;
            background: var(--glass-bg-dark);
            backdrop-filter: blur(24px);
            border-radius: 24px;
            border: 1px solid rgba(255, 255, 255, 0.15);
            box-shadow: 0 16px 40px rgba(0, 5, 15, 0.4);
            position: relative;
            overflow: hidden;
        }

        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 1rem;
        }

        .logo-icon {
            font-size: 2.5rem;
            background: linear-gradient(45deg, var(--neon-cyan), #4a5de6);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: pulse 2s infinite;
        }

        .logo-text {
            font-size: 2.5rem;
            font-weight: 700;
            background: linear-gradient(45deg, #e0e0ff, var(--neon-cyan));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: -1px;
        }

        /* Achievement Cards Grid */
        .achievements-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
            gap: 2rem;
            margin-top: 3rem;
        }

        .achievement-card {
            background: var(--glass-bg-dark);
            backdrop-filter: blur(24px);
            border-radius: 24px;
            padding: 2rem;
            border: 1px solid rgba(255, 255, 255, 0.15);
            position: relative;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            min-height: 220px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            cursor: pointer;
        }

        .achievement-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 16px 40px rgba(0, 5, 15, 0.4);
        }

        /* Card Action Buttons */
        .card-actions {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            display: flex;
            gap: 1rem;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .achievement-card:hover .card-actions {
            opacity: 1;
        }

        .action-btn {
            width: 42px;
            height: 42px;
            border: none;
            border-radius: 12px;
            background: rgba(0, 0, 30, 0.5);
            color: var(--text-primary);
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(6px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .action-btn:hover {
            background: var(--accent-coral);
            color: white;
            transform: scale(1.08);
        }

        /* Modal System */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 20, 0.98);
            backdrop-filter: blur(16px);
            z-index: 2000;
            justify-content: center;
            align-items: center;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .modal.active {
            display: flex;
            opacity: 1;
        }

        .modal-content {
            background: var(--glass-bg-dark);
            backdrop-filter: blur(32px);
            padding: 3rem;
            border-radius: 24px;
            width: 90%;
            max-width: 560px;
            transform: translateY(30px);
            transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 24px 48px rgba(0, 5, 15, 0.5);
        }

        /* Delete Confirmation Modal */
        .confirmation-modal .modal-content {
            text-align: center;
            padding: 4rem 3rem;
        }

        .confirmation-text {
            font-size: 1.2rem;
            margin: 2rem 0;
            line-height: 1.6;
            color: rgba(224, 224, 255, 0.9);
        }

        .modal-buttons {
            display: flex;
            gap: 1.5rem;
            justify-content: center;
            margin-top: 2rem;
        }

        /* Enhanced Achievement Form */
        .achievement-form {
            display: flex;
            flex-direction: column;
            gap: 2rem;
        }

        .form-group {
            position: relative;
        }

        input, textarea, input[type="date"] {
            width: 100%;
            padding: 1.2rem;
            background: rgba(0, 0, 30, 0.4);
            border: 2px solid rgba(74, 93, 230, 0.2);
            border-radius: 16px;
            color: var(--text-primary);
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        input:focus, textarea:focus {
            border-color: var(--neon-cyan);
            box-shadow: 0 0 20px rgba(0, 243, 255, 0.3);
            background: rgba(0, 0, 30, 0.5);
        }

        input:focus, textarea:focus {
            outline: none;
        }
        
        
        /* Enhanced Buttons */
        .btn {
            padding: 1.2rem 2.5rem;
            border: none;
            border-radius: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 0.75rem;
            font-size: 1rem;
        }

        .btn-primary {
            background: linear-gradient(135deg, #4a5de6 0%, #00a3ff 100%);
            color: white;
            box-shadow: 0 8px 24px rgba(74, 93, 230, 0.4);
        }

        .btn-danger {
            background: linear-gradient(135deg, #ff4d6d 0%, #ff758c 100%);
            color: white;
            box-shadow: 0 8px 24px rgba(255, 77, 109, 0.3);
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-primary);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        /* Floating Action Button */
        .fab {
            position: fixed;
            bottom: 2.5rem;
            right: 2.5rem;
            background: linear-gradient(135deg, #4a5de6 0%, #00a3ff 100%);
            width: 68px;
            height: 68px;
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 12px 32px rgba(74, 93, 230, 0.4);
            transition: all 0.3s ease;
            z-index: 1000;
        }

        .fab:hover {
            transform: scale(1.15) rotate(180deg);
            box-shadow: 0 16px 40px rgba(74, 93, 230, 0.5);
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .modal-content {
                padding: 2rem;
            }

            .confirmation-modal .modal-content {
                padding: 3rem 2rem;
            }

            .btn {
                padding: 1rem 2rem;
            }

            .achievement-card {
                min-height: auto;
                padding: 1.5rem;
            }

            .card-actions {
                opacity: 1;
                top: 1rem;
                right: 1rem;
            }

            .fab {
                bottom: 1.5rem;
                right: 1.5rem;
                width: 56px;
                height: 56px;
            }
        }

        @media (max-width: 480px) {
            .modal-content {
                padding: 1.5rem;
                border-radius: 16px;
            }

            .confirmation-text {
                font-size: 1rem;
            }

            .modal-buttons {
                flex-direction: column;
                gap: 1rem;
            }

            .btn {
                width: 100%;
                justify-content: center;
            }

            input, textarea {
                padding: 1rem;
                font-size: 0.95rem;
            }

            .header {
                padding: 2rem 1rem;
            }
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.08); }
            100% { transform: scale(1); }
        }

        /* Error States */
        .error {
            animation: shake 0.4s ease;
            border-color: var(--accent-coral) !important;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(8px); }
            75% { transform: translateX(-8px); }
        }

        /* Character Counter */
        .char-counter {
            position: absolute;
            right: 1rem;
            bottom: 1rem;
            font-size: 0.8rem;
            color: rgba(224, 224, 255, 0.6);
        }
        
                .achievement-date {
            color: rgba(224, 224, 255, 0.6);
            font-size: 0.8rem;
            margin-top: 1rem;
            position: absolute;
            bottom: 1rem;
            right: 1rem;
        }
        
    </style>
</head>
<body>
    <div class="container">
        <header class="header">
            <div class="logo">
                <i class="fas fa-rocket logo-icon"></i>
                <h1 class="logo-text">LiveTrack</h1>
            </div>
            <p>Illuminate your journey through the cosmos of accomplishments</p>
        </header>

        <div class="achievements-container" id="achievementsContainer">
            <div class="empty-state">
                <i class="fas fa-stars fa-3x" style="margin-bottom: 1.5rem; opacity: 0.6;"></i>
                <p>Your achievement constellation awaits<br>Click the nova below to begin</p>
            </div>
        </div>

        <div class="fab" id="addFab">
            <i class="fas fa-plus"></i>
        </div>
    </div>

    <!-- Add Achievement Modal -->
    <div class="modal" id="addModal">
        <div class="modal-content">
            <form class="achievement-form" id="achievementForm">
                <h2><i class="fas fa-star-shooting"></i> New Stellar Achievement</h2>
                <div class="form-group">
                    <label for="title">Achievement Title</label>
                    <input type="text" id="title" required maxlength="60" placeholder="Enter achievement title">
                    <span class="char-counter" id="titleCounter">0/60</span>
                </div>
                <div class="form-group">
                    <label for="description">Description</label>
                    <textarea id="description" rows="4" required maxlength="200" 
                        placeholder="Describe your accomplishment"></textarea>
                    <span class="char-counter" id="descCounter">0/200</span>
                </div>
                <div class="form-group">
                    <label for="date">Date Achieved</label>
                    <input type="date" id="date" required>
                </div>
                <div class="modal-buttons">
                    <button type="submit" class="btn btn-primary">
                        <i class="fas fa-save"></i> Save
                    </button>
                    <button type="button" class="btn btn-secondary" id="cancelBtn">
                        Cancel
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Delete Confirmation Modal -->
    <div class="modal confirmation-modal" id="deleteModal">
        <div class="modal-content">
            <i class="fas fa-exclamation-triangle fa-3x" style="color: var(--accent-coral);"></i>
            <p class="confirmation-text">This achievement will be permanently removed from your constellation.<br>Are you sure you want to continue?</p>
            <div class="modal-buttons">
                <button class="btn btn-danger" id="confirmDelete">Delete Forever</button>
                <button class="btn btn-secondary" id="cancelDelete">Cancel</button>
            </div>
        </div>
    </div>

    <script>
        const addFab = document.getElementById('addFab');
        const addModal = document.getElementById('addModal');
        const deleteModal = document.getElementById('deleteModal');
        const achievementForm = document.getElementById('achievementForm');
        const achievementsContainer = document.getElementById('achievementsContainer');
        const confirmDeleteBtn = document.getElementById('confirmDelete');
        const titleInput = document.getElementById('title');
        const descInput = document.getElementById('description');
        const titleCounter = document.getElementById('titleCounter');
        const descCounter = document.getElementById('descCounter');
        
        let achievements = JSON.parse(localStorage.getItem('achievements')) || [];
        let currentDeleteIndex = -1;

        // Initialize character counters
        titleInput.addEventListener('input', updateTitleCounter);
        descInput.addEventListener('input', updateDescCounter);

        function updateTitleCounter() {
            titleCounter.textContent = `${titleInput.value.length}/60`;
        }

        function updateDescCounter() {
            descCounter.textContent = `${descInput.value.length}/200`;
        }

        // Modal Management
        function toggleModal(modal, show) {
            modal.classList.toggle('active', show);
            document.body.style.overflow = show ? 'hidden' : 'auto';
            if (show) {
                modal.querySelector('.modal-content').style.transform = 'translateY(0)';
            }
        }

        // Add Achievement Flow
        addFab.addEventListener('click', () => {
            toggleModal(addModal, true);
            document.getElementById('date').valueAsDate = new Date();
            titleInput.focus();
            updateTitleCounter();
            updateDescCounter();
        });

        // Delete Achievement Flow
        function showDeleteConfirmation(id) {
            currentDeleteIndex = achievements.findIndex(a => a.id === id);
            toggleModal(deleteModal, true);
        }

        confirmDeleteBtn.addEventListener('click', () => {
            if (currentDeleteIndex !== -1) {
                achievements.splice(currentDeleteIndex, 1);
                saveAchievements();
                displayAchievements();
                currentDeleteIndex = -1;
            }
            toggleModal(deleteModal, false);
        });

        document.getElementById('cancelDelete').addEventListener('click', () => {
            toggleModal(deleteModal, false);
            currentDeleteIndex = -1;
        });

        // Form Handling
        achievementForm.addEventListener('submit', (e) => {
            e.preventDefault();
            
            const achievement = {
                title: titleInput.value.trim(),
                description: descInput.value.trim(),
                date: document.getElementById('date').value,
                id: Date.now()
            };

            // Validation
            if (!achievement.title || !achievement.description) {
                [titleInput, descInput].forEach(input => {
                    if (!input.value.trim()) input.classList.add('error');
                });
                setTimeout(() => {
                    [titleInput, descInput].forEach(input => input.classList.remove('error'));
                }, 1000);
                return;
            }

            achievements.push(achievement);
            saveAchievements();
            displayAchievements();
            achievementForm.reset();
            toggleModal(addModal, false);
        });

        document.getElementById('cancelBtn').addEventListener('click', () => {
            toggleModal(addModal, false);
        });

        // Render Achievements
        function displayAchievements() {
            achievementsContainer.innerHTML = achievements.length ? '' : `
                <div class="empty-state">
                    <i class="fas fa-stars fa-3x" style="margin-bottom: 1.5rem; opacity: 0.6;"></i>
                    <p>Your achievement constellation awaits<br>Click the nova below to begin</p>
                </div>`;

            // فرز الإنجازات من الأحدث إلى الأقدم
            achievements
                .slice() // إنشاء نسخة من المصفوفة للحفاظ على الأصلية
                .sort((a, b) => new Date(b.date) - new Date(a.date)) // الترتيب التنازلي حسب التاريخ
                .forEach((achievement) => {
                    const card = document.createElement('div');
                    card.className = 'achievement-card';
                    card.innerHTML = `
                        <div class="card-actions">
                            <button class="action-btn" onclick="event.stopPropagation(); showDeleteConfirmation(${achievement.id})">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                        <div>
                            <h3 class="achievement-title">${achievement.title}</h3>
                            <p>${achievement.description}</p>
                        </div>
                        <p class="achievement-date">
                            <i class="fas fa-calendar-star"></i>
                            ${new Date(achievement.date).toLocaleDateString('en-US', { 
                                year: 'numeric', 
                                month: 'long', 
                                day: 'numeric' 
                            })}
                        </p>`;
                    achievementsContainer.appendChild(card);
                });
        }

        function saveAchievements() {
            localStorage.setItem('achievements', JSON.stringify(achievements));
        }

        // Initial Setup
        displayAchievements();

        // Close Modals
        document.querySelectorAll('.modal').forEach(modal => {
            modal.addEventListener('click', (e) => {
                if (e.target === modal) toggleModal(modal, false);
            });
        });

        // Keyboard Controls
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                document.querySelectorAll('.modal').forEach(modal => toggleModal(modal, false));
            }
        });
    </script>
</body>
</html>
