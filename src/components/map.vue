<template id="child-map">
  <div id="map" class="mystyle"></div>
</template>

<script>
import BMap from 'BMap'

export default{
  data () {
    return {

    }
  },
  props: ['m'],
  mounted () {
    this.mapInit()
  },
  methods: {
    mapInit () {
      const _this = this
      const map = new BMap.Map('map')
      _this.map = map
      map.centerAndZoom(new BMap.Point(116.404, 39.915), 15)
      map.enableScrollWheelZoom()
    }
  }
}
</script>
<style>
.mystyle {
width: 100%;
height: 600px;
border: 1px solid #eee;
}

</style>
