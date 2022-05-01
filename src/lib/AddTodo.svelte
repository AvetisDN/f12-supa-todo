<script>
  export let newTask, setNewTask, insertTodo;
</script>

<div class="p-2 flex gap-1 items-center">
  <input
    type="text"
    class="input input-bordered w-full max-w-xs"
    placeholder="New Task"
    value={newTask}
    on:input={(e) => {
      setNewTask(e.currentTarget.value);
    }}
  />
  <button class="btn btn-primary" on:click={insertTodo}>
    <i class="fa-solid fa-plus" />
  </button>
</div>
