# Intership01
Internshipproject
Author - Shorya Singh

 <!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>⚡ Powercut Tracker</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css"/>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;700;800&display=swap');
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
--bg: #0f0f13;
--card: #1a1a22;
--border: #2e2e3e;
--accent: #f5c518;
--red: #ff4d4d;
--green: #39d98a;
--text: #e8e8f0;
--muted: #888;
}
body {
font-family: 'Syne', sans-serif;
background: var(--bg);
color: var(--text);
min-height: 100vh;
display: flex;
flex-direction: column;
}
header {
padding: 16px 24px;
display: flex;
align-items: center;
gap: 12px;
border-bottom: 1px solid var(--border);
background: var(--card);
}
header h1 { font-size: 1.3rem; font-weight: 800; letter-spacing: -0.5px; }
header span { font-size: 1.5rem; }
.live-badge {
margin-left: auto;
background: var(--red);
color: #fff;
font-family: 'Space Mono', monospace;
font-size: 0.65rem;
padding: 3px 8px;
border-radius: 20px;
animation: pulse 1.5s infinite;
}
@keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.5} }
.layout {
display: flex;
flex: 1;
gap: 0;
height: calc(100vh - 57px);
}
/* MAP */
#map {
flex: 1;
cursor: crosshair !important;
}
/* SIDEBAR */
.sidebar {
width: 300px;
background: var(--card);
border-left: 1px solid var(--border);
display: flex;
flex-direction: column;
overflow: hidden;
}
.sidebar-section {
padding: 16px;
border-bottom: 1px solid var(--border);
}
.sidebar-section h2 {
font-size: 0.7rem;
letter-spacing: 2px;
text-transform: uppercase;
color: var(--muted);
margin-bottom: 12px;
}
.form-row { display: flex; flex-direction: column; gap: 8px; margin-bottom: 10px; }
.form-row label { font-size: 0.75rem; color: var(--muted); font-family: 'Space Mono', monospace; }
input, select, textarea {
background: var(--bg);
border: 1px solid var(--border);
color: var(--text);
border-radius: 6px;
padding: 8px 10px;
font-family: 'Space Mono', monospace;
font-size: 0.75rem;
width: 100%;
outline: none;
transition: border 0.2s;
}
input:focus, select:focus, textarea:focus { border-color: var(--accent); }
textarea { resize: none; height: 60px; }
.coord-box {
background: var(--bg);
border: 1px dashed var(--border);
border-radius: 6px;
padding: 8px 10px;
font-family: 'Space Mono', monospace;
font-size: 0.7rem;
color: var(--accent);
min-height: 34px;
}
button {
width: 100%;
padding: 10px;
background: var(--accent);
color: #111;
border: none;
border-radius: 6px;
font-family: 'Syne', sans-serif;
font-weight: 700;
font-size: 0.85rem;
cursor: pointer;
transition: opacity 0.2s, transform 0.1s;
}
button:hover { opacity: 0.85; }
button:active { transform: scale(0.98); }
button.danger { background: var(--red); color: #fff; margin-top: 6px; }
/* REPORTS LIST */
.reports-list {
flex: 1;
overflow-y: auto;
padding: 12px;
display: flex;
flex-direction: column;
gap: 8px;
}
.reports-list::-webkit-scrollbar { width: 4px; }
.reports-list::-webkit-scrollbar-thumb { background: var(--border); border-radius: 4px; }
.report-card {
background: var(--bg);
border: 1px solid var(--border);
border-radius: 8px;
padding: 10px 12px;
cursor: pointer;
transition: border-color 0.2s;
}
.report-card:hover { border-color: var(--accent); }
.report-card .rc-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px; }
.report-card .rc-area { font-weight: 700; font-size: 0.85rem; }
.report-card .rc-status {
font-size: 0.6rem;
font-family: 'Space Mono', monospace;
padding: 2px 6px;
border-radius: 20px;
background: var(--red);
color: #fff;
}
.report-card .rc-status.resolved { background: var(--green); color: #111; }
.report-card .rc-meta { font-size: 0.65rem; color: var(--muted); font-family: 'Space Mono', monospace; }
.empty { text-align: center; color: var(--muted); font-size: 0.8rem; padding: 24px; font-family: 'Space Mono', monospace; }
/* Stats bar */
.stats-bar {
display: flex;
gap: 0;
border-bottom: 1px solid var(--border);
}
.stat {
flex: 1;
text-align: center;
padding: 10px 6px;
border-right: 1px solid var(--border);
}
.stat:last-child { border-right: none; }
.stat .num { font-size: 1.3rem; font-weight: 800; color: var(--accent); }
.stat .lbl { font-size: 0.6rem; color: var(--muted); text-transform: uppercase; letter-spacing: 1px; font-family: 'Space Mono', monospace; }
/* Custom marker */
.cut-marker {
width: 16px; height: 16px;
background: var(--red);
border-radius: 50%;
border: 3px solid #fff;
box-shadow: 0 0 10px rgba(255,77,77,0.7);
animation: pulse-marker 1.5s infinite;
}
.cut-marker.resolved { background: var(--green); box-shadow: 0 0 10px rgba(57,217,138,0.5); animation: none; }
@keyframes pulse-marker { 0%,100%{box-shadow:0 0 6px rgba(255,77,77,0.7)} 50%{box-shadow:0 0 16px rgba(255,77,77,1)} }
</style>
</head>
<body>
<header>
<span>⚡</span>
<h1>Powercut Tracker</h1>
<div class="live-badge">LIVE</div>
</header>
Your paragraph text<div class="layout">
<div id="map"></div>
<div class="sidebar">
<!-- Stats -->
<div class="stats-bar">
<div class="stat"><div class="num" id="s-total">0</div><div class="lbl">Total</div></div>
<div class="stat"><div class="num" id="s-active" style="color:var(--red)">0</div><div class="lbl">Active</div></div>
<div class="stat"><div class="num" id="s-resolved" style="color:var(--green)">0</div><div class="lbl">Resolved</div></div>
</div>
<!-- Report Form -->
<div class="sidebar-section">
<h2>Report Powercut</h2>
<div class="form-row">
<label>AREA NAME</label>
<input id="areaName" type="text" placeholder="e.g. Malviya Nagar"/>
</div>
<div class="form-row">
<label>CLICK MAP TO SELECT LOCATION</label>
<div class="coord-box" id="coordBox">No location selected</div>
</div>
<div class="form-row">
<label>SEVERITY</label>
<select id="severity">
<option value="Low">Low</option>
<option value="Medium" selected>Medium</option>
<option value="High">High</option>
</select>
</div>
<div class="form-row">
<label>NOTE (OPTIONAL)</label>
<textarea id="note" placeholder="Any details..."></textarea>
</div>
<button onclick="addReport()">+ Add Report</button>
</div>
<!-- Reports list -->
<div class="sidebar-section" style="padding-bottom:8px; flex:1; display:flex; flex-direction:column; overflow:hidden;">
<h2>Reports</h2>
<div class="reports-list" id="reportsList">
<div class="empty">Click on the map to pin a powercut location</div>
</div>
</div>
</div>
</div>
<script>
// Init map centered on Jaipur
const map = L.map('map').setView([26.9124, 75.7873], 12);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
attribution: '© OpenStreetMap'
}).addTo(map);
let selectedLatLng = null;
let tempMarker = null;
let reports = [];
// Click map to select location
map.on('click', e => {
selectedLatLng = e.latlng;
document.getElementById('coordBox').textContent =
`${e.latlng.lat.toFixed(4)}, ${e.latlng.lng.toFixed(4)}`;
if (tempMarker) map.removeLayer(tempMarker);
tempMarker = L.marker(e.latlng, {
icon: L.divIcon({ className: '', html: '<div class="cut-marker" style="background:#f5c518;box-shadow:0 0 10px rgba(245,197,24,0.8)"></div>', iconSize: [16,16], iconAnchor: [8,8] })
}).addTo(map).bindPopup('📍 Selected location').openPopup();
});
function makeIcon(resolved) {
return L.divIcon({
className: '',
html: `<div class="cut-marker ${resolved ? 'resolved' : ''}"></div>`,
iconSize: [16, 16],
iconAnchor: [8, 8]
});
}
function addReport() {
const area = document.getElementById('areaName').value.trim();
if (!area) return alert('Please enter an area name.');
if (!selectedLatLng) return alert('Please click on the map to select a location.');
const report = {
id: Date.now(),
area,
lat: selectedLatLng.lat,
lng: selectedLatLng.lng,
severity: document.getElementById('severity').value,
note: document.getElementById('note').value.trim(),
time: new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'}),
resolved: false
};
// Add permanent marker
report.marker = L.marker([report.lat, report.lng], { icon: makeIcon(false) })
.addTo(map)
.bindPopup(`<b>${report.area}</b><br>Severity: ${report.severity}<br>${report.note || ''}`);
reports.push(report);
// Cleanup temp
if (tempMarker) { map.removeLayer(tempMarker); tempMarker = null; }
selectedLatLng = null;
document.getElementById('coordBox').textContent = 'No location selected';
document.getElementById('areaName').value = '';
document.getElementById('note').value = '';
renderList();
updateStats();
}
function toggleResolve(id) {
const r = reports.find(x => x.id === id);
if (!r) return;
r.resolved = !r.resolved;
r.marker.setIcon(makeIcon(r.resolved));
renderList();
updateStats();
}
function deleteReport(id) {
const idx = reports.findIndex(x => x.id === id);
if (idx === -1) return;
map.removeLayer(reports[idx].marker);
reports.splice(idx, 1);
renderList();
updateStats();
}
function renderList() {
const el = document.getElementById('reportsList');
if (!reports.length) { el.innerHTML = '<div class="empty">No reports yet</div>'; return; }
el.innerHTML = reports.slice().reverse().map(r => `
<div class="report-card" onclick="map.flyTo([${r.lat},${r.lng}],15)">
<div class="rc-top">
<span class="rc-area">${r.area}</span>
<span class="rc-status ${r.resolved ? 'resolved' : ''}">${r.resolved ? 'Resolved' : 'Active'}</span>
</div>
<div class="rc-meta">${r.severity} · ${r.time}${r.note ? ' · ' + r.note : ''}</div>
<div style="display:flex;gap:6px;margin-top:8px">
<button style="font-size:0.7rem;padding:5px;background:${r.resolved?'var(--red)':'var(--green)'};color:${r.resolved?'#fff':'#111'}"
onclick="event.stopPropagation();toggleResolve(${r.id})">
${r.resolved ? 'Mark Active' : '✓ Resolve'}
</button>
<button style="font-size:0.7rem;padding:5px;background:#333;color:var(--red);width:36px"
onclick="event.stopPropagation();deleteReport(${r.id})">✕</button>
</div>
</div>`).join('');
}
function updateStats() {
const active = reports.filter(r => !r.resolved).length;
document.getElementById('s-total').textContent = reports.length;
document.getElementById('s-active').textContent = active;
document.getElementById('s-resolved').textContent = reports.length - active;
}
</script>
</body>
</html>


