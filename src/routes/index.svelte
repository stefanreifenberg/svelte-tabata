<script>
    import Timer from '$lib/timer.svelte'
    
</script>

<main>
    <h1>interval timer</h1>
    <Timer/>    
</main>
<div class="coffee">
  <script data-name="BMC-Widget"
   data-cfasync="false"
    src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js"
    data-id="reifenberg" data-description="Support me on Buy me a coffee!"
    data-message="" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18">
  </script>
</div>

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
      font-size: 4em;
      margin: 0;
      font-weight: 100;
    }
    .coffee {
      margin-top: 29px;
      padding: 19px;
    }
    
    @media (max-width: 640px) {
      
      h1 {
        font-size: 3em;        
      }  
    }
  </style>
  