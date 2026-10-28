---
layout: page
title: "Προορισμοί"
permalink: /pois/
---

<style>
.intro {line-height:1.7;color:#444;margin:20px 0 28px}
.poi-nav {display:flex;flex-wrap:wrap;gap:10px;margin-bottom:28px}
.poi-nav a {padding:9px 14px;border:1px solid #b7d3c2;border-radius:24px;text-decoration:none;color:#1b4332;background:#f1f8f3}
.region-section {margin:42px 0;scroll-margin-top:90px}
.region-section h2 {color:#1b4332;margin-bottom:8px}
.region-description {color:#555;margin-bottom:18px}
.poi-grid {display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:18px}
.poi-card {border:1px solid #e0e5e2;border-radius:13px;padding:18px;background:#fff;box-shadow:0 2px 8px rgba(0,0,0,.07);transition:transform .2s,box-shadow .2s}
.poi-card:hover {transform:translateY(-3px);box-shadow:0 7px 18px rgba(0,0,0,.13)}
.poi-card a {color:#1b4332;text-decoration:none;font-weight:700;font-size:1.05rem}
.poi-card a:hover {text-decoration:underline}
.poi-card p {font-size:.9rem;line-height:1.5;color:#555;margin:9px 0 0}
.poi-type {display:inline-block;font-size:.78rem;color:#2d6a4f;background:#e8f5e9;padding:4px 9px;border-radius:12px;margin-bottom:10px}
.poi-count {font-size:.88rem;color:#64756c}
.category-section {margin-top:58px;padding-top:22px;border-top:1px solid #ddd}
.category-section h2 {color:#1b4332}
.category-section h3 {margin-top:28px}
</style>

<div class="intro">
  <strong>🗺️ Εξερευνήστε τη Δυτική Ελλάδα ανά περιοχή</strong>
  <p>Ανακαλύψτε ποτάμια, λίμνες, μονοπάτια και ιστορικά μνημεία στο Ζαγόρι, τη Δυτική Στερεά Ελλάδα και την περιοχή Άρτας–Ιωαννίνων. Επιλέξτε μια περιοχή για να οργανώσετε τις εξορμήσεις σας.</p>
</div>

<nav class="poi-nav" aria-label="Επιλογή περιοχής">
  <a href="#zagori">🏔️ Ζαγόρι</a>
  <a href="#western-sterea">🌿 Δυτική Στερεά</a>
  <a href="#arta-ioannina">🌊 Άρτα – Ιωάννινα</a>
  <a href="#by-category">🔎 Ανά είδος</a>
</nav>

{% assign regions = "zagori,western-sterea,arta-ioannina" | split: "," %}
{% for region in regions %}
  {% assign region_pois = site.pois | where: "region", region | sort: "title" %}
  {% case region %}
    {% when "zagori" %}
      {% assign region_name = "Ζαγόρι" %}
      {% assign region_desc = "Φαράγγια, ορεινές λίμνες, ποτάμια και μοναστήρια στα Ζαγοροχώρια." %}
    {% when "western-sterea" %}
      {% assign region_name = "Δυτική Στερεά Ελλάδα" %}
      {% assign region_desc = "Λίμνη Τριχωνίδα, Εύηνος, φυσικές διαδρομές και αρχαιολογικοί θησαυροί." %}
    {% when "arta-ioannina" %}
      {% assign region_name = "Άρτα – Ιωάννινα" %}
      {% assign region_desc = "Άραχθος, λίμνη Ζηρού, μονοπάτια στα Τζουμέρκα και αρχαίοι οικισμοί." %}
  {% endcase %}
  <section class="region-section" id="{{ region }}">
    <h2>{{ region_name }}</h2>
    <p class="region-description">{{ region_desc }}</p>
    <p class="poi-count">{{ region_pois.size }} προορισμοί</p>
    <div class="poi-grid">
      {% for p in region_pois %}
        <article class="poi-card">
          <span class="poi-type">
            {% case p.category %}
              {% when "lake" %}💧 Λίμνη
              {% when "river" %}🌊 Ποτάμι
              {% when "mountain" %}🥾 Βουνό / πεζοπορία
              {% when "monument" %}🏛️ Μνημείο
              {% when "archaeological" %}🏛️ Αρχαιολογικός χώρος
              {% when "trail" %}🥾 Μονοπάτι
              {% else %}📍 Αξιοθέατο
            {% endcase %}
          </span>
          <div><a href="{{ p.url | relative_url }}">{{ p.title }}</a></div>
          {% if p.card_description %}<p>{{ p.card_description | escape }}</p>{% endif %}
        </article>
      {% endfor %}
    </div>
  </section>
{% endfor %}

<section id="by-category" class="category-section">
  <h2>🔎 Αναζήτηση ανά είδος αξιοθέατου</h2>
  {% assign categories = "lake,river,mountain,monument,archaeological,trail" | split: "," %}
  {% for category in categories %}
    {% assign cat_pois = site.pois | where: "category", category | sort: "title" %}
    {% if cat_pois.size > 0 %}
      {% case category %}
        {% when "lake" %}{% assign cat_name = "💧 Λίμνες" %}
        {% when "river" %}{% assign cat_name = "🌊 Ποτάμια" %}
        {% when "mountain" %}{% assign cat_name = "🏔️ Βουνά και φαράγγια" %}
        {% when "monument" %}{% assign cat_name = "🏛️ Ιστορικά μνημεία" %}
        {% when "archaeological" %}{% assign cat_name = "🏛️ Αρχαιολογικοί χώροι" %}
        {% when "trail" %}{% assign cat_name = "🥾 Μονοπάτια" %}
      {% endcase %}
      <h3>{{ cat_name }}</h3>
      <div class="poi-grid">
        {% for p in cat_pois %}
          <article class="poi-card">
            <a href="{{ p.url | relative_url }}">{{ p.title }}</a>
          </article>
        {% endfor %}
      </div>
    {% endif %}
  {% endfor %}
</section>
