---
title: "Network Map"
permalink: /map/
excerpt: "Our community mesh router node infrastructure"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

# Interactive Mesh Network Topology Map

If you would like to make a change to this map, please send a detailed update request to [mapupdates@temagami-mesh.net](mailto:mapupdates@temagami-mesh.net). 

[download the raw GeoJSON data here](https://raw.githubusercontent.com/Temagami-Mesh/Mesh-Router-Sites/refs/heads/main/Mesh%20Router%20Sites.geojson)

<!-- 1. Map container and layout dimensions -->
<div id="map" style="height: 600px; width: 100%; border: 1px solid #ccc; margin: 20px 0; z-index: 1;"></div>

<!-- 2. Leaflet Assets hosted via highly compatible CDN -->
<link rel="stylesheet" href="https://cloudflare.com" integrity="sha512-Zcn6cHskjwhWgKs07u3Gsc6vSAsP96oN0fMksE37V9vj0fN2H3n2A7N1hGZgC8tS9wM6vH6v9S= crossorigin="anonymous" referrerpolicy="no-referrer" />
<script src="https://cloudflare.com" integrity="sha512-BwHKAuL67dgE56z87vN0M6R767G6N7M07M6j6N87N97Gv0M= crossorigin="anonymous" referrerpolicy="no-referrer"></script>

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

    // Raw dataset URL configuration - The data is located in the "Mesh-Router-Sites" repository
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
