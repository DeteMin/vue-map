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
      _this.getPointMarker(new BMap.Point(116.404, 39.915))
    },
    // 获取坐标信息并标记
    getPointMarker (point) {
      const _this = this
      let marker = new BMap.Marker(point)
      let url = 'https://a.amap.com/jsapi_demos/static/video/cloud.mov'
      let content = "<video width='320' height='240' autoplay='autoplay' controls='controls'>" +
      '<source src=' + url + " type='video/mp4' />" + '</video>'
      _this.map.addOverlay(marker)
      marker.setAnimation(BMAP_ANIMATION_BOUNCE)
      // _this.setMarkerMenu(marker)
      marker.addEventListener('click', function (e) {
        console.log(content)
        var opts = {
          width: 500, // 信息窗口宽度
          height: 400, // 信息窗口高度
          title: '信息窗口', // 信息窗口标题
          enableMessage: true// 设置允许信息窗发送短息
        }
        let infoWindow = new BMap.InfoWindow(content, opts)
        _this.map.openInfoWindow(infoWindow, point) // 开启信息窗口
      })
      // window.onmousewheel=document.onmousewheel=_this.getScrollFunc();
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
