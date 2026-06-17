# teacher-sanchez-plataforma
enseñanza del ingles
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Quest 🌟 Trivia de Colores</title>
    <link href="https://googleapis.com" rel="stylesheet">
    <style>
        :root {
            --primary-color: #FF6B6B;
            --secondary-color: #4D96FF;
            --success-color: #6BCB77;
            --error-color: #FF6B6B;
            --warning-color: #FFD93D;
            --background-color: #F0F3F8;
            --text-color: #2C3E50;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Quicksand', sans-serif;
        }

        body {
            background-color: var(--background-color);
            color: var(--text-color);
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            overflow: hidden;
        }

        h1, h2, h3 {
            font-family: 'Fredoka One', cursive;
        }

        /* --- CONTENEDOR PRINCIPAL --- */
        .trivia-card {
            width: 100%;
            max-width: 600px;
            background: white;
            border-radius: 30px;
            padding: 40px 30px;
            box-shadow: 0 12px 0 #E2E8F0;
            text-align: center;
            border: 3px solid #E2E8F0;
            margin-top: 40px;
            position: relative;
        }

        .question-box {
            margin-bottom: 30px;
        }

        .question-visual {
            font-size: 6rem;
            margin: 20px 0;
            animation: pulse 2s infinite;
        }

        .question-text {
            font-size: 1.8rem;
            color: var(--text-color);
            margin-bottom: 10px;
        }

        /* --- MARCADOR DE PUNTOS --- */
        .score-badge {
            background: #FFF9E6;
            color: #FFAA00;
            border: 2px solid #FFD93D;
            padding: 5px 15px;
            border-radius: 15px;
            font-family: 'Fredoka One', cursive;
            display: inline-block;
            margin-top: 10px;
            font-size: 1.1rem;
        }

        /* --- BOTONES DE OPCIONES --- */
        .options-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .option-btn {
            background: #F8FAFC;
            border: 3px solid #E2E8F0;
            border-radius: 20px;
            padding: 20px;
            font-family: 'Fredoka One', cursive;
            font-size: 1.4rem;
            color: var(--text-color);
            cursor: pointer;
            box-shadow: 0 6px 0 #E2E8F0;
            transition: all 0.1s ease;
        }

        .option-btn:hover {
            transform: translateY(-2px);
            border-color: var(--secondary-color);
            box-shadow: 0 8px 0 #BCDCFF;
            background: #F0F7FF;
        }

        .option-btn:active {
            transform: translateY(4px);
            box-shadow: 0 2px 0 #E2E8F0;
        }

        /* Estados de respuesta */
        .option-btn.correct {
            background: var(--success-color) !important;
            color: white !important;
            border-color: #04A75B !important;
            box-shadow: 0 6px 0 #04A75B !important;
        }

        .option-btn.wrong {
            background: var(--error-color) !important;
            color: white !important;
            border-color: #D32F2F !important;
            box-shadow: 0 6px 0 #D32F2F !important;
            animation: shake 0.3s ease;
        }

        /* --- PANTALLA DE REINICIO / RECOMPENSA --- */
        .restart-btn {
            background: var(--secondary-color);
            color: white;
            border: none;
            border-radius: 20px;
            padding: 15px 30px;
            font-family: 'Fredoka One', cursive;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 6px 0 #2A75E6;
            margin-top: 20px;
            transition: all 0.1s;
        }
        .restart-btn:hover { transform: translateY(-2px); box-shadow: 0 8px 0 #2A75E6; }
        .restart-btn:active { transform: translateY(4px); box-shadow: 0 2px 0 #2A75E6; }

        .confetti {
            position: absolute;
            width: 12px;
            height: 12px;
            top: 50%;
            left: 50%;
            opacity: 0;
            border-radius: 50%;
            pointer-events: none;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-8px); }
            75% { transform: translateX(8px); }
        }
    </style>
</head>
<body>

    <div class="trivia-card" id="game-card">
        <!-- El contenido se genera dinámicamente desde JavaScript -->
    </div>

<script>
    // --- BANCO DE PREGUNTAS MÚLTIPLES ---
    const triviaData = [
        {
            emoji: "🍌",
            question: "What color is the banana?",
            options: [
                { text: "Red 🔴", isCorrect: false },
                { text: "Yellow 🟡", isCorrect: true },
                { text: "Blue 🔵", isCorrect: false },
                { text: "Green 🟢", isCorrect: false }
            ],
            audioCorrect: "Excellent! Yellow!"
        },
        {
            emoji: "🐸",
            question: "What color is the frog?",
            options: [
                { text: "Green 🟢", isCorrect: true },
                { text: "Pink 🌸", isCorrect: false },
                { text: "Orange 🍊", isCorrect: false },
                { text: "Purple 🍇", isCorrect: false }
            ],
            audioCorrect: "Great job! Green!"
        },
        {
            emoji: "🍎",
            question: "What color is the apple?",
            options: [
                { text: "Yellow 🟡", isCorrect: false },
                { text: "Black ⚫", isCorrect: false },
                { text: "Red 🔴", isCorrect: true },
                { text: "White ⚪", isCorrect: false }
            ],
            audioCorrect: "Awesome! Red!"
        },
        {
            emoji: "🍊",
            question: "What color is the orange?",
            options: [
                { text: "Blue 🔵", isCorrect: false },
                { text: "Orange 🍊", isCorrect: true },
                { text: "Green 🟢", isCorrect: false },
                { text: "Red 🔴", isCorrect: false }
            ],
            audioCorrect: "Perfect! Orange!"
        }
    ];

    // --- VARIABLES DE ESTADO ---
    let currentQuestionIndex = 0;
    let score = 0;
    let canAnswer = true;

    // --- MOTOR DE AUDIO (TTS) ---
    function speak(text) {
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'en-US';
            utterance.rate = 0.85; 
            window.speechSynthesis.speak(utterance);
        }
    }

    // --- RENDERIZAR INTERFAZ ---
    function loadQuestion() {
        const card = document.getElementById('game-card');
        canAnswer = true;

        // Validar si terminamos todas las preguntas
        if (currentQuestionIndex >= triviaData.length) {
            showVictoryScreen();
            return;
        }

        const currentData = triviaData[currentQuestionIndex];

        // Crear estructura HTML dinámica
        card.innerHTML = `
            <div class="question-box">
                <small style="color: var(--secondary-color); font-weight: 700; font-size: 1.1rem; display:block; margin-bottom:5px;">
                    PREGUNTA ${currentQuestionIndex + 1} DE ${triviaData.length}
                </small>
                <div class="score-badge">⭐ Puntos: ${score}</div>
                <div class="question-visual">${currentData.emoji}</div>
                <h2 class="question-text" id="q-text">${currentData.question}</h2>
            </div>
            <div class="options-grid">
                ${currentData.options.map((option, index) => `
                    <button class="option-btn" onclick="processAnswer(this, ${option.isCorrect})">
                        ${option.text}
                    </button>
                `).join('')}
            </div>
        `;

        // Hablar la pregunta automáticamente
        setTimeout(() => speak(currentData.question), 400);
    }

    // --- PROCESAR LA RESPUESTA ---
    function processAnswer(button, isCorrect) {
        if (!canAnswer) return;
        canAnswer = false;

        const currentData = triviaData[currentQuestionIndex];

        if (isCorrect) {
            score++;
            button.classList.add('correct');
            speak(currentData.audioCorrect);
            createConfettiExplosion();

            // Avanzar a la siguiente pregunta después de 2.5 segundos
            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 2500);

        } else {
            button.classList.add('wrong');
            speak("Try again!");

            // Permitir reintentar la misma pregunta
            setTimeout(() => {
                button.classList.remove('wrong');
                canAnswer = true;
            }, 1000);
        }
    }

    // --- PANTALLA FINAL ---
    function showVictoryScreen() {
        const card = document.getElementById('game-card');
        speak("Congratulations! You completed the quest!");
        
        card.innerHTML = `
            <div class="question-visual">🏆</div>
            <h2 class="question-text" style="font-size: 2.5rem; color: var(--success-color);">¡Excelente Trabajo!</h2>
