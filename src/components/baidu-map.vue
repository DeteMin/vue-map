<template>
	<div id="baiduMap" class="mystyle">
	</div>
	
</template>

<script>
export default {
	mounted() {
		this.mapInit()
		/*注册事件*/
	    // if(document.addEventListener){
	    // 	console.log('mounted')
	    //     document.addEventListener('DOMMouseScroll',this.onMouseWheel,false);
		// }//W3C
		
		// let oDiv = document.getElementById('app')
		// this.addEvent(oDiv,'mousewheel',this.onMouseWheel)
		// this.addEvent(oDiv,'DOMMouseScroll',this.onMouseWheel)
	},
	data() {
		return {
			map: ''
		}
	},
	props: [
		'centers'
	],
	methods: {
		//初始化地图
		mapInit(){
			const _this = this
			const map = new BMap.Map('baiduMap')
			_this.map = map
			map.centerAndZoom(new BMap.Point(116.404, 39.915),15)
			map.enableScrollWheelZoom()
			_this.getPointMarker(new BMap.Point(116.404, 39.915))
			_this.setTileLayer()
		},
		
		//获取坐标信息并标记
		getPointMarker(point){
			const _this = this
			let marker = new BMap.Marker(point)
			let url = 'https://a.amap.com/jsapi_demos/static/video/cloud.mov'
			let content = "<video width='320' height='240' autoplay='autoplay' controls='controls'>"+
					"<source src="+url+" type='video/mp4' />"+
					"</video>"
			_this.map.addOverlay(marker);
			marker.setAnimation(BMAP_ANIMATION_BOUNCE);
			_this.setMarkerMenu(marker);
			marker.addEventListener('click',function(e){
				console.log(content)
				var opts = {
	                width : 500,     // 信息窗口宽度
	                height: 400,     // 信息窗口高度
	                title : "信息窗口" , // 信息窗口标题
	                enableMessage:true//设置允许信息窗发送短息
	            };
				let infoWindow = new BMap.InfoWindow(content,opts)
				_this.map.openInfoWindow(infoWindow,point); //开启信息窗口
			})
//			window.onmousewheel=document.onmousewheel=_this.getScrollFunc();
			
		},
		
		//设置标记点的右击事件
		setMarkerMenu(marker){
			const _this = this
			let markerMenu = new BMap.ContextMenu();
			markerMenu.addItem(new BMap.MenuItem('打开建筑构造图',function(e){
				// let opts = {
	            //     width : 500,     // 信息窗口宽度
	            //     height: 400,     // 信息窗口高度
	            //     title : "" , // 信息窗口标题
	            //     enableMessage:true//设置允许信息窗发送短息
	            // };
	            
//	            let content = "<div id='ThreeJS' style='position: absolute; left: 0px; top: 0px'></div>"+
//	            	"<script>"+
//	            	"var scene, camera, renderer, controls,tween,door; "+
//	            	"var keyboard = new THREEx.KeyboardState();"+
//	            	"var clock = new THREE.Clock();"+
//	            	"var SCREEN_WIDTH = window.innerWidth, SCREEN_HEIGHT = window.innerHeight;"+
//	            	"var VIEW_ANGLE = 75, ASPECT = SCREEN_WIDTH / SCREEN_HEIGHT, NEAR = 0.1, FAR = 10000;"+
//	            	"var materialArrayA = [];var materialArrayB = [];var matArrayA=[];var matArrayB = [];var dummy= new THREE.Object3D();init();animate();";
//	            let content = "<iframe scrolling=auto src='../pages/room.html'/>"
	            // let content = '<object type="text/html" data="http://127.0.0.1:8020/vue-map/static/pages/test.html" width="100%" height="350px"></object>';
	            // let infoWindow = new BMap.InfoWindow(content,opts)
	            // console.log(infoWindow)
				// marker.openInfoWindow(infoWindow); //开启信息窗口

				window.location.href="http://127.0.0.1:8020/vue-map/static/pages/room.html"
			}));
			marker.addContextMenu(markerMenu);
		},
		
		//获取地图缩放级别
		getScrollFunc(){
			const _this = this

			let zoomsize = _this.map.getZoom();
			console.log(zoomsize)
			while(zoomsize > 15 ){
				let marker = new BMap.Marker(new BMap.Point(116.404, 39.915))
				_this.map.addOverlay(marker);
				let opts = {
	                width : 500,     // 信息窗口宽度
	                height: 400,     // 信息窗口高度
	                title : "" , // 信息窗口标题
	                enableMessage:true//设置允许信息窗发送短息
	            };
	            let content = '<object type="text/html" data="http://127.0.0.1:8020/vue-map/static/pages/test.html" width="100%" height="350px"></object>';
	            let infoWindow = new BMap.InfoWindow(content,opts)
	            marker.openInfoWindow(infoWindow); 
			}
			
		},
		
		//设置一个图层
		setTileLayer(){
			console.log('tilelayer')
			const _this =this
			var tileLayer = new BMap.TileLayer({ isTransparentPng: true })
			tileLayer.getTilesUrl = function(tileCoord, zoom) {
				var x = tileCoord.x
				var y = tileCoord.y
				var url = 'http://lbsyun.baidu.com/jsdemo/demo/tiles/' + zoom + '/tile' + x + '_' + y + '.png';     //根据当前坐标，选取合适的瓦片图
				return url
			}
			_this.map.addTileLayer(tileLayer);
		},

		addEvent(obj,xEvent,fn){
			if(obj.attachEvent){
				obj.attachEvent('on'+xEvent,fn);
			}else{
				obj.addEventListener(xEvent,fn,false);
			}
		},

		onMouseWheel(){
			const _this = this
			console.log('zoom='+_this.map.getZoom())
			if(_this.map.getZoom() >= 17){
//				let path = _this.getRootPath()
				let path = 'http://localhost:8080/#/map'
				console.log(path)
				_this.setCookie('path',path,1)
				 window.location.href="http://127.0.0.1:8020/vue-map/static/pages/test3.html"
			}

		},

		setCookie(c_name, value, expiredays) {
			var exdate = new Date()
			exdate.setDate(exdate.getDate() + expiredays)
			document.cookie = c_name + "=" + escape(value) +
				((expiredays == null) ? "" : ";expires=" + exdate.toGMTString())
		},
		
		getRootPath(){
			var currentPagepath=location.href
			var pathName = window.document.location.pathname
			var pos = currentPagepath.indexOf(pathName)
			var localhostPath = currentPagepath.substring(0,pos)
			var projectName = pathName.substring(0,pathName.substr(1).indexOf("/")+1)
			return localhostPath+projectName
		},

		//测试看看能不能通过打开一个新页面的方式进行打开
		openNewWin(){
			window.location.href="http://127.0.0.1:8020/vue-map/static/pages/room.html"
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
