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

<!-- 2. CORRECTED Leaflet Assets hosted via CDN -->
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
    const dataUrl = 'https://raw.githubusercontent.com/Temagami-Mesh/Mesh-Router-Sites/refs/heads/main/Mesh%20Router%20Sites.geojson';

    // Fetch and bind dataset
    fetch(dataUrl)
      .then(response => {
        if (!response.ok) throw new Error('Data fetch failed');
        return response.json();
      })
      .then(geojsonData => {
        const geojsonLayer = L.geoJSON(geojsonData, {
          // Fix for point data: Ensure markers pull the standard CDN imagery
          pointToLayer: function (feature, latlng) {
            const defaultIcon = L.icon({
              iconUrl: 'https://unpkg.com',
              iconRetinaUrl: 'https://unpkg.com',
              shadowUrl: 'https://unpkg.com',
              iconSize:,
              iconAnchor:,
              popupAnchor: [1, -34],
              shadowSize: [41, 41]
            });
            return L.marker(latlng, { icon: defaultIcon });
          },
          onEachFeature: function (feature, layer) {
            // Check for 'name' or fallback to other common attributes if blank
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
