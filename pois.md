---
layout: page
title: "Προορισμοί"
permalink: /pois/
---

<style>
.poi-grid{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
  gap:20px;
  margin-bottom:40px;
}

.poi-card{
  border:1px solid #ddd;
  border-radius:12px;
  padding:20px;
  background:white;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
  transition:0.3s;
}

.poi-card:hover{
  transform:translateY(-4px);
  box-shadow:0 6px 16px rgba(0,0,0,0.15);
}

.poi-card a{
  text-decoration:none;
  font-weight:bold;
  color:#2c3e50;
}

.category{
  margin-top:50px;
}

.category h2{
  margin-bottom:10px;
}

.category p{
  color:#555;
  margin-bottom:20px;
  line-height:1.6;
}

.intro{
  font-size:1rem;
  line-height:1.5;
  margin-bottom:40px;
  text-align:left;
  color:#444;
}
  
</style>

<div class="intro">
 <br>
  
<strong>🗺️ Εξερευνήστε διαφορετικούς Προορισμούς</strong>
  <br>
  <br>
<p style="text-align: justify;">
Περιηγηθείτε στις ομορφότερες τοποθεσίες της Ελλάδας, οργανωμένες ανά είδος και εμπειρία που προσφέρουν. Από γαλήνιες λίμνες και ορμητικά ποτάμια μέχρι επιβλητικά φαράγγια και καταπράσινες ορεινές κορυφές, κάθε προορισμός υπόσχεται μοναδικές δραστηριότητες, εντυπωσιακές εικόνες και μια αυθεντική επαφή με τον φυσικό πλούτο της Χώρας.</p>
</div>

<div class="category">
<h2>🌿 Λίμνες & Φυσική Γαλήνη</h2>
<p>Εικόνες απόλυτης ηρεμίας, αυθεντική φυσική ομορφιά και στιγμές χαλάρωσης δίπλα στο νερό.</p>

<div class="poi-grid">
{% for p in site.pois %}
{% if p.category == "lake" %}
<div class="poi-card">
<a href="{{ p.url | relative_url }}">
{{ p.title }}
</a>
</div>
{% endif %}
{% endfor %}
</div>
</div>

<div class="category">
<h2>🌊 Ποτάμια & Δραστηριότητες</h2>
<p>Κρύσταλλινα νερά, ορμητικές διαδρομές και μοναδικές εμπειρίες στην καρδιά της ελληνικής φύσης.</p>

<div class="poi-grid">
{% for p in site.pois %}
{% if p.category == "river" %}
<div class="poi-card">
<a href="{{ p.url | relative_url }}">
{{ p.title }}
</a>
</div>
{% endif %}
{% endfor %}
</div>
</div>

<div class="category">
<h2>🏔️ Ορεινές Αποδράσεις & Πεζοπορία</h2>
<p>Επιβλητικές κορυφές, αλπικές λίμνες, εντυπωσιακά φαράγγια και τοπία που κόβουν την ανάσα.</p>

<div class="poi-grid">
{% for p in site.pois %}
{% if p.category == "mountain" %}
<div class="poi-card">
<a href="{{ p.url | relative_url }}">
{{ p.title }}
</a>
</div>
{% endif %}
{% endfor %}
</div>
</div>

