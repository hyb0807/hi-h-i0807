<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>블랙잭 게임</title>
    <style>
        /* CSS 스타일 */
        .card {
            width: 50px;
            height: 75px;
            border: 1px solid black;
            display: inline-block;
            margin: 5px;
            text-align: center;
            line-height: 75px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <h1>블랙잭 게임</h1>
    <div id="player-hand">
        <h2>플레이어</h2>
    </div>
    <div id="dealer-hand">
        <h2>딜러</h2>
    </div>
    <button onclick="hit()">Hit</button>
    <button onclick="stand()">Stand</button>
    <button onclick="reset()">Reset</button>

    <script>
        // 카드 덱 배열
        const deck = [2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11];

        // 플레이어와 딜러의 카드 배열
        let playerCards = [];
        let dealerCards = [];

        // 카드 뽑기 함수
        function drawCard() {
            const index = Math.floor(Math.random() * deck.length);
            return deck.splice(index, 1)[0];
        }

        // 초기 게임 시작 함수
        function startGame() {
            playerCards.push(drawCard(), drawCard());
            dealerCards.push(drawCard(), drawCard());
            renderCards();
        }

        // 플레이어가 히트할 때의 함수
        function hit() {
            playerCards.push(drawCard());
            renderCards();
            checkGameOver();
        }

        // 플레이어가 스탠드할 때의 함수
        function stand() {
            while (calculateScore(dealerCards) < 17) {
                dealerCards.push(drawCard());
            }
            renderCards();
            checkGameOver();
        }

        // 게임 종료 후 결과 확인 함수
        function checkGameOver() {
            const playerScore = calculateScore(playerCards);
            const dealerScore = calculateScore(dealerCards);

            if (playerScore > 21 || dealerScore === 21 || (dealerScore > playerScore && dealerScore <= 21)) {
                alert("딜러가 이겼습니다!");
            } else if (dealerScore > 21 || playerScore === 21 || (playerScore > dealerScore && playerScore <= 21)) {
                alert("플레이어가 이겼습니다!");
            } else {
                alert("무승부입니다!");
            }
        }

        // 카드 점수 계산 함수
        function calculateScore(cards) {
            let score = cards.reduce((sum, card) => sum + card, 0);
            // A 카드의 점수 처리
            while (score > 21 && cards.includes(11)) {
                cards[cards.indexOf(11)] = 1;
                score -= 10;
            }
            return score;
        }

        // 카드 렌더링 함수
        function renderCards() {
            document.getElementById('player-hand').innerHTML = `<h2>플레이어 (${calculateScore(playerCards)})</h2>` + playerCards.map(card => `<div class="card">${card}</div>`).join('');
            document.getElementById('dealer-hand').innerHTML = `<h2>딜러 (${calculateScore(dealerCards)})</h2>` + dealerCards.map(card => `<div class="card">${card}</div>`).join('');
        }

        // 게임 초기화 함수
        function reset() {
            // 카드 덱 초기화
            deck.splice(0, deck.length);
            for (let i = 0; i < 4; i++) {
                deck.push(2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11);
            }

            // 플레이어와 딜러의 카드 배열 초기화
            playerCards = [];
            dealerCards = [];

            // 게임 재시작
            startGame();
        }

        // 초기 게임 시작
        startGame();
    </script>
</body>
</html>
