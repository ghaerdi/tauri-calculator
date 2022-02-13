<script lang="ts">
  let display = "0";

  const add = (n1: number, n2: number) => n1 + n2;
  const diff = (n1: number, n2: number) => n1 - n2;
  const mult = (n1: number, n2: number) => n1 * n2;
  const div = (n1: number, n2: number) => n1 / n2;

  document.onkeydown = (e) => {
    const validKeyCodes = [
      46, 111, 107, 103, 104, 105, 109, 100, 101, 102, 106, 97, 98, 99, 96, 110,
      13,
    ];

    const index = validKeyCodes.findIndex(el => el === e.which);

    if (index !== -1) {
      const btn = document.querySelectorAll(".buttons button") as NodeListOf<HTMLButtonElement>;
      btn[index].click();
    };
  };

  function updateDisplay(e: Event) {
    const value = (e.target as HTMLButtonElement).innerHTML;
    const operations = /(\+|\-|\*|\/)/;

    if (display === "0" && !value.match(operations) && value !== "." || display === "NaN") {
      display = value;
    } else {
      if (display.match(`${operations.source}$`) && value.match(operations)) {
        display = display.slice(0, -1);
      }

      if (display.match(/^\d+(\+|\-|\*|\/)\d+$/) && value.match(operations)) {
        const [n1, n2] = display.match(/\d+/g);
        const operator = display.match(operations).at(0);
        display = calculate(parseInt(n1), parseInt(n2), operator);
      }

      display += value;
    }
  }

  function calculateOnSubmit() {
    const operations = /(\+|\-|\*|\/)/;
    const [n1, n2] = display.match(/[\d.]+/g);
    const operator = display.match(operations).at(0);
    display = calculate(parseFloat(n1), parseFloat(n2), operator);
  }

  function clearDisplay() {
    display = "0";
  }

  function calculate(n1: number, n2: number, operator: string): string {
    const operations = {
      "+": add,
      "-": diff,
      "*": mult,
      "/": div,
    };

    return String(operations[operator](n1, n2));
  }

  interface CalcButton {
    value: string;
    className?: string;
  }

  const buttons: CalcButton[] = [
    {
      value: "/",
    },
    {
      value: "+",
    },
    {
      value: "7",
    },
    {
      value: "8",
    },
    {
      value: "9",
    },
    {
      value: "-",
    },
    {
      value: "4",
    },
    {
      value: "5",
    },
    {
      value: "6",
    },
    {
      value: "*",
    },
    {
      value: "1",
    },
    {
      value: "2",
    },
    {
      value: "3",
    },
    {
      value: "0",
      className: "zero",
    },
    {
      value: ".",
    },
  ];
</script>

<form class="calculator" on:submit|preventDefault={calculateOnSubmit}>
  <input disabled class="display" value={display} />
  <div class="buttons">
    <button type="button" class="delete" on:click={clearDisplay}>CE</button>

    {#each buttons as { value, className }}
      <button type="button" class={className || ""} on:click={updateDisplay}
        >{value}</button
      >
    {/each}

    <button type="submit" class="result">=</button>
  </div>
</form>

<style>
  .calculator {
    font-size: 1.5rem;
  }

  .result {
    background-color: rgb(122, 122, 99) !important;
    grid-column: 4;
    grid-row: 4 / 6;
  }

  .display {
    width: 100%;
    height: 20%;
    font-size: 3rem;
    text-align: end;
    margin: 0;
    background-color: transparent;
    border-color: transparent;
  }

  .delete,
  .zero {
    grid-column: 1/ 3;
  }

  .result {
    background-color: white;
  }

  .buttons {
    display: grid;
    gap: 5px;
    grid-template-columns: repeat(4, 1fr);
    height: 80%;
  }

  .buttons > button {
    cursor: pointer;
    border-radius: 6px;
    background-color: black;
    color: white;
    height: 100%;
    border-color: transparent;
    box-shadow: rgba(255, 255, 255, 0.3) 0px 1px 2px 0px,
      rgba(255, 255, 255, 0.15) 0px 2px 6px 2px;
  }

  .buttons button:active {
    background-color: rgb(39, 39, 39);
  }

  .calculator {
    height: 100vh;
    width: 100vw;
  }
</style>
