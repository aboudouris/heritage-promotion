---
layout: page
title: "Προορισμοί"
permalink: /pois/
---

<style>
.dest-intro {line-height:1.75;color:#425449;margin:20px 0 26px}
.dest-nav {display:flex;flex-wrap:wrap;gap:9px;margin:18px 0 30px}
.dest-nav a {border:1px solid #b6d5c1;border-radius:24px;padding:9px 14px;text-decoration:none!important;color:#1b4332!important;background:#eef7f0;font-size:.94rem}
.dest-nav a:hover {background:#d9efde}
.dest-section {margin:40px 0;scroll-margin-top:80px}
.dest-section h2 {color:#1b4332;border-bottom:2px solid #dcebdd;padding-bottom:9px}
.dest-section p {color:#506158;line-height:1.6}
.dest-grid {display:grid;grid-template-columns:repeat(auto-fit,minmax(235px,1fr));gap:18px;margin-top:18px}
.dest-card {overflow:hidden;border:1px solid #dbe9df;border-radius:14px;background:white;box-shadow:0 3px 12px rgba(0,0,0,.07);transition:transform .2s,box-shadow .2s}
.dest-card:hover {transform:translateY(-3px);box-shadow:0 8px 18px rgba(0,0,0,.12)}
.dest-card img {display:block;width:100%;height:158px;object-fit:cover}
.dest-placeholder {height:110px;display:flex;align-items:center;justify-content:center;background:linear-gradient(135deg,#d7ecdd,#edf7ef);font-size:2.3rem}
.dest-card-body {padding:15px 17px 19px}
.dest-type {display:inline-block;font-size:.77rem;background:#e6f3e9;color:#256040;border-radius:12px;padding:3px 9px;margin-bottom:9px}
.dest-card h3 {font-size:1.05rem;margin:4px 0 8px}
.dest-card h3 a {color:#1b4332!important;text-decoration:none!important}
.dest-card h3 a:hover {text-decoration:underline!important}
.dest-card p {font-size:.9rem;margin:0 0 12px;color:#526359}
.dest-more {color:#2d6a4f!important;font-weight:700;font-size:.9rem;text-decoration:none!important}
.dest-meta {font-size:.9rem;color:#60746a;margin:8px 0 16px}
.dest-empty {padding:14px;background:#f5f8f5;border-radius:9px;color:#65756c}
</style>

<div class="dest-intro">
  <h2>🗺️ Εξερευνήστε τη Δυτική Ελλάδα</h2>
  <p>Ανακαλύψτε το Ζαγόρι, τη Δυτική Στερεά και την περιοχή Άρτας–Ιωαννίνων μέσα από ποτάμια, λίμνες, πεζοπορικές διαδρομές και ιστορικά μνημεία. Επιλέξτε μία περιοχή ή έναν τύπο αξιοθέατου και ανοίξτε την αναλυτική σελίδα κάθε προορισμού.</p>
</div>

<nav class="dest-nav" aria-label="Μετακίνηση στις ενότητες">
  <a href="#zagori">🏔️ Ζαγόρι</a>
  <a href="#western-sterea">💧 Δυτική Στερεά</a>
  <a href="#arta-ioannina">🌿 Άρτα–Ιωάννινα</a>
  <a href="#categories">🔎 Ανά κατηγορία</a>
</nav>

{% comment %}
  Αξιόπιστη ομαδοποίηση: πρώτα ελέγχεται το region του YAML,
  διαφορετικά το όνομα αρχείου/slug για τα 15 επιλεγμένα POIs.
  Για μελλοντικά POIs προσθέστε region: zagori / western-sterea / arta-ioannina.
{% endcomment %}
{% assign sorted_pois = site.pois | sort: "title" %}
{% assign region_keys = "zagori|western-sterea|arta-ioannina" | split: "|" %}
{% for current_region in region_keys %}
  {% case current_region %}
    {% when "zagori" %}
      {% assign region_title = "🏔️ Ζαγόρι" %}
      {% assign region_description = "Φαράγγι Βίκου, Βοϊδομάτης, Κολυμπήθρες Ρογκοβού, Δρακόλιμνη Τύμφης και Μονή Αγίας Παρασκευής." %}
    {% when "western-sterea" %}
      {% assign region_title = "💧 Δυτική Στερεά Ελλάδα" %}
      {% assign region_description = "Τριχωνίδα, Φωτμού, Εύηνος, Στενά Ευήνου και αρχαίος Θέρμος." %}
    {% when "arta-ioannina" %}
      {% assign region_title = "🌿 Άρτα – Ιωάννινα" %}
      {% assign region_description = "Άραχθος και Γεφύρι Πλάκας, Μονοπάτι Βίδρας, λίμνη Ζηρού, Κεφαλόβρυσος και αρχαίο Όρραον." %}
  {% endcase %}

  <section class="dest-section" id="{{ current_region }}">
    <h2>{{ region_title }}</h2>
    <p>{{ region_description }}</p>
    <div class="dest-grid">
      {% assign region_count = 0 %}
      {% for p in sorted_pois %}
        {% assign filename = p.path | split: "/" | last | remove: ".md" | remove: ".markdown" | downcase | replace: "-", "" | replace: "_", "" | replace: " ", "" %}
        {% assign slugname = p.slug | default: "" | downcase | replace: "-", "" | replace: "_", "" | replace: " ", "" %}
        {% assign inferred_region = "" %}
        {% assign z_names = "vikos|voidomatis|drakolimni|rogovou|agiaparaskevimonastery" | split: "|" %}
        {% assign s_names = "trichonida|fotmou|evinos|stenaevinou|ancientthermos" | split: "|" %}
        {% assign a_names = "arachthosplaka|vidratrail|ziros|kefalovryso|ancientorraon" | split: "|" %}
        {% if z_names contains filename or z_names contains slugname %}
          {% assign inferred_region = "zagori" %}
        {% elsif s_names contains filename or s_names contains slugname %}
          {% assign inferred_region = "western-sterea" %}
        {% elsif a_names contains filename or a_names contains slugname %}
          {% assign inferred_region = "arta-ioannina" %}
        {% endif %}
        {% assign region_of_poi = inferred_region %}
        {% if p.region %}{% assign region_of_poi = p.region %}{% endif %}
        {% if region_of_poi == current_region %}
          {% assign region_count = region_count | plus: 1 %}
          <article class="dest-card">
            {% assign picture = p.image | default: p.thumbnail | default: p.card_image %}
            {% if picture %}
              {% assign first_char = picture | slice: 0, 1 %}
              {% if picture contains '://' %}
                <img src="{{ picture }}" alt="{{ p.title | escape }}" loading="lazy">
              {% elsif first_char == '/' %}
                <img src="{{ picture | relative_url }}" alt="{{ p.title | escape }}" loading="lazy">
              {% else %}
                <img src="{{ picture | relative_url }}" alt="{{ p.title | escape }}" loading="lazy">
              {% endif %}
            {% else %}
              <div class="dest-placeholder" aria-hidden="true">🌲</div>
            {% endif %}
            <div class="dest-card-body">
              <span class="dest-type">{% case p.category %}{% when 'lake' %}💧 Λίμνη{% when 'river' %}🌊 Ποτάμι{% when 'mountain' %}🥾 Βουνό / φαράγγι{% when 'trail' %}🥾 Μονοπάτι{% when 'monument' %}🏛️ Μνημείο{% when 'archaeological' %}🏛️ Αρχαιολογικός χώρος{% else %}📍 Αξιοθέατο{% endcase %}</span>
              <h3><a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a></h3>
              {% if p.card_description %}<p>{{ p.card_description | escape }}</p>{% endif %}
              <a class="dest-more" href="{{ p.url | relative_url }}">Ανακαλύψτε τον προορισμό →</a>
            </div>
          </article>
        {% endif %}
      {% endfor %}
    </div>
    <p class="dest-meta">{{ region_count }} διαθέσιμοι προορισμοί</p>
    {% if region_count == 0 %}<p class="dest-empty">Δεν βρέθηκαν καταχωρισμένα σημεία για αυτήν την περιοχή. Ελέγξτε τα ονόματα αρχείων και το πεδίο region στα _pois.</p>{% endif %}
  </section>
{% endfor %}

<section id="categories" class="dest-section">
  <h2>🔎 Προορισμοί ανά κατηγορία</h2>
  <p>Κάτω από κάθε κατηγορία εμφανίζονται οι αντίστοιχοι προορισμοί από τα πραγματικά αρχεία <code>_pois</code>.</p>
  {% assign category_keys = "lake|river|mountain|trail|monument|archaeological" | split: "|" %}
  {% for current_category in category_keys %}
    {% assign category_pois = sorted_pois | where: "category", current_category %}
    {% if category_pois.size > 0 %}
      {% case current_category %}
        {% when 'lake' %}{% assign category_title = '💧 Λίμνες' %}
        {% when 'river' %}{% assign category_title = '🌊 Ποτάμια' %}
        {% when 'mountain' %}{% assign category_title = '🏔️ Βουνά & φαράγγια' %}
        {% when 'trail' %}{% assign category_title = '🥾 Πεζοπορικά μονοπάτια' %}
        {% when 'monument' %}{% assign category_title = '🏛️ Ιστορικά μνημεία' %}
        {% when 'archaeological' %}{% assign category_title = '🏛️ Αρχαιολογικοί χώροι' %}
      {% endcase %}
      <h3 style="color:#1b4332;margin-top:30px">{{ category_title }} <small>({{ category_pois.size }})</small></h3>
      <div class="dest-grid">
        {% for p in category_pois %}
          <article class="dest-card">
            {% assign picture = p.image | default: p.thumbnail | default: p.card_image %}
            {% if picture %}
              {% if picture contains '://' %}
                <img src="{{ picture }}" alt="{{ p.title | escape }}" loading="lazy">
              {% else %}
                <img src="{{ picture | relative_url }}" alt="{{ p.title | escape }}" loading="lazy">
              {% endif %}
            {% else %}<div class="dest-placeholder" aria-hidden="true">📍</div>{% endif %}
            <div class="dest-card-body">
              <h3><a href="{{ p.url | relative_url }}">{{ p.title | escape }}</a></h3>
              {% if p.card_description %}<p>{{ p.card_description | escape }}</p>{% endif %}
              <a class="dest-more" href="{{ p.url | relative_url }}">Περισσότερα →</a>
            </div>
          </article>
        {% endfor %}
      </div>
    {% endif %}
  {% endfor %}
</section>

<p class="dest-meta">Συνολικά καταχωρισμένα αρχεία στη συλλογή: {{ site.pois.size }}</p>
<p style="text-align:center;margin:32px 0"><a href="{{ '/map.html' | relative_url }}" style="display:inline-block;background:#2d6a4f;color:white;padding:12px 24px;border-radius:25px;text-decoration:none;font-weight:700">🗺️ Δείτε τον ενιαίο χάρτη</a></p>
