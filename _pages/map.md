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

Live mesh router nodes pulled from the [Mesh-Router-Sites](https://github.com/Temagami-Mesh/Mesh-Router-Sites) repository.

<div id="map" style="height: 70vh; min-height: 520px; width: 100%; border: 1px solid #ccc; margin: 20px 0; border-radius: 4px; z-index: 1;"></div>

<div id="map-legend" style="display:none; margin-bottom: 1.5rem; font-size: 0.95em;">
  Markers use the <code>marker-symbol</code> and <code>marker-color</code> properties from the GeoJSON.
</div>

<!-- Local Leaflet assets -->
<link rel="stylesheet" href="{{ site.baseurl }}/assets/leaflet/leaflet.css" />
<script src="{{ site.baseurl }}/assets/leaflet/leaflet.js"></script>

<script>
document.addEventListener("DOMContentLoaded", function () {
  const mapEl = document.getElementById('map');
  const legendEl = document.getElementById('map-legend');

  // Safety check for local Leaflet
  if (typeof L === 'undefined') {
    mapEl.innerHTML = `
      <div style="padding: 24px; text-align: center; color: #721c24; background: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;">
        <strong>Map Loading Error</strong><br>
        Local Leaflet assets could not be found at <code>/assets/leaflet/</code>.
      </div>`;
    return;
  }

  // Loading state
  mapEl.innerHTML = `
    <div style="display:flex; align-items:center; justify-content:center; height:100%; background:#f8f9fa; color:#555;">
      Loading mesh node data…
    </div>`;

  // Initialize map
  const map = L.map('map').setView([47.06, -79.80], 10);

  // Topographic tiles
  L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
    maxZoom: 17,
    attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, SRTM | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (CC-BY-SA)'
  }).addTo(map);

  // GeoJSON source
  const dataUrl = 'https://raw.githubusercontent.com/Temagami-Mesh/Mesh-Router-Sites/main/Mesh%20Router%20Sites.geojson';

  // ---------- Icon factory ----------
  function createMakiIcon(symbol, color) {
    // Normalize color names to hex
    const colorMap = {
      yellow: '#f1c40f',
      green:  '#28a745',
      red:    '#e74c3c',
      blue:   '#3498db',
      orange: '#fd7e14',
      black:  '#222222',
      white:  '#ffffff'
    };
    color = colorMap[color] || color || '#3388ff';

    // Approximate hue-rotate for black Maki SVGs
    // (For perfect colors, edit each SVG once and set fill="currentColor")
    const hueMap = {
      '#f1c40f': '48deg',
      '#28a745': '100deg',
      '#e74c3c': '0deg',
      '#3498db': '200deg',
      '#fd7e14': '28deg',
      '#3388ff': '210deg'
    };
    const hue = hueMap[color.toLowerCase()] || '210deg';

    // Default symbol if none is provided
    symbol = symbol || 'communications-tower';

    return L.divIcon({
      className: 'maki-marker',
      html: `
        <div style="
          width: 30px;
          height: 30px;
          display: flex;
          align-items: center;
          justify-content: center;
          filter: drop-shadow(0 1px 3px rgba(0,0,0,0.45));
        ">
          <img
            src="{{ site.baseurl }}/assets/maki/${symbol}.svg"
            alt="${symbol}"
            style="
              width: 24px;
              height: 24px;
              filter:
                brightness(0)
                saturate(100%)
                invert(1)
                sepia(1)
                saturate(10000%)
                hue-rotate(${hue})
                brightness(1.05);
            "
            onerror="this.style.display='none'"
          />
        </div>
      `,
      iconSize: [30, 30],
      iconAnchor: [15, 30],
      popupAnchor: [0, -28]
    });
  }

  // ---------- Load data ----------
  fetch(dataUrl)
    .then(response => {
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return response.json();
    })
    .then(geojsonData => {
      mapEl.innerHTML = '';   // clear loading message

      const geojsonLayer = L.geoJSON(geojsonData, {
        pointToLayer: function (feature, latlng) {
          const props = feature.properties || {};

          // Prefer marker-symbol (actual property in your GeoJSON),
          // fall back to marker-type, then to a default
          const symbol = props['marker-symbol'] || props['marker-type'] || 'communications-tower';
          const color  = props['marker-color']  || '#3388ff';

          return L.marker(latlng, {
            icon: createMakiIcon(symbol, color)
          });
        },

        onEachFeature: function (feature, layer) {
          const p = feature.properties || {};
          const name = p.name || p.Name || 'Mesh Node';
          const status = p.status || 'Unknown';
          const hardware = p.hardware || '';

          let html = `<strong>${name}</strong><br>`;
          html += `Status: ${status}`;
          if (hardware) html += `<br>Hardware: ${hardware}`;

          layer.bindPopup(html);
        }
      }).addTo(map);

      // Fit the view to the data
      if (geojsonLayer.getLayers().length > 0) {
        map.fitBounds(geojsonLayer.getBounds(), {
          padding: [50, 50],
          maxZoom: 12
        });
      }

      legendEl.style.display = 'block';
    })
    .catch(error => {
      console.error('Map loading error:', error);
      mapEl.innerHTML = `
        <div style="padding: 24px; text-align: center; color: #721c24; background: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;">
          <strong>Unable to load node data</strong><br>
          ${error.message}<br>
          <small>Check the <a href="https://github.com/Temagami-Mesh/Mesh-Router-Sites" target="_blank">data repository</a>.</small>
        </div>`;
    });
});
</script>
