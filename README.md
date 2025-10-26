<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora OS</title>
  <style>
    body {
      background: #000;
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    .calculadora {
      background: #333;
      border-radius: 20px;
      width: 320px;
      padding: 20px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.7);
    }
    .display {
      background: #222;
      color: white;
      font-family: monospace;
      font-size: 2.5em;
      text-align: right;
      padding: 10px;
      border-radius: 10px;
      margin-bottom: 20px;
      min-height: 60px;
      word-wrap: break-word;
    }
    .botones {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      grid-gap: 12px;
    }
    button {
      font-size: 1.4em;
      border: none;
      border-radius: 10px;
      padding: 20px 0;
      color: #fff;
      background: #555;
      cursor: pointer;
      transition: background 0.2s;
    }
    button:hover {
      background: #666;
    }
    button.operador {
      background: #f90;
    }
    button.operador:hover {
      background: #e80;
    }
    button.clear {
      background: #d33;
    }
    button.clear:hover {
      background: #b22;
    }
    button.return {
      background: #777;
    }
    button.return:hover {
      background: #999;
    }
  </style>
</head>
<body>
  <div class="calculadora">
    <div class="display" id="display">0</div>
    <div class="botones">
      <button class="clear" id="btn-clear">C</button>
      <button class="return" id="btn-back">←</button>
      <button disabled></button>
      <button class="operador" id="btn-divide">÷</button>
      
      <button id="btn-7">7</button>
      <button id="btn-8">8</button>
      <button id="btn-9">9</button>
      <button class="operador" id="btn-mul">×</button>
      
      <button id="btn-4">4</button>
      <button id="btn-5">5</button>
      <button id="btn-6">6</button>
      <button class="operador" id="btn-sub">−</button>
      
      <button id="btn-1">1</button>
      <button id="btn-2">2</button>
      <button id="btn-3">3</button>
      <button class="operador" id="btn-add">+</button>
      
      <button id="btn-0">0</button>
      <button disabled></button>
      <button class="operador" id="btn-equals" style="grid-column: span 2;">=</button>
    </div>
  </div>

  <script>
    const displayEl = document.getElementById("display");
    let current = "0";
    let previous = null;
    let operator = null;
    let justCalculated = false;

    function updateDisplay() {
      displayEl.textContent = current;
    }

    function inputDigit(digit) {
      if (justCalculated) {
        current = digit;
        justCalculated = false;
      } else {
        current = current === "0" ? digit : current + digit;
      }
    }

    function clearAll() {
      current = "0";
      previous = null;
      operator = null;
      justCalculated = false;
    }

    function backspace() {
      if (justCalculated) {
        clearAll();
        updateDisplay();
        return;
      }
      if (current.length > 1) {
        current = current.slice(0, -1);
      } else {
        current = "0";
      }
    }

    function handleOperator(nextOperator) {
      const currNum = parseInt(current);
      if (previous === null) {
        previous = currNum;
      } else if (operator) {
        const result = operate(previous, currNum, operator);
        previous = result;
        current = String(result);
      }
      operator = nextOperator;
      justCalculated = false;
    }

    function operate(a, b, op) {
      switch(op) {
        case "+": return a + b;
        case "−": return a - b;
        case "×": return a * b;
        case "÷": return b !== 0 ? Math.floor(a / b) : "Err";
      }
      return b;
    }

    document.getElementById("btn-clear").addEventListener("click", () => {
      clearAll();
      updateDisplay();
    });
    document.getElementById("btn-back").addEventListener("click", () => {
      backspace();
      updateDisplay();
    });
    document.getElementById("btn-equals").addEventListener("click", () => {
      if (operator && previous !== null) {
        const result = operate(previous, parseInt(current), operator);
        current = String(result);
        previous = null;
        operator = null;
        justCalculated = true;
        updateDisplay();
      }
    });

    ["btn-0","btn-1","btn-2","btn-3","btn-4","btn-5","btn-6","btn-7","btn-8","btn-9"].forEach(id => {
      document.getElementById(id).addEventListener("click", (e) => {
        inputDigit(e.target.textContent);
        updateDisplay();
      });
    });

    document.getElementById("btn-add").addEventListener("click", () => { handleOperator("+"); updateDisplay(); });
    document.getElementById("btn-sub").addEventListener("click", () => { handleOperator("−"); updateDisplay(); });
    document.getElementById("btn-mul").addEventListener("click", () => { handleOperator("×"); updateDisplay(); });
    document.getElementById("btn-divide").addEventListener("click", () => { handleOperator("÷"); updateDisplay(); });

    updateDisplay();
  </script>
</body>
</html>
