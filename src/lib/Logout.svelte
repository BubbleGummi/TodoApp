<script>
  import { auth } from '../firebase.js';

  const logout = async () => {
    try {
      await auth.signOut();
      console.log('Logged out successfully');
    } catch (error) {
      console.error('Error logging out:', error.message);
    }
  };
</script>

<div>
  <h2>Logout</h2>
  <button on:click={logout}>Logout</button>
</div>
