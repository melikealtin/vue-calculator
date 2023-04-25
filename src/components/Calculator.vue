<template>
  <div class="calculator">
    <div class="display">{{ current || "0" }}</div>
    <div class="item">
      <div @click="clear" class="btn clear">C</div>
      <div @click="sign" class="btn gray">+/-</div>
      <div @click="percent" class="btn gray">%</div>
      <div @click="divide" class="btn operator">÷</div>
      <div @click="append('7')" class="btn">7</div>
      <div @click="append('8')" class="btn">8</div>
      <div @click="append('9')" class="btn">9</div>
      <div @click="times" class="btn operator">x</div>
      <div @click="append('4')" class="btn">4</div>
      <div @click="append('5')" class="btn">5</div>
      <div @click="append('6')" class="btn">6</div>
      <div @click="minus" class="btn operator">-</div>
      <div @click="append('1')" class="btn">1</div>
      <div @click="append('2')" class="btn">2</div>
      <div @click="append('3')" class="btn">3</div>
      <div @click="add" class="btn operator">+</div>
      <div @click="append('0')" class="btn zero">0</div>
      <div @click="dot" class="btn">.</div>
      <div @click="equal" class="btn blue">=</div>
    </div>
  </div>
</template>

<script>
import { create, all } from "mathjs";
const math = create(all);
export default {
  data() {
    return {
      previous: null,
      current: "",
      operator: null,
      operatorClicked: false,
    };
  },
  methods: {
    clear() {
      this.current = "";
    },
    sign() {
      this.current =
        this.current.charAt(0) === "-"
          ? this.current.slice(1)
          : `-${this.current}`;
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = "";
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    dot() {
      if (this.current.indexOf(".") === -1) {
        this.append(".");
      }
    },
    setPrevious() {
      this.previous = this.current;
      this.operatorClicked = true;
    },
    divide() {
      this.operator = (a, b) => {
        const ans = math.evaluate(`${a} / ${b}`);
        return math.format(ans, { precision: 14 });
      };
      this.setPrevious();
    },
    times() {
      this.operator = (a, b) => {
        const ans = math.evaluate(`${a} * ${b}`);
        return math.format(ans, { precision: 14 });
      };
      this.setPrevious();
    },
    minus() {
      this.operator = (a, b) => {
        const ans = math.evaluate(`${a} - ${b}`);
        return math.format(ans, { precision: 14 });
      };
      this.setPrevious();
    },
    add() {
      this.operator = (a, b) => {
        const ans = math.add(a, b);
        return math.format(ans, { precision: 14 });
      };
      this.setPrevious();
    },
    equal() {
      this.current = `${this.operator(
        parseFloat(this.previous),
        parseFloat(this.current)
      )}`;
      this.previous = null;
    },
  },
};
</script>

<style>
.calculator {
  width: 320px;
  background: #fff;
  border: 3px solid #e0e0e0;
  border-radius: 10px;
  margin-left: auto;
  margin-right: auto;
  padding: 40px 10px 10px 10px;
}
.display {
  border-top-color: #c0c0c0;
  border: 1px solid #d9d9d9;
  padding: 7px 10px 0 10px;
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 36px;
  margin-bottom: 10px;
}
.item {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}

.btn {
  height: 30px;
  width: 66px;
  border: 1px solid #d0d0d0;
  background-color: whiteSmoke;
  border-radius: 3px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000000;
  transition: 150ms transform ease-in-out;
}
.btn:hover {
  transform: scale(1.03);
}
.operator {
  background-color: #666;
  border-color: #444;
  color: #fff;
}
.clear {
  background-color: #d14836;
  border: 1px solid #d14836;
  color: #fff;
}
.gray {
  background-color: #d0d0d0;
  border: 1px solid #d0d0d0;
}
.blue {
  border: 1px solid #3079ed;
  color: #fff;
  width: 150px;
  background: #4f80d6;
}
</style>
