<script>
  import { onMount } from "svelte";
  import * as L from "leaflet";

  let map;

  onMount(() => {
    map = L.map("map").setView([47.41322, -1.319482], 6);
    L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png").addTo(map);
  });
</script>

<div id="map" />

<style>
  #map {
    width: 1080px;
    height: 720px;
  }
</style>
