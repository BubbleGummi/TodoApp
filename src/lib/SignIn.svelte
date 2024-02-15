<script>
  import { auth } from '../firebase.js';
  import { signInWithEmailAndPassword } from 'firebase/auth';
  import { navigate } from 'svelte-routing'; // Import navigate function for redirection

  let email = '';
  let password = '';

  const signIn = async () => {
    try {
      const userCredential = await signInWithEmailAndPassword(auth, email, password);
      console.log('Signed in successfully:', userCredential.user.uid);
      navigate('/dashboard'); 
    } catch (error) {
      console.error('Error signing in:', error.message);
    }
  };
</script>

<div>
  <h2>Sign In</h2>
  <form on:submit|preventDefault={signIn}>
    <input type="email" bind:value={email} placeholder="Email" required />
    <input type="password" bind:value={password} placeholder="Password" required />
    <button type="submit">Sign In</button>
  </form>
</div>
