<script>
  let email = '';
  let password = '';

  const signUp = () => {
    // Add your sign-up logic here
    console.log('Signing up:', email, password);
  };
</script>

<div>
  <h2>Sign Up</h2>
  <input type="text" bind:value={email} placeholder="Email" />
  <input type="password" bind:value={password} placeholder="Password" />
  <button on:click={signUp}>Sign Up</button>
</div>
