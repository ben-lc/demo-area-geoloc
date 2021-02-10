<script>
  import Map from "./Map.svelte";
  import Datatable from "./Datatable.svelte";
</script>

<main class="container">
  <Map />
  <hr />
  <Datatable />
</main>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
    integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
    crossorigin=""
  />
</svelte:head>

<style>
  .container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
  }
</style>
