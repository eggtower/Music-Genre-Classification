# Music-Genre-Classification README
Music genre classification

## 執行環境
建構虛擬環境(python 版本為 3.7)
```
conda create --name <env_name> python=3.7
```
啟動虛擬環境
```
conda activate <env_name>
```
安裝相依套件(於檔案目錄下)
```
pip install -r requirements.txt
```
執行 jupyter notebook
```
jupyter notebook 
```
執行 `Music Genre Classifier.ipynb` 即可

**須注意，存放音樂類型的檔案路徑名稱設定為 music**

存放音樂類型的目錄則為 ex: `../music/blue`

## 使用的 audio feature

為了對音頻樣本進行分類，將通過計算它們的 MFCC 來對它們進行預處理，這是每個感知頻帶的能量的時間表示。

在這種情況下，選擇了 MFCC 的 13 個波段。

除此之外，也計算出它們的 RMS(root-mean-square), ZCR(zero-crossing rate), spectral centroid, roll-off，作為特徵嘗試用來輔助訓練。

經過多次嘗試，以 MFCC 個別搭配 RMS, ZCR, spectral centroid, roll-off，訓練的準確率極差(皆約 10%±2%)，

如下圖，為 MFCC 搭配 RMS 做訓練的 accuracy & loss

![](https://i.imgur.com/EHYLqpW.png)

這幾乎等於是從 10 種音樂類型隨便亂猜猜對的機率，

故最終模型訓練使用的特徵以 MFCC 的 13 個波段呈現。

## 分類方法或模型

前處理我是將每個音樂片段 (30s) 裁切成 10 等分，來增加訓練資料，

並使用 librosa (python package) 對這些音樂片段進行分析以獲得 MFCC 的數值。

接著使用 sklearn 的 train_test_split 將這些數據切割，

切割訓練資料與測試資料，train_size : test_size = 8 : 2

其中，訓練資料又將被切分出訓練集與驗證集 train : validation = 8 : 2

下圖為模型的summary

![](https://github.com/eggtower/Music-Genre-Classification/blob/master/model.summary.png)

hyperparameter:
- epochs=30
- batch_size=32
- learning_rate=0.001

## 分類準確率

||第一次|第二次|第三次|第四次|第五次|平均|
|--|--|--|--|--|--|--|
|accuracy|0.82625 (661 / 800)|0.8275 (662 / 800)|0.835 (668 / 800)|0.83 (664 / 800)|0.85875 (687 / 800)|0.8355|

下圖為其中一次訓練過程的 accuracy 與 loss

![train & validation accuracy](https://github.com/eggtower/Music-Genre-Classification/blob/master/accuracy.png)

![train & validation accuracy](https://github.com/eggtower/Music-Genre-Classification/blob/master/loss.png)
