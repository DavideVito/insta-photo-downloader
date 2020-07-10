<script>
  import Prof from "./Profilo.svelte";

  let baseURL = "https://www.instagram.com/";

  export let profilo = "niente";

  let privato = false;

  let testo = "";

  let nomeUtente = "";

  async function getProfilo() {
    privato = false;
    let ris = await fetch(baseURL + nomeUtente);

    testo = await ris.text();

    let tmpHtml = document.createElement("html");

    tmpHtml.innerHTML = testo;

    let elementi = tmpHtml.getElementsByTagName("script");
    elementi = [...elementi];

    let elemento = getElemento(elementi);

    let jsonPayload = elemento.innerText;

    jsonPayload = jsonPayload.substring("window._sharedData = ".length);

    jsonPayload = JSON.parse(jsonPayload);
    let utente = getUtente(jsonPayload);

    if (!utente.blocked_by_viewer && !utente.is_private) {
      let a = getPhotos(utente);
      a.nome = nomeUtente;
      profilo = a;
    } else {
      privato = true;
    }
  }

  function getPhotos(utente) {
    let foto = utente["edge_owner_to_timeline_media"]["edges"];
    let video = utente["edge_felix_video_timeline"]["edges"];

    let linkFoto = foto.map(f => {
      return f.node.display_url;
    });
    let linkVideo = video.map(f => {
      return f.node.display_url;
    });

    let immagineProfilo = utente.profile_pic_url_hd;

    return { foto: linkFoto, video: linkVideo, profilo: immagineProfilo };
  }

  function getUtente(payload) {
    return payload["entry_data"]["ProfilePage"][0]["graphql"]["user"];
  }

  function getElemento(elementi, str) {
    for (let elemento of elementi) {
      if (elemento.innerText.includes("window._sharedData =")) {
        return elemento;
      }
    }
  }
</script>

<style>
  div {
    margin-top: "25px";
    text-align: center;
    font-size: 2rem;
    font-family: "Courier New", Courier, monospace;
  }
  button {
    cursor: pointer;
  }
</style>

<main>
  <div>
    <input type="text" required bind:value={nomeUtente} />
    <br />
    {#if nomeUtente !== ''}
      <button on:click={getProfilo}>Ottieni il profilo di {nomeUtente}</button>
    {/if}
  </div>
  {#if !privato}
    {#if profilo !== 'niente'}
      <Prof {...profilo} />
    {/if}
  {:else}
    <center>
      <p>{nomeUtente} ha il profilo privato :(</p>
    </center>
  {/if}

</main>
