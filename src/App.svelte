<!-- JAVASCRIPT/LOGIC-->
<script>
  // import Github from "./Github.svelte";
  // import GithubAwait from "./GithubAwait.svelte";
  import { setContext, onMount, afterUpdate } from "svelte";
  //components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  import Modal from "./Modal.svelte";
  //data
  // import expensesData from "./expenses";
  //variables
  let expenses = [];
  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  // toggle form variables
  let isFormOpen = false;
  // what I'm doing right now is I'm setting up my local variable
  // because we would want to add some changes
  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((acc, curr) => {
    console.log({ acc, amount: curr.amount });
    return (acc += curr.amount);
  }, 0);
  console.log(expenses);
  // functions

  function showForm() {
    isFormOpen = true;
  }

  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  }

  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }
  function addExpense({ name, amount }) {
    // console.log(name, amount);
    let expense = {
      id: Math.random() * Date.now(),
      name: name,
      amount: amount,
    };
    expenses = [expense, ...expenses];
  }
  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setAmount = null;
    setName = "";
  }
  // context
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);
  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(() => {
    console.log("after update");
    setLocalStorage();
  });
</script>

<Navbar {showForm} />

<main class="content">
  <!-- <GithubAwait /> -->
  {#if isFormOpen}
    <Modal>
      <ExpenseForm
        {addExpense}
        name={setName}
        amount={setAmount}
        {isEditing}
        {editExpense}
        {hideForm}
      />
    </Modal>
  {/if}

  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <!--   <ExpensesList expense={expenses} />
right side is value and left side is prop -->

  <button
    type="button"
    class="btn btn-primary btn-block"
    on:click={clearExpenses}>clear expenses</button
  >
</main>
