<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LM Win - Jogo da Memória Deluxe Avançado</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        /* styles.css */
        :root {
            --lm-win-header-text: #293A80;
            --lm-win-body-bg-start: #E8EEF9;
            --lm-win-body-bg-end: #D7E3F8;
            --lm-win-card-bg: #FFFFFF;
            --lm-win-text-light: #FFFFFF;
            --lm-win-text-dark: #2C3A47;
            --lm-win-panel-bg: rgba(255, 255, 255, 0.8); /* Aumentei um pouco a opacidade para legibilidade */
            --lm-win-panel-border: rgba(46, 58, 135, 0.2);

            --card-border-radius: 16px; /* Ajustado para consistência */
            --panel-border-radius: 14px;
            --button-border-radius: 10px; /* Ajustado */
            --card-flip-duration: 0.6s;
            --font-family: 'Inter', sans-serif;
            
            --success-color: #27AE60;
            --error-color: #E74C3C; 
            
            --shadow-color-light: rgba(46, 58, 135, 0.1);
            --shadow-color-medium: rgba(46, 58, 135, 0.18); /* Ajustado */
            --shadow-color-dark: rgba(46, 58, 135, 0.25); /* Ajustado */

            /* Tamanhos de carta revisados para melhor escalonamento */
            --card-width-lg: 140px; 
            --card-height-lg: 210px;
            --card-width-md: 115px; /* Ajustado */
            --card-height-md: 173px; /* Ajustado */
            --card-width-sm: 95px;  /* Ajustado */
            --card-height-sm: 143px; /* Ajustado */
            --card-width-xs: 80px;  /* Novo tamanho para telas muito pequenas */
            --card-height-xs: 120px; /* Novo tamanho para telas muito pequenas */
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: var(--font-family);
            background-image: url('assets/images/background/background-page.png');
            background-size: cover;
            background-position: center center;
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-color: var(--lm-win-body-bg-start);
            color: var(--lm-win-text-dark);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center; 
            min-height: 100vh;
            padding: 15px; /* Padding base reduzido */
            text-align: center;
            position: relative;
            overflow-x: hidden;
        }

        body::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(232, 238, 249, 0.8), rgba(215, 227, 248, 0.8)); /* Opacidade ajustada */
            z-index: -1;
        }

        .main-header { 
            width: 100%;
            max-width: 800px; 
            margin-bottom: 25px; /* Ajustado */
            background-color: var(--lm-win-panel-bg);
            padding: 12px 25px; /* Ajustado */
            border-radius: var(--panel-border-radius);
            box-shadow: 0 4px 12px var(--shadow-color-medium); /* Ajustado */
        }

        .main-header h1 {
            font-size: 2.4rem; /* Ajustado */
            color: var(--lm-win-header-text);
            font-weight: 800;
            text-shadow: 1px 1px 2px rgba(255,255,255,0.5);
        }

        .screen {
            display: none; 
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
            max-width: 650px; /* Ajustado */
            padding: 25px 30px; /* Ajustado */
            background-color: var(--lm-win-panel-bg);
            border-radius: var(--panel-border-radius);
            box-shadow: 0 7px 20px var(--shadow-color-dark); /* Ajustado */
            animation: fadeInScreen 0.5s ease-out forwards;
        }
        .screen.active {
            display: flex;
        }

        @keyframes fadeInScreen {
            from { opacity: 0; transform: translateY(15px); } /* Animação mais sutil */
            to { opacity: 1; transform: translateY(0); }
        }

        .screen h2 {
            font-size: 2rem; /* Ajustado */
            color: var(--lm-win-header-text);
            margin-bottom: 18px; /* Ajustado */
        }
        .screen p {
            font-size: 1rem; /* Ajustado */
            line-height: 1.5; /* Ajustado */
            margin-bottom: 20px; /* Ajustado */
            color: var(--lm-win-text-dark);
        }
        .screen .stats-item {
            font-size: 1.1rem; /* Ajustado */
            margin-bottom: 8px; /* Ajustado */
        }
        .screen .stats-item strong {
            color: var(--lm-win-header-text);
        }

        .screen-button {
            background-image: linear-gradient(to right, #5C85D6 0%, #293A80 50%, #5C85D6 100%);
            background-size: 220% auto;
            color: var(--lm-win-text-light);
            border: none;
            padding: 12px 28px; /* Ajustado */
            font-size: 1rem; /* Ajustado */
            font-weight: 700;
            border-radius: var(--button-border-radius);
            cursor: pointer;
            transition: all 0.35s ease; /* Ajustado */
            box-shadow: 0 4px 8px var(--shadow-color-medium); /* Ajustado */
            text-transform: uppercase;
            letter-spacing: 0.7px;
            margin-top: 12px; /* Ajustado */
        }
        .screen-button:hover {
            background-position: right center;
            box-shadow: 0 6px 12px var(--shadow-color-dark); /* Ajustado */
            transform: translateY(-2px);
        }

        #game-main-content { 
            display: none; 
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 850px; /* Ajustado */
        }
        #game-main-content.active {
            display: flex;
            animation: fadeInScreen 0.5s ease-out forwards;
        }

        .game-info-panel {
            display: flex;
            justify-content: space-around;
            align-items: center;
            flex-wrap: wrap; /* Permite quebrar linha em telas menores */
            gap: 10px 15px; /* Espaçamento entre itens */
            width: 100%;
            max-width: 650px; /* Ajustado */
            padding: 12px 18px; /* Ajustado */
            margin-bottom: 20px; /* Ajustado */
            background-color: var(--lm-win-panel-bg);
            border-radius: var(--panel-border-radius);
            box-shadow: 0 4px 12px var(--shadow-color-medium);
            border: 1px solid var(--lm-win-panel-border);
        }
        .info-item {
            font-size: 1rem; /* Ajustado */
            font-weight: 600;
            color: var(--lm-win-header-text);
            white-space: nowrap; /* Evita quebra de linha no texto do item */
        }
        .info-item span {
            font-weight: 700;
            color: var(--lm-win-text-dark);
            background-color: rgba(255,255,255,0.6); /* Ajustado */
            padding: 2px 7px; /* Ajustado */
            border-radius: 5px; /* Ajustado */
        }

        .memory-game-board {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 15px; /* Ajustado */
            width: 100%;
            max-width: calc(5 * var(--card-width-lg) + 4 * 15px);
            perspective: 1500px;
            margin-bottom: 20px; /* Ajustado */
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: var(--card-border-radius);
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
        }

        .card-face img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
            border-radius: var(--card-border-radius);
            transition: outline 0.2s ease-out, border 0.2s ease-out;
        }

        .card {
            background-color: var(--lm-win-card-bg);
            width: var(--card-width-lg);
            height: var(--card-height-lg);
            border-radius: var(--card-border-radius);
            cursor: pointer;
            position: relative;
            transform-style: preserve-3d;
            transition: transform var(--card-flip-duration), box-shadow 0.3s ease-out;
            box-shadow: 0 6px 14px var(--shadow-color-medium); /* Ajustado */
        }

        .card:hover:not(.matched):not(.flipped) {
            box-shadow: 0 9px 20px var(--shadow-color-dark); /* Ajustado */
            transform: translateY(-4px) scale(1.02); /* Ajustado */
        }

        .card.flipped {
            transform: rotateY(180deg);
            box-shadow: 0 6px 14px var(--shadow-color-medium);
        }

        .card.correct-match .card-face--front img {
            outline: 3px solid var(--success-color); /* Ajustado */
            outline-offset: -3px; 
            animation: pulseCorrect 0.5s ease-out; /* Ajustado */
        }
        .card.wrong-match .card-face--front img {
            outline: 3px solid var(--error-color); /* Ajustado */
            outline-offset: -3px;
            animation: shakeWrong 0.35s ease-in-out; /* Ajustado */
        }

        @keyframes pulseCorrect {
            0% { transform: scale(1); } 
            50% { transform: scale(1.02); } /* Ajustado */
            100% { transform: scale(1); }
        }
        @keyframes shakeWrong {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-4px); } /* Ajustado */
            75% { transform: translateX(4px); } /* Ajustado */
        }

        .card.matched {
            transform: rotateY(180deg) scale(0.9);
            opacity: 0.6;
            cursor: default;
            box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.07), 0 1px 2px var(--shadow-color-light); /* Ajustado */
        }
      
        .card.matched .card-face--front img { 
             border: 3px solid var(--success-color); /* Ajustado */
             box-sizing: border-box; 
        }

        .card-face--front {
            transform: rotateY(180deg);
        }

        .game-controls {
            margin-top: 18px; /* Ajustado */
        }

        #restart-game-button { 
            background-image: linear-gradient(to right, #d65cc2 0%, #722980 50%, #7536bd 100%);
            background-size: 220% auto;
            color: var(--lm-win-text-light);
            border: none;
            padding: 12px 30px; /* Ajustado */
            font-size: 1rem; /* Ajustado */
            font-weight: 700;
            border-radius: var(--button-border-radius);
            cursor: pointer;
            transition: all 0.35s ease; /* Ajustado */
            box-shadow: 0 4px 8px var(--shadow-color-medium);
            text-transform: uppercase;
            letter-spacing: 0.7px;
        }
        #restart-game-button:hover {
            background-position: right center;
            box-shadow: 0 6px 12px var(--shadow-color-dark);
            transform: translateY(-2px);
        }

        .game-message { 
            font-size: 1.5rem; /* Ajustado */
            font-weight: 700;
            color: var(--success-color);
            margin-bottom: 18px; /* Ajustado */
        }

        .visually-hidden {
            position: absolute;
            width: 1px;
            height: 1px;
            margin: -1px;
            padding: 0;
            overflow: hidden;
            clip: rect(0, 0, 0, 0);
            border: 0;
        }

        /* Responsividade Detalhada */
        @media (max-width: 1024px) { /* Tablets e Desktops Menores */
            body { padding: 15px; }
            .main-header h1 { font-size: 2.2rem; }
            .screen { max-width: 600px; padding: 20px 25px; }
            .screen h2 { font-size: 1.8rem; }
            .screen p { font-size: 0.95rem; }
            .game-info-panel { max-width: 600px; padding: 10px 15px; }
            .info-item { font-size: 0.95rem; }

            .memory-game-board {
                grid-template-columns: repeat(4, 1fr);
                max-width: calc(4 * var(--card-width-md) + 3 * 15px); /* Usando card-width-md */
                gap: 15px;
            }
            .card {
                width: var(--card-width-md);
                height: var(--card-height-md);
            }
        }

        @media (max-width: 768px) { /* Tablets Menores e Celulares Maiores (Paisagem) */
            body { padding: 10px; }
            .main-header { margin-bottom: 20px; padding: 10px 20px;}
            .main-header h1 { font-size: 1.9rem; }
            .screen { padding: 20px 15px; max-width: 90%;}
            .screen h2 { font-size: 1.6rem; }
            .screen p { font-size: 0.9rem; }
            .screen .stats-item { font-size: 1rem; }
            .screen-button, #restart-game-button { font-size: 0.95rem; padding: 10px 22px;}
            .game-info-panel { max-width: 100%; } /* Ocupa mais da largura */


            .memory-game-board {
                /* Mantém 4 colunas se couber, senão o browser pode ajustar com auto-fit se usássemos */
                /* Para forçar, podemos mudar para 3 colunas aqui se 4 SM ficar muito apertado */
                grid-template-columns: repeat(4, 1fr); 
                max-width: calc(4 * var(--card-width-sm) + 3 * 12px); /* Usando card-width-sm */
                gap: 12px;
            }
            .card {
                width: var(--card-width-sm);
                height: var(--card-height-sm);
            }
        }

        @media (max-width: 520px) { /* Celulares (Retrato) - Ajuste do breakpoint anterior */
            .main-header h1 { font-size: 1.6rem; }
            .screen h2 { font-size: 1.5rem; }
            .screen p { font-size: 0.85rem; }
            .info-item { font-size: 0.85rem; }
            .info-item span { padding: 2px 5px; }
            .game-info-panel { gap: 8px; }


            .memory-game-board {
                grid-template-columns: repeat(3, 1fr); /* 3 Colunas para celulares */
                max-width: calc(3 * var(--card-width-sm) + 2 * 10px); /* Usando card-width-sm */
                gap: 10px;
            }
             /* .card já está com var(--card-width-sm) do breakpoint anterior, o que é 95px.
                3 * 95px + 2 * 10px = 285 + 20 = 305px. Isso cabe bem em telas > 320px.
             */
        }
        
        @media (max-width: 380px) { /* Celulares Muito Pequenos */
            body { padding: 10px 5px; } /* Menos padding lateral */
            .main-header { padding: 8px 15px; margin-bottom: 15px; }
            .main-header h1 { font-size: 1.4rem; }
            .screen { padding: 15px 10px; }
            .screen h2 { font-size: 1.3rem; }
            .screen p { font-size: 0.8rem; margin-bottom: 15px; }
            .screen .stats-item { font-size: 0.9rem; }
            .screen-button, #restart-game-button { font-size: 0.85rem; padding: 8px 18px;}
            .game-info-panel { padding: 8px 10px; }
            .info-item { font-size: 0.8rem; }


            .memory-game-board {
                max-width: calc(3 * var(--card-width-xs) + 2 * 8px); /* Usando card-width-xs */
                gap: 8px;
            }
            .card {
                width: var(--card-width-xs);
                height: var(--card-height-xs);
            }
            .game-message { font-size: 1.3rem; }
        }

    </style>
</head>

<body>
    <header class="main-header"> 
        <h1>Jogo da Memória LM Win</h1>
    </header>

    <section id="start-screen" class="screen active">
        <h2>Bem-vindo ao LM Win!</h2>
        <p>Teste sua memória e encontre todos os pares de cartas. Clique em "Iniciar Jogo" para começar a diversão!</p>
        <button id="start-game-button" class="screen-button">Iniciar Jogo</button>
    </section>

    <main id="game-main-content">
        <div class="game-info-panel">
            <div class="info-item">Tempo: <span id="time-display">00:00</span></div>
            <div class="info-item">Tentativas: <span id="attempts-display">0</span></div>
            <div class="info-item">Pontos: <span id="score-display">---</span></div>
        </div>

        <div id="memory-game-board" class="memory-game-board" role="grid" aria-label="Tabuleiro do Jogo da Memória">
            <!-- Cartas serão geradas pelo JavaScript -->
        </div>

        <div class="game-controls">
            <button id="restart-game-button">Reiniciar Jogo</button>
        </div>
    </main>

    <section id="end-screen" class="screen">
        <h2 id="end-game-message">Parabéns, você venceu!</h2>
        <div class="stats">
            <p class="stats-item">Seu tempo: <strong id="final-time">00:00</strong></p>
            <p class="stats-item">Tentativas: <strong id="final-attempts">0</strong></p>
            <p class="stats-item">Pontuação Final: <strong id="final-score">0</strong></p>
        </div>
        <button id="play-again-button" class="screen-button">Jogar Novamente</button>
    </section>

    <div id="sr-announcements" class="visually-hidden" aria-live="assertive" aria-atomic="true"></div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const gameBoard = document.getElementById('memory-game-board');
            const restartGameButton = document.getElementById('restart-game-button');
            const srAnnouncements = document.getElementById('sr-announcements');
            
            const timeDisplay = document.getElementById('time-display');
            const attemptsDisplay = document.getElementById('attempts-display');
            const scoreDisplay = document.getElementById('score-display'); 

            const startScreen = document.getElementById('start-screen');
            const startGameButton = document.getElementById('start-game-button');
            const gameMainContent = document.getElementById('game-main-content');
            const endScreen = document.getElementById('end-screen');
            const playAgainButton = document.getElementById('play-again-button');
            const finalTimeDisplay = document.getElementById('final-time');
            const finalAttemptsDisplay = document.getElementById('final-attempts');
            const finalScoreDisplay = document.getElementById('final-score');

            const backImageOptions = [
                "assets/images/cards/cover.png",
                "assets/images/cards/pink-cover.png"
            ];
            const LMcardsData = [
                { uniqueId: 1, pairKey: "par1", imageFront: "assets/images/cards/helmet-1.png", altTextFront: "Capacete Azul"},
                { uniqueId: 2, pairKey: "par1", imageFront: "assets/images/cards/helmet-1.png", altTextFront: "Capacete Azul"},
                { uniqueId: 3, pairKey: "par2", imageFront: "assets/images/cards/potion-1.png", altTextFront: "Poção Vermelha"},
                { uniqueId: 4, pairKey: "par2", imageFront: "assets/images/cards/potion-1.png", altTextFront: "Poção Vermelha"},
                { uniqueId: 5, pairKey: "par3", imageFront: "assets/images/cards/ring-1.png", altTextFront: "Anel Dourado"},
                { uniqueId: 6, pairKey: "par3", imageFront: "assets/images/cards/ring-1.png", altTextFront: "Anel Dourado"},
                { uniqueId: 7, pairKey: "par4", imageFront: "assets/images/cards/scroll-1.png", altTextFront: "Pergaminho Antigo"},
                { uniqueId: 8, pairKey: "par4", imageFront: "assets/images/cards/scroll-1.png", altTextFront: "Pergaminho Antigo"},
                { uniqueId: 9, pairKey: "par5", imageFront: "assets/images/cards/shield-1.png", altTextFront: "Escudo de Madeira"},
                { uniqueId: 10, pairKey: "par5", imageFront: "assets/images/cards/shield-1.png", altTextFront: "Escudo de Madeira"},
                { uniqueId: 11, pairKey: "par6", imageFront: "assets/images/cards/sword-1.png", altTextFront: "Espada Reluzente"},
                { uniqueId: 12, pairKey: "par6", imageFront: "assets/images/cards/sword-1.png", altTextFront: "Espada Reluzente"},
                { uniqueId: 13, pairKey: "par7", imageFront: "assets/images/cards/coin-1.png", altTextFront: "Moeda Dourada"},
                { uniqueId: 14, pairKey: "par7", imageFront: "assets/images/cards/coin-1.png", altTextFront: "Moeda Dourada"},
                { uniqueId: 15, pairKey: "par8", imageFront: "assets/images/cards/fire-1.png", altTextFront: "Chama Ardente"},
                { uniqueId: 16, pairKey: "par8", imageFront: "assets/images/cards/fire-1.png", altTextFront: "Chama Ardente"},
                { uniqueId: 17, pairKey: "par9", imageFront: "assets/images/cards/axe-1.png", altTextFront: "Machado de Batalha"},
                { uniqueId: 18, pairKey: "par9", imageFront: "assets/images/cards/axe-1.png", altTextFront: "Machado de Batalha"},
                { uniqueId: 19, pairKey: "par10", imageFront: "assets/images/cards/bone-1.png", altTextFront: "Osso"},
                { uniqueId: 20, pairKey: "par10", imageFront: "assets/images/cards/bone-1.png", altTextFront: "Osso"}
            ];

            let flippedCards = [];
            let matchedPairsCount = 0;
            let lockBoard = false;
            let gameStarted = false;
            let timerInterval;
            let secondsElapsed = 0;
            let attempts = 0;

            function shuffleCards(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function formatTime(seconds) {
                const minutes = Math.floor(seconds / 60);
                const remainingSeconds = seconds % 60;
                return `${minutes.toString().padStart(2, '0')}:${remainingSeconds.toString().padStart(2, '0')}`;
            }

            function startTimer() {
                if (timerInterval) clearInterval(timerInterval);
                secondsElapsed = 0;
                timeDisplay.textContent = formatTime(secondsElapsed);
                timerInterval = setInterval(() => {
                    secondsElapsed++;
                    timeDisplay.textContent = formatTime(secondsElapsed);
                }, 1000);
            }

            function stopTimer() {
                clearInterval(timerInterval);
            }
            
            function calculateScore() {
                const baseScore = 10000;
                const timePenalty = secondsElapsed * 10;
                const attemptsPenalty = attempts * 50;
                let finalScore = baseScore - timePenalty - attemptsPenalty;
                return Math.max(0, finalScore); 
            }

            function createBoard() {
                gameBoard.innerHTML = ''; 
                const shuffledGameCards = shuffleCards([...LMcardsData]);

                shuffledGameCards.forEach((cardData, index) => {
                    const cardElement = document.createElement('div');
                    cardElement.classList.add('card');
                    cardElement.dataset.pairKey = cardData.pairKey;
                    cardElement.dataset.uniqueId = cardData.uniqueId.toString();
                    cardElement.setAttribute('role', 'gridcell');
                    cardElement.setAttribute('tabindex', '0');

                    const randomBackImageSrc = backImageOptions[Math.floor(Math.random() * backImageOptions.length)];
                    const versoDesc = randomBackImageSrc.includes('pink') ? 'rosa' : 'azul';
                    cardElement.dataset.versoDesc = versoDesc;
                    cardElement.setAttribute('aria-label', `Carta ${index + 1}, verso ${versoDesc}. Virada para baixo.`);

                    const cardFaceBack = document.createElement('div');
                    cardFaceBack.classList.add('card-face', 'card-face--back');
                    const backImage = document.createElement('img');
                    backImage.src = randomBackImageSrc;
                    backImage.alt = `Verso da carta ${index + 1} (${versoDesc})`;
                    cardFaceBack.appendChild(backImage);

                    const cardFaceFront = document.createElement('div');
                    cardFaceFront.classList.add('card-face', 'card-face--front');
                    const frontImage = document.createElement('img');
                    frontImage.src = cardData.imageFront;
                    frontImage.alt = cardData.altTextFront;
                    cardFaceFront.appendChild(frontImage);
                    
                    cardElement.appendChild(cardFaceBack);
                    cardElement.appendChild(cardFaceFront);

                    cardElement.addEventListener('click', handleCardClick);
                    cardElement.addEventListener('keydown', (event) => {
                        if (event.key === 'Enter' || event.key === ' ') {
                            event.preventDefault();
                            handleCardClick.call(cardElement);
                        }
                    });
                    gameBoard.appendChild(cardElement);
                });
            }

            function handleCardClick() {
                if (lockBoard || this.classList.contains('flipped') || this.classList.contains('matched')) {
                    return;
                }

                if (!gameStarted) {
                    gameStarted = true;
                    startTimer();
                }

                this.classList.add('flipped');
                const uniqueIdClicked = this.dataset.uniqueId;
                const cardDataObject = LMcardsData.find(card => card.uniqueId === parseInt(uniqueIdClicked));
                
                if (cardDataObject) {
                    this.setAttribute('aria-label', `Carta: ${cardDataObject.altTextFront}. Virada para cima.`);
                    srAnnouncements.textContent = `Carta virada: ${cardDataObject.altTextFront}.`;
                }

                flippedCards.push(this);

                if (flippedCards.length === 2) {
                    lockBoard = true;
                    attempts++;
                    attemptsDisplay.textContent = attempts;
                    checkForPair();
                }
            }

            function checkForPair() {
                const [cardOne, cardTwo] = flippedCards;
                const isMatch = cardOne.dataset.pairKey === cardTwo.dataset.pairKey;

                cardOne.classList.add(isMatch ? 'correct-match' : 'wrong-match');
                cardTwo.classList.add(isMatch ? 'correct-match' : 'wrong-match');
                
                setTimeout(() => {
                    cardOne.classList.remove('correct-match', 'wrong-match');
                    cardTwo.classList.remove('correct-match', 'wrong-match');

                    if (isMatch) {
                        const altTextOne = LMcardsData.find(c => c.uniqueId === parseInt(cardOne.dataset.uniqueId))?.altTextFront || "Carta 1";
                        const altTextTwo = LMcardsData.find(c => c.uniqueId === parseInt(cardTwo.dataset.uniqueId))?.altTextFront || "Carta 2";
                        srAnnouncements.textContent = `Par encontrado! ${altTextOne} e ${altTextTwo}.`;
                        disableCards();
                    } else {
                        const altTextOne = LMcardsData.find(c => c.uniqueId === parseInt(cardOne.dataset.uniqueId))?.altTextFront || "Carta 1";
                        const altTextTwo = LMcardsData.find(c => c.uniqueId === parseInt(cardTwo.dataset.uniqueId))?.altTextFront || "Carta 2";
                        srAnnouncements.textContent = `Não combinam. ${altTextOne} e ${altTextTwo}. Virando de volta.`;
                        unflipCards();
                    }
                }, 600); 
            }

            function disableCards() {
                flippedCards.forEach(card => {
                    card.classList.add('matched');
                    card.setAttribute('tabindex', '-1');
                    const cardDataObject = LMcardsData.find(c => c.uniqueId === parseInt(card.dataset.uniqueId));
                    if (cardDataObject) {
                        card.setAttribute('aria-label', `Carta: ${cardDataObject.altTextFront}. Combinada.`);
                    }
                });
                matchedPairsCount++;
                resetTurn(); 
                checkWinCondition();
            }

            function unflipCards() {
                setTimeout(() => {
                    try {
                        flippedCards.forEach((card) => {
                            card.classList.remove('flipped');
                            const cardElementIndex = Array.from(gameBoard.children).indexOf(card);
                            const versoDesc = card.dataset.versoDesc || "desconhecido";
                            if (cardElementIndex !== -1) {
                                card.setAttribute('aria-label', `Carta ${indexToNumber(cardElementIndex)}, verso ${versoDesc}. Virada para baixo.`);
                            }
                        });
                    } catch (e) {
                        console.error("Erro dentro do setTimeout de unflipCards:", e);
                    } finally {
                        resetTurn(); 
                    }
                }, 1000); 
            }
            
            function indexToNumber(index) {
                return index + 1;
            }

            function resetTurn(isEndOfGame = false) {
                flippedCards = [];
                if (!isEndOfGame) { 
                    lockBoard = false;
                }
            }

            function checkWinCondition() {
                if (matchedPairsCount === LMcardsData.length / 2) {
                    stopTimer();
                    lockBoard = true; 
                    const finalScore = calculateScore();
                    
                    finalTimeDisplay.textContent = formatTime(secondsElapsed);
                    finalAttemptsDisplay.textContent = attempts;
                    finalScoreDisplay.textContent = finalScore;
                    scoreDisplay.textContent = finalScore; 

                    setTimeout(() => { 
                        gameMainContent.classList.remove('active');
                        endScreen.classList.add('active');
                        srAnnouncements.textContent = 'Parabéns, você venceu o jogo!';
                    }, 800);
                }
            }
            
            function initializeGame() {
                matchedPairsCount = 0;
                lockBoard = false;
                flippedCards = [];
                gameStarted = false;
                secondsElapsed = 0;
                attempts = 0;
                
                timeDisplay.textContent = formatTime(0);
                attemptsDisplay.textContent = '0';
                scoreDisplay.textContent = '---';
                if(timerInterval) clearInterval(timerInterval);

                createBoard(); 
                
                startScreen.classList.remove('active');
                endScreen.classList.remove('active');
                gameMainContent.classList.add('active');
                srAnnouncements.textContent = 'Jogo iniciado.';
            }

            startGameButton.addEventListener('click', initializeGame);
            playAgainButton.addEventListener('click', initializeGame);
            restartGameButton.addEventListener('click', () => {
                initializeGame();
            });

            startScreen.classList.add('active');
            gameMainContent.classList.remove('active');
            endScreen.classList.remove('active');
        });
    </script>
</body>
</html>
