# CzmlFirstPersonPlayer

## 
## Parameters
|URL parameter | JS var | 意味 | default |
|---|---|---|---|
|czmlurl||CZML URL to load||
|tileset||'none':読み込まない。'google3dtile':Google Photorealistic 3D tileset。それ以外の場合:3D tileset URL. 複数回指定した場合は全て読み込む。|'https://assets.cms.plateau.reearth.io/assets/aa/ecf312-95c2-4e24-8351-642f27e447b6/13100_tokyo23-ku_2022_3dtiles_1_1_op_bldg_13101_chiyoda-ku_lod1/tileset.json'|
|height|HEIGHT_OFFSET|terrainからの視点の高さ。CZML内のmodel.heightReferenceがCLAMPで始まる場合、terrainに対してこの固定値を足した値を視点の高さとして使う。|2|
|minimap|MINIMAPZOOM|minimap zoom level. 負の値の場合はminimapを非表示|17|

## CZML内で表示に影響する要素
* model.heightReference
  * CLAMPで始まる場合: terrainに対してHEIGHT_OFFSET固定値を足した値を視点の高さとして使う。
  * RELATIVEで始まる場合: terrainに対してCZML内の高さを足した値を視点の高さとして使う。
  * NONEまたは指定無しの場合: CZML内の高さを絶対値として使う。(ellipsoidからの高さ)

## 参考
* https://github.com/3DGISKing/CesiumJsFirstPersonCameraController
