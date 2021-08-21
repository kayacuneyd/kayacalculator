<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { quintOut } from "svelte/easing";
  import { getContext } from "svelte";
  /* export let expense;
  let { name, amount, id } = expense;
  // we access right away to name & amount
  // the reason for that is
  // because I know that this particular object already has these properties. */

  export let id;
  export let name = "default expense name";
  export let amount = 0;

  let displayAmount = false;

  function toggleAmount(event) {
    event.preventDefault();
    displayAmount = !displayAmount;
  }

  const removeExpense = getContext("remove");
  const setModifiedExpense = getContext("modify");
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button on:click={toggleAmount} class="amount-btn">
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 transition:slide>
        amount: ${amount}
      </h4>
      <h4 transition:slide>
        expense id: {id}
      </h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}
    >
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
