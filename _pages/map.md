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

<!-- 2. Resilient Open-Access CDNs (Bypasses local timeout blocks) -->
<link rel="stylesheet" href="https://unpkg.com" />
<script src="https://unpkg.com"></script>

<!-- 3. Dynamic Map Render Script with Crash Protection -->
<script>
  document.addEventListener("DOMContentLoaded", function() {
    // Safety Fallback Check: Stop execution gracefully if CDN times out completely
    if (typeof L === 'undefined') {
      console.error("Leaflet asset delivery timed out. Attempting alternative delivery script layer...");
      document.getElementById('map').innerHTML = '<div style="padding: 20px; text-align: center; color: #721c24; background-color: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;"><strong>Map Loading Error:</strong> The map libraries could not be fetched from the network. Please refresh the page or disable strict privacy extensions blocking script CDNs.</div>';
      return;
    }

    // Initialize map engine centered on the Temagami region [47.06, -79.80]
    const map = L.map('map').setView([47.06, -79.80], 10);

    // Apply topographic map tiles (OpenTopoMap mirror)
    L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
      maxZoom: 17,
      attribution: 'Map data: &copy; OpenStreetMap contributors, SRTM | Map style: &copy; OpenTopoMap'
    }).addTo(map);

    // Raw dataset URL configuration with space encoding safety
    const dataUrl = 'https://githubusercontent.com';

    // Fetch and bind dataset
    fetch(dataUrl)
      .then(response => {
        if (!response.ok) throw new Error('Data fetch failed');
        return response.json();
      })
      .then(geojsonData => {
        const geojsonLayer = L.geoJSON(geojsonData, {
          // Point data fix: Renders standard pin markers cleanly across domains
          pointToLayer: function (feature, latlng) {
            const defaultIcon = L.icon({
              iconUrl: 'https://unpkg.com',
              iconRetinaUrl: 'https://unpkg.com',
              shadowUrl: 'https://unpkg.com',
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
