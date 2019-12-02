<template>
  <Row id="bottomMenu">
    <!-- <Button class="pad" @click="copy">
      Copy to clipboard
    </Button> -->

    <Button @click="saveToGeojson">
      Save as geojson
    </Button>
    <!-- <ButtonGroup>
      <Dropdown trigger="click" placement="top-end" @on-click="saveInFormats">
        <a href="javascript:void(0)">
          <Button icon="arrow-up-b" />
        </a>
        <DropdownMenu slot="list">
          <DropdownItem v-for="format in supportedFormats" :key="format.value" :name="format.value">
            {{ format.label }}
          </DropdownItem>
        </DropdownMenu>
      </Dropdown>
    </ButtonGroup> -->
  </Row>
</template>

<script>
import FileSaver from 'file-saver'
import { topology } from 'topojson-server'
import wkt from 'wellknown'
import shape from 'shp-write'
import store from '../store'

export default {
  name: 'BottomMenu',
  data () {
    return {
      supportedFormats: [{
        label: 'Shapefile',
        value: 'shp'
      },
      {
        label: 'TopoJSON',
        value: 'topojson'
      },
      {
        label: 'WKT',
        value: 'wkt'
      }
      ]
    }
  },
  methods: {
    copy: function () {
      const el = document.createElement('textarea');
      el.value = this.$store.state.geojson;
      document.body.appendChild(el);
      el.select();
      document.execCommand('copy');
      document.body.removeChild(el);

      this.$Notice.open({
        title: 'Copied to clipboard',
        duration: 2.5,
      })

    },
    saveToGeojson: function () {
      if (this.$store.getters.aditionalProps.length === 0 ) {
        alert('you must fill at least a name for this route');
        return;
      }
    
      if (this.$store.getters.geojson.features.length === 1
        && this.$store.getters.geojson.features[0].geometry.type === "LineString"
      ) {
        
        let geoJsonObject = this.$store.getters.geojson;
        let aditionalProps = this.$store.getters.aditionalProps;
        geoJsonObject.features[0].properties = aditionalProps;
        store.commit('setGeoJSON', geoJsonObject);

        console.log("FINAL GEOJSON ");
        console.log(this.$store.getters.geojson);

        var file = new File([JSON.stringify(this.$store.getters.geojson)], "exported.geojson", {
          type: "text/plain;charset=utf-8"
        });
        FileSaver.saveAs(file);
        // var file = new File([this.code], "exported.geojson", {
        //   type: "text/plain;charset=utf-8"
        // });
        // FileSaver.saveAs(file);

        }
        else {
          alert('you must have one route drawed');
          return;
        }
        

        
      // }
    },
    saveInFormats: function (e) {
      let outData = null
      let outName = e
      if (e === 'topo') {
        outData = topojson.topology(this.$store.state.geojsonString)
      }
      if (e === 'wkt') {
        outData = wkt.stringify({
          type: 'GeometryCollection',
          geometries: this.$store.getters.geojson.features.map(function (f) {
            return f.geometry
          })
        })
      }
      if (e == 'shp') {
        var options = {
            folder: 'myshapes',
            types: {
                point: 'mypoints',
                polygon: 'mypolygons',
                line: 'mylines'
            }
        }
        shape.download(this.$store.getters.geojson, options)
        return
      }

      var file = new File([JSON.stringify(outData)], `export.${outName}`, {
        type: "text/plain;charset=utf-8"
      });
      FileSaver.saveAs(file);
    }
  }
}
</script>

<style>
#bottomMenu{
  position: absolute;
  height: 50px;
  bottom: 0px;
  padding: 10px;
  background: #F3F3F3;
  width: 100%;
  text-align: right;
  z-index: 1000;
}
.pad{
  margin-right: 10px;
}
.ivu-notice {
  top: calc(100vh - 115px)!important;
  width: 200px;
}
.ivu-notice-with-normal:after{
  background: #BFC0C0;
}
</style>
