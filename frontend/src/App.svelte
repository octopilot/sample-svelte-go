<script>
  let name = ''
  let birthYear = ''
  let apiBase = 'http://localhost:8081'
  let result = null
  let error = null

  async function handleSubmit(e) {
    e.preventDefault()
    error = null
    result = 'Loading…'
    try {
      const base = (apiBase || '').replace(/\/$/, '')
      const url = `${base}/greet?name=${encodeURIComponent(name)}&birth_year=${encodeURIComponent(birthYear)}`
      const res = await fetch(url)
      const data = await res.json()
      result = res.ok ? JSON.stringify(data, null, 2) : `Error: ${data.error || res.status}`
    } catch (err) {
      error = err.message
      result = null
    }
  }
</script>

<div style="max-width: 400px; margin: 2rem auto; padding: 16px; font-family: system-ui">
  <h1>Greet (Svelte + Go)</h1>
  <p>Svelte frontend + Go API — one repo.</p>
  <form on:submit={handleSubmit}>
    <label style="display:block;margin-bottom:8px">Name</label>
    <input bind:value={name} required placeholder="Alice" style="width:100%;padding:8px;margin-bottom:12px;box-sizing:border-box" />
    <label style="display:block;margin-bottom:8px">Birth year</label>
    <input type="number" bind:value={birthYear} required placeholder="1990" style="width:100%;padding:8px;margin-bottom:12px;box-sizing:border-box" />
    <label style="display:block;margin-bottom:8px">API URL</label>
    <input bind:value={apiBase} placeholder="http://localhost:8081" style="width:100%;padding:8px;margin-bottom:12px;box-sizing:border-box" />
    <button type="submit" style="padding:8px 16px">Greet</button>
  </form>
  {#if error}<p style="color:#c00">Error: {error}</p>{/if}
  {#if result}<pre style="background:#f0f0f0;padding:16px;border-radius:4px;white-space:pre-wrap">{result}</pre>{/if}
</div>
