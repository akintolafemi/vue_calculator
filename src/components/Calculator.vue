<template>
  <div class="calculator">
    <div class="screen">
      <div class="title">
        <p>Team Hypertext Calculator App</p>
      </div>
      <p class="response">
        {{ current || '0' }}
      </p>
    </div>
    <div class="buttons">
      <div class="operators">
        <button @click="doClear">C</button>
        <button @click="doDel">D</button>
        <button @click="doSym">+/-</button>
        <button @click="doPerc">%</button>
        <button @click="doAdd">+</button>
        <button @click="doSub">-</button>
        <button @click="doMul">*</button>
        <button @click="doDiv">/</button>
      </div>
      <div class="numbers">
        <button @click="append('9')" value="9">9</button>
        <button @click="append('8')" value="8">8</button>
        <button @click="append('7')" value="7">7</button>
        <button @click="append('6')" value="6">6</button>
        <button @click="append('5')" value="5">5</button>
        <button @click="append('4')" value="4">4</button>
        <button @click="append('3')" value="3">3</button>
        <button @click="append('2')" value="2">2</button>
        <button @click="append('1')" value="1">1</button>
        <button @click="dot" value=".">.</button>
        <button @click="append('0')" value="0">0</button>
        <button @click="calculate">=</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
      current: '',
      previous: '',
      operator: null,
      operatorClicked: false,
    }
  },
  methods: {
    doClear: function() {
      this.current = '';
    },
    doDel: function() {
      this.current = this.current.slice(0, -1);
    },
    doSym() {
      this.current = this.current.charAt(0) === '-' ?
        this.current.slice(1) : `-${this.current}`;
    },
    doPerc() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    dot() {
      if (this.current.indexOf('.') === -1) {
        this.append('.');
      }
    },
    doAdd() {
      this.operator = (a, b) => a + b;
      this.setPrevious();
    },
    doSub() {
      this.operator = (a, b) => b - a;
      this.setPrevious();
    },
    doMul() {
      this.operator = (a, b) => a * b;
      this.setPrevious();
    },
    doDiv() {
      this.operator = (a, b) => a / b;
      this.setPrevious();
    },
    break() {
      switch (this.current.length) {
        case 13:
          this.current = this.current + '\n';
          break;
        case 25:
          this.current = this.current + '\n';
          break;
        default:
          break;
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    calculate() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )}`;
      this.previous = null;
    },
    append: function(number) {
      this.break();
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    width: 320px;
    height: 620px;
    background-color: #FFF;
    margin: 0 auto;
    border: 1px solid #EEE;
    border-radius: 5px;
  }

  .screen {
    height: 200px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 10px 0 rgba(0, 0, 0, 0.19);
  }

  .title {
    height: 20px;
    padding: 5px 10px 0 10px;
    font-size: 12px;
    color: #444;
    font-family: monospace;
  }

  .response {
    width: 280px;
    height: 180px;
    font-size: 40px;
    padding-right: 10px;
    padding-left: 10px;
    text-align: right;
  }

  .buttons {
    height: 420px;
    padding-bottom: 20px;
  }

  .numbers {
    width: 250px;
    height: 400px;
    padding: 20px 11px 20px 11px;
  }

  .operators {
    float: right;
    text-align: center;
    width: 70px;
    height: 400px;
    border-left: 1px solid rgba(0, 0, 0, 0.19);
  }

  .operators button {
    width: 55px;
    height: 38px;
    margin-top: 7px;
    margin-bottom: 7px;
    font-size: 14px;
  }

  .numbers button {
    width: 76px;
    height: 76px;
    margin-top: 7px;
    margin-bottom: 7px;
    font-size: 20px;
  }

  .numbers button, .operators button {
    border-style: none;
    background-color: #FFF;
  }

  .numbers button:hover, .operators button:hover {
    background-color: #BF1463;
  }

  .numbers button:active, .operators button:active {
    background-color: #EEE;
  }
</style>
