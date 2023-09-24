<script setup lang="ts">
import { ref } from "vue";
import { API_KEY, BASE_URL } from "@/constants/";
import "leaflet/dist/leaflet.css";

const mapInfo = ref({
  url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
  attribution:
    '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
  zoom: 3,
  center: [43.6135, -116.2035] as [number, number],
  markerLatLng: [43.6135, -116.2035] as [number, number],
  caller: null as string | null,
  circle: {
    center: [43.6135, -116.2035] as [number, number],
    radius: 1600,
    color: "red",
  },
});

const city = ref("");
const locationInfo = ref(null);

const getlocation = () => {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => {
      locationInfo.value = data;
      addLocation(data.coord.lon, data.coord.lat);
    });
};

const addLocation = (a: number, b: number) => {
  console.log( mapInfo.value.center);
  
  mapInfo.value.center = [b, a];
  mapInfo.value.circle.center = [b, a];
  mapInfo.value.markerLatLng = [b, a];
  mapInfo.value.zoom = 8;
};


</script>

<template>
  <h1 class="text-center  font-bold text-4xl my-10">
    Please enter the city
  </h1>
  <UInput v-model="city" :myFunction="getlocation" />

  <l-map
    style="height: 75vh; width: 100%"
    :zoom="mapInfo.zoom"
    :center="mapInfo.center"
  >
    <l-marker
      :lat-lng="mapInfo.markerLatLng"
    ></l-marker>
    <l-tile-layer
      :url="mapInfo.url"
      :attribution="mapInfo.attribution"
    ></l-tile-layer>
    <l-circle
      :lat-lng="mapInfo.circle.center"
      :radius="mapInfo.circle.radius"
      :color="mapInfo.circle.color"
    />
  </l-map>
</template>
