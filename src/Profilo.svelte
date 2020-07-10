<script>
  export let foto;
  export let video;
  export let profilo;
  export let nome;
</script>

<style>
  #imgProfilo {
    align-content: center;
  }
</style>

<div>
  <center>
    <div>Foto di profilo {nome}</div>

    <a href={profilo}>
      <img src={profilo} alt="Ciao" width="150px" height="150px" />
    </a>
  </center>
  <center>
    <div>Foto pubblicate da {nome}</div>
  </center>
  <div>
    {#each foto as f}
      <a href={f}>
        <img src={f} alt="Ciao" width="150px" height="150px" />
      </a>
    {/each}
  </div>
  <center>
    <div>Video pubblicati da {nome}</div>
  </center>
  <div>
    {#each video as f}
      <a href={f}>
        <img src={f} alt="Ciao" width="150px" height="150px" />
      </a>
    {/each}
  </div>

</div>
