## ESMap室内地图详解

> 因医疗系统接触ESMap地图编辑器，目前大概介绍一下ESMap使用情况

### 一：地图编辑器

ESMap最主要就是地图编辑器，没有内容的地图编辑器，SDK开发就不从谈起。地图编辑器如下图

> 包括房间编辑，模型库，POI编辑，导航编辑，楼层编辑等

### 二： vue加载ESMap室内地图

> SDK目前不支持用require js引用，只能使用<script src="lib/esmap.min.js"></script >引用

1. Vue-cli3加载地图vue-cli3没有 **static** 文件夹，而使用 **public** 文件夹代替。其余操作步骤同vue-cli2，详见如下

2. Vue-cli2加载地图步骤

   ##### 第一步:把esmap.min.js文件、地图相应的图标、地图文件、主题文件放在根目录 /static 文件夹下。

```vue
// 第二步:在 index.html 文件下引入esmap sdk
<body>
  <div id="app"> </div>
  <script src="./static/esmap/esmap.min.js"> </script>
  <!-- built files will be auto injected -->
</body>

// 第三步:创建一个单独的地图容器组件
<template>
  <div id="map-container"></div>
</template>

```

```javascript
// 第四步:组件挂载完成后初始化地图
export default {
  name: "MapContainer",
  mounted() {
    window.map = new esmap.ESMap({
      container: document.getElementById("map-container"),
      //如果地图、主题文件放在static目录下，请使用static路径
      mapDataSrc: "/esmap/data",
      mapThemeSrc: "/esmap/data/theme"
      // themeID: defaultOpt.themeID,
      // token: 'escope'
    });
    map.openMapById(10001);
    
    var ctlOpt = new esmap.ESControlOptions({
      position: esmap.ESControlPositon.RIGHT_TOP,
      //注意：imgURL参数是static目录下图片的路径
      imgURL: "static/esmap/resource/style/wedgets/img/" 
    });

    map.on("loadComplete", function() {
      //创建楼层控件
      var floorControl = new esmap.ESScrollFloorsControl(map, ctlOpt);

    });
  }
};
```



### 三：SDK开发编辑

地图初始化配置详情

```javascript
var map = new esmap.ESMap({
    container: document.getElementById('map-container'),  //地图显示容器
    mapDataSrc: '/data/',               //离线地图数据位置
    mapThemeSrc: '/data/theme/',        //离线主题数据位置
    focusAlphaMode: true,               //对不可见图层启用透明设置 默认为true
    focusAnimateMode: false,            //开启聚焦层切换的动画显示
    focusAlpha: 0.1,                    //对不聚焦图层启用透明设置，当focusAlphaMode = true时有效
    viewModeAnimateMode: false,         //开启2维，3维切换的动画显示
    moveToAnimateMode: false,           //地图定位跳转动画设置
    defaultScaleLevel: 12,              //设置默认显示级别,1到24之间的整数
    modelSelectedEffect: true,          //支持单击模型高亮，false为单击时模型不高亮
    visibleFloors: [1,2,3],             //初始显示楼层
    focusFloor: 1,                      //初始聚焦楼层
    themeID: '2001',                    //自定义样式主题ID
    token:'',                            //打开地图对应的token
    viewMode: esmap.ESViewMode.MODE_3D 	//初始二维还是三维状态
});
```

> map一定要设置为全局对象，window.map =这样来赋值初始化，不然其它函数报map未定义.



如： 

```javascript
// 初始化内容
const esmap = window.esmap // 赋值初始化
const container = document.getElementById('map-container')
    this.map = new esmap.ESMap({
      container: container, // 渲染dom
      mapDataSrc: configMapOpt.mapDataSrc, // 地图数据位置
      mapThemeSrc: configMapOpt.mapThemeSrc, // 主题数据位置
      // visibleFloors: 'all',//更多初始化参数配置请参考https://www.esmap.cn/escopemap/content/cn/develope/map-config.html
      // focusFloor: 1,
      // viewModeAnimateMode: false,
      defaultScaleLevel: 4,
      viewMode: esmap.ESViewMode.MODE_2D,
      token: configMapOpt.mapToken
    })
    // 打开地图数据
    this.map.openMapById(configMapOpt.mapID)
    this.map.showCompass = true // 显示指南针
```

```javascript
 // 增加标签
    addMarker ({ x, y, name, img, floorNum = 1, RoomHigh = 0.1 }) {
      const _that = this
      // _that.remove()
      const floor = _that.map.getFloor(floorNum)
      const layer = new esmap.ESLayer(
        esmap.ESLayerType.IMAGE_MARKER,
        'myImageMarkers'
      )
      floor.addLayer(layer)
      this.floor = floor
      const tm = new esmap.ESImageMarker({
        x: x,
        y: y,
        name: name,
        size: 48,
        height: RoomHigh,
        spritify: true,
        seeThrough: true,
        url: img
      })
      this.tag = tm
      layer.addMarker(tm)
      this.layer = layer
    }
```

