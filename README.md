# Calculadora-NyxTech
Calculadora Funcional Feita Com JavaScript, Html, e Css
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>NyxTech Calculator</title>

<style>
* { box-sizing: border-box; }

body {
  margin: 0;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;

  background-image: url("https://img.freepik.com/fotos-gratis/piso-de-madeira-preto_53876-89522.jpg?w=740");
  background-size: cover;
  background-position: center;
}

.calculadora-wrapper {
  position: relative;
}

.calculadora-wrapper::after {
  content: "";
  position: absolute;
  bottom: -25px;
  left: 50%;
  transform: translateX(-50%);
  width: 80%;
  height: 25px;
  background: radial-gradient(ellipse, rgba(0,0,0,0.5), transparent);
  filter: blur(6px);
}

.calculadora {
  width: 280px;
  padding: 18px;
  border-radius: 22px;
  background: linear-gradient(145deg, #2a2e36, #1c1f26);
  box-shadow:
    inset 0 2px 4px rgba(255,255,255,0.08),
    inset 0 -3px 6px rgba(0,0,0,0.6),
    0 20px 40px rgba(0,0,0,0.8);
}

.marca {
  color: #cfcfcf;
  font-size: 14px;
  text-align: right;
  margin-bottom: 6px;
  letter-spacing: 1px;
}

#display {
  width: 100%;
  height: 70px;
  border-radius: 10px;
  border: 3px solid #5f705f;
  background:
    linear-gradient(180deg, rgba(255,255,255,0.15), transparent),
    linear-gradient(#a9c0a0, #8ea784);
  box-shadow:
    inset 0 0 12px rgba(0,0,0,0.6),
    inset 0 3px 5px rgba(255,255,255,0.15);
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 10px;
  margin-bottom: 14px;
}

#display::before {
  content: "";
  position: absolute;
  top: 5px;
  left: 10px;
  width: 60%;
  height: 40%;
  background: linear-gradient(to bottom, rgba(255,255,255,0.25), transparent);
  border-radius: 10px;
}

#displayText {
  white-space: nowrap;
  font-family: "Courier New", monospace;
  color: #1b2b1b;
  font-size: 34px;
  transform-origin: right center;
}

.botoes {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
}

button {
  height: 45px;
  border-radius: 10px;
  border: none;
  font-size: 15px;
  color: #fff;
  cursor: pointer;
  background: linear-gradient(145deg, #4a4f5c, #2c303a);
  box-shadow:
    inset 0 2px 2px rgba(255,255,255,0.1),
    inset 0 -3px 4px rgba(0,0,0,0.6),
    0 4px 8px rgba(0,0,0,0.8);
}

button:active {
  transform: translateY(3px);
  box-shadow: inset 0 4px 6px rgba(0,0,0,0.8);
}

.operador { background: linear-gradient(145deg, #6b7280, #4a4f5c); }
.memoria { background: linear-gradient(145deg, #7b8794, #5a6470); font-size: 12px; }
.limpar  { background: linear-gradient(145deg, #ff5f52, #c0392b); }
.igual   { background: linear-gradient(145deg, #5dd36c, #3aa84f); }

.copyright {
  position: fixed;
  bottom: 10px;
  left: 15px;
  color: rgba(255,255,255,0.7);
  font-size: 13px;
  font-family: Arial, sans-serif;
  letter-spacing: 0.5px;
  text-shadow: 0 1px 3px rgba(0,0,0,0.7);
}
</style>
</head>

<body>

<div class="calculadora-wrapper">
  <div class="calculadora">
    <div class="marca">NyxTech</div>

    <div id="display">
      <div id="displayText">0</div>
    </div>

    <div class="botoes">
      <button class="memoria" onclick="mMais()">M+</button>
      <button class="memoria" onclick="mMenos()">M-</button>
      <button class="memoria" onclick="mRecall()">MR</button>
      <button class="memoria" onclick="mClear()">MC</button>
      <button class="limpar" onclick="limpar()">CE</button>

      <button onclick="add('7')">7</button>
      <button onclick="add('8')">8</button>
      <button onclick="add('9')">9</button>
      <button class="operador" onclick="add('÷')">÷</button>
      <button class="operador" onclick="add('%')">%</button>

      <button onclick="add('4')">4</button>
      <button onclick="add('5')">5</button>
      <button onclick="add('6')">6</button>
      <button class="operador" onclick="add('×')">×</button>
      <button class="operador" onclick="add('-')">−</button>

      <button onclick="add('1')">1</button>
      <button onclick="add('2')">2</button>
      <button onclick="add('3')">3</button>
      <button class="operador" onclick="add('+')">+</button>
      <button class="igual" onclick="calc()">=</button>

      <button onclick="add('0')">0</button>
      <button onclick="add('00')">00</button>
      <button onclick="add('.')">.</button>
    </div>
  </div>
</div>

<div class="copyright">
  © Todos os Direitos Reservados para Nyxstarzx
</div>

<script>
const display = document.getElementById("display");
const textEl = document.getElementById("displayText");
let memoria = 0;

function fitText() {
  const maxWidth = display.clientWidth - 20;
  textEl.style.transform = "scale(1)";
  let width = textEl.scrollWidth;

  if (width > maxWidth) {
    let scale = maxWidth / width;
    textEl.style.transform = `scale(${scale})`;
  }
}

function setText(val) {
  let t = String(val);
  if (t.length > 20) t = t.slice(-20);
  textEl.textContent = t;
  fitText();
}

function add(v) {
  if (textEl.textContent === "0") setText(v);
  else setText(textEl.textContent + v);
}

function limpar() { setText("0"); }

function calc() {
  try {
    let expr = textEl.textContent
      .replace(/×/g, '*')
      .replace(/÷/g, '/');
    setText(eval(expr));
  } catch {
    setText("Erro");
  }
}

function mMais(){ memoria += Number(textEl.textContent)||0 }
function mMenos(){ memoria -= Number(textEl.textContent)||0 }
function mRecall(){ setText(memoria) }
function mClear(){ memoria = 0 }

document.addEventListener("keydown", e => {
  const k = e.key;
  if (!isNaN(k) || "+-*/.".includes(k)) add(k);
  if (k === "Enter") calc();
  if (k === "Backspace") setText(textEl.textContent.slice(0,-1));
  if (k === "Escape") limpar();
});
</script>

</body>
</html>
