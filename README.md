# 加载geoserver发布的矢量切片的示例

其中包括 leaflet 和 mapbox。

## vector layer url

**WMTS:**

```js
var vectorLayerUrl = "http://localhost:8088/geoserver/gwc/service/wmts?"+
"REQUEST=GetTile&SERVICE=WMTS&VERSION=1.0.0&LAYER=traffic:test_road" +
"&STYLE=&TILEMATRIX=EPSG:900913:{z}&TILEMATRIXSET=EPSG:900913&" +
"FORMAT=application/x-protobuf;type=mapbox-vector&TILECOL={x}&TILEROW={y}";
```

**TMS:**

```js
vectorLayerUrl = "http://localhost:8088/geoserver/gwc/service/tms/1.0.0/"+
"traffic:test_road@EPSG%3A900913@pbf/{z}/{x}/{-y}.pbf";
```

## 加载方式

+ leaflet

```js
L.vectorGrid.protobuf(vectorLayerUrl, getVectorOptions('speed')).addTo(map);
```

+ mapbox

```js
var vectorLayer = {
                "id": 'road-layer',
                "type": "line",
                "source": {
                    // "scheme": 'tms', // 如果使用 TMS，则开启
                    "type": 'vector',
                    "tiles": [vectorLayerUrl]
                },
                "source-layer": layerName,
                "layout": {
                    "line-join": "round",
                    "line-cap": "round"
                },
                "paint": {
                    "line-color": getLineStyle('speed'),
                    "line-width": 3,
                    'line-opacity': 0.9
                },
                'minzoom': 8
            };
map.addLayer(vectorLayer);
```

`注意：如果使用TMS，则必须设置属性：`

```js
"scheme": 'tms'
```
