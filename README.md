# Microsoft Copilot - Curso da DIO (inputs e outputs)
O exercício pede para demonstar o conhecimento aprendido durante o percorrer do curso, relacionado a ferramenta microsoft copilot, e fazer a demonstração de um uso com entradas e saídas (especificamente relacionadas a imagens).

Para esse exercício foi utilizado o chat do copilot e ferramentasb de edição de texto para utilizar os recursos enviados para esse caso.
Os tópicos relacionados a desenvolvimento de jogos e visão computacional.

(input de texto + imagem)
(input) Recrie esse jogo para rodar no navegador

![download](https://github.com/user-attachments/assets/0250056c-c4cc-41d6-88ed-90a874bd5d9c)

(Output)
Analisando a imagem: os rostos podem estar desfocados para proteger a privacidade

Enviado por Copilot:
Para recriar esse jogo clássico de empilhamento de blocos no navegador, podemos usar HTML, CSS e JavaScript. Vou te mostrar um exemplo básico de como começar:

Estrutura HTML
HTML

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Blocos</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="game-container">
        <canvas id="gameCanvas" width="300" height="600"></canvas>
        <div class="score-board">
            <p>Pontuação: <span id="score">0</span></p>
            <p>Nível: <span id="level">1</span></p>
            <p>Próxima peça:</p>
            <canvas id="nextPieceCanvas" width="100" height="100"></canvas>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Estilo CSS
CSS

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #000;
    color: #fff;
    font-family: Arial, sans-serif;
}

.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

canvas {
    border: 1px solid #fff;
}

.score-board {
    margin-top: 20px;
    text-align: center;
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Lógica JavaScript
JavaScript

const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const nextPieceCanvas = document.getElementById('nextPieceCanvas');
const nextCtx = nextPieceCanvas.getContext('2d');

const ROWS = 20;
const COLS = 10;
const BLOCK_SIZE = 30;

let board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
let score = 0;
let level = 1;

function drawBoard() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (let row = 0; row < ROWS; row++) {
        for (let col = 0; col < COLS; col++) {
            if (board[row][col]) {
                ctx.fillStyle = 'cyan';
                ctx.fillRect(col * BLOCK_SIZE, row * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
                ctx.strokeRect(col * BLOCK_SIZE, row * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
            }
        }
    }
}

function updateScore() {
    document.getElementById('score').innerText = score;
    document.getElementById('level').innerText = level;
}

function gameLoop() {
    drawBoard();
    updateScore();
    requestAnimationFrame(gameLoop);
}

gameLoop();
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Este é um ponto de partida básico. Você pode expandir isso adicionando a lógica para as peças caírem, rotacionarem, e a detecção de linhas completas. Se precisar de mais ajuda ou tiver alguma dúvida específica, estou aqui para ajudar! 😊


(imagem do código enviado:)
![image](https://github.com/user-attachments/assets/4e719997-e947-419e-b29b-1ebdfd52d183)


Em seguida:
(input) quero que ele seja jogável

(output)
Entendi! Vamos adicionar a lógica para tornar o jogo jogável. Vou incluir a movimentação das peças, rotação e a detecção de colisões. Aqui está uma versão mais completa:

Estrutura HTML
HTML

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Blocos</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="game-container">
        <canvas id="gameCanvas" width="300" height="600"></canvas>
        <div class="score-board">
            <p>Pontuação: <span id="score">0</span></p>
            <p>Nível: <span id="level">1</span></p>
            <p>Próxima peça:</p>
            <canvas id="nextPieceCanvas" width="100" height="100"></canvas>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Estilo CSS
CSS

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #000;
    color: #fff;
    font-family: Arial, sans-serif;
}

.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

canvas {
    border: 1px solid #fff;
}

.score-board {
    margin-top: 20px;
    text-align: center;
}
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Lógica JavaScript
JavaScript

const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const nextPieceCanvas = document.getElementById('nextPieceCanvas');
const nextCtx = nextPieceCanvas.getContext('2d');

const ROWS = 20;
const COLS = 10;
const BLOCK_SIZE = 30;

const SHAPES = [
    [[1, 1, 1, 1]], // I
    [[1, 1], [1, 1]], // O
    [[0, 1, 0], [1, 1, 1]], // T
    [[1, 1, 0], [0, 1, 1]], // S
    [[0, 1, 1], [1, 1, 0]], // Z
    [[1, 0, 0], [1, 1, 1]], // L
    [[0, 0, 1], [1, 1, 1]]  // J
];

let board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
let score = 0;
let level = 1;
let currentPiece = getRandomPiece();
let nextPiece = getRandomPiece();
let gameOver = false;

function getRandomPiece() {
    const shape = SHAPES[Math.floor(Math.random() * SHAPES.length)];
    return {
        shape,
        row: 0,
        col: Math.floor(COLS / 2) - Math.floor(shape[0].length / 2)
    };
}

function drawBoard() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (let row = 0; row < ROWS; row++) {
        for (let col = 0; col < COLS; col++) {
            if (board[row][col]) {
                ctx.fillStyle = 'cyan';
                ctx.fillRect(col * BLOCK_SIZE, row * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
                ctx.strokeRect(col * BLOCK_SIZE, row * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
            }
        }
    }
}

function drawPiece(piece, context = ctx) {
    context.fillStyle = 'cyan';
    piece.shape.forEach((row, rIdx) => {
        row.forEach((cell, cIdx) => {
            if (cell) {
                context.fillRect((piece.col + cIdx) * BLOCK_SIZE, (piece.row + rIdx) * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
                context.strokeRect((piece.col + cIdx) * BLOCK_SIZE, (piece.row + rIdx) * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
            }
        });
    });
}

function movePiece(dx, dy) {
    currentPiece.col += dx;
    currentPiece.row += dy;
    if (isCollision()) {
        currentPiece.col -= dx;
        currentPiece.row -= dy;
        return false;
    }
    return true;
}

function rotatePiece() {
    const shape = currentPiece.shape;
    const newShape = shape[0].map((_, idx) => shape.map(row => row[idx]).reverse());
    const oldShape = currentPiece.shape;
    currentPiece.shape = newShape;
    if (isCollision()) {
        currentPiece.shape = oldShape;
    }
}

function isCollision() {
    return currentPiece.shape.some((row, rIdx) => {
        return row.some((cell, cIdx) => {
            if (cell) {
                const newRow = currentPiece.row + rIdx;
                const newCol = currentPiece.col + cIdx;
                return newRow < 0 || newRow >= ROWS || newCol < 0 || newCol >= COLS || board[newRow][newCol];
            }
            return false;
        });
    });
}

function mergePiece() {
    currentPiece.shape.forEach((row, rIdx) => {
        row.forEach((cell, cIdx) => {
            if (cell) {
                board[currentPiece.row + rIdx][currentPiece.col + cIdx] = cell;
            }
        });
    });
}

function clearLines() {
    board = board.filter(row => row.some(cell => !cell));
    const clearedLines = ROWS - board.length;
    score += clearedLines * 10;
    level = Math.floor(score / 100) + 1;
    while (board.length < ROWS) {
        board.unshift(Array(COLS).fill(0));
    }
}

function updateScore() {
    document.getElementById('score').innerText = score;
    document.getElementById('level').innerText = level;
}

function gameLoop() {
    if (gameOver) return;
    if (!movePiece(0, 1)) {
        mergePiece();
        clearLines();
        currentPiece = nextPiece;
        nextPiece = getRandomPiece();
        if (isCollision()) {
            gameOver = true;
            alert('Game Over');
        }
    }
    drawBoard();
    drawPiece(currentPiece);
    drawPiece(nextPiece, nextCtx);
    updateScore();
    setTimeout(gameLoop, 1000 / level);
}

document.addEventListener('keydown', (e) => {
    if (gameOver) return;
    if (e.key === 'ArrowLeft') movePiece(-1, 0);
    if (e.key === 'ArrowRight') movePiece(1, 0);
    if (e.key === 'ArrowDown') movePiece(0, 1);
    if (e.key === 'ArrowUp') rotatePiece();
});

gameLoop();
Código gerado por IA. Examine e use com cuidado. Mais informações em perguntas frequentes.
Agora, o jogo deve ser jogável no navegador. Você pode mover as peças com as setas do teclado e rotacioná-las com a seta para cima. Se precisar de mais alguma coisa, é só falar! 😊

(imagem do jogo)
![image](https://github.com/user-attachments/assets/0655d57b-2244-439a-b4c4-f7d669cf8800)



-------------------------------------------------------CONCLUSÕES SOBRE O TESTE------------------------------------------------------------------------------

O teste inicial foi propositalmente vago, sem passar muitos detalhes, apenas uma imagem e a orientação de "recrie esse jogo para rodar no navegador", então a resposta foram os elementos Web (no caso não foi especificado em organização) porém deu um resultado interessante, pois os elementos se comunicavam e trouxeram algo similar em questão de estilo. Então o segundo fez um reajuste no código, trazendo um resultado ainda mais interessante.

Com poucas instruções o copilot recriou de forma parecida o jogo em questão, o que realmente torna isso interessante é que ele tem o contexto de qual jogo era, portanto foi criada a lógica do jogo apenas com a imagem e o contexto prévio do que seria o jogo pela ferramenta.
Já que poderia ser qualquer regra, porém ele seguiu a regra original do jogo sem ter especificações de como deve ser jogado, criando também a lógica para as peças e para a pontuação.

Esses outputs podem demonstrar a utilização de ferramentas de copiloto na programação orientada a jogos, trabalhando e auxiliando não só na parte de lógica estrutural da aplicação quanto tambémn no estilo, sendo um grande facilitador. Porém é importante ressaltar o uso como ferramenta, já que podemos ver que ela passou a orientação baseada na premissa do usuário saber sobre a criação de pastas e utilização.

Algo também interessante é a conversa amigável do copiloto que transforma a experiência em algo muito agradável e chamativo para os usuários.
É uma ferramenta muito poderosa e útil, com a utilização de diversos tipos de dados e a interface de fácil entendimento, pode ser de grande ajuda para quem necessita, e em diversos assuntos por ter o quesito generativo.
