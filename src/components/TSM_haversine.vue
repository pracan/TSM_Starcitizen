<template>
  <div>
    <label for="point1">Point 1:</label>
    <input id="point1" v-model="point1" type="text" />
    <br />
    <label for="point2">Point 2:</label>
    <input id="point2" v-model="point2" type="text" />
    <br />
    <label for="radius">Planet radius:</label>
    <input id="radius" v-model="radius" type="number" step="any" />
    <br />
    <button @click="calculateDistance">Calculate Distance</button>
    <br />
    <div v-if="distance">
      The distance between the two points is {{ distance }} kilometers.
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      point1: null,
      point2: null,
      radius: null,
      distance: null,
    };
  },
  methods: {
    calculateDistance() {
      const planetRadius = this.radius;
      const [lat1, lon1] = this.parseLatLong(this.point1);
      const [lat2, lon2] = this.parseLatLong(this.point2);
      const dLat = this.degreesToRadians(lat2 - lat1);
      const dLon = this.degreesToRadians(lon2 - lon1);

      const a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(this.degreesToRadians(lat1)) *
          Math.cos(this.degreesToRadians(lat2)) *
          Math.sin(dLon / 2) *
          Math.sin(dLon / 2);

      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

      const distance = planetRadius * c;
      this.distance = distance.toFixed(2);
    },
    degreesToRadians(degrees) {
      return degrees * (Math.PI / 180);
    },
    parseLatLong(str) {
      const latLongPattern =
        /([NS])\s*(\d+)°\s*(\d+\.\d+)'\s*\/\s*([EW])\s*(\d+)°\s*(\d+\.\d+)'/;
      const match = str.match(latLongPattern);
      if (!match) {
        throw new Error('Invalid latitude/longitude format');
      }
      const [
        ,
        latDirection,
        latDegrees,
        latMinutes,
        lonDirection,
        lonDegrees,
        lonMinutes,
      ] = match;
      const latitude = this.convertDMSToDD(
        Number(latDegrees),
        Number(latMinutes),
        latDirection
      );
      const longitude = this.convertDMSToDD(
        Number(lonDegrees),
        Number(lonMinutes),
        lonDirection
      );
      return [latitude, longitude];
    },
    convertDMSToDD(degrees, minutes, direction) {
      let dd = degrees + minutes / 60;
      if (direction === 'S' || direction === 'W') {
        dd = -dd;
      }
      return dd;
    },
  },
};
</script>
