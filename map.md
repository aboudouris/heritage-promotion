---
layout: page
title: "Χάρτης Προορισμών"
permalink: /map.html
---

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">

<style>
.region-filters { display:flex; flex-wrap:wrap; justify-content:center; gap:10px; margin:20px 0; }
.region-filter { border:1px solid #2d6a4f; border-radius:24px; padding:10px 16px; background:white; color:#1b4332; font-weight:600; cursor:pointer; }
.region-filter:hover, .region-filter[aria-pressed="true"] { background:#2d6a4f; color:white; }
#all-pois-map { width:100%; height:580px; border-radius:14px; margin:16px 0; }
#map-status { text-align:center; color:#41584d; }
@media(max-width:600px){ #all-pois-map { height:430px; } }
</style>

<h2 style="color:#1b4332;text-align:center;">🗺️ Εξερευνήστε τους προορισμούς μας</h2>
<p style="text-align:center;">Επιλέξτε περιοχή για να δείτε τα αξιοθέατα στον χάρτη.</p>

<div class="region-filters" role="group" aria-label="Επιλογή περιοχής">
  <button type="button" class="region-filter" data-region="all" aria-pressed="true">🌍 Όλα</button>
  <button type="button" class="region-filter" data-region="zagori" aria-pressed="false">🏔️ Ζαγόρι</button>
  <button type="button" class="region-filter" data-region="western-sterea" aria-pressed="false">💧 Δυτική Στερεά</button>
  <button type="button" class="region-filter" data-region="arta-ioannina" aria-pressed="false">🌿 Άρτα – Ιωάννινα</button>
</div>
<div id="all-pois-map" aria-label="Χάρτης αξιοθέατων"></div>
<p id="map-status" role="status">Φόρτωση αξιοθέατων...</p>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script>
// Τα ονόματα αρχείων είναι ο κατάλογος των 15 επιλεγμένων σημείων.
// Δεν απαιτείται αλλαγή στα ήδη υπάρχοντα YAML front matters.
const pois = [
{% assign separator = "" %}
{% for p in site.pois %}
  {% assign filename = p.path | split: "/" | last | remove: ".md" | downcase %}
  {% assign region = "" %}
  {% case filename %}
    {% when "vikos" or "voidomatis" or "drakolimni" or "rogovou" or "agiaparaskevimonastery" %}
      {% assign region = "zagori" %}
    {% when "trichonida" or "fotmou" or "evinos" or "stenaevinou" or "ancientthermos" %}
      {% assign region = "western-sterea" %}
    {% when "arachthosplaka" or "vidratrail" or "ziros" or "kefalovryso" or "ancientorraon" %}
      {% assign region = "arta-ioannina" %}
  {% endcase %}
  {% if region != "" %}
    {{ separator }}{
      "title": {{ p.title | jsonify }},
      "url": {{ p.url | relative_url | jsonify }},
      "category": {{ p.category | jsonify }},
      "region": {{ region | jsonify }},
      "wikidatum": {{ p.wikidatum | default: "" | jsonify }},
      "lat": {{ p.lat | jsonify }},
      "lon": {{ p.lon | jsonify }}
    }
    {% assign separator = "," %}
  {% endif %}
{% endfor %}
];

const map = L.map("all-pois-map").setView([39.25, 21.25], 8);
L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: '&copy; OpenStreetMap contributors', maxZoom: 19
}).addTo(map);
const markers = L.featureGroup().addTo(map);
const loaded = [];
let selectedRegion = "all";
let finishedLoading = false;

function addPOI(p, lat, lon) {
  if (!Number.isFinite(lat) || !Number.isFinite(lon)) return;
  const symbol = ({river:"🌊",lake:"💧",mountain:"🥾",monument:"🏛️",historical:"🏛️"})[p.category] || "📍";
  const popup = document.createElement("div");
  const heading = document.createElement("strong");
  heading.textContent = symbol + " " + p.title;
  const link = document.createElement("a");
  link.href = p.url;
  link.textContent = "Δείτε τον προορισμό";
  popup.append(heading, document.createElement("br"), link);
  loaded.push({region:p.region, marker:L.marker([lat,lon]).bindPopup(popup)});
}

async function loadPOI(p) {
  if (p.lat !== null && p.lon !== null && p.lat !== "" && p.lon !== "") {
    const lat = Number(p.lat), lon = Number(p.lon);
    if (Number.isFinite(lat) && Number.isFinite(lon)) {
      addPOI(p, lat, lon);
      return;
    }
  }
  if (!/^Q\d+$/.test(p.wikidatum || "")) return;
  const params = new URLSearchParams({ action:"wbgetentities", ids:p.wikidatum, props:"claims", format:"json", origin:"*" });
  try {
    const response = await fetch("https://www.wikidata.org/w/api.php?" + params);
    if (!response.ok) throw new Error("Wikidata HTTP " + response.status);
    const data = await response.json();
    const claims = data.entities?.[p.wikidatum]?.claims?.P625 || [];
    const coords = claims.find(c => c.mainsnak?.datavalue?.value)?.mainsnak?.datavalue?.value;
    if (coords) addPOI(p, coords.latitude, coords.longitude);
  } catch (err) {
    console.warn("Αδυναμία ανάκτησης συντεταγμένων:", p.title, err);
  }
}

function showRegion(region) {
  selectedRegion = region;
  markers.clearLayers();
  loaded.forEach(item => {
    if (region === "all" || item.region === region) markers.addLayer(item.marker);
  });
  document.querySelectorAll(".region-filter").forEach(button => {
    button.setAttribute("aria-pressed", String(button.dataset.region === region));
  });
  const count = markers.getLayers().length;
  const total = pois.filter(p => region === "all" || p.region === region).length;
  document.getElementById("map-status").textContent =
    (finishedLoading ? "" : "Φόρτωση... ") + `Εμφανίζονται ${count} από ${total} επιλεγμένους προορισμούς.`;
  if (count > 0) map.fitBounds(markers.getBounds().pad(0.18), {maxZoom:11});
}

document.querySelectorAll(".region-filter").forEach(button => {
  button.addEventListener("click", () => showRegion(button.dataset.region));
});
showRegion("all");
Promise.all(pois.map(loadPOI)).then(() => {
  finishedLoading = true;
  showRegion(selectedRegion);
  map.invalidateSize();
});
</script>

