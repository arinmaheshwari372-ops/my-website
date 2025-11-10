<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Calculator</title>


<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(135deg, #74ebd5, #ACB6E5);
    margin: 0;
  }

  .calculator {
    background-color: #f3f3f3;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    width: 340px;
  }

  .display {
    background-color: #222;
    color: #fff;
    font-size: 2em;
    text-align: right;
    padding: 10px;
    border-radius: 10px;
    margin-bottom: 15px;
    word-wrap: break-word;
  }

  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }

  button {
    padding: 20px;
    font-size: 1.2em;
    border: none;
    border-radius: 10px;
    background-color: #ddd;
    cursor: pointer;
    transition: 0.2s;
  }

  button:hover {
    background-color: #ccc;
  }

  .operator {
    background-color: #f9a825;
    color: white;
  }

  .operator:hover {
    background-color: #f57f17;
  }

  .equal {
    background-color: #00c853;
    color: white;
    grid-column: span 2;
  }

  .equal:hover {
    background-color: #009624;
  }

  .clear {
    background-color: #d50000;
    color: white;
  }

  .clear:hover {
    background-color: #9b0000;
  }

  .remove {
    background-color: #ff6d00;
    color: white;
  }

  .remove:hover {
    background-color: #e65100;
  }
</style>
</head>
<body>
<div class="calculator">
  <div id="display" class="display">0</div>
  <div class="buttons">
    <button class="clear" onclick="clearDisplay()">C</button>
    <button class="remove" onclick="removeLast()">⌫</button>
    <button onclick="appendNumber('(')">(</button>
    <button onclick="appendNumber(')')">)</button>

    <button onclick="appendNumber('7')">7</button>
    <button onclick="appendNumber('8')">8</button>
    <button onclick="appendNumber('9')">9</button>
    <button class="operator" onclick="appendOperator('×')">×</button>

    <button onclick="appendNumber('4')">4</button>
    <button onclick="appendNumber('5')">5</button>
    <button onclick="appendNumber('6')">6</button>
    <button class="operator" onclick="appendOperator('-')">-</button>

    <button onclick="appendNumber('1')">1</button>
    <button onclick="appendNumber('2')">2</button>
    <button onclick="appendNumber('3')">3</button>
    <button class="operator" onclick="appendOperator('+')">+</button>

    <button onclick="appendNumber('0')">0</button>
    <button onclick="appendNumber('.')">.</button>
    <button class="equal" onclick="calculateResult()">=</button>
  </div>
</div>

<script>
  let display = document.getElementById('display');
  let currentInput = '';

  function appendNumber(number) {
    currentInput += number;
    display.textContent = currentInput;
  }

  function appendOperator(operator) {
    if(currentInput === '') return;
    const lastChar = currentInput.slice(-1);
    if('+-×*/'.includes(lastChar)) return;
    currentInput += operator;
    display.textContent = currentInput;
  }

  function clearDisplay() {
    currentInput = '';
    display.textContent = '0';
  }

  function removeLast() {
    currentInput = currentInput.slice(0, -1);
    display.textContent = currentInput || '0';
  }

  function calculateResult() {
    try {
      let expression = currentInput
        .replace(/×/g, '*')
        .replace(/(\d)(\()/g, '$1*(')
        .replace(/(\))(\d)/g, ')*$2');
      currentInput = eval(expression).toString();
      display.textContent = currentInput;
    } catch (e) {
      display.textContent = 'Error';
      currentInput = '';
    }
  }

  // Keyboard support
  document.addEventListener('keydown', function(event) {
    const key = event.key;
    if(/\d/.test(key)) {
      appendNumber(key);
    } else if(key === '(' || key === ')') {
      appendNumber(key);
    } else if(key === '+' || key === '-' || key === '/') {
      appendOperator(key);
    } else if(key === '*' || key === 'x' || key === 'X') {
      appendOperator('×');
    } else if(key === 'Backspace') {
      removeLast();
    } else if(key === 'Enter' || key === '=') {
      calculateResult();
    } else if(key === 'c' || key === 'C') {
      clearDisplay();
    } else if(key === '.') {
      appendNumber('.');
    }
  });
</script>
</body>
</html>
