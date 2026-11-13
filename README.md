# Botanist ML｜38 類葉片影像分類

Oregon State University AI 539 期末課堂競賽。輸入一張葉片照片，預測植物種類與健康狀態對應的 38 個類別之一，例如健康蘋果葉與患病蘋果葉。課程提供葉片照片和對應的數字標籤，最終交付為每張測試影像的預測標籤。

## 我的貢獻

我全程負責 Team 5 的 Botanist 子專案。使用 **Python、PyTorch、torchvision 與 Google Colab**，自行組合四層 CNN，從隨機初始化權重開始訓練。我實作了影像載入與資料增強、模型 forward 和訓練迴圈，依 validation 結果比較模型並調整訓練設定，最後產出競賽預測。

加深 CNN 後，訓練準確率達 99.1%，驗證準確率只有 71.4%。我保留四層架構，加入 Batch Normalization 和 Dropout，下一版的驗證準確率提高到 89.2%。最終版本再加入資料增強，並使用 Adam 與 Cosine Annealing 訓練。

## 成果

| 項目 | 結果 | 來源 |
|---|---:|---|
| 調參 validation accuracy | **98.16%** | 團隊最終報告圖 7a |
| 課堂競賽 test accuracy | **98.52%** | 課堂成績表 Team 5、團隊最終報告 §2.5 |
| 競賽預測 | **4,000 筆** | 提交用 CSV 筆數 |

## 程式與技術細節

- [訓練 notebook](notebooks/botanist_training.ipynb) 包含影像前處理、CNN 架構及全量重訓程式。執行需要課程提供的影像和標籤 CSV。
- [模型開發紀錄](docs/model-development.md) 包含模型迭代、架構、訓練設定及評估限制。
- [Team 5 期末報告｜Botanist 章節](docs/botanist-report-excerpt.pdf) 摘自原報告 §2（第 6–10 頁），含當時的模型迭代與訓練曲線。報告由 Chun-Kai Yao、Pin-Cheng Wu 與 Te-Hong Hou 共同撰寫。Botanist 子專案由 Te-Hong Hou 負責。
