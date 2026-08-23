<script>
document.addEventListener("DOMContentLoaded", function () {
  const mapEl = document.getElementById('map');
  const legendEl = document.getElementById('map-legend');

  if (typeof L === 'undefined') {
    mapEl.innerHTML = `
      <div style="padding: 24px; text-align: center; color: #721c24; background: #f8d7da; border: 1px solid #f5c6cb; border-radius: 4px;">
        <strong>Map Loading Error</strong><br>
        Local Leaflet assets could not be found at <code>/assets/leaflet/</code>.
      </div>`;
    return;
  }

  // Create the map first (container must stay intact)
  const map = L.map('map').setView([47.06, -79.80], 10);

  // Add a temporary loading control
  const loadingControl = L.control({ position: 'topright' });
  loadingControl.onAdd = function () {
    const div = L.DomUtil.create('div', 'leaflet-bar');
    div.innerHTML = `<div style="padding:8px 12px; background:white; font-size:13px;">Loading nodes…</div>`;
    return div;
  };
  loadingControl.addTo(map);

  // Base layer
  L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
    maxZoom: 17,
    attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, SRTM | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a> (CC-BY-SA)'
  }).addTo(map);

  const dataUrl = 'https://raw.githubusercontent.com/Temagami-Mesh/Mesh-Router-Sites/main/Mesh%20Router%20Sites.geojson';

  // ---------- Icon factory ----------
  function createMakiIcon(symbol, color) {
    const colorMap = {
      yellow: '#f1c40f',
      Yellow: '#f1c40f',
      green:  '#28a745',
      red:    '#e74c3c',
      blue:   '#3498db',
      orange: '#fd7e14',
      black:  '#222222',
      white:  '#ffffff'
    };
    color = colorMap[color] || color || '#3388ff';

    const hueMap = {
      '#f1c40f': '48deg',
      '#28a745': '100deg',
      '#e74c3c': '0deg',
      '#3498db': '200deg',
      '#fd7e14': '28deg',
      '#3388ff': '210deg'
    };
    const hue = hueMap[color.toLowerCase()] || '210deg';

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
      // Remove the loading control
      map.removeControl(loadingControl);

      const geojsonLayer = L.geoJSON(geojsonData, {
        pointToLayer: function (feature, latlng) {
          const props = feature.properties || {};
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
      map.removeControl(loadingControl);

      // Show error as a Leaflet control instead of destroying the map
      const errorControl = L.control({ position: 'topright' });
      errorControl.onAdd = function () {
        const div = L.DomUtil.create('div', 'leaflet-bar');
        div.style.padding = '10px 14px';
        div.style.background = '#f8d7da';
        div.style.color = '#721c24';
        div.style.maxWidth = '260px';
        div.innerHTML = `<strong>Unable to load node data</strong><br>${error.message}`;
        return div;
      };
      errorControl.addTo(map);
    });
});
</script>
