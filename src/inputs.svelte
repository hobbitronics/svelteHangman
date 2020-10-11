<script>
    import { createEventDispatcher } from 'svelte'
    const dispatch = createEventDispatcher();
    let text = '';
    async function onSubmit (e) {
        e.preventDefault();
        let url = `https://api.datamuse.com/words?ml=${text}&max=10`
          try {
              const response = await fetch(url)
              const data = await response.json()	 
              dispatch("getlist", data);
          } catch (error) {
            console.error(error)
          }
    }
</script>

<form on:submit={onSubmit}>
    <input type="text" placeholder="Enter a topic" bind:value={text}>
    <input type="submit" value="Generate wordlist" class ="btn"/>
</form>