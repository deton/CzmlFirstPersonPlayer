# CzmlFirstPersonPlayer

CZMLを一人称視点で再生します。
主に歩行者からの視点での表示を目的にしています。

## CZML作成方法

ブラウザだけで作成可能:
* OpenStreetMapデータから道路ネットワークデータを作成して (Appendix A)
* その道路ネットワークデータを使って、地図上で指定した出発地と目的地間の経路を見つけて、道路上を移動するCZMLを作成 (Appendix B)

## Example

### PLATEAUの3Dタイル
https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/e008acad-fe89-4c27-9cf0-01cde59ad7ff

(PLATEAUのテクスチャ付きの建築物の3Dタイルは、
[建築物モデル（千代田区）](https://www.geospatial.jp/ckan/dataset/plateau)を
手元にダウンロードした上で、localhostから取得する形で表示しています。)

テクスチャ無しの建築物モデルでの表示例:
https://deton.github.io/CzmlFirstPersonPlayer/?czmlurl=https://deton.github.io/CzmlFirstPersonPlayer/sample.czml

### Google Photorealistic 3D Tiles

![google3dtile.png](https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/18d4a062-e8b3-49bb-a819-9b10e92bcd27)

https://deton.github.io/CzmlFirstPersonPlayer/?tileset=google3dtile&czmlurl=https://deton.github.io/CzmlFirstPersonPlayer/sample.czml

### [ZENRIN City Asset Series](https://www.zenrin.co.jp/contents/product/service/3d/asset/)

![zenrin3dtile](https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/bb64b306-2459-43b5-9f7a-7f71bfbbc069)

(ZENRIN City Asset SeriesのJapanese Otaku CityのFBXを3D Tilesに変換して使用。Appendix C)

## Parameters
|URL parameter | JS var | 意味 | default |
|---|---|---|---|
|czmlurl||CZML URL to load||
|viewrect||初期表示領域(west,south,east,north)|'139.7688,35.6970,139.7761,35.7031'|
|tileset||'none':読み込まない。'google3dtile':Google Photorealistic 3D tileset。それ以外の場合:3D tileset URL. 複数回指定した場合は全て読み込む。|'https://assets.cms.plateau.reearth.io/assets/aa/ecf312-95c2-4e24-8351-642f27e447b6/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13101_chiyoda-ku_lod1/tileset.json'|
|height|HEIGHT_OFFSET|terrainからの視点の高さ。CZML内のmodel.heightReferenceがCLAMPで始まる場合、terrainに対してこの固定値を足した値を視点の高さとして使う。|2|
|minimap|MINIMAPZOOM|minimap zoom level. 負の値の場合はminimapを非表示|17|

* Example([PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial)): https://deton.github.io/CzmlFirstPersonPlayer/?tileset=https://assets.cms.plateau.reearth.io/assets/e3/b36cac-6c5e-430c-8366-f43e8efcf146/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13113_shibuya-ku_lod1/tileset.json&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml
* Example(Google Photorealistic 3D tileset): https://deton.github.io/CzmlFirstPersonPlayer/?tileset=google3dtile&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml

## CZML内で表示に影響する要素
* 位置は、czml[1].positionのcartographicDegreesのみ対応(cartesian等には未対応)。
* czml[1].model.heightReference
  * CLAMPで始まる場合: terrainに対して、上記HEIGHT_OFFSET固定値を足した値を視点の高さとして使う。
  * RELATIVEで始まる場合: terrainに対してCZML内の高さを足した値を視点の高さとして使う。
  * NONEまたは指定無しの場合: CZML内の高さを絶対値として使う。(ellipsoidからの高さ)

## Appendix A: 道路上を移動するCZMLを作成
* Load the network.geojson and export CZML using [geojson-path-finder](https://github.com/deton/geojson-path-finder)
  * 使用する道路ネットワークデータは、Appendix Bの方法で作成。
    * (または、オープンデータの[歩行空間ネットワークデータ(GeoJSON)](https://www.geospatial.jp/ckan/dataset/0401)が使えるかもしれません。)
  * Example: https://deton.github.io/geojson-path-finder/?networkjson=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/f1ab3e649e2bde20c6be67ec5be2d3c0f467e040/network-shibuya.geojson&waypointLatLng=35.6590,139.7011&waypointLatLng=35.65956,139.6998&waypointLatLng=35.6601,139.6956

## Appendix B: 道路ネットワークデータをOpenStreetMapデータから作成
* [Generate network.geojson](https://github.com/deton/GraphFromOSM)
  * Example: https://deton.github.io/GraphFromOSM/?bbox=139.69543755054477,35.65887407735725,139.7011882066727,35.66062621586084


## Appendix C: ZENRIN City Asset SeriesのJapanese Otaku CityのFBXを3D Tilesに変換して使用
* FBXを[FBX2glTF](https://github.com/godotengine/FBX2glTF)でglbに変換。(Textures/等のファイル群をfbxファイルと同じ場所に集めて実行)
* Cesium ionにUploadして緯度経度高さとscaleを決定。
* ローカルでtileset.jsonを作る場合は、
  [3d-tiles-tools](https://github.com/CesiumGS/3d-tiles-tools#createtilesetjson)で、
  `npx 3d-tiles-tools createTilesetJson -i PQ_Remake_AKIHABARA.glb -o tileset.json --cartographicPositionDegrees 139.77024 35.69602 38`
  * 表示させてみると小さいのでscaleを100にするため、(3d-tiles-toolsのソースを参考に)以下で調べたtransform値で、tileset.jsonのroot.transformを更新。
  ```js
  var transform = bldg3dtiles.get(0).root.transform;
  var scale = 100;
  var modelMatrix = Cesium.Matrix4.multiplyByUniformScale(transform, scale, new Cesium.Matrix4());
  console.log(Cesium.Matrix4.toArray(modelMatrix));
  ```
  * creditは後から追加。
  `viewer.creditDisplay.addStaticCredit(new Cesium.Credit('3D city model by &copy;ZENRIN', true));`
