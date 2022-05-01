<script>
  export let todo, updateTodo, deleteTodo;
</script>

<div class="p-2 flex gap-1 items-center">
  <label class="swap">
    <input
      type="checkbox"
      checked={todo.completed}
      on:change={(e) => {
        todo.completed = e.currentTarget.checked;
        updateTodo(todo);
      }}
    />
    <div class="swap-on btn btn-success">
      <i class="fa-solid fa-check" />
    </div>
    <div class="swap-off btn btn-secondary">
      <i class="fa-regular fa-square" />
    </div>
  </label>
  <input
    type="text"
    class="input input-bordered w-full max-w-xs"
    value={todo.task}
    on:input={(e) => {
      todo.task = e.currentTarget.value;
      updateTodo(todo);
    }}
  />
  <button
    class="btn btn-error"
    on:click={() => {
      deleteTodo(todo);
    }}
  >
    <i class="fa-solid fa-trash" />
  </button>
</div>
