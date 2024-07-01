# CzmlFirstPersonPlayer

* CZMLファイルを一人称視点で再生します。(CesiumJSを利用)
* 道案内用途など、主に歩行者からの視点での表示を目的にしています。

## 道にそって移動するCZMLファイルの作成方法

ブラウザだけで作成可能:
* OpenStreetMapデータから道路ネットワークデータを作成して (Appendix B)
* その道路ネットワークデータを使って、地図上で指定した出発地と目的地間の経路を見つけて、道路上を移動するCZMLを作成 (Appendix A)

## Example
### PLATEAUの3Dタイル
https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/b254c367-1292-4354-87d1-58f754ceffcd

(PLATEAUのテクスチャ付きの建築物の3Dタイルは、
[建築物モデル（千代田区）](https://www.geospatial.jp/ckan/dataset/plateau)を
手元にダウンロードした上で、`http://localhost:8000`から取得する形で表示しています。)

テクスチャ無しの建築物モデルでの表示例:
https://deton.github.io/CzmlFirstPersonPlayer/?czmlurl=https://deton.github.io/CzmlFirstPersonPlayer/sample.czml

### Google Photorealistic 3D Tiles

![google3dtile.png](https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/18d4a062-e8b3-49bb-a819-9b10e92bcd27)

https://deton.github.io/CzmlFirstPersonPlayer/?tileset=google3dtile&czmlurl=https://deton.github.io/CzmlFirstPersonPlayer/sample.czml

### [ZENRIN City Asset Series](https://www.zenrin.co.jp/contents/product/service/3d/asset/)

![zenrin3dtile](https://github.com/deton/CzmlFirstPersonPlayer/assets/761487/8c506870-addc-467d-af10-57d65f3b4630)

(ZENRIN City Asset SeriesのJapanese Otaku CityのFBXを3D Tilesに変換して使用。Appendix C)

## URL Parameters
* czmlurl: 読み込むCZMLファイルのURL。
* viewrect: 初期表示領域(west,south,east,north)
  * (default): 139.7688,35.6970,139.7761,35.703
* tileset: 3D tileset URL
  * `none`: 読み込まない。
  * `google3dtile`: Google Photorealistic 3D tileset
  * 数値: Cesium ion asset id
  * それ以外の場合: 3D tileset URL。複数回指定した場合は全て読み込む。
  * (default): https://assets.cms.plateau.reearth.io/assets/aa/ecf312-95c2-4e24-8351-642f27e447b6/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13101_chiyoda-ku_lod1/tileset.json
  * (備考): この他、CZMLファイル内で指定したtilesetも読み込まれます。
* tilesetHeightOffset: tilesetの高さ調整用のオフセット値[m]。
  * (default): 0
* height: terrainからの視点の高さ[m]。CZML内のmodel.heightReferenceがCLAMPで始まる場合、terrainに対してこの固定値を足した値を視点の高さとして使います。
  * (default): 2
  * (備考): (JavaScript ConsoleでHEIGHT_OFFSETを変更することで調整可能)
* minimap: minimap zoom level. 負の値の場合はminimapを非表示
  * (default): 17
* imageryIdx: 地表面画像のindex。後からBaseLayerPicker(画面右上端から2個目のアイコン)で変更可能。
  * `-1`: [PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial/blob/main/ortho/plateau-ortho-streaming.md)のPLATEAU-Ortho
  * `-2`: [地理院タイル(写真)](https://maps.gsi.go.jp/development/ichiran.html#seamlessphoto)
  * (default): 0 (CesiumJSデフォルトのBing Maps Aerial)
* terrainIdx: 地表面地形のindex。後からBaseLayerPicker(画面右上端から2個目のアイコン)で変更可能。
  * `-1`: [PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial/blob/main/terrain/plateau-terrain-streaming.md)のPLATEAU-Terrain
  * `-2`: 産総研 地質調査総合センターの[標高タイルサービス](https://github.com/aistairc/gsj_numerical_png_terrain_provider)
  * (ただし、token指定時はPLATEAU-Terrainを追加しないので、`-1`が標高タイルサービスになります。)
  * (default): -1
* token: Cesium ion access token
  * (default): ([PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial/blob/main/terrain/plateau-terrain-streaming.md#21-%E3%82%A2%E3%82%AF%E3%82%BB%E3%82%B9%E3%83%88%E3%83%BC%E3%82%AF%E3%83%B3%E5%8F%8A%E3%81%B3%E3%82%A2%E3%82%BB%E3%83%83%E3%83%88id)のtoken)

### URL Parameter Example
* [PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial): https://deton.github.io/CzmlFirstPersonPlayer/?tileset=https://assets.cms.plateau.reearth.io/assets/e3/b36cac-6c5e-430c-8366-f43e8efcf146/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13113_shibuya-ku_lod1/tileset.json&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml
* Google Photorealistic 3D tileset: https://deton.github.io/CzmlFirstPersonPlayer/?tileset=google3dtile&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml

## 建物の3Dタイル
* [PLATEAU配信サービス](https://github.com/Project-PLATEAU/plateau-streaming-tutorial/blob/main/3d-tiles/plateau-3dtiles-streaming.md#4-%E9%85%8D%E4%BF%A1%E3%83%87%E3%83%BC%E3%82%BF3d-tiles%E4%B8%80%E8%A6%A7)
  * (表示に時間がかかったり、テクスチャが表示されなかったりする場合は、[3D都市モデル（Project PLATEAU）ポータルサイト](https://www.geospatial.jp/ckan/dataset/plateau)から3D Tilesのzipをダウンロード・展開して、localhostのhttp serverから取得する形にすると良いかもしれません。)
  * (PLATEAU VIEWで見ると、歩道橋などは橋梁モデル(LOD2)に含まれている場合があるようです。)

## CZML内で表示に影響する要素
* 位置は、czml[1].positionのcartographicDegreesのみ対応(cartesian等には未対応)。
* czml[1].model.heightReference
  * CLAMPで始まる場合: terrainに対して、上記HEIGHT_OFFSET固定値を足した値を視点の高さとして使う。
  * RELATIVEで始まる場合: terrainに対してCZML内の高さを足した値を視点の高さとして使う。
  * NONEまたは指定無しの場合: CZML内の高さを絶対値として使う。(ellipsoidからの高さ)

## 備考
* 再生開始後はドラッグ等によるCamera操作が効かなくなります。
  * 再生を中止して、Camera操作を有効にするには、JavaScript Consoleで`stopPlayAndEnableCameraControl();`

## Appendix A: 道路上を移動するCZMLを作成
* [geojson-path-finder](https://github.com/deton/geojson-path-finder)を使って、道路ネットワークデータのnetwork.geojsonをLoadして、地図上で出発地と目的地を指定して、CZMLをexport。
  * 使用する道路ネットワークデータは、Appendix Bの方法で作成。
    * (または、オープンデータの[歩行空間ネットワークデータ(GeoJSON)](https://www.geospatial.jp/ckan/dataset/0401)が使えるかもしれません。)
  * Example: https://deton.github.io/geojson-path-finder/?networkjson=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/28913a796a5ecad82b5990ed10c6d9b5c981c4a1/network-shibuya.geojson&waypointLatLng=35.6590,139.7011&waypointLatLng=35.65956,139.6998&waypointLatLng=35.6601,139.6956
  * 備考: 地表に表示される移動経路の線を、polylineとしてCZMLに入れて生成しています。
    * 非表示にしたい場合は、CZMLからpolylineを削除するか、`"show": false`としてください。
    * 移動経路のpolylineは`"clampToGround": true`で表示しているので、橋の下を通る場合など、橋の上に線が表示されます。地表の高さで線を表示するデータを作る場合は、CzmlFirstPersonPlayerのJavaScript Consoleで`console.log(updatePathHeightFromTerrain(czml[2].polyline.positions.cartographicDegrees));`

## Appendix B: 道路ネットワークデータをOpenStreetMapデータから作成
* [Generate network.geojson](https://github.com/deton/GraphFromOSM)
  * Example: https://deton.github.io/GraphFromOSM/?bbox=139.6954429149628,35.65743572597212,139.70327496528628,35.66243933363614

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
