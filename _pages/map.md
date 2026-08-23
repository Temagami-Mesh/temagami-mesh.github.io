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

<!-- 2. jsDelivr Leaflet Assets (Eliminates CORS and Hash Mismatches) -->
<link rel="stylesheet" href="https://jsdelivr.net" />
<script src="https://jsdelivr.net"></script>

<!-- 3. Dynamic Map Render Script -->
<script>
  document.addEventListener("DOMContentLoaded", function() {
    // FIXED: Default centering on the Temagami region [47.06, -79.80]
    const map = L.map('map').setView([47.06, -79.80], 10);

    // Apply topographic map tiles
    L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
      maxZoom: 17,
      attribution: 'Map data: &copy; OpenStreetMap contributors, SRTM | Map style: &copy; OpenTopoMap'
    }).addTo(map);

    // FIXED: Properly space-encoded URL configuration
    const dataUrl = 'https://githubusercontent.com';

    // Fetch and bind dataset
    fetch(dataUrl)
      .then(response => {
        if (!response.ok) throw new Error('Data fetch failed');
        return response.json();
      })
      .then(geojsonData => {
        const geojsonLayer = L.geoJSON(geojsonData, {
          // FIXED: Placed concrete dimension arrays to replace empty brackets
          pointToLayer: function (feature, latlng) {
            const defaultIcon = L.icon({
              iconUrl: 'https://cloudflare.com',
              iconRetinaUrl: 'https://cloudflare.com',
              shadowUrl: 'https://cloudflare.com',
              iconSize: [25, 41],
              iconAnchor: [12, 41],
              popupAnchor: [1, -34],
              shadowSize: [41, 41]
            });
            return L.marker(latlng, { icon: defaultIcon });
          },
          onEachFeature: function (feature, layer) {
            // Check for lowercase and uppercase variants of 'name' property fields
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
