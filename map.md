
---
layout: page
title: "Διαδραστικός Χάρτης Αξιοθέατων"
permalink: /map/
---

<link rel="stylesheet"
 href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

<style>
#all-pois-map {
  height: 600px;
  width: 100%;
  border-radius: 14px;
  margin: 20px 0;
}
.map-legend {
  padding: 12px;
  background: #e8f5e9;
  border-radius: 8px;
}
</style>

<p>
Εξερευνήστε φυσικά και ιστορικά αξιοθέατα
της Ελλάδας μέσα από έναν διαδραστικό χάρτη.
</p>

<div class="map-legend">
  🌊 Ποτάμια &nbsp;
  💧 Λίμνες &nbsp;
  🥾 Πεζοπορία &nbsp;
  🏛️ Πολιτισμός
</div>

<div id="all-pois-map"></div>
<p id="map-status">Φόρτωση αξιοθέατων...</p>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
const pois = [
{% for p in site.pois %}
  {
    title: {{ p.title | jsonify }},
    url: {{ p.url | relative_url | jsonify }},
    category: {{ p.category | jsonify }},
    wikidatum: {{ p.wikidatum | default: "" | jsonify }},
    lat: {{ p.lat | default: "null" }},
    lon: {{ p.lon | default: "null" }}
  }{% unless forloop.last %},{% endunless %}
{% endfor %}
];

const map = L.map("all-pois-map")
  .setView([39.5, 21.0], 8);

L.tileLayer(
  "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  {
    attribution: '&copy; OpenStreetMap contributors',
    maxZoom: 19
  }
).addTo(map);

const markers = L.featureGroup().addTo(map);

function addMarker(p, lat, lon) {
  if (!Number.isFinite(lat) || !Number.isFinite(lon)) return;

  const icon = {
    river: "🌊",
    lake: "💧",
    mountain: "🥾",
    monument: "🏛️"
  }[p.category] || "📍";

  const popup = document.createElement("div");
  const title = document.createElement("strong");
  title.textContent = icon + " " + p.title;

  const link = document.createElement("a");
  link.href = p.url;
  link.textContent = "Περισσότερες πληροφορίες";

  popup.append(title, document.createElement("br"), link);

  L.marker([lat, lon])
    .bindPopup(popup)
    .addTo(markers);
}

async function loadPOI(p) {
  if (Number.isFinite(p.lat) && Number.isFinite(p.lon)) {
    addMarker(p, p.lat, p.lon);
    return;
  }

  if (!/^Q\d+$/.test(p.wikidatum)) return;

  const endpoint = "https://www.wikidata.org/w/api.php";
  const params = new URLSearchParams({
    action: "wbgetentities",
    ids: p.wikidatum,
    props: "claims",
    format: "json",
    origin: "*"
  });

  try {
    const response = await fetch(endpoint + "?" + params);
    if (!response.ok) throw new Error("Wikidata request failed");

    const data = await response.json();
    const entity = data.entities?.[p.wikidatum];
    const claims = entity?.claims?.P625;
    const coords = claims?.find(
      c => c.mainsnak?.datavalue?.value
    )?.mainsnak.datavalue.value;

    if (coords) {
      addMarker(p, coords.latitude, coords.longitude);
    }
  } catch (error) {
    console.warn("Δεν φορτώθηκε:", p.title, error);
  }
}

Promise.all(pois.map(loadPOI)).then(() => {
  const count = markers.getLayers().length;
  document.getElementById("map-status").textContent =
    `Εμφανίζονται ${count} από ${pois.length} αξιοθέατα.`;

  if (count > 0) {
    map.fitBounds(markers.getBounds().pad(0.15));
  }
});
</script>
