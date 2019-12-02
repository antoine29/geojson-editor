<template>
  <div>
    <div v-if="!validGeoJson">
      <h1>Please draw a valid route (line)</h1>
      <ol>
        <li>The geoJson code should contain only a 'type' field equal to "FeatureCollection" </li>
        <li>The geoJson code should contain only a 'features' array field with length equal to 1</li>
        <li>The first object in 'features' array should have a 'geometry.type' field equals to "LineString"</li>
      </ol>
    </div>
    <div v-if="validGeoJson">
        <p>
          <label for="Route">Route*</label>
          <input
            id="Route"
            type="text"
            name="Route"
            placeholder="A code or short name to identify this route"
            v-on:change="updateFields"
          >
        </p>

        <p>
          <label for="Author">Author</label>
          <input
            id="Author"
            type="text"
            name="Author"
            placeholder="(Optional) Your name"
            v-on:change="updateFields"
          >
        </p>
    </div>
  </div>
</template>

<script>
// @input="updateLocalUser($event);" 
import store from '../store'

export default {
  name: 'Form',
  data() {
    return {
      errors:[]
    }
  },
  computed: {
    validGeoJson: function() {
      return this.$store.getters.geojson.features.length === 1
        && this.$store.getters.geojson.features[0].type === "Feature"
        && this.$store.getters.geojson.features[0].geometry.type === "LineString";
    },
    getAgency: function() {
      return this.$store.getters.geojson.features[0].properties.agency
        || this.$store.getters.geojson.features[0].properties.agencia
        || "";
    }
  },
  methods: {
    checkForm: function(e) {
      if(this.agency && this.author) return true;
      this.errors = [];
      if(!this.route) this.errors.push("Route identifier is required.");
      e.preventDefault();
    },
    updateFields: function(event) {
      let aditionalProps = this.$store.getters.aditionalProps;
      aditionalProps[event.target.name+''] = event.target.value+''; 
      store.commit('addAditionalProp', aditionalProps);

      // let geoJsonObject = this.$store.getters.geojson;
      // if (geoJsonObject.features.length === 1
      //   && geoJsonObject.features[0].type === "geometry") {
      //     geoJsonObject.features[0].properties = aditionalProps; 
      //     store.commit('setGeoJSON', geoJsonObject);
      // }
    }
  }
}
</script>

<style>
  input:invalid {
    background-color: #ffdddd;
  }

  input:required {
    border-color: #800000;
    border-width: 1px;
  }
</style>