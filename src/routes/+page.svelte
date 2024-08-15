<script>
    // Declare a variable to hold the data
    let data = [];
    let loading = true;
    let error = null;
  
    // Fetch data when the component mounts
   async function loadData() {
      try {
        const response = await fetch('/data.json');
        if (response.ok) {
          data = await response.json();
       } else {
          throw new Error(`Failed to fetch data: ${response.statusText}`);
        }
     } catch (err) {
        error = err.message;
      } finally {
        loading = false;
      }
    }
  
   let newItem=[]; 
  let message = '';

  async function addData() 
  {
    const response = await fetch('http://localhost:5174/data', {
      method: 'POST',
      headers: {
        'Content-Type': 'data/json'
      },
      body: JSON.stringify(newItem)
    });

    if (response.ok) {
      message = 'Data added successfully';
      newItem = { name: '', age: '' }; // Clear the input fields
    } else {
      message = 'Failed to add data';
    }
    // Load data when the component mounts
    loadData();
  }
</script>
  
  <main>
    <h1>Data from JSON</h1>
  
    {#if loading}
      <p>Loading...</p>
    {:else}
      <ul>
        {#each data as item }
          <li>{item.name}  {item.age} </li>
        {/each}
      </ul>
    {/if}
  </main>

  <h1><p>Data to the JSON </p></h1>
  

  
  <main>

    <form on:submit|preventDefault={addData}>
      <label>
        Name:
        <input type="text" bind:value={data.name} required />
      </label>
      <label>
        Age:
        <input type="number" bind:value={data.age} required />
      </label>
      <button type="submit">Add</button>
    </form>

    {#if message}
      <p>{message}</p>
    {/if}
    
  </main>
  
  
  
  
  