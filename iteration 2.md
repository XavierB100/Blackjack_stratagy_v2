<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blackjack - Perfect Strategy & Card Counting</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: #e2e8f0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .game-container {
            max-width: 1200px;
            width: 100%;
            background: rgba(30, 41, 59, 0.8);
            border-radius: 20px;
            padding: 20px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            backdrop-filter: blur(10px);
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 2.5rem;
            background: linear-gradient(45deg, #fbbf24, #f59e0b);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 10px;
        }

        .count-display {
            background: rgba(239, 68, 68, 0.2);
            border: 2px solid #ef4444;
            border-radius: 15px;
            padding: 15px;
            margin: 20px 0;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .count-display::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(239, 68, 68, 0.2), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { left: -100%; }
            100% { left: 100%; }
        }

        .count-info {
            display: flex;
            justify-content: space-around;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }

        .count-item {
            background: rgba(15, 23, 42, 0.7);
            padding: 10px 20px;
            border-radius: 10px;
            border: 1px solid rgba(239, 68, 68, 0.3);
        }

        .count-item h3 {
            color: #fbbf24;
            margin-bottom: 5px;
        }

        .count-value {
            font-size: 1.5rem;
            font-weight: bold;
            color: #ef4444;
        }

        .betting-advice {
            background: rgba(34, 197, 94, 0.2);
            border: 2px solid #22c55e;
            border-radius: 10px;
            padding: 10px;
            margin: 10px 0;
            text-align: center;
            font-weight: bold;
        }

        .game-area {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        .hand-section {
            background: rgba(15, 23, 42, 0.5);
            border-radius: 15px;
            padding: 20px;
            border: 2px solid rgba(148, 163, 184, 0.3);
        }

        .hand-title {
            font-size: 1.5rem;
            margin-bottom: 15px;
            text-align: center;
            color: #fbbf24;
        }

        .cards-display {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin-bottom: 15px;
            min-height: 120px;
        }

        .card {
            width: 80px;
            height: 112px;
            background: linear-gradient(145deg, #ffffff, #f1f5f9);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
            font-weight: bold;
            color: #1e293b;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            border: 2px solid #e2e8f0;
            transition: transform 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card.red {
            color: #dc2626;
        }

        .card-back {
            background: linear-gradient(135deg, #dc2626 0%, #7f1d1d 100%);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .card-back::before {
            content: '🂠';
            font-size: 2rem;
            opacity: 0.7;
        }

        .hand-value {
            text-align: center;
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: #e2e8f0;
        }

        .strategy-advice {
            background: rgba(59, 130, 246, 0.2);
            border: 2px solid #3b82f6;
            border-radius: 10px;
            padding: 15px;
            margin: 10px 0;
            text-align: center;
            min-height: 50px;
        }

        .strategy-advice h3 {
            color: #60a5fa;
            margin-bottom: 5px;
        }

        .strategy-text {
            font-size: 1.1rem;
            font-weight: bold;
            color: #93c5fd;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }

        .btn:active {
            transform: translateY(0);
        }

        .btn-primary {
            background: linear-gradient(45deg, #3b82f6, #1d4ed8);
            color: white;
        }

        .btn-success {
            background: linear-gradient(45deg, #22c55e, #16a34a);
            color: white;
        }

        .btn-warning {
            background: linear-gradient(45deg, #f59e0b, #d97706);
            color: white;
        }

        .btn-danger {
            background: linear-gradient(45deg, #ef4444, #dc2626);
            color: white;
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .game-stats {
            background: rgba(15, 23, 42, 0.7);
            border-radius: 15px;
            padding: 20px;
            margin-top: 20px;
            text-align: center;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .stat-item {
            background: rgba(30, 41, 59, 0.8);
            padding: 15px;
            border-radius: 10px;
            border: 1px solid rgba(148, 163, 184, 0.3);
        }

        .stat-label {
            color: #94a3b8;
            font-size: 0.9rem;
            margin-bottom: 5px;
        }

        .stat-value {
            color: #fbbf24;
            font-size: 1.5rem;
            font-weight: bold;
        }

        .message {
            text-align: center;
            font-size: 1.2rem;
            margin: 20px 0;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .message.win {
            background: rgba(34, 197, 94, 0.2);
            border: 2px solid #22c55e;
            color: #22c55e;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }

        .message.blackjack {
            background: linear-gradient(45deg, rgba(34, 197, 94, 0.2), rgba(234, 179, 8, 0.2));
            border: 2px solid #fbbf24;
            color: #fbbf24;
            text-shadow: 0 0 10px rgba(251, 191, 36, 0.5);
            animation: pulseGold 1.5s infinite;
        }

        @keyframes pulseGold {
            0% { transform: scale(1); box-shadow: 0 0 0 rgba(251, 191, 36, 0); }
            50% { transform: scale(1.03); box-shadow: 0 0 20px rgba(251, 191, 36, 0.3); }
            100% { transform: scale(1); box-shadow: 0 0 0 rgba(251, 191, 36, 0); }
        }

        .message.lose {
            background: rgba(239, 68, 68, 0.2);
            border: 2px solid #ef4444;
            color: #ef4444;
        }

        .message.push {
            background: rgba(168, 162, 158, 0.2);
            border: 2px solid #a8a29e;
            color: #a8a29e;
        }

        .split-container {
            display: flex;
            flex-direction: column;
            gap: 20px;
            margin-bottom: 20px;
        }

        .split-hand {
            background: rgba(15, 23, 42, 0.7);
            border-radius: 10px;
            padding: 15px;
            border: 1px solid rgba(148, 163, 184, 0.3);
        }

        .split-hand-title {
            color: #fbbf24;
            margin-bottom: 10px;
        }

        .active-hand {
            border: 2px solid #3b82f6;
            box-shadow: 0 0 15px rgba(59, 130, 246, 0.3);
        }

        @media (max-width: 768px) {
            .game-area {
                grid-template-columns: 1fr;
            }
            
            .count-info {
                flex-direction: column;
            }
            
            .controls {
                flex-direction: column;
                align-items: center;
            }
            
            .btn {
                width: 100%;
                max-width: 200px;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="header">
            <h1>🃏 Blackjack Strategy Master</h1>
            <p>Perfect Basic Strategy with Card Counting</p>
        </div>

        <div class="count-display">
            <div class="count-info">
                <div class="count-item">
                    <h3>Running Count</h3>
                    <div class="count-value" id="runningCount">0</div>
                </div>
                <div class="count-item">
                    <h3>True Count</h3>
                    <div class="count-value" id="trueCount">0.0</div>
                </div>
                <div class="count-item">
                    <h3>Decks Left</h3>
                    <div class="count-value" id="decksLeft">6.0</div>
                </div>
                <div class="count-item">
                    <h3>Penetration</h3>
                    <div class="count-value" id="penetration">0%</div>
                </div>
            </div>
            <div class="betting-advice" id="bettingAdvice">
                Bet table minimum - True count is not favorable
            </div>
        </div>

        <div id="splitContainer" class="split-container" style="display: none;">
            <!-- Split hands will be dynamically inserted here -->
        </div>

        <div class="game-area">
            <div class="hand-section">
                <div class="hand-title">Dealer's Hand</div>
                <div class="cards-display" id="dealerCards"></div>
                <div class="hand-value" id="dealerValue">Value: 0</div>
            </div>

            <div class="hand-section">
                <div class="hand-title">Your Hand</div>
                <div class="cards-display" id="playerCards"></div>
                <div class="hand-value" id="playerValue">Value: 0</div>
            </div>
        </div>

        <div class="strategy-advice" id="strategyAdvice">
            <h3>Strategy Recommendation</h3>
            <div class="strategy-text">Click "New Game" to start</div>
        </div>

        <div class="controls">
            <button class="btn btn-primary" onclick="newGame()">New Game</button>
            <button class="btn btn-success" id="hitBtn" onclick="hit()" disabled>Hit</button>
            <button class="btn btn-warning" id="standBtn" onclick="stand()" disabled>Stand</button>
            <button class="btn btn-danger" id="doubleBtn" onclick="double()" disabled>Double</button>
            <button class="btn btn-primary" id="splitBtn" onclick="split()" disabled>Split</button>
        </div>

        <div class="message" id="gameMessage" style="display: none;"></div>

        <div class="game-stats">
            <h3>Game Statistics</h3>
            <div class="stats-grid">
                <div class="stat-item">
                    <div class="stat-label">Games Played</div>
                    <div class="stat-value" id="gamesPlayed">0</div>
                </div>
                <div class="stat-item">
                    <div class="stat-label">Wins</div>
                    <div class="stat-value" id="wins">0</div>
                </div>
                <div class="stat-item">
                    <div class="stat-label">Losses</div>
                    <div class="stat-value" id="losses">0</div>
                </div>
                <div class="stat-item">
                    <div class="stat-label">Win Rate</div>
                    <div class="stat-value" id="winRate">0%</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Game state
        let deck = [];
        let playerHands = [];
        let currentHandIndex = 0;
        let dealerHand = [];
        let gameOver = false;
        let runningCount = 0;
        let cardsDealt = 0;
        let totalDecks = 6;
        let initialDeckSize = totalDecks * 52;
        let gameStats = {
            played: 0,
            wins: 0,
            losses: 0,
            pushes: 0,
            blackjacks: 0
        };

        // Card values and suits
        const suits = ['♠', '♥', '♦', '♣'];
        const ranks = ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K'];
        const cardValues = {
            'A': 11, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10, 'J': 10, 'Q': 10, 'K': 10
        };

        // Initialize deck
        function createDeck() {
            deck = [];
            for (let d = 0; d < totalDecks; d++) {
                for (let suit of suits) {
                    for (let rank of ranks) {
                        deck.push({
                            rank: rank,
                            suit: suit,
                            value: cardValues[rank]
                        });
                    }
                }
            }
            initialDeckSize = deck.length;
            shuffleDeck();
        }

        function shuffleDeck() {
            for (let i = deck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [deck[i], deck[j]] = [deck[j], deck[i]];
            }
            runningCount = 0;
            cardsDealt = 0;
            updateCountDisplay();
        }

        function dealCard() {
            if (deck.length === 0) {
                createDeck();
            }
            const card = deck.pop();
            cardsDealt++;
            updateCount(card);
            return card;
        }

        function updateCount(card) {
            const rank = card.rank;
            if (['2', '3', '4', '5', '6'].includes(rank)) {
                runningCount += 1;
            } else if (['10', 'J', 'Q', 'K', 'A'].includes(rank)) {
                runningCount -= 1;
            }
            // 7,8,9 are neutral (0)
            updateCountDisplay();
        }

        function updateCountDisplay() {
            const decksLeft = Math.max(0.5, (deck.length / 52));
            const trueCount = runningCount / decksLeft;
            const penetration = ((initialDeckSize - deck.length) / initialDeckSize) * 100;
            
            document.getElementById('runningCount').textContent = runningCount;
            document.getElementById('trueCount').textContent = trueCount.toFixed(1);
            document.getElementById('decksLeft').textContent = decksLeft.toFixed(1);
            document.getElementById('penetration').textContent = penetration.toFixed(1) + '%';
            
            const bettingAdvice = document.getElementById('bettingAdvice');
            if (trueCount >= 2) {
                bettingAdvice.textContent = `🔥 RAISE YOUR BET! True count is +${trueCount.toFixed(1)} (Favorable)`;
                bettingAdvice.style.background = 'rgba(34, 197, 94, 0.3)';
                bettingAdvice.style.borderColor = '#22c55e';
            } else {
                bettingAdvice.textContent = `Bet table minimum - True count: ${trueCount.toFixed(1)} (Not favorable)`;
                bettingAdvice.style.background = 'rgba(239, 68, 68, 0.2)';
                bettingAdvice.style.borderColor = '#ef4444';
            }
        }

        function calculateHandValue(hand) {
            let value = 0;
            let aces = 0;
            
            for (let card of hand) {
                value += card.value;
                if (card.rank === 'A') aces++;
            }
            
            while (value > 21 && aces > 0) {
                value -= 10;
                aces--;
            }
            
            return value;
        }

        function hasUsableAce(hand) {
            let value = 0;
            let aces = 0;
            
            for (let card of hand) {
                value += card.value;
                if (card.rank === 'A') aces++;
            }
            
            while (value > 21 && aces > 0) {
                value -= 10;
                aces--;
            }
            
            return aces > 0;
        }

        function isPair(hand) {
            return hand.length === 2 && hand[0].rank === hand[1].rank;
        }

        function canSplit() {
            if (playerHands.length >= 4) return false; // Max 4 hands
            const currentHand = playerHands[currentHandIndex];
            return currentHand.length === 2 && currentHand[0].rank === currentHand[1].rank;
        }

        function canDouble() {
            const currentHand = playerHands[currentHandIndex];
            return currentHand.length === 2;
        }

        function getBasicStrategy(playerHand, dealerUpCard) {
            const playerValue = calculateHandValue(playerHand);
            const dealerUpValue = dealerUpCard.rank === 'A' ? 1 : (cardValues[dealerUpCard.rank] === 10 ? 10 : cardValues[dealerUpCard.rank]);
            
            // Check for pairs first
            if (isPair(playerHand)) {
                return getPairStrategy(playerHand[0].rank, dealerUpValue);
            }
            
            // Check for soft hands
            if (hasUsableAce(playerHand)) {
                return getSoftHandStrategy(playerHand, dealerUpValue);
            }
            
            // Hard hands
            return getHardHandStrategy(playerValue, dealerUpValue);
        }

        function getPairStrategy(rank, dealerUpValue) {
            switch (rank) {
                case 'A':
                case '8':
                    return 'SPLIT';
                case '2':
                case '3':
                case '7':
                    return (dealerUpValue >= 2 && dealerUpValue <= 7) ? 'SPLIT' : 'HIT';
                case '4':
                    return 'HIT'; // Never split 4s according to perfect strategy
                case '6':
                    return (dealerUpValue >= 2 && dealerUpValue <= 6) ? 'SPLIT' : 'HIT';
                case '9':
                    return (dealerUpValue >= 2 && dealerUpValue <= 9 && dealerUpValue !== 7) ? 'SPLIT' : 'STAND';
                case '5':
                    return (dealerUpValue >= 2 && dealerUpValue <= 9) ? 'DOUBLE' : 'HIT';
                default: // 10, J, Q, K
                    return 'STAND';
            }
        }

        function getSoftHandStrategy(hand, dealerUpValue) {
            let nonAceValue = 0;
            for (let card of hand) {
                if (card.rank !== 'A') {
                    nonAceValue += card.value;
                }
            }
            
            if (nonAceValue === 2 || nonAceValue === 3) { // A,2 or A,3
                return (dealerUpValue >= 5 && dealerUpValue <= 6) ? 'DOUBLE' : 'HIT';
            } else if (nonAceValue === 4 || nonAceValue === 5) { // A,4 or A,5
                return (dealerUpValue >= 4 && dealerUpValue <= 6) ? 'DOUBLE' : 'HIT';
            } else if (nonAceValue === 6) { // A,6
                return (dealerUpValue >= 3 && dealerUpValue <= 6) ? 'DOUBLE' : 'HIT';
            } else if (nonAceValue === 7) { // A,7
                if (dealerUpValue >= 3 && dealerUpValue <= 6) return 'DOUBLE';
                if (dealerUpValue === 2 || dealerUpValue === 7 || dealerUpValue === 8) return 'STAND';
                return 'HIT';
            } else { // A,8 or A,9
                return 'STAND';
            }
        }

        function getHardHandStrategy(playerValue, dealerUpValue) {
            if (playerValue <= 8) {
                return 'HIT';
            } else if (playerValue === 9) {
                return (dealerUpValue >= 3 && dealerUpValue <= 6) ? 'DOUBLE' : 'HIT';
            } else if (playerValue === 10) {
                return (dealerUpValue >= 2 && dealerUpValue <= 9) ? 'DOUBLE' : 'HIT';
            } else if (playerValue === 11) {
                return (dealerUpValue >= 2 && dealerUpValue <= 10) ? 'DOUBLE' : 'HIT';
            } else if (playerValue === 12) {
                return (dealerUpValue >= 4 && dealerUpValue <= 6) ? 'STAND' : 'HIT';
            } else if (playerValue >= 13 && playerValue <= 16) {
                return (dealerUpValue >= 2 && dealerUpValue <= 6) ? 'STAND' : 'HIT';
            } else {
                return 'STAND';
            }
        }

        function displayCards(hand, containerId, hideSecondCard = false) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            hand.forEach((card, index) => {
                const cardElement = document.createElement('div');
                if (hideSecondCard && index === 1 && containerId === 'dealerCards' && !gameOver) {
                    cardElement.className = 'card card-back';
                } else {
                    cardElement.className = 'card';
                    if (card.suit === '♥' || card.suit === '♦') {
                        cardElement.classList.add('red');
                    }
                    cardElement.textContent = `${card.rank}${card.suit}`;
                }
                container.appendChild(cardElement);
            });
        }

        function updateStrategy() {
            if (gameOver || dealerHand.length === 0 || playerHands.length === 0) return;
            
            const currentHand = playerHands[currentHandIndex];
            const strategy = getBasicStrategy(currentHand, dealerHand[0]);
            const strategyElement = document.getElementById('strategyAdvice');
            const strategyText = strategyElement.querySelector('.strategy-text');
            
            let advice = '';
            let color = '#93c5fd';
            
            switch (strategy) {
                case 'HIT':
                    advice = '👆 HIT - Take another card';
                    color = '#60a5fa';
                    break;
                case 'STAND':
                    advice = '✋ STAND - Keep your current hand';
                    color = '#34d399';
                    break;
                case 'DOUBLE':
                    advice = '📈 DOUBLE - Double your bet and take one card';
                    color = '#fbbf24';
                    break;
                case 'SPLIT':
                    advice = '↔️ SPLIT - Split your pair into two hands';
                    color = '#f87171';
                    break;
            }
            
            strategyText.textContent = advice;
            strategyText.style.color = color;
        }

        function updateButtonStates() {
            if (gameOver || playerHands.length === 0) {
                document.getElementById('hitBtn').disabled = true;
                document.getElementById('standBtn').disabled = true;
                document.getElementById('doubleBtn').disabled = true;
                document.getElementById('splitBtn').disabled = true;
                return;
            }
            
            const currentHand = playerHands[currentHandIndex];
            const handValue = calculateHandValue(currentHand);
            
            document.getElementById('hitBtn').disabled = handValue >= 21;
            document.getElementById('standBtn').disabled = false;
            document.getElementById('doubleBtn').disabled = !canDouble() || handValue >= 21;
            document.getElementById('splitBtn').disabled = !canSplit();
        }

        function displayActiveHand() {
            const currentHand = playerHands[currentHandIndex];
            displayCards(currentHand, 'playerCards');
            document.getElementById('playerValue').textContent = `Value: ${calculateHandValue(currentHand)}`;
        }

        function newGame() {
            if (deck.length < 20) { // Reshuffle when deck is low
                createDeck();
            }
            
            playerHands = [[]];
            currentHandIndex = 0;
            dealerHand = [];
            gameOver = false;
            
            // Deal initial cards
            playerHands[0].push(dealCard());
            dealerHand.push(dealCard());
            playerHands[0].push(dealCard());
            dealerHand.push(dealCard());
            
            displayActiveHand();
            displayCards(dealerHand, 'dealerCards', true);
            
            document.getElementById('dealerValue').textContent = `Value: ${cardValues[dealerHand[0].rank]}`;
            
            // Hide split container
            document.getElementById('splitContainer').style.display = 'none';
            
            // Update UI
            updateButtonStates();
            document.getElementById('gameMessage').style.display = 'none';
            updateStrategy();
            
            // Check for blackjack
            if (calculateHandValue(playerHands[0]) === 21) {
                if (calculateHandValue(dealerHand) === 21) {
                    endGame('push');
                } else {
                    endGame('blackjack');
                }
            }
        }

        function hit() {
            if (gameOver) return;
            
            const currentHand = playerHands[currentHandIndex];
            currentHand.push(dealCard());
            displayActiveHand();
            
            const playerValue = calculateHandValue(currentHand);
            
            if (playerValue > 21) {
                if (playerHands.length > 1 && currentHandIndex < playerHands.length - 1) {
                    // Move to next split hand
                    currentHandIndex++;
                    displayActiveHand();
                    updateStrategy();
                } else {
                    // All hands are done, dealer's turn
                    stand();
                }
            } else {
                updateStrategy();
                // Disable double after hitting
                document.getElementById('doubleBtn').disabled = true;
            }
            
            updateButtonStates();
        }

        function stand() {
            if (gameOver) return;
            
            if (playerHands.length > 1 && currentHandIndex < playerHands.length - 1) {
                // Move to next split hand
                currentHandIndex++;
                displayActiveHand();
                updateStrategy();
                updateButtonStates();
                return;
            }
            
            // Dealer's turn
            gameOver = true;
            displayCards(dealerHand, 'dealerCards');
            document.getElementById('dealerValue').textContent = `Value: ${calculateHandValue(dealerHand)}`;
            
            // Dealer hits on 16 or less, stands on soft 17
            while (calculateHandValue(dealerHand) < 17 || 
                  (calculateHandValue(dealerHand) === 17 && hasUsableAce(dealerHand))) {
                dealerHand.push(dealCard());
                displayCards(dealerHand, 'dealerCards');
                document.getElementById('dealerValue').textContent = `Value: ${calculateHandValue(dealerHand)}`;
            }
            
            // Evaluate all hands
            const dealerValue = calculateHandValue(dealerHand);
            let allResults = [];
            
            for (let i = 0; i < playerHands.length; i++) {
                const playerValue = calculateHandValue(playerHands[i]);
                
                if (playerValue > 21) {
                    allResults.push('lose');
                    gameStats.losses++;
                } else if (dealerValue > 21) {
                    allResults.push('win');
                    gameStats.wins++;
                } else if (playerValue === 21 && playerHands[i].length === 2 && 
                          (dealerValue !== 21 || dealerHand.length !== 2)) {
                    allResults.push('blackjack');
                    gameStats.wins++;
                    gameStats.blackjacks++;
                } else if (playerValue > dealerValue) {
                    allResults.push('win');
                    gameStats.wins++;
                } else if (playerValue < dealerValue) {
                    allResults.push('lose');
                    gameStats.losses++;
                } else {
                    allResults.push('push');
                    gameStats.pushes++;
                }
            }
            
            // Show final message based on all results
            if (allResults.includes('blackjack')) {
                endGame('blackjack');
            } else if (allResults.includes('win')) {
                endGame('win');
            } else if (allResults.includes('lose')) {
                endGame('lose');
            } else {
                endGame('push');
            }
        }

        function double() {
            if (gameOver) return;
            
            const currentHand = playerHands[currentHandIndex];
            currentHand.push(dealCard());
            displayActiveHand();
            
            // Double means we must stand after one card
            stand();
        }

        function split() {
            if (!canSplit()) return;
            
            const currentHand = playerHands[currentHandIndex];
            const newHand1 = [currentHand[0], dealCard()];
            const newHand2 = [currentHand[1], dealCard()];
            
            // Replace current hand with two new hands
            playerHands.splice(currentHandIndex, 1, newHand1, newHand2);
            currentHandIndex = 0;
            
            // Update UI for split hands
            updateSplitUI();
            
            // Special rule for Aces: get only one card per Ace and can't hit again
            if (newHand1[0].rank === 'A') {
                for (let i = 0; i < playerHands.length; i++) {
                    if (playerHands[i].length === 1) {
                        playerHands[i].push(dealCard());
                    }
                }
                
                // Can't hit or double after splitting Aces
                document.getElementById('hitBtn').disabled = true;
                document.getElementById('doubleBtn').disabled = true;
                
                // Show all split hands
                updateSplitUI();
                
                // Automatically stand if all split hands have 2 cards
                let allHandsComplete = true;
                for (let hand of playerHands) {
                    if (hand.length < 2) {
                        allHandsComplete = false;
                        break;
                    }
                }
                
                if (allHandsComplete) {
                    stand();
                }
            } else {
                // For non-Ace splits, continue with current hand
                displayActiveHand();
                updateStrategy();
                updateButtonStates();
            }
        }

        function updateSplitUI() {
            const splitContainer = document.getElementById('splitContainer');
            splitContainer.style.display = 'flex';
            splitContainer.innerHTML = '';
            
            for (let i = 0; i < playerHands.length; i++) {
                const hand = playerHands[i];
                const handElement = document.createElement('div');
                handElement.className = `split-hand ${i === currentHandIndex ? 'active-hand' : ''}`;
                
                const title = document.createElement('div');
                title.className = 'split-hand-title';
                title.textContent = `Hand ${i + 1}`;
                
                const cardsDisplay = document.createElement('div');
                cardsDisplay.className = 'cards-display';
                
                hand.forEach(card => {
                    const cardElement = document.createElement('div');
                    cardElement.className = 'card';
                    if (card.suit === '♥' || card.suit === '♦') {
                        cardElement.classList.add('red');
                    }
                    cardElement.textContent = `${card.rank}${card.suit}`;
                    cardsDisplay.appendChild(cardElement);
                });
                
                const valueDisplay = document.createElement('div');
                valueDisplay.className = 'hand-value';
                valueDisplay.textContent = `Value: ${calculateHandValue(hand)}`;
                
                handElement.appendChild(title);
                handElement.appendChild(cardsDisplay);
                handElement.appendChild(valueDisplay);
                splitContainer.appendChild(handElement);
            }
        }

        function endGame(result) {
            gameOver = true;
            
            // Show dealer's full hand
            displayCards(dealerHand, 'dealerCards');
            document.getElementById('dealerValue').textContent = `Value: ${calculateHandValue(dealerHand)}`;
            
            // Disable buttons
            document.getElementById('hitBtn').disabled = true;
            document.getElementById('standBtn').disabled = true;
            document.getElementById('doubleBtn').disabled = true;
            document.getElementById('splitBtn').disabled = true;
            
            // Show message
            const messageElement = document.getElementById('gameMessage');
            messageElement.style.display = 'block';
            
            let message = '';
            let className = '';
            
            switch (result) {
                case 'win':
                    message = '🎉 You Win!';
                    className = 'win';
                    break;
                case 'lose':
                    message = '😞 You Lose!';
                    className = 'lose';
                    break;
                case 'push':
                    message = '🤝 Push (Tie)';
                    className = 'push';
                    break;
                case 'blackjack':
                    message = '🃏 Blackjack! You Win 3:2!';
                    className = 'blackjack';
                    break;
            }
            
            messageElement.textContent = message;
            messageElement.className = `message ${className}`;
            
            // Update game stats
            gameStats.played++;
            updateGameStats();
            
            // Clear strategy advice
            const strategyText = document.querySelector('.strategy-text');
            strategyText.textContent = 'Game Over - Click "New Game" to play again';
            strategyText.style.color = '#94a3b8';
        }

        function updateGameStats() {
            document.getElementById('gamesPlayed').textContent = gameStats.played;
            document.getElementById('wins').textContent = gameStats.wins;
            document.getElementById('losses').textContent = gameStats.losses;
            document.getElementById('winRate').textContent = gameStats.played > 0 ? 
                Math.round((gameStats.wins / gameStats.played) * 100) + '%' : '0%';
        }

        // Initialize the game
        createDeck();
    </script>
</body>
</html>
