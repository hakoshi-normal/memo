# シカのトレースについて

[YouTubeで公開](https://youtu.be/e-enwk27O7Y)されていた日中22℃下におけるドローンサーモカメラ映像を入力とし，イノシシの自動検出処理を試験的に実装しました  
（見づらいですが緑の枠で囲んでいる部分です）

![result](img/result.png)
![result1](img/result1.png)
![result2](img/result2.png)
![result3](img/result3.png)

## 実装概要

- 画像中の最も高温な画素座標に基づきルールベースで領域を特定
- 領域をマスク処理し再帰的に検出処理を行うことで複数頭の領域検出が可能
- 現段階の実装では特定の閾値設定は不要
- 改善事項
  - ノイズ除去処理や解像度変更などの前処理により精度改善の余地あり
  - サーモカメラの入力を直接利用できればより詳細な検出が期待できる
  - **検出領域の識別機能**

## シカ検出手段

- サーモカメラ
  - シカ以外の高温オブジェクトに対してはサーモカメラのみで識別が困難
    - 日中は可視光カメラと組み合わせて識別（後述）
  - 気温が検出精度に与える影響の程度は不明

- 可視光カメラ
  - YOLO，U-Net等オブジェクト検出手法
    - 学習データの必要量が不明であるため，検出精度の見通しが立たない
    - 季節，気候，植生によって背景にばらつきがあり，学習データが膨大になる可能性
  - 高温領域画像を個別にクラス分類
    - 高温領域画像のみを切り出し推論の対象とすれば背景の影響を受けづらい
    - CNNベースの軽量画像分類モデル（ResNetとか）
    - YOLOv8にも画像**分類**モデルがあるらしい（Object DetectionではなくImage Classification）

## 要確認事項

- **サンプル画像** ← 欲しい
- 想定稼働時期，気候，地域
- 検出・分類対象（シカ，人，その他...？）
- ドローン飛行速度，検出のリアルタイム性  
  シカ検出通知の遅延の許容範囲
- 複数頭のシカに対する必要検出精度  
  （上記方法だと極度に群れているイノシシは個別の検出が困難）


## 先行研究
- [Using YOLO Object Detection to Identify Hare and Roe Deer in Thermal Aerial Video Footage—Possible Future Applications in Real-Time Automatic Drone Surveillance and Wildlife Monitoring](https://doi.org/10.3390/drones8010002)
- [Deer survey from drone thermal imagery using enhanced faster R-CNN based on ResNets and FPN](https://doi.org/10.1016/j.ecoinf.2023.102383)

## データセット
- データセットリスト[Datasets with annotated wildlife in drone/aerial images](https://github.com/agentmorris/agentmorrispublic/blob/main/drone-datasets.md)
