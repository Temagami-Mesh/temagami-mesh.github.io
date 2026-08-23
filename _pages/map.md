---
title: "Network Map"
permalink: /map/
excerpt: "Our community mesh router node infrastructure"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

# Interactive Topographic Map

Below is the live tracking data pulled directly from the repository.

<!-- 1. Map container and layout dimensions -->
<div id="map" style="height: 600px; width: 100%; border: 1px solid #ccc; margin: 20px 0; z-index: 1;"></div>

<!-- 2. Leaflet Assets hosted via CDN -->
<link rel="stylesheet" href="https://unpkg.com" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
<script src="https://unpkg.com" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<!-- 3. Dynamic Map Render Script -->
<script>
  document.addEventListener("DOMContentLoaded", function() {
    // Initialize map engine
    const map = L.map('map').setView([0, 0], 2);

    // Apply topographic map tiles
    L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
      maxZoom: 17,
      attribution: 'Map data: &copy; OpenStreetMap contributors, SRTM | Map style: &copy; OpenTopoMap'
    }).addTo(map);

    // Raw dataset URL configuration
    const dataUrl = 'https://githubusercontent.com';

    // Fetch and bind dataset
    fetch(dataUrl)
      .then(response => {
        if (!response.ok) throw new Error('Data fetch failed');
        return response.json();
      })
      .then(geojsonData => {
        const geojsonLayer = L.geoJSON(geojsonData, {
          onEachFeature: function (feature, layer) {
            if (feature.properties && feature.properties.name) {
              layer.bindPopup('<strong>' + feature.properties.name + '</strong>');
            }
          }
        }).addTo(map);

        // Adjust view bounding boxes dynamically to feature sizes
        map.fitBounds(geojsonLayer.getBounds());
      })
      .catch(error => console.error('Map loading error:', error));
  });
</script>
