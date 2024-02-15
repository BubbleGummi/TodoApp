<script>
  import { auth } from '../firebase.js';
  import { createUserWithEmailAndPassword } from 'firebase/auth';
  import { navigate } from 'svelte-routing'; // Import navigate function for redirection

  let email = '';
  let password = '';

  const signUp = async () => {
    try {
      const userCredential = await createUserWithEmailAndPassword(auth, email, password);
      console.log('Signed up successfully:', userCredential.user.uid);
      navigate('/dashboard'); 
    } catch (error) {
      console.error('Error signing up:', error.message);
    }
  };
</script>
<div>
  <h2>Sign Up</h2>
  <form on:submit|preventDefault={signUp}>
    <input type="email" bind:value={email} placeholder="Email" required />
    <input type="password" bind:value={password} placeholder="Password" required />
    <button type="submit">Sign Up</button>
  </form>
</div>
