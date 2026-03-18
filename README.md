<script>

// =========================
// 📊 LIVE SURVEY DATA (SIMULATION)
// =========================

// initial values
let surveyData = [4.6, 4.5, 4.7, 4.4, 4.8, 4.5];

const ctx = document.getElementById('surveyChart');

const chart = new Chart(ctx, {
    type: 'bar',
    data: {
        labels: [
            'Financial Integrity',
            'Gatekeeping',
            'Bayanihan Ledger',
            'GIS Routing',
            'Data Integrity',
            'Accountability'
        ],
        datasets: [{
            label: 'Average Agreement Score (1-5)',
            data: surveyData
        }]
    },
    options: {
        responsive:true,
        scales: {
            y: {
                beginAtZero: true,
                max: 5
            }
        }
    }
});

// 🔄 Simulate LIVE updates every 3 seconds
setInterval(() => {
    surveyData = surveyData.map(val => {
        let change = (Math.random() * 0.2 - 0.1); // small variation
        let newVal = val + change;
        return Math.min(5, Math.max(3.5, newVal)); // keep realistic
    });

    chart.data.datasets[0].data = surveyData;
    chart.update();

}, 3000);


// =========================
// 🗺 LIVE MAP (FIRE + JAIL)
// =========================

const map = L.map('map').setView([12.8797,121.7740],6);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution:'Map data © OpenStreetMap contributors'
}).addTo(map);


// 🔥 Fire Stations
const fireIcon = L.icon({
    iconUrl: 'https://cdn-icons-png.flaticon.com/512/482/482132.png',
    iconSize: [30,30]
});

// 🔒 Jail Facilities
const jailIcon = L.icon({
    iconUrl: 'https://cdn-icons-png.flaticon.com/512/565/565547.png',
    iconSize: [30,30]
});


// 🔥 FIRE STATIONS DATA
const fireStations = [
    {name: "Manila Fire Station", coords: [14.5995,120.9842]},
    {name: "Cebu Fire Station", coords: [10.3157,123.8854]},
    {name: "Davao Fire Station", coords: [7.1907,125.4553]},
    {name: "Baguio Fire Station", coords: [16.4023,120.5960]}
];

// 🔒 JAIL FACILITIES DATA
const jailFacilities = [
    {name: "Manila City Jail", coords: [14.6170,120.9830]},
    {name: "Cebu City Jail", coords: [10.2920,123.9020]},
    {name: "Davao City Jail", coords: [7.0731,125.6128]},
    {name: "Laguna Provincial Jail", coords: [14.2770,121.4120]}
];


// ADD FIRE STATIONS
fireStations.forEach(station => {
    L.marker(station.coords, {icon: fireIcon})
        .addTo(map)
        .bindPopup("🔥 " + station.name);
});

// ADD JAIL FACILITIES
jailFacilities.forEach(jail => {
    L.marker(jail.coords, {icon: jailIcon})
        .addTo(map)
        .bindPopup("🔒 " + jail.name);
});


// =========================
// 🔄 OPTIONAL: LIVE MAP ACTIVITY SIMULATION
// =========================

// randomly highlight a location every 4 seconds
setInterval(() => {
    const allLocations = [...fireStations, ...jailFacilities];
    const random = allLocations[Math.floor(Math.random() * allLocations.length)];

    L.popup()
        .setLatLng(random.coords)
        .setContent("📍 Activity detected at: " + random.name)
        .openOn(map);

}, 4000);

</script>
