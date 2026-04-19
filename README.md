# SounnyMap – Online GIS Mapping Platform

A modern, lightweight GIS web platform built with plain HTML, CSS, and vanilla JavaScript. Runs entirely as a static site on **GitHub Pages** – no server required.

---

## Features

| Feature | Description |
|---|---|
| 🗺 **Basemap Switcher** | Toggle between a street map (OpenFreeMap) and satellite imagery (ESRI World Imagery) |
| 📂 **GeoJSON Loader** | Upload local `.geojson` / `.json` files via file picker or drag-and-drop |
| 📋 **Layer Management** | Toggle visibility, zoom to extent, or remove any loaded layer |
| 💬 **Feature Popups** | Click any feature to inspect its properties |
| 🔍 **Map Controls** | Zoom in/out buttons, compass, and a live scale bar |
| 🖨 **Layout / Export** | Full layout editor – add title, legend, north arrow, scale bar, and sources – then export to PDF |
| 🌍 **EO / Agent Hooks** | Built-in extension points for connecting Earth Observation raster layers and live agent data feeds |

---

## Project Structure

```
map/
├── index.html   – Full-screen map layout (header + sidebar + map)
├── style.css    – Dark-theme GIS interface styles
├── app.js       – Core application logic (map, layers, layout, export)
└── README.md    – This file
```

---

## Running Locally

You need a simple local HTTP server (browsers block `file://` for security reasons).

### Option A – Python (built in on most systems)

```bash
# Python 3
cd map/
python3 -m http.server 8080

# Python 2
python -m SimpleHTTPServer 8080
```

Then open **http://localhost:8080** in your browser.

### Option B – Node.js `serve`

```bash
npm install -g serve
serve .
```

Then open the URL shown in the terminal.

### Option C – VS Code Live Server

Install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click `index.html`, and choose **"Open with Live Server"**.

---

## Deploying to GitHub Pages

1. Push this repository to GitHub.
2. Go to **Settings → Pages**.
3. Set **Source** to the `main` branch, root folder (`/`).
4. GitHub Pages will publish the site at `https://<username>.github.io/<repo>/`.

No build step needed – the site is pure static HTML/CSS/JS.

---

## Using the Map

### Loading Data
- Click **Browse Files** in the sidebar (or drag a `.geojson` file onto the map or the upload area).
- Each layer gets an auto-assigned colour and is listed in the **Layers** panel.
- Click the **⊕** button on any layer to zoom to it; the **👁** button toggles visibility; **✕** removes it.

### Switching Basemaps
- Click **Street** or **Satellite** in the **Basemap** panel.

### Layout & PDF Export
1. Click **Layout / Export** in the top header.
2. In the **Layout Editor** panel (bottom-right), configure:
   - **Map Title** – enter a title and check the box to show it.
   - **Legend** – automatically populated from loaded layers.
   - **North Arrow** – toggle the compass indicator.
   - **Scale Bar** – shows the current map scale.
   - **Data Sources** – enter attribution text.
3. Click **Export to PDF** to open the browser print dialog.
4. In the dialog, set the **Destination** to **Save as PDF**, choose **Landscape** orientation, and click **Save**.

---

## Connecting Earth Observation Data & Agent Tools

`app.js` exposes a global `window.SounnyMap` object with three key methods:

### Add a GeoJSON layer programmatically

```javascript
// Load a GeoJSON object directly (e.g. from an API response)
const geojson = await fetch('https://my-api.example.com/features').then(r => r.json());
window.SounnyMap.loadGeoJSON('My Layer Name', geojson);
```

### Update a layer with new data (live feeds)

```javascript
// Update an existing layer without removing it (good for real-time data)
const newData = await fetch('https://my-api.example.com/live').then(r => r.json());
window.SounnyMap.updateLayerData('layer-1', newData);
```

### Add a raster tile layer (WMS / TMS / XYZ)

```javascript
// NASA GIBS MODIS True Colour (example)
window.SounnyMap.addRasterLayer(
  'modis-terra',
  'https://gibs.earthdata.nasa.gov/wmts/epsg3857/best/' +
  'MODIS_Terra_CorrectedReflectance_TrueColor/default/2024-01-01/GoogleMapsCompatible/{z}/{y}/{x}.jpg',
  { opacity: 0.9, attribution: 'NASA GIBS' }
);

// Sentinel-2 via a custom tile server
window.SounnyMap.addRasterLayer('sentinel2', 'https://my-tileserver.io/s2/{z}/{x}/{y}.png');
```

### Direct MapLibre access

```javascript
// The raw MapLibre GL JS map instance is exposed for advanced use
const map = window.SounnyMap.map;

// Add a custom layer, listen to events, change the camera, etc.
map.flyTo({ center: [34.0, 31.5], zoom: 8 });
map.on('click', (e) => console.log('Clicked:', e.lngLat));
```

---

## Technology Stack

| Library | Purpose | Link |
|---|---|---|
| **MapLibre GL JS 4** | WebGL map rendering engine | https://maplibre.org |
| **OpenFreeMap** | Free vector tile basemap (street) | https://openfreemap.org |
| **ESRI World Imagery** | Free satellite raster tiles | https://www.esri.com |

All libraries are loaded from CDN – no `npm install` needed.

---

## Browser Support

Any modern browser with WebGL support: Chrome, Firefox, Edge, Safari 15+.

---

## License

MIT – see `LICENSE`.
