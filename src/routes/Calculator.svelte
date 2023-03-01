<script>
  import {buttons} from '$lib/calculator/Lets.js';
  
  let textInput = '';
  let previousAction = '';

  const inputContentAfterInput = (e) => {
    const input = e.target.textContent;
    const [firstNum, operator, secondNum] = textInput.split(/(\+|-|\*|\/|%|√)/);
    if (input === '=') {
      if (operator === '/' && secondNum === '0') {
        textInput = 'press AC, please';
      } else {
        const result = calculate(firstNum, operator, secondNum);
        previousAction = textInput;
        textInput = Number.isNaN(result) ? 'press AC, please' : result.toString();
      }
    } else if (input === '√') {
      previousAction = `${input}${textInput} `;
      const result = calculate(firstNum, input);
      textInput = Number.isNaN(result) ? 'press AC, please' : result.toString();
    } else if (input === 'AC') {
      textInput = '';
      previousAction = '';
    } else {
      if (textInput === 'press AC, please') {
        textInput = '';
      }
      if (textInput === '' && Number.isNaN(input)) {
        textInput = 'press AC, please';
      } else {
        textInput += input;
      }
    }
  };


  const calculate = (firstNum, operator, secondNum) => {
    switch (operator) {
      case '+':
        return Number(firstNum) + Number(secondNum);
      case '-':
        return Number(firstNum) - Number(secondNum);
      case '*':
        return Number(firstNum) * Number(secondNum);
      case '/':
        return Number(firstNum) / Number(secondNum);
      case '√':
        return Number(Math.sqrt(firstNum));
      case '%':
        return Number(secondNum) * Number(firstNum) / 100;
      default:
        if (operator.endsWith('√')) {
          const num = operator.slice(0, -1);
          return Number(num) * Math.sqrt(firstNum);
        }
        return NaN;
    }
  };


</script>

<div>
  <input type="text" bind:value={previousAction}>
  <input type="text" bind:value={textInput}>
  <ul>
    {#each buttons as button, i}
      <li key={button.id}>
        <button on:click={inputContentAfterInput}>
          {button.text}</button>
      </li>
    {/each}
  </ul>
</div>


<style>
  
  input {
    display: block;
    padding: 1rem;
    height: 2rem;
    margin-bottom: .3rem;
    font-size: 1.5rem;
    border: none;
    box-shadow: 0 0 6px;
    outline: transparent;
  }
  
  ul {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: .2rem;
    margin: 0;
    padding: 0;
  }
  
  li {
    list-style: none;
  }
  
  li:nth-child(16),
  li:nth-child(18) {
    grid-column: 1/4;
  }
  
  li:nth-child(17),
  li:nth-child(19) {
    grid-column: 4/6;
  }
  
  button {
    width: 100%;
    height: 4rem;
    font-size: 2rem;
    border-radius: 10%;
    border: none;
    box-shadow: 0 0 6px;
  }
  
  button:active {
    background-color: rgba(255, 100, 100, .2);
  }
</style>