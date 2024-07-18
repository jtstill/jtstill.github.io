---
layout: archive
title:  "Route Marker Project"
tagline: "Current Progress"
author: Jack Still
author_profile: true
header:
 teaser: "assets/images/north_of_sedona.jpg"
 overlay_image: "assets/images/north_of_sedona.jpg"
 # caption: "Photo credit: me"
 description: A picture I took north of Sedona, AZ in August 2022
---
<a href="javascript:window.history.back();">Go Back</a>

<h3 class="archive__subtitle">Progress!</h3>

<!--<div class="gallery">
  {% for file in site.static_files %}
      {% if file.path contains '/geography/route_marker_project/images/hwy_pics/' and file.extname == '.jpg' %}
          {{ file.name }}
      {% endif %}
  {% endfor %}
</div>-->

<div class="gallery">
  {% for file in site.static_files %}
      {% if file.path contains '/geography/route_marker_project/images/hwy_pics/' and file.extname == '.jpg' %}
          <img src="{{ file.path }}" alt="Gallery Image" width="20%">
      {% endif %}
  {% endfor %}
</div>