<template>
  <table class="calcualtor">
    <tr>
      <td>{{ currentCount }}</td>
    </tr>
    <tr>
      <td @click="clear">AC</td>
      <td @click="setNegativeOrPositive">+/-</td>
      <td @click="percentage">%</td>
      <td @click="operationProcess('divide')">÷</td>
    </tr>
    <tr>
      <td @click="addCount(7)">7</td>
      <td @click="addCount(8)">8</td>
      <td @click="addCount(9)">9</td>
      <td @click="operationProcess('multiply')">×</td>
    </tr>
    <tr>
      <td @click="addCount(4)">4</td>
      <td @click="addCount(5)">5</td>
      <td @click="addCount(6)">6</td>
      <td @click="operationProcess('substract')">-</td>
    </tr>
    <tr>
      <td @click="addCount(1)">1</td>
      <td @click="addCount(2)">2</td>
      <td @click="addCount(3)">3</td>
      <td @click="operationProcess('add')">+</td>
    </tr>
    <tr>
      <td @click="addCount(0)" colspan="2">0</td>
      <td @click="addDot">.</td>
      <td @click="equal">=</td>
    </tr>
  </table>
</template>

<script>
import { ref, watch } from "vue";
export default {
  name: "Calcualtor",
  setup() {
    const currentCount = ref("0");
    const prevCount = ref(null);
    const operation = ref(null);
    const operationClicked = ref(false);

    const clear = () => {
      currentCount.value = "0";
      prevCount.value = "";
      operationClicked.value = false;
    };

    const addCount = (num) => {
      if (operationClicked.value) {
        currentCount.value = "";
      }
      currentCount.value === "0"
        ? (currentCount.value = `${num}`)
        : (currentCount.value = `${currentCount.value}${num}`);
    };

    const setNegativeOrPositive = () => {
      if (currentCount.value === "0") return;
      currentCount.value =
        currentCount.value[0] === "-"
          ? currentCount.value.slice(1)
          : `-${currentCount.value}`;
    };

    const addDot = () => {
      if (currentCount.value.indexOf(".") !== -1) return;
      currentCount.value = `${currentCount.value}.`;
    };

    const percentage = () => {
      currentCount.value = `${parseFloat(currentCount.value) / 100}`;
    };

    const operationProcess = (str) => {
      if (str === "add") {
        operation.value = (a, b) => {
          return parseFloat(a) + parseFloat(b);
        };
      } else if (str === "divide") {
        operation.value = (a, b) => {
          return parseFloat(a) / parseFloat(b);
        };
      } else if (str === "substract") {
        operation.value = (a, b) => {
          return parseFloat(a) - parseFloat(b);
        };
      } else if (str === "multiply") {
        operation.value = (a, b) => {
          return parseFloat(a) * parseFloat(b);
        };
      }

      if (operationClicked.value) {
        currentCount.value = `${operation.value(
          prevCount.value,
          currentCount.value
        )}`;
      }

      operationClicked.value = true;
      prevCount.value = currentCount.value;
    };

    const equal = () => {
      operationClicked.value = false;
      if (prevCount.value === "") {
        return;
      } else {
        currentCount.value = `${operation.value(
          prevCount.value,
          currentCount.value
        )}`;
        prevCount.value = "";
      }
    };

    watch(currentCount, (curVal) => {
      console.log(curVal.length);
      if (curVal.length > 12) {
        currentCount.value = curVal.slice(0, 12);
      }
    });

    return {
      currentCount,
      prevCount,
      operation,
      operationClicked,
      clear,
      addCount,
      percentage,
      setNegativeOrPositive,
      addDot,
      operationProcess,
      equal,
    };
  },
};
</script>

<style scoped>
.calcualtor {
  display: flex;
  flex-direction: column;
  width: 400px;
  height: 600px;
  background: #f4f6f9;
  border-radius: 15px;
  box-shadow: 2px 2px 3px 3px rgba(29, 27, 27, 0.267);
}
tr:first-child {
  height: 120px;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
}
tr:first-child td {
  width: fit-content;
  text-align: end;
  font-size: 48px;
  cursor: initial;
  white-space: nowrap;
}
tr {
  height: calc(100% - 120px);
  padding: 15px;
  background: #eee;
  border-radius: 15px;
  display: flex;
}
td {
  flex: 1;
  width: 0;
  align-self: center;
  padding: 15px;
  font-size: 24px;
  border-radius: 15px;
  box-shadow: 0 0 6px 2px rgba(148, 148, 148, 0.4);
  margin: 0 15px;
  text-align: center;
  cursor: pointer;
  font-weight: bold;
}
</style>
