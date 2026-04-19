# 📚 智能單字卡應用程式

一個基於 Streamlit 的智能單字卡學習工具,支援滑動操作和智能頻率調整。

## ✨ 功能特色

- ✅ **左右滑動評分**: 向左滑標記不熟,向右滑標記很熟
- 🎯 **智能出現頻率**: 根據熟悉度自動調整單字出現機率(不熟的單字出現頻率最高 20 倍)
- ➕ **快速新增生字**: 底部輸入欄位可隨時新增新單字
- 📊 **學習統計**: 即時顯示學習進度和單字熟悉度
- 💾 **自動儲存**: 所有學習進度自動保存到本地檔案
- 🎨 **漂亮介面**: 漸層紫色卡片設計,視覺效果佳

## 🚀 部署到 Streamlit Cloud

### 步驟 1: 上傳到 GitHub

1. 在 GitHub 建立新的 repository
2. 將所有檔案上傳到 repository 根目錄:
   - `app.py`
   - `requirements.txt`
   - `.python-version`
   - `.streamlit/config.toml`
   - `README.md`

### 步驟 2: 部署到 Streamlit Cloud

1. 前往 [streamlit.io/cloud](https://streamlit.io/cloud)
2. 使用 GitHub 帳號登入
3. 點擊 "New app"
4. 選擇你的 repository
5. 主檔案路徑設為 `app.py`
6. 點擊 "Deploy"

大約 2-3 分鐘後,你的 App 就會上線!

## 💻 本地運行

```bash
# 安裝依賴
pip install -r requirements.txt

# 運行應用程式
streamlit run app.py
```

瀏覽器會自動開啟 `http://localhost:8501`

## 📖 使用說明

### 基本操作

1. **查看單字**: 應用程式會自動顯示一個單字(根據熟悉度加權隨機選擇)
2. **顯示翻譯**: 點擊「👁️ 顯示/隱藏翻譯」按鈕
3. **評分**:
   - 點擊「⬅️ 不熟」: 熟悉度 -2 分,增加未來出現頻率
   - 點擊「➡️ 很熟」: 熟悉度 +2 分,減少未來出現頻率
4. **新增生字**: 在底部輸入欄位填寫英文單字和中文翻譯,點擊「新增單字」

### 側邊欄功能

- **學習統計**: 顯示總單字數、熟悉單字數、需加強單字數
- **單字列表**: 按熟悉度排序的完整單字列表
  - 🟢 綠色: 熟悉(≥5 分)
  - 🟡 黃色: 一般(0-4 分)
  - 🔴 紅色: 不熟(≤-1 分)
- **重置進度**: 將所有單字熟悉度歸零

## 🎲 熟悉度系統

- **範圍**: -10 (很不熟) 到 +10 (很熟)
- **權重計算**: 
  - 熟悉度 -10: 權重 20 (最常出現)
  - 熟悉度 0: 權重 10 (中等頻率)
  - 熟悉度 +10: 權重 1 (最少出現)
- **每次評分**: 
  - 不熟: -2 分
  - 很熟: +2 分

## 💾 資料儲存

所有單字和學習進度儲存在 `vocabulary_data.json` 檔案中:

```json
{
  "words": [
    {
      "word": "example",
      "translation": "範例",
      "familiarity": 5,
      "last_seen": "2026-04-19T17:30:00.123456"
    }
  ]
}
```

## 🛠 技術架構

- **前端框架**: Streamlit 1.32+
- **程式語言**: Python 3.11
- **資料格式**: JSON
- **部署平台**: Streamlit Cloud
- **版本控制**: GitHub

## ⚙️ 自訂設定

你可以根據需求修改 `app.py` 中的以下參數:

### 調整熟悉度變化幅度

```python
# 在按鈕事件中,目前為 ±2
word['familiarity'] = max(-10, word['familiarity'] - 2)  # 不熟
word['familiarity'] = min(10, word['familiarity'] + 2)   # 很熟
```

### 調整熟悉度範圍

```python
# 在 calculate_weight 函數中
def calculate_weight(familiarity):
    # 可以改成 -20 到 +20,或其他範圍
    return max(1, 20 - familiarity)
```

### 修改初始單字

```python
# 在 load_data 函數的 else 區塊中
"words": [
    {"word": "example", "translation": "範例", "familiarity": 0, "last_seen": None},
    # 加入你的初始單字
]
```

## 📂 專案結構

```
vocabulary/
├── app.py                    # 主程式
├── requirements.txt          # Python 依賴套件
├── .python-version          # Python 版本指定
├── .streamlit/
│   └── config.toml          # Streamlit 配置
├── README.md                # 專案說明
└── vocabulary_data.json     # 資料檔案(自動生成)
```

## 🐛 常見問題

### Q: 為什麼需要 `.python-version` 檔案?
A: Streamlit Cloud 預設使用 Python 3.14,但某些依賴套件尚未相容,指定 Python 3.11 可確保穩定運行。

### Q: 資料會遺失嗎?
A: 在 Streamlit Cloud 上,每次重新部署會清空資料。本地運行則會持久保存在 `vocabulary_data.json`。

### Q: 可以匯入大量單字嗎?
A: 可以!直接編輯 `vocabulary_data.json` 檔案,或修改程式加入批次匯入功能。

### Q: 能支援其他語言嗎?
A: 可以!只需在新增單字時輸入對應語言即可,例如日文、韓文等。

## 📝 授權

MIT License

## 🤝 貢獻

歡迎提交 Issue 或 Pull Request!

## 📧 聯絡

如有問題或建議,歡迎開 Issue 討論。

---

**祝你學習愉快!** 📚✨
