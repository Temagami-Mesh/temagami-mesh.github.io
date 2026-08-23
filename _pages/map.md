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

<!-- 2. LOCAL ASSETS (Eliminates CORS and external network dependencies) -->
<link rel="stylesheet" href="{{ site.baseurl }}/assets/leaflet/leaflet.css" />
<script src="{{ site.baseurl }}/assets/leaflet/leaflet.js"></script>

<!-- 3. Dynamic Map Render Script -->
<script>
  document.addEventListener("DOMContentLoaded", function() {
    // Safety Fallback Check: Verifies local file extraction
    if (typeof L === 'undefined') {
      document.getElementById('map').innerHTML = '<div style="padding: 20px; text-align: center; color: #721c24; background-color: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;"><strong>Map Loading Error:</strong> The local leaflet assets could not be found at /assets/leaflet/. Verify they are committed to your branch.</div>';
      return;
    }

    // Initialize map engine centered on the Temagami region [47.06, -79.80]
    const map = L.map('map').setView([47.06, -79.80], 10);

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
            const title = feature.properties && (feature.properties.name || feature.properties.Name || "Mesh Node");
            layer.bindPopup('<strong>' + title + '</strong>');
          }
        }).addTo(map);

        // Adjust view bounding boxes dynamically to feature sizes
        map.fitBounds(geojsonLayer.getBounds());
      })
      .catch(error => console.error('Map loading error:', error));
  });
</script>
