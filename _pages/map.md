---
title: "Network Map"
permalink: /map/
excerpt: "Live mesh router nodes pulled from the [Mesh-Router-Sites](https://github.com/Temagami-Mesh/Mesh-Router-Sites) repository."
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---



<div id="map" style="height: 70vh; min-height: 520px; width: 100%; border: 1px solid #ccc; margin: 20px 0; border-radius: 4px; z-index: 1;"></div>

<!-- Local Leaflet assets -->
<link rel="stylesheet" href="{{ site.baseurl }}/assets/leaflet/leaflet.css" />
<script src="{{ site.baseurl }}/assets/leaflet/leaflet.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function () {
  const mapEl = document.getElementById('map');

  // Safety check
  if (typeof L === 'undefined') {
    mapEl.innerHTML = `
      <div style="padding: 24px; text-align: center; color: #721c24; background: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;">
        <strong>Map Loading Error</strong><br>
        Local Leaflet assets could not be found at <code>/assets/leaflet/</code>.
      </div>`;
    return;
  }

  // Create the map
  const map = L.map('map').setView([47.06, -79.80], 10);

  // Loading indicator
  const loadingControl = L.control({ position: 'topright' });
  loadingControl.onAdd = function () {
    const div = L.DomUtil.create('div', 'leaflet-bar');
    div.innerHTML = `<div style="padding:8px 12px; background:white; font-size:13px;">Loading nodes…</div>`;
    return div;
  };
  loadingControl.addTo(map);

  // Base map
  L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
    maxZoom: 17,
    attribution: 'Map data: &copy; OpenStreetMap contributors, SRTM | Map style: &copy; OpenTopoMap (CC-BY-SA)'
  }).addTo(map);

  // GeoJSON source
  const dataUrl = 'https://raw.githubusercontent.com/Temagami-Mesh/Mesh-Router-Sites/main/Mesh%20Router%20Sites.geojson';

  // Color helper
  function getColor(color) {
    const map = {
      yellow: '#f1c40f',
      Yellow: '#f1c40f',
      green:  '#28a745',
      red:    '#e74c3c',
      blue:   '#3498db',
      orange: '#fd7e14',
      black:  '#222222'
    };
    return map[color] || color || '#3388ff';
  }

  // Load data
  fetch(dataUrl)
    .then(response => {
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return response.json();
    })
    .then(geojsonData => {
      map.removeControl(loadingControl);

      const geojsonLayer = L.geoJSON(geojsonData, {
        pointToLayer: function (feature, latlng) {
          const color = getColor(feature.properties['marker-color']);

          return L.circleMarker(latlng, {
            radius: 9,
            fillColor: color,
            color: '#ffffff',
            weight: 2,
            opacity: 1,
            fillOpacity: 0.9
          });
        },

        onEachFeature: function (feature, layer) {
          const p = feature.properties || {};
          const name = p.name || 'Mesh Node';
          const status = p.status || 'Unknown';
          const hardware = p.hardware || '';

          let html = `<strong>${name}</strong><br>Status: ${status}`;
          if (hardware) html += `<br>Hardware: ${hardware}`;

          layer.bindPopup(html);
        }
      }).addTo(map);

      // Fit to data
      if (geojsonLayer.getLayers().length > 0) {
        map.fitBounds(geojsonLayer.getBounds(), {
          padding: [50, 50],
          maxZoom: 12
        });
      }
    })
    .catch(error => {
      console.error(error);
      map.removeControl(loadingControl);

      const errorControl = L.control({ position: 'topright' });
      errorControl.onAdd = function () {
        const div = L.DomUtil.create('div', 'leaflet-bar');
        div.style.padding = '10px 14px';
        div.style.background = '#f8d7da';
        div.style.color = '#721c24';
        div.innerHTML = `<strong>Unable to load data</strong><br>${error.message}`;
        return div;
      };
      errorControl.addTo(map);
    });
});
</script>
