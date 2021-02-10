<script>
  import { onMount } from "svelte";
  import * as L from "leaflet";
  import { areas, searchPoint } from "./stores";
  import traverson from "traverson";
  import JsonHalAdapter from "traverson-hal";

  traverson.registerMediaType(JsonHalAdapter.mediaType, JsonHalAdapter);

  const znieff = "znieff";
  const natura = "natura";
  const ep = "ep";

  let map;
  let searchMarker = null;
  let layer = null;

  onMount(() => {
    map = L.map("map").setView([47.41322, -1.319482], 6);
    L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png").addTo(map);
    map.on("click", handleClick);
  });

  function onEachFeature(feature, layer) {
    layer.bindTooltip(
      `${getAreaTypeFromId(feature.properties.geomId)}: ${
        feature.properties.name
      }`,
      {
        permanent: false,
        sticky: true,
      }
    );
  }

  function getAreaTypeFromId(id) {
    switch (id.slice(0, 4)) {
      case "I032":
        return znieff;
      case "I056":
        return ep;
      case "I098":
        return natura;
    }
  }

  function style(feature) {
    switch (getAreaTypeFromId(feature.properties.geomId)) {
      case znieff:
        return { color: "#ff0000" };
      case ep:
        return { color: "#0000ff" };
      case natura:
        return { color: "#f1dc21" };
    }
  }

  async function handleClick(e) {
    $searchPoint = e.latlng;

    searchMarker = !!searchMarker
      ? searchMarker.setLatLng($searchPoint)
      : L.marker($searchPoint).addTo(map);

    traverson
      .from("https://odata-inpn.mnhn.fr")
      .jsonHal()
      .disableAutoHeaders()
      .follow("geolocation", "areas")
      .withTemplateParameters({
        size: 10,
        lat: $searchPoint.lat,
        lng: $searchPoint.lng,
      })
      .getResource((err, doc, traversal) => {
        $areas = doc._embedded.areas;
        traversal
          .continue()
          .follow("geometry")
          .withTemplateParameters({ geometryType: "POLYGON_SIMPLE_T1" })
          .get((err, geojson) => {
            if (err) {
              console.log(err);
            }
            if (!!layer) {
              layer.clearLayers();
              layer.addData(geojson);
            } else {
              layer = L.geoJSON(geojson, { onEachFeature, style }).addTo(map);
            }
          });
      });
  }
</script>

<div id="map" />

<style>
  #map {
    width: 1500px;
    height: 700px;
  }
</style>
