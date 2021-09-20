<script>
    import Timer from '$lib/timer.svelte'
    let title = "tabata timer";
</script>

<main>
    <h1>{title}</h1>
    <Timer/>    
</main>

<style>
    main {
      text-align: center;
      padding: 1em;
      max-width: 240px;
      margin: 0 auto;
    }
    h1 {
      color: var(--accent);
      text-transform: uppercase;
      font-size: 6em;
      margin: 0;
      font-weight: 100;
    }
    @media (min-width: 640px) {
      main {
        max-width: none;
      }
    }
  </style>
  