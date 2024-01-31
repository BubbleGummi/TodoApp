<script>
  import { Route, Router } from 'svelte-routing'; // Import the 'Link' component
  import SignIn from '../src/lib/SignIn.svelte';
  import SignUp from '../src/lib/SignUp.svelte';
  import App from '../src/App.svelte';
  import { Link } from 'svelte-routing';
</script>

<Router>
  <Route path="" component={App} />
  <Route path="/sign-up" component={SignUp} />
  <Route path="/sign-in" component={SignIn} />

  <div>
    <h1>Welcome to Your App</h1>
    <p>Choose an action:</p>

    <div>
      <Link to="/sign-up">
        <button>Sign Up</button>
      </Link>

      <Link to="/sign-in">
        <button>Sign In</button>
      </Link>
    </div>
  </div>
</Router>
