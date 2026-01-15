<script setup lang="ts">
import { ref, defineComponent, h, provide, inject } from "vue";
import Card from "@/components/Card.vue";

class Calculator {
  private display: string = "0";
  private previousValue: number | null = null;
  private operation: string | null = null;
  private newNumber: boolean = true;

  getDisplay(): string {
    return this.display;
  }

  handleNumber(num: number): void {
    if (this.newNumber) {
      this.display = String(num);
      this.newNumber = false;
    } else {
      this.display = this.display === "0" ? String(num) : this.display + num;
    }
  }

  handleOperation(op: string): void {
    const current = parseFloat(this.display);

    if (this.previousValue === null) {
      this.previousValue = current;
    } else if (this.operation) {
      const result = this.calculate(this.previousValue, current, this.operation);
      this.display = String(result);
      this.previousValue = result;
    }

    this.operation = op;
    this.newNumber = true;
  }

  private calculate(prev: number, current: number, op: string): number {
    switch (op) {
      case "+":
        return prev + current;
      case "-":
        return prev - current;
      case "*":
        return prev * current;
      case "/":
        return prev / current;
      default:
        return current;
    }
  }

  handleEquals(): void {
    if (this.operation && this.previousValue !== null) {
      const result = this.calculate(this.previousValue, parseFloat(this.display), this.operation);
      this.display = String(result);
      this.previousValue = null;
      this.operation = null;
      this.newNumber = true;
    }
  }

  handleClear(): void {
    this.display = "0";
    this.previousValue = null;
    this.operation = null;
    this.newNumber = true;
  }

  handleDecimal(): void {
    if (!this.display.includes(".")) {
      this.display += ".";
      this.newNumber = false;
    }
  }
}

const calculator = new Calculator();
const display = ref(calculator.getDisplay());

const performAction = (action: () => void) => {
  action();
  display.value = calculator.getDisplay();
};

const handleButtonClick = (text: string) => {
  switch (text) {
    case "C":
      performAction(() => calculator.handleClear());
      break;
    case "=":
      performAction(() => calculator.handleEquals());
      break;
    case ".":
      performAction(() => calculator.handleDecimal());
      break;
    case "+":
    case "-":
    case "*":
    case "/":
      performAction(() => calculator.handleOperation(text));
      break;
    default:
      const num = parseInt(text);
      if (!isNaN(num)) {
        performAction(() => calculator.handleNumber(num));
      }
  }
};

provide("handleButtonClick", handleButtonClick);

const Button = defineComponent({
  props: {
    operator: { type: Boolean, default: false },
    rowSpan: { type: Number, default: 1 },
    colSpan: { type: Number, default: 1 },
  },
  setup(props, { slots }) {
    const handleButtonClick = inject("handleButtonClick") as (text: string) => void;

    return () => {
      const buttonText = slots.default?.()[0]?.children || "";

      return h(
        "button",
        {
          class: ["button", { operator: props.operator }],
          style: {
            gridRow: `span ${props.rowSpan}`,
            gridColumn: `span ${props.colSpan}`,
          },
          onClick: () => handleButtonClick(String(buttonText)),
        },
        slots.default?.()
      );
    };
  },
});
</script>

<template>
  <Card>
    <div class="calculator">
      <div class="display">{{ display }}</div>
      <div class="buttons">
        <Button operator>C</Button>
        <Button operator>/</Button>
        <Button operator>*</Button>
        <Button operator>-</Button>

        <Button>7</Button>
        <Button>8</Button>
        <Button>9</Button>
        <Button operator :row-span="2">+</Button>

        <Button>4</Button>
        <Button>5</Button>
        <Button>6</Button>

        <Button>1</Button>
        <Button>2</Button>
        <Button>3</Button>
        <Button operator :row-span="2">=</Button>

        <Button :col-span="2">0</Button>
        <Button>.</Button>
      </div>
    </div>
  </Card>
</template>

<style scoped>
.calculator {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.display {
  background: #f0f0f0;
  padding: 0.5rem 1rem;
  text-align: right;
  font-size: 2.5rem;
  font-weight: bold;
  border-radius: 4px;
  min-height: 3rem;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  text-shadow: 2px 2px 0px rgba(0, 0, 0, 0.1);
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(5, 1fr);
  gap: 0.5rem;
  aspect-ratio: 1.6;
}

.button {
  padding: 0;
  font-size: 1.5rem;
  font-weight: bold;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  background-color: #e0e0e0;
  color: #000;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.button:hover {
  background-color: #d0d0d0;
}

.button.operator {
  background-color: #ff9800;
  color: white;
}

.button.operator:hover {
  background-color: #fb8c00;
}
</style>
