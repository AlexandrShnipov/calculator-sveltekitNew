<script>
  import { buttons } from '$lib/calculator/Lets.js';
  
  let textInput = '';
  let previousAction = '';

  const inputContentAfterInput = (e) => {
    const input = e.target.textContent;
    const [lastNum, lastOperator] = textInput.match(/(-?\d*\.?\d+)(?!.*\d*\.?\d+)/g) || [null, null];
  
    if (input === '=') {
      if (lastOperator === '/' && lastNum === '0') {
        textInput = 'press AC, please';
      } else {
        const result = calculate(textInput);
        previousAction = textInput + '=';
        textInput = Number.isNaN(result) ? 'press AC, please' : result.toString();
      }
    } else if (input === '√') {
      previousAction = `${input}${lastNum} `;
      const result = Math.sqrt(Number(lastNum));
      textInput = Number.isNaN(result) ? 'press AC, please' : textInput.replace(/(-?\d*\.?\d+)(?!.*\d*\.?\d+)/g, result.toString());
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


  const calculate = (input) => {
    const operators = ["+", "-", "*", "/", "%"];
    let nums = input.split(new RegExp(`[${operators.join()}]`));
    let ops = input.split(/[0-9\.]+/).filter((val) => val !== "");
    
    for (let i = 0; i < ops.length; i++) {
      if (ops[i] === "√") {
        nums[i] = Math.sqrt(Number(nums[i]));
      } else if (ops[i] === "%") {
        nums[i] = Number(nums[i+1])* Number(nums[i]) / 100;
      } else if (ops[i] === "*") {
        nums.splice(i, 2, Number(nums[i]) * Number(nums[i+1]));
        ops.splice(i, 1);
        i--;
      } else if (ops[i] === "/") {
        if (Number(nums[i+1]) === 0) {
          return "Division by zero";
        }
        nums.splice(i, 2, Number(nums[i]) / Number(nums[i+1]));
        ops.splice(i, 1);
        i--;
      }
    }
    
    let result = Number(nums[0]);
    for (let i = 0; i < ops.length; i++) {
      if (ops[i] === "+") {
        result += Number(nums[i+1]);
      } else if (ops[i] === "-") {
        result -= Number(nums[i+1]);
      }
    }
    
    return result;
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