---
layout: page
title: "Δραστηριότητες"
permalink: /activities/
---

<style>
.activities-intro {line-height:1.75;color:#3b4b43;margin:20px 0 34px}
.activity-section {margin:40px 0 45px}
.activity-section h2 {color:#1b4332;border-bottom:2px solid #d8eadc;padding-bottom:10px}
.activity-section > p {line-height:1.7;color:#4c5a52}
.activity-grid {display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:16px;margin-top:18px}
.activity-card {display:block;padding:18px 20px;background:#f7fbf7;border:1px solid #dbe9dd;border-left:4px solid #2d6a4f;border-radius:12px;text-decoration:none!important;color:#1b4332!important;box-shadow:0 2px 7px rgba(0,0,0,.05);transition:transform .2s,box-shadow .2s}
.activity-card:hover,.activity-card:focus {transform:translateY(-3px);box-shadow:0 7px 16px rgba(0,0,0,.12)}
.activity-card strong {display:block;font-size:1.05rem;margin-bottom:7px}
.activity-card small {display:block;color:#577061;line-height:1.4}
.activity-note {padding:15px 18px;background:#fff9e9;border-left:4px solid #b68a25;border-radius:8px;color:#584c30;line-height:1.6;margin:30px 0}
.activity-map-link {display:inline-block;background:#2d6a4f;color:white!important;padding:12px 24px;border-radius:24px;text-decoration:none!important;font-weight:bold}
</style>

<div class="activities-intro">
  <strong>🎒 Επιλέξτε τη δική σας εμπειρία</strong>
  <p style="text-align:justify">Ανακαλύψτε τη Δυτική Ελλάδα πέρα από τα συνηθισμένα: δροσερά ποτάμια, ήρεμες λίμνες, μονοπάτια που διασχίζουν φαράγγια και ιστορικά μνημεία που αφηγούνται αιώνες ιστορίας. Από το Ζαγόρι μέχρι τη Δυτική Στερεά και την περιοχή Άρτας–Ιωαννίνων, επιλέξτε την εμπειρία που σας ταιριάζει.</p>
</div>

{% comment %}
Οι κάρτες αντλούνται από τα υπάρχοντα αρχεία _pois.
Τα ονόματα πρέπει να συμφωνούν με τα πραγματικά ονόματα αρχείων.
Για κάθε δραστηριότητα δηλώνουμε ονόματα αρχείων χωρίς την κατάληξη .md.
{% endcomment %}

{% assign swimming = "Voidomatis|Rogovou|Trichonida|Fotmou|Evinos|ArachthosPlaka" | split: "|" %}
{% assign water_sports = "Voidomatis|Evinos|ArachthosPlaka|StenaEvinou" | split: "|" %}
{% assign hiking = "Vikos|Drakolimni|Rogovou|Voidomatis|StenaEvinou|VidraTrail|Kefalovryso|Ziros" | split: "|" %}
{% assign photography = "Vikos|Drakolimni|Rogovou|Trichonida|Fotmou|Ziros|Kefalovryso|ArachthosPlaka" | split: "|" %}
{% assign culture = "AgiaParaskeviMonastery|AncientThermos|AncientOrraon|ArachthosPlaka" | split: "|" %}
{% assign relaxation = "Trichonida|Fotmou|Ziros|Rogovou|Voidomatis" | split: "|" %}

<div class="activity-section">
  <h2>🏊 Κολύμβηση &amp; φυσικές βάθρες</h2>
  <p>Γνωρίστε τοπία με ποτάμια, λίμνες και φυσικές βάθρες. Η κολύμβηση είναι εποχική και προϋποθέτει έλεγχο των τοπικών συνθηκών· δεν είναι κατάλληλο κάθε σημείο ενός ποταμού ή μιας λίμνης.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if swimming contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Περισσότερες πληροφορίες για την τοποθεσία" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-section">
  <h2>🚣 Rafting, καγιάκ &amp; ποταμοπορία</h2>
  <p>Ανακαλύψτε τον Άραχθο, τον Εύηνο και τον Βοϊδομάτη μέσα από οργανωμένες υδάτινες δραστηριότητες. Τα Στενά Ευήνου απευθύνονται σε επισκέπτες με κατάλληλη εμπειρία ή επαγγελματική συνοδεία.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if water_sports contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Εξερευνήστε τον προορισμό" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-section">
  <h2>🥾 Πεζοπορία &amp; εξερεύνηση</h2>
  <p>Ακολουθήστε ήπια παραλίμνια μονοπάτια ή πιο απαιτητικές ορεινές διαδρομές, όπως η ανάβαση προς τη Δρακόλιμνη Τύμφης και οι πεζοπορίες στα φαράγγια. Η δυσκολία διαφέρει σημαντικά ανά διαδρομή.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if hiking contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Εξερευνήστε τον προορισμό" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-section">
  <h2>📸 Φωτογραφία φύσης &amp; τοπίου</h2>
  <p>Από τους απόκρημνους βράχους του Βίκου έως τη γαλήνη των λιμνών και τα γεφύρια των Τζουμέρκων, αναζητήστε τις εικόνες που κάνουν κάθε εξόρμηση αξέχαστη.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if photography contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Εξερευνήστε τον προορισμό" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-section">
  <h2>🏛️ Ιστορία, αρχαιολογία &amp; πολιτισμός</h2>
  <p>Γνωρίστε την ιστορική κληρονομιά της περιοχής: τη Μονή Αγίας Παρασκευής, το ιερό του αρχαίου Θέρμου, το αρχαίο Όρραον και το ιστορικό Γεφύρι της Πλάκας.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if culture contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Εξερευνήστε τον προορισμό" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-section">
  <h2>🌿 Χαλάρωση &amp; φυσιολατρικές αποδράσεις</h2>
  <p>Αφιερώστε χρόνο στην παρατήρηση του τοπίου, σε χαλαρούς περιπάτους και στην ηρεμία δίπλα στο νερό. Δεν απαιτούν όλες οι εξορμήσεις έντονη δραστηριότητα.</p>
  <div class="activity-grid">
    {% for p in site.pois %}
      {% assign filename = p.path | split: "/" | last | remove: ".md" %}
      {% if relaxation contains filename %}
      <a class="activity-card" href="{{ p.url | relative_url }}"><strong>{{ p.title | escape }}</strong><small>{{ p.card_description | default: "Εξερευνήστε τον προορισμό" | escape }}</small></a>
      {% endif %}
    {% endfor %}
  </div>
</div>

<div class="activity-note">⚠️ <strong>Σημαντικό:</strong> Τα φυσικά νερά δεν επιτηρούνται πάντα και η στάθμη ή τα ρεύματα μπορεί να αλλάζουν. Ελέγξτε τις συνθήκες και τυχόν περιορισμούς πριν από κολύμβηση ή ποταμοπορία. Η Δρακόλιμνη Τύμφης προτείνεται για πεζοπορία και παρατήρηση, όχι για κολύμβηση.</div>

<div style="text-align:center;margin:35px 0">
  <a class="activity-map-link" href="{{ '/map.html' | relative_url }}">🗺️ Δείτε όλους τους προορισμούς στον χάρτη</a>
</div>
