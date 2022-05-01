<script>
  import supabase from "$lib/db.js";
  import { onMount } from "svelte";
  import Todo from "$lib/Todo.svelte";
  import AddTodo from "$lib/AddTodo.svelte";
  import { user } from "$lib/store.js";
  import { goto } from "$app/navigation";

  let todos = [];
  let newTask = "";

  const setNewTask = (newValue) => {
    if (!$user) goto("/auth");
    newTask = newValue;
  };

  onMount(async () => {
    if (!$user) goto("/auth");
    else await fetchTodo();
    supabase
      .from("todos")
      .on("*", (payload) => {
        if (payload.eventType === "UPDATE") {
          let index = todos.findIndex((t) => t.id === payload.new.id);
          todos[index] = payload.new;
          todos = [...todos];
        }
        if (payload.eventType === "DELETE") {
          let index = todos.findIndex((t) => t.id === payload.old.id);
          todos.splice(index, 1);
          todos = [...todos];
        }
        if (payload.eventType === "INSERT") {
          todos = [...todos, payload.new];
        }
      })
      .subscribe();
  });

  const fetchTodo = async () => {
    try {
      let { data, error } = await supabase
        .from("todos")
        .select("*")
        .eq("user_uid", $user.id)
        .order("created_at", { ascending: true });
      todos = data;
    } catch (error) {
      console.error(error);
    }
  };

  const updateTodo = async (todo) => {
    try {
      const { data, error } = await supabase
        .from("todos")
        .update({ task: todo.task, completed: todo.completed })
        .eq("id", todo.id);
    } catch (error) {
      console.error(error);
    }
  };

  const deleteTodo = async (todo) => {
    if (confirm("Are you sure?")) {
      try {
        const { data, error } = await supabase
          .from("todos")
          .delete()
          .eq("id", todo.id);
      } catch (error) {
        console.error(error);
      }
    }
  };

  const insertTodo = async () => {
    const newTodo = {
      task: newTask,
      user_uid: $user.id,
    };
    try {
      const { data, error } = await supabase.from("todos").insert([newTodo]);
      newTask = "";
    } catch (error) {
      console.error(error);
    }
  };
</script>

<div class="bg-base-100 p-4 shadow-lg">
  <AddTodo {newTask} {setNewTask} {insertTodo} />
</div>
<div class="bg-base-100 p-4 shadow-lg">
  {#each todos as todo}
    <Todo {todo} {updateTodo} {deleteTodo} />
  {:else}
    <p>No todos yet</p>
  {/each}
</div>
