<template>
    <div class="map-wrapper">
        <l-map id="map" :zoom="zoom" :center="center" :options="mapOptions">
            <l-tile-layer :url="url" :attribution="attribution" />
            <l-marker v-for="(marker, i) in markers" :key="i" :lat-lng="[marker.lat, marker.long]">
                <l-popup>
                    <ul>
                        <li>Altitude : {{ marker.alt.toFixed(2) }}</li>
                        <li>Latitude : {{ marker.lat.toFixed(2) }}</li>
                        <li>Longtude : {{ marker.long.toFixed(2) }}</li>
                        <li>PPM1: {{ marker.pm1.toFixed(2) }}</li>
                        <li>PPM10 : {{ marker.pm10.toFixed(2) }}</li>
                        <li>PPM25 : {{ marker.pm25.toFixed(2) }}</li>
                    </ul>
                </l-popup>
            </l-marker>
            <l-polyline :lat-lngs="polylines" color="blue" />
        </l-map>
    </div>
</template>

<script lang="ts">
/* eslint-disable */
import { Prop, Vue, Component } from 'vue-property-decorator';
import { PropType } from 'vue';
import { latLng } from 'leaflet';
import { LMap, LTileLayer, LMarker, LPopup, LTooltip, LPolyline } from 'vue2-leaflet';
import { Coordinate } from '../types';

@Component({
    components: {
        LMap,
        LTileLayer,
        LMarker,
        LPopup,
        LTooltip,
        LPolyline,
    },
})
export default class MapCompontent extends Vue {
    // Props
    @Prop({ required: true, type: Array as PropType<Coordinate[]> })
    public markers!: Coordinate[];

    // Data
    showMap = false;

    zoom = 16;

    center = latLng(50.953245, 5.354043);

    url = 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png';

    attribution = '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors';

    mapOptions = {
        zoomSnap: 0.5,
    };

    get polylines() {
        /* eslint-disable */
        const filtered: any = [];
        this.markers.forEach((marker) => filtered.push([marker.lat, marker.long]));
        return filtered;
    }
}
</script>

<style scoped>
.map-wapper {
    position: relative;
    width: 100%;
}
#map {
    height: 100%;
    z-index: 0;
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
}
</style>
