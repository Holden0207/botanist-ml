# Botanist ML 模型開發紀錄

## 資料與選模

課程提供葉片影像與標籤 CSV。原始標籤為 1–38，Dataset 讀入時轉為 0–37，競賽提交使用 1–38。訓練影像轉為 RGB、縮放到 224×224，套用隨機水平與垂直翻轉、±15° 旋轉、色彩調整與 normalization。課堂報告記載開發階段使用 90/10 train/validation split，選定模型後使用全量訓練資料重訓。原 notebook 的訓練日誌分母為 50,000。

來源為課程 `Project Description.pdf` pp. 1–2、`Final_Report_of_Team_5.pdf` §§2.2–2.4，以及 notebook 的 `BotanistDataset` 與 `train_transform`。

## 模型迭代

| 版本 | Train accuracy | Validation accuracy | 觀察 |
|---|---:|---:|---|
| 兩層 CNN | 約 68% | 約 61% | 訓練與驗證表現都偏低 |
| 四層 CNN | 約 99.1% | 約 71.4% | 出現 27.7 個百分點的差距，報告圖 6 的 loss 曲線也顯示訓練與驗證走勢分離 |
| 四層 CNN + Batch Normalization + Dropout | 約 93.5% | 約 89.2% | 驗證表現提升 17.8 個百分點。兩項改動在同一輪加入，各自效果需透過 ablation 評估 |
| 最終調參版本 | — | **98.16%** | 資料增強、Adam 與 Cosine Annealing 一併納入最終訓練流程 |

數字來自團隊最終報告 §2.3 與圖 6–7。競賽測試的 **98.52%** 來自 Team 5 課堂成績表和團隊最終報告 §2.5，對應另一個評估階段。

## 最終架構與訓練

- 四層 3×3 Conv 的輸出 channels 為 32、64、128、256，每層後接 Batch Normalization 和 ReLU，每兩層接一次 Max Pool。
- Adaptive Average Pool 將 feature map 固定為 256×4×4，攤平後接 Dropout 0.5、Linear 4096→512、ReLU、Linear 512→38，再輸出 log probabilities。
- 全量訓練使用 batch size 64、NLLLoss、Adam（初始 learning rate 0.001）及 Cosine Annealing（T_max=19），程式設定 19 epochs。

程式位置見 [訓練 notebook](../notebooks/botanist_training.ipynb) 的 `BotanistCNN`、`train` 及最終訓練迴圈。公開 notebook 保留訓練邏輯並移除原執行輸出。完整重現競賽結果還需要課程資料、開發切分、訓練紀錄、checkpoint、推論程式及官方測試正解。
