# CzmlFirstPersonPlayer

CZMLを一人称視点で再生します。

## Example

### PLATEAUの3Dタイル
![sample.gif](sample.gif)

(PLATEAUのテクスチャ付きの建築物の3Dタイルは、
[建築物モデル（千代田区）](https://www.geospatial.jp/ckan/dataset/plateau)を
手元にダウンロードした上で、localhostから取得する形で表示しています。)

### Google Photorealistic 3D Tiles

![google3dtile.png](google3dtile.png)

## Parameters
|URL parameter | JS var | 意味 | default |
|---|---|---|---|
|czmlurl||CZML URL to load||
|tileset||'none':読み込まない。'google3dtile':Google Photorealistic 3D tileset。それ以外の場合:3D tileset URL. 複数回指定した場合は全て読み込む。|'https://assets.cms.plateau.reearth.io/assets/aa/ecf312-95c2-4e24-8351-642f27e447b6/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13101_chiyoda-ku_lod1/tileset.json'|
|height|HEIGHT_OFFSET|terrainからの視点の高さ。CZML内のmodel.heightReferenceがCLAMPで始まる場合、terrainに対してこの固定値を足した値を視点の高さとして使う。|2|
|minimap|MINIMAPZOOM|minimap zoom level. 負の値の場合はminimapを非表示|17|

* Example(PLATEAU配信サービス): https://deton.github.io/CzmlFirstPersonPlayer/?tileset=https://assets.cms.plateau.reearth.io/assets/e3/b36cac-6c5e-430c-8366-f43e8efcf146/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13113_shibuya-ku_lod1/tileset.json&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml
* Example(Google Photorealistic 3D tileset): https://deton.github.io/CzmlFirstPersonPlayer/?tileset=google3dtile&czmlurl=https://gist.githubusercontent.com/deton/c030eae2af830364580727a291913f8e/raw/4b580611bf0ca785830ba483d32085a2dbe8b0aa/path-shibuya.czml

## CZML内で表示に影響する要素
* 位置は、czml[1].positionのcartographicDegreesのみ対応(cartesian等には未対応)。
* czml[1].model.heightReference
  * CLAMPで始まる場合: terrainに対して、上記HEIGHT_OFFSET固定値を足した値を視点の高さとして使う。
  * RELATIVEで始まる場合: terrainに対してCZML内の高さを足した値を視点の高さとして使う。
  * NONEまたは指定無しの場合: CZML内の高さを絶対値として使う。(ellipsoidからの高さ)

## Appendix: Create other CZML
* Export CZML using [geojson-path-finder](https://deton.github.io/geojson-path-finder/)

## Appendix: Create other CZML on other area
* [Generate network.geojson](https://deton.github.io/GraphFromOSM/)
* Load the network.geojson and export CZML using [geojson-path-finder](https://deton.github.io/geojson-path-finder/)

## See Also
* https://github.com/3DGISKing/CesiumJsFirstPersonCameraController
