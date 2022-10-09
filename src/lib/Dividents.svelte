<script lang="ts">
  let initialValue = 5000;
  let dividentRate = 4;
  let montlyAddition = 500;
  
  function handleCalc(){
    // console.log("handle")
    // for (let i = 0;i<12;i++){
    //   rows.push({index:i+1})
    // }
    // console.log("handle",rows)
    // rows = [...rows];
  }

  $:rows = calculate(initialValue,dividentRate,montlyAddition);

  function calculate(initialValue,dividentRate,montlyAddition) {
      console.log("calc")
      let rows = [];
      let valueCalc = initialValue;
      let amount = initialValue;
      let earnValue = 0
    for (let i = 0;i<12;i++){
        earnValue =parseInt( amount * dividentRate / 100);
        rows.push({index:i+1,earnValue:Math.round(earnValue),amount:Math.round(amount)});
        amount =parseInt( parseInt(amount) + earnValue + montlyAddition);
      }
      return rows;
  }

  $:totalEarn = calculateTotalEarn(rows);

  function calculateTotalEarn(rows) {
    if (!rows) return 0;
    let total = 0;
    rows.forEach(row =>{
      total = total + row.earnValue;
    })
    return total;
  }
  
</script>
<template>
  <div>
    <input bind:value={initialValue}/>
    <input bind:value={dividentRate}/>
    <input bind:value={montlyAddition}/>
    <button on:click={handleCalc} >click</button>
    <div>
      <h3>Results</h3>
    </div>
    <table>
    {#if rows}
      {#each rows as row}
        <tr>
          <td>
            {row.index}
          </td>
          <td>
          {row.amount}
          </td>
          <td>
            {row.earnValue}
          </td>
        </tr>
      {/each}
    {/if}
    <tr>
    <td>
       {#if rows}                 
         {totalEarn}
        {/if}
    </td>
    </tr>
    </table>
  </div>
</template>

<style>
  button {
    font-family: inherit;
    font-size: inherit;
    padding: 1em 2em;
    color: #ff3e00;
    background-color: rgba(255, 62, 0, 0.1);
    border-radius: 2em;
    border: 2px solid rgba(255, 62, 0, 0);
    outline: none;
    width: 200px;
    font-variant-numeric: tabular-nums;
    cursor: pointer;
  }

  button:focus {
    border: 2px solid #ff3e00;
  }

  button:active {
    background-color: rgba(255, 62, 0, 0.2);
  }
</style>
