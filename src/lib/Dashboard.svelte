<script>
  import { auth } from '../firebase.js'; 
  import { getDatabase, ref, push, remove, update, onValue } from 'firebase/database';
  import { onMount } from 'svelte';

  let user = null;
  let newTodo = '';
  let todos = [];

  // Function to fetch todos from the database
  const fetchTodos = () => {
    if (user) {
      const todosRef = ref(getDatabase(), `todos/${user.uid}`);
      onValue(todosRef, (snapshot) => {
        todos = snapshot.val() ? Object.entries(snapshot.val()).map(([key, value]) => ({ id: key, ...value })) : [];
      });
    }
  };

  // Fetch todos when the component mounts
  onMount(() => {
    auth.onAuthStateChanged(currentUser => {
      user = currentUser;
      if (user) {
        fetchTodos();
      }
    });
  });

  //  add a new todo
  const addTodo = () => {
    if (user && newTodo.trim() !== '') {
      const todosRef = ref(getDatabase(), `todos/${user.uid}`);
      push(todosRef, { text: newTodo, completed: false })
        .then(() => {
          fetchTodos();
          newTodo = '';
        })
        .catch(error => {
          console.error('Error adding todo:', error);
        });
    }
  };

  // Function to toggle todo completion status
  const toggleTodoCompletion = (todoId, completed) => {
    const todoRef = ref(getDatabase(), `todos/${user.uid}/${todoId}`);
    update(todoRef, { completed: !completed });
  };

  // delete a todo
  const deleteTodo = (todoId) => {
    const todoRef = ref(getDatabase(), `todos/${user.uid}/${todoId}`);
    remove(todoRef);
  };
</script>

<div>
  <h2>Welcome, {user ? user.email : 'Guest'}</h2>
  
  <!-- Add new todo form -->
  <form on:submit|preventDefault={addTodo}>
    <input type="text" bind:value={newTodo} placeholder="Add a new todo" />
    <button type="submit">Add Todo</button>
  </form>
  
  <!-- Display todo list -->
  <ul>
    {#each todos as { id, text, completed }}
      <li>
        <input type="checkbox" bind:checked={completed} on:change={() => toggleTodoCompletion(id, completed)} />
        {text}
        <button on:click={() => deleteTodo(id)}>Delete</button>
      </li>
    {/each}
  </ul>
</div>
