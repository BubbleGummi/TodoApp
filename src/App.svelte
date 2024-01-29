<script>
  import { onMount } from 'svelte';
  import { auth, db, createUserWithEmailAndPassword, signInWithEmailAndPassword, onAuthStateChanged, signOut, onValue, update, remove, push, ref } from './firebase';

  let user;
  let todos = [];
  let newTodo = '';
  let email = '';
  let password = '';

  onMount(() => {
    onAuthStateChanged(auth, (authUser) => {
      user = authUser;
      if (user) {
        // Fetch todos when the user is authenticated
        fetchTodos();
      }
    });
  });

  const signUp = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      email = '';
      password = '';
    } catch (error) {
      console.error('Error signing up:', error.message);
    }
  };

  const signIn = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      email = '';
      password = '';
    } catch (error) {
      console.error('Error signing in:', error.message);
    }
  };

  const signOutUser = () => {
    signOut(auth);
    user = null;
    todos = [];
  };

  const addTodo = () => {
    if (newTodo.trim() !== '') {
      const todoRef = push(ref(db, `todos/${user.uid}`), {
        text: newTodo,
        completed: false,
      });
      newTodo = '';
    }
  };

  const deleteTodo = (todoKey) => {
    remove(ref(db, `todos/${user.uid}/${todoKey}`));
  };

  const toggleComplete = (todoKey, completed) => {
    update(ref(db, `todos/${user.uid}/${todoKey}`), { completed });
  };

  const fetchTodos = () => {
    onValue(ref(db, `todos/${user.uid}`), (snapshot) => {
      todos = [];
      snapshot.forEach((childSnapshot) => {
        const todo = {
          key: childSnapshot.key,
          ...childSnapshot.val(),
        };
        todos = [todo, ...todos];
      });
    });
  };
</script>

{#if user}
  <h2>Welcome, {user.email}!</h2>
  <button on:click={signOutUser}>Sign Out</button>

  <div>
    <input bind:value={newTodo} placeholder="New Todo" />
    <button on:click={addTodo}>Add Todo</button>
  </div>

  <ul>
    {#each todos as { key, text, completed }}
      <li>
        <input type="checkbox" bind:checked={completed} on:change={() => toggleComplete(key, !completed)} />
        {text} <button on:click={() => deleteTodo(key)}>Delete</button>
      </li>
    {/each}
  </ul>
{:else}
  <div>
    <h2>Sign Up</h2>
    <input type="text" bind:value={email} placeholder="Email" />
    <input type="password" bind:value={password} placeholder="Password" />
    <button on:click={signUp}>Sign Up</button>
  </div>
{/if}
