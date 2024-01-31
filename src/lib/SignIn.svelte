<script>
  let email = '';
  let password = '';

  const signIn = () => {
    // Add your sign-in logic here
    console.log('Signing in:', email, password);
  };
</script>

<div>
  <h2>Sign In</h2>
  <input type="text" bind:value={email} placeholder="Email" />
  <input type="password" bind:value={password} placeholder="Password" />
  <button on:click={signIn}>Sign In</button>
</div>
