<!-- App.svelte -->
<script>
  import { auth } from './firebase.js';
  import { onMount } from 'svelte';
  import { Router, Route, navigate } from 'svelte-routing';
  import SignUp from '../src/lib/SignUp.svelte';
  import SignIn from '../src/lib/SignIn.svelte';
  import Dashboard from '../src/lib/Dashboard.svelte';
  import Logout from '../src/lib/Logout.svelte';



  let user = null;

  onMount(() => {
    // Listen for changes in authentication state
    const unsubscribe = auth.onAuthStateChanged(currentUser => {
      console.log('Current User:', currentUser);
      user = currentUser;

      // If a user is logged in, navigate to the dashboard
      if (user) {
        navigate('/dashboard', { replace: true }); // Navigate to the dashboard route
      }
    });

    // Cleanup function to unsubscribe from the onAuthStateChanged listener
    return () => {
      unsubscribe();
    };
  });
</script>

<Router>
  <Route path="/">
    {#if user}
      <Dashboard />
    {:else}
      <SignIn />
    {/if}
  </Route>

  <!-- Add a route for the dashboard -->
  <Route path="/dashboard" component={Dashboard} />

  <Route path="/signup" component={SignUp} />
  <Route path="/logout" component={Logout} />
</Router>