# probspace_kaokore_status_1st_solution

## Competition URL
https://comp.probspace.com/competitions/kaokore_status

## Pipeline

![image](https://user-images.githubusercontent.com/95401302/183533421-0ea89864-8e26-4bd4-852b-16d4ac9430a4.png)

- Stage 1<br>
  アンサンブル後の予測結果をハードラベルでPseudo Label作成
- Stage 2<br>
  各モデルで不正解データを抽出<br>
- Stage 3<br>
  各モデルの予測結果をOptunaを用いて重み最適化を実施<br>
  結果を最終サブのうちの一つに選定
- Stage 4<br>
  Stage 3の予測結果のうち、ラベル0の予測確率を用いて2値分類モデルの予測確率とともにOptunaで重み最適化を実施<br>
  最適化された確率を用いて、元の確率との平均を算出し、採用
- Stage 5<br>
  学習データを全て用いた全学習モデルを作成し、Stage 4の結果と1:5の割合でアンサンブル<br>
  アンサンブル後のラベルとStage 4で作成した2値分類モデルを比較し、2値分類モデルで0と予測した結果で上書きしたものを最終サブの一つに選定<br>
  ただし、ラベル2と予測しているケースについては、マルチクラス分類モデルで十分に当てられていると考え、上書きしないように処理
  
- 最終サブの結果<br>
  最終サブ①： Public LB: 0.911, Private LB: 0.904<br>
  最終サブ①： Public LB: 0.915, Private LB: 0.907  

![image](https://user-images.githubusercontent.com/95401302/183533561-f4863d25-4c7a-44e2-886a-abd50e6e74a6.png)

## Augmentation Pattern

- Pattern①<br>
  ・Resize<br>
  ・ShiftScaleRotate<br>
  ・HorizontalFlip<br>
  ・Blur<br>
  ・Rotate<br>
  ・HueSaturationValue<br>
  ・Normalize
- Pattern②<br>
  ・Resize<br>
  ・HorizontalFlip<br>
  ・Blur<br>
  ・HueSaturationValue<br>
  ・Normalize
