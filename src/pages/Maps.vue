<template>
    <card type="plain" title="Google Maps">
      <div id="map" class="map">
      </div>
    </card>
</template>
<script>
export default {
  mounted() {
    let myLatlng = new window.google.maps.LatLng(40.748817, -73.985428);
    let recaptchaScript = document.createElement('script');
    recaptchaScript.setAttribute('src', 'https://unpkg.com/leaflet@1.6.0/dist/leaflet.js')
    document.head.appendChild(recaptchaScript);
    var map = L.map('map').setView([51.505, -0.09], 13);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    L.marker([51.5, -0.09]).addTo(map)
        .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
        .openPopup();
    }
};
</script>
<style>
#map { height: 180px; }
</style>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>