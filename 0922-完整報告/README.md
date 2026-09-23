# 醫療人工智慧應用研究 筆記整理

## 1. 日期、講者、題目

| 項目 | 內容 |
|---|---|
| 日期 | 2026/9/23 |
| 講者 | 彭徐鈞 |
| 題目 | 從數據驅動到臨床轉譯： 人工智慧於醫學訊號診斷與預後評估之最新進展 |


---

## 2. 簡介

本次分享以「AI如何真正落地於臨床」為主軸，涵蓋AI醫療應用開發的完整流程——從資料收集、前處理、特徵工程、模型訓練與驗證，到可解釋性AI（XAI）與模型部署，並以四個具體臨床案例貫穿說明：

1. **聽神經瘤**：自動分割與治療後預後（四分類）預測
2. **黃斑部手術**：以OCT影像預測術後視力恢復
3. **小兒低惡性度膠質瘤**：以影像組學結合腫瘤位置預測術後癲癇風險
4. **重度憂鬱症**：以腦電波（EEG）預測長期治療療效

核心主張是：AI在醫療中的角色是**輔助**而非取代醫師判斷，模型的可解釋性與臨床醫師的信任建立，是AI真正能落地應用的關鍵。

---

## 3. 相關技術說明

### 3.1 AI模型開發流程
- **資料收集與前處理**：資料清洗、缺失值處理、Min/Max正規化。
- **類別不平衡處理**：捨棄容易造成資料分佈失真的 SMOTE 過採樣，改採 **class weight／sample weight**，在不更動原始資料分佈的前提下加重少數類別權重。
- **特徵工程與篩選**：自影像（形狀、紋理、灰階）與訊號（時域、頻域、時頻域）擷取特徵；以變異數過濾、相關係數去共線性，並用 **MRMR**、**Lasso** 等方法篩選特徵；嚴防**資料洩漏（Data Leakage）**，特徵篩選僅能在訓練集上進行。
- **模型訓練與驗證**：常見以多模型比較（如十個模型）搭配 **k 折交叉驗證**（如5折）做超參數調校，並保留**獨立測試集**全程不參與訓練/篩選。

### 3.2 可解釋性AI（XAI）
- **機器學習模型**：SHAP（SHapley Additive exPlanations）分析特徵貢獻度。
- **深度學習（影像）模型**：Grad-CAM／IMAP 等熱區視覺化技術，觀察模型關注區域是否與臨床知識一致。
- 目的：讓臨床醫師理解模型判斷依據，是AI獲得採用的關鍵前提。

### 3.3 各案例關鍵技術
| 案例 | 資料型態 | 核心方法 | 主要成果 |
|---|---|---|---|
| 聽神經瘤 | MRI影像 | UNet自動分割；class weight處理四分類不平衡 | 分割 Dice ≈ 91–92%，全球排名第二 |
| 黃斑部手術視力預測 | OCT影像 | RPE/RNFL區域ROI裁切；遷移學習（ResNet-101、VGG-19）；回歸轉二分類 | AUC ≈ 0.90；外部測試 Recall／F1 達0.93 |
| 小兒膠質瘤癲癇預測 | T2-FLAIR MRI | 影像組學特徵＋腫瘤解剖位置定量；MRMR特徵篩選 | 位置＋組學特徵結合後 AUC > 0.95 |
| 重度憂鬱症療效預測 | EEG（19導程） | ICA去雜訊；PLV／PLI／wPLI功能性腦網路特徵＋功率特徵；留一法交叉驗證 | AUC由約50%提升至70–80% |

### 3.4 模型部署與落地考量
- 建立使用者友善介面（UI）。
- 確保跨醫院／跨資料集的**泛化能力**。
- 須通過如台灣TFDA、美國FDA等法規認證。
- 最終決策仍須由醫師負責，AI僅作輔助工具。

---

## 4. 心得報告

這次分享讓人深刻體會到，醫療AI的價值並不只在於模型準確率的高低，而在於**能否解決真實的臨床痛點**，以及**能否被臨床醫師信任並採用**。幾個案例都反覆強調同一件事：模型效能相近時，可解釋性強、且判斷依據符合臨床直覺的模型，才更容易被醫師接受（如黃斑部案例中ResNet-101關注區域貼近臨床醫師視角，因而更受信賴）。

另外印象深刻的是資料處理的嚴謹度遠超一般AI專案的想像——從嚴防資料洩漏、堅持特徵篩選只能用訓練集，到小兒膠質瘤案例中以病理報告為金標準、無病理報告時堅持三位資深醫師共識標註，都顯示醫療領域「錯一步就是人命」的高標準要求。同時也體認到臨床資料取得的限制（如小兒膠質瘤僅23例、憂鬱症僅77例），如何在小樣本下仍謹慎地建立可信模型（如留一法交叉驗證），是醫療AI研究中很實際的挑戰。

整體而言，這些案例示範了一個完整且值得參考的醫療AI研究範式：從貼近臨床問題出發、嚴謹處理資料、重視模型可解釋性，到最終仍將決策權交還給醫師——AI是「放大醫師判斷力的工具」，而非取代醫師的角色。

---

## 5. 關鍵字

`醫療人工智慧` `可解釋性AI (XAI)` `影像組學 (Radiomics)` `遷移學習` `資料洩漏`
`聽神經瘤` `伽馬刀` `黃斑部手術` `小兒低惡性度膠質瘤` `癲癇` `重度憂鬱症` `腦電波 (EEG)`


---

## 6. 參考文獻
**聽神經瘤 AI 預後與腫瘤反應研究（台北榮總資料庫）**
- Yang HC, Wu CC, Lee CC, Huang HE, Lee WK, Chung WY, Wu HM, Guo WY, Wu YT, Lu CF. *Prediction of pseudoprogression and long-term outcome of vestibular schwannoma after Gamma Knife radiosurgery based on preradiosurgical MR radiomics.* Radiotherapy and Oncology. 2021;155:123–130. https://doi.org/10.1016/j.radonc.2020.10.041
- Huang CY, Peng SJ, Wu HM, Yang HC, Chen CJ, Wang MC, Hu YS, Chen YW, Lin CJ, Guo WY, Pan DH. *Quantification of tumor response of cystic vestibular schwannoma to Gamma Knife radiosurgery by using artificial intelligence.* Journal of Neurosurgery. 2021;136(5):1298–1306.

**小兒低惡性度膠質瘤癲癇預測（影像組學＋腫瘤位置）**
- *Morphometric and radiomics analysis toward the prediction of epilepsy associated with supratentorial low-grade glioma in children.* Cancer Imaging. 2025;25:Article. https://doi.org/10.1186/s40644-025-00881-1
  （台北醫學大學附設醫院IRB核准研究；採用T2-FLAIR、MRMR特徵篩選、留一法交叉驗證，並發現顳葉與中腦位置為關鍵預測因子，與筆記內容高度吻合。）

**重度憂鬱症EEG長期療效預測（台北醫學大學）**
- *Predicting the longitudinal efficacy of medication for depression using electroencephalography and machine learning.* Journal of Psychiatric Research. 2025 (Epub ahead of print). PMID: 40829357. https://doi.org/10.1016/j.jpsychires.2025 （77名重度憂鬱症病人、治療前及第1週EEG、預測第4/6/8週療效，準確率分別為83.1%／73.3%／80.0%，與筆記內容高度吻合。）
- 台北醫學大學醫學院報導：AI EEG Drug-Response Prediction System: Offers a New Path Toward Personalized Depression Treatment. https://medicine-en.tmu.edu.tw/ai-eeg-drug-response-prediction-system-offers-a-new-path-toward-personalized-depression-treatment/

**黃斑部手術／OCT視力預測（相關領域代表性論文，機構與筆記所述之台北市立聯合醫院研究未能於網路檢索中確認完全對應，以下為方法高度相似之公開文獻，供參考）**
- Ganjee R, et al. *Predicting Visual Improvement After Macular Hole Surgery: A Combined Model Using Deep Learning and Clinical Features.* Translational Vision Science & Technology. 2022;11(4):3. https://doi.org/10.1167/tvst.11.4.3
- *Deep learning-based postoperative visual acuity prediction in idiopathic epiretinal membrane.* （ResNet系列遷移學習、OCT影像預測術後視力之相似方法論文）https://pmc.ncbi.nlm.nih.gov/articles/PMC10440890/
