<script>
  let num1 = '';
  let num2 = '';
  let result = 0;
  let operation = '+';
  let error = '';
  let history = [];

  $: {
    const n1 = parseFloat(num1) || 0;
    const n2 = parseFloat(num2) || 0;
    error = '';
    
    try {
      switch (operation) {
        case '+': 
          result = n1 + n2; 
          break;
        case '-': 
          result = n1 - n2; 
          break;
        case '*': 
          result = n1 * n2; 
          break;
        case '/': 
          if (n2 === 0 && num2 !== '') {
            error = 'Cannot divide by zero';
            result = 0;
          } else {
            result = n2 !== 0 ? n1 / n2 : 0;
          }
          break;
      }
    } catch (e) {
      error = 'Calculation error';
      result = 0;
    }
  }

  function addToHistory() {
    if (num1 && num2 && !error) {
      const n1 = parseFloat(num1);
      const n2 = parseFloat(num2);
      const opSymbol = operation === '*' ? '×' : operation === '/' ? '÷' : operation;
      history = [`${n1} ${opSymbol} ${n2} = ${Math.round(result * 10000) / 10000}`, ...history].slice(0, 5);
    }
  }

  function clearAll() {
    num1 = '';
    num2 = '';
    result = 0;
    operation = '+';
    error = '';
  }

  function clearHistory() {
    history = [];
  }
</script>

<main class="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900 flex items-center justify-center p-4">
  <div class="calculator-container">
    <div class="calculator-box">
      <div class="calculator-header">
        <h1 class="title">Calculator</h1>
      </div>
      
      <div class="calculator-content">
        <div class="input-grid">
          <div class="input-group">
            <label for="num1" class="input-label">First Number</label>
            <input 
              id="num1"
              type="number" 
              bind:value={num1}
              class="input-field"
              placeholder="0"
            />
          </div>

          <div class="input-group">
            <label for="num2" class="input-label">Second Number</label>
            <input 
              id="num2"
              type="number" 
              bind:value={num2}
              class="input-field"
              placeholder="0"
            />
          </div>
        </div>

        <fieldset class="operation-section">
          <legend class="input-label">Operation</legend>
          <div class="operation-grid">
            <button 
              on:click={() => operation = '+'}
              class="operation-btn {operation === '+' ? 'active blue' : ''}"
            >
              +
            </button>
            <button 
              on:click={() => operation = '-'}
              class="operation-btn {operation === '-' ? 'active green' : ''}"
            >
              -
            </button>
            <button 
              on:click={() => operation = '*'}
              class="operation-btn {operation === '*' ? 'active purple' : ''}"
            >
              *
            </button>
            <button 
              on:click={() => operation = '/'}
              class="operation-btn {operation === '/' ? 'active pink' : ''}"
            >
              ÷
            </button>
          </div>
        </fieldset>

        <div class="result-container">
          <div class="result-glow"></div>
          <div class="result-box">
            {#if error}
              <div class="error-text">
                <span>{error}</span>
              </div>
            {:else}
              <p class="result-label">RESULT</p>
              <div class="result-value">
                {Math.round(result * 10000) / 10000}
              </div>
            {/if}
          </div>
        </div>

        <div class="action-buttons">
          <button 
            on:click={addToHistory}
            class="action-btn save-btn"
          >
            Save
          </button>
          <button 
            on:click={clearAll}
            class="action-btn clear-btn"
          >
            Clear
          </button>
        </div>

        {#if history.length > 0}
          <div class="history-section">
            <div class="history-header">
              <h2 class="history-title">
                <span>History</span>
              </h2>
              <button 
                on:click={clearHistory}
                class="history-clear-btn"
              >
                Clear
              </button>
            </div>
            <div class="history-list">
              {#each history as entry}
                <div class="history-item">
                  {entry}
                </div>
              {/each}
            </div>
          </div>
        {/if}
      </div>
    </div>
  </div>
</main>


<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .calculator-container {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    max-width: 500px;
    margin: 0 auto;
  }

  .calculator-box {
    width: 100%;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(20px);
    border-radius: 32px;
    padding: 40px;
    box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5),
                0 0 0 1px rgba(255, 255, 255, 0.2);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .calculator-header {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 16px;
    margin-bottom: 32px;
  }

  .icon-box {
    width: 56px;
    height: 56px;
    background: linear-gradient(135deg, #60a5fa 0%, #a855f7 100%);
    border-radius: 16px;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 10px 25px -5px rgba(168, 85, 247, 0.4);
  }

  .icon {
    font-size: 28px;
  }

  .title {
    font-size: 3rem;
    font-weight: 800;
    background: linear-gradient(90deg, #60a5fa 0%, #a855f7 50%, #ec4899 100%);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    letter-spacing: -0.02em;
  }

  .calculator-content {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .input-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  .input-group {
    display: flex;
    flex-direction: column;
  }

  .input-label {
    color: rgba(255, 255, 255, 0.9);
    font-size: 0.875rem;
    font-weight: 500;
    margin-bottom: 8px;
  }

  .input-field {
    width: 100%;
    padding: 16px;
    background: rgba(255, 255, 255, 0.15);
    border: 2px solid rgba(255, 255, 255, 0.2);
    border-radius: 12px;
    color: white;
    font-size: 1.25rem;
    outline: none;
    transition: all 0.3s ease;
  }

  .input-field::placeholder {
    color: rgba(255, 255, 255, 0.4);
  }

  .input-field:focus {
    border-color: #a855f7;
    box-shadow: 0 0 0 3px rgba(168, 85, 247, 0.3);
  }

  .input-field:hover {
    border-color: rgba(255, 255, 255, 0.3);
  }

  .operation-section {
    border: none;
  }

  .operation-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
  }

  .operation-btn {
    padding: 16px;
    background: rgba(255, 255, 255, 0.1);
    border: none;
    border-radius: 12px;
    color: rgba(255, 255, 255, 0.8);
    font-size: 1.5rem;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .operation-btn:hover {
    background: rgba(255, 255, 255, 0.2);
    transform: translateY(-2px);
  }

  .operation-btn.active {
    color: white;
    transform: scale(1.05);
    box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.3);
  }

  .operation-btn.active.blue {
    background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
  }

  .operation-btn.active.green {
    background: linear-gradient(135deg, #10b981 0%, #059669 100%);
  }

  .operation-btn.active.purple {
    background: linear-gradient(135deg, #a855f7 0%, #9333ea 100%);
  }

  .operation-btn.active.pink {
    background: linear-gradient(135deg, #ec4899 0%, #db2777 100%);
  }

  .result-container {
    position: relative;
    overflow: hidden;
  }

  .result-glow {
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, 
      rgba(59, 130, 246, 0.2) 0%,
      rgba(168, 85, 247, 0.2) 50%,
      rgba(236, 72, 153, 0.2) 100%);
    filter: blur(40px);
  }

  .result-box {
    position: relative;
    text-align: center;
    padding: 32px;
    background: linear-gradient(135deg, 
      rgba(255, 255, 255, 0.1) 0%,
      rgba(255, 255, 255, 0.05) 100%);
    border-radius: 16px;
    border: 2px solid rgba(255, 255, 255, 0.2);
    backdrop-filter: blur(10px);
  }

  .error-text {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    font-size: 1.875rem;
    font-weight: 700;
    color: #f87171;
    animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
  }

  .result-label {
    font-size: 0.875rem;
    color: rgba(255, 255, 255, 0.6);
    font-weight: 500;
    letter-spacing: 0.1em;
    margin-bottom: 8px;
  }

  .result-value {
    font-size: 3.75rem;
    font-weight: 900;
    background: linear-gradient(90deg, #60a5fa 0%, #a855f7 50%, #ec4899 100%);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .action-buttons {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  .action-btn {
    padding: 16px;
    border: none;
    border-radius: 12px;
    color: white;
    font-weight: 700;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3);
  }

  .action-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.3);
  }

  .action-btn:active {
    transform: scale(0.95);
  }

  .save-btn {
    background: linear-gradient(90deg, #10b981 0%, #14b8a6 100%);
  }

  .save-btn:hover {
    background: linear-gradient(90deg, #059669 0%, #0d9488 100%);
  }

  .clear-btn {
    background: linear-gradient(90deg, #475569 0%, #334155 100%);
  }

  .clear-btn:hover {
    background: linear-gradient(90deg, #334155 0%, #1e293b 100%);
  }

  .history-section {
    margin-top: 8px;
    padding: 20px;
    background: rgba(0, 0, 0, 0.2);
    border-radius: 16px;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .history-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 12px;
  }

  .history-title {
    color: rgba(255, 255, 255, 0.8);
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .history-clear-btn {
    font-size: 0.75rem;
    color: rgba(255, 255, 255, 0.6);
    background: rgba(255, 255, 255, 0.1);
    border: none;
    padding: 6px 12px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .history-clear-btn:hover {
    color: rgba(255, 255, 255, 0.9);
    background: rgba(255, 255, 255, 0.2);
  }

  .history-list {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .history-item {
    color: rgba(255, 255, 255, 0.7);
    font-size: 0.875rem;
    font-family: 'Courier New', monospace;
    background: rgba(255, 255, 255, 0.05);
    padding: 12px;
    border-radius: 8px;
    transition: all 0.3s ease;
  }

  .history-item:hover {
    background: rgba(255, 255, 255, 0.1);
  }

  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    opacity: 0.5;
  }

  input[type="number"]:hover::-webkit-inner-spin-button,
  input[type="number"]:hover::-webkit-outer-spin-button {
    opacity: 1;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.7; }
  }

  @media (max-width: 640px) {
    .calculator-box {
      padding: 24px;
    }

    .title {
      font-size: 2rem;
    }

    .input-grid {
      grid-template-columns: 1fr;
    }

    .result-value {
      font-size: 2.5rem;
    }
  }
</style>
