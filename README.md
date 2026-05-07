# Whisper 逐字稿產生器

給定 YouTube 網址、本機 MP3 或上傳音訊檔，使用本機 [OpenAI Whisper](https://github.com/openai/whisper) 產出帶時間戳記的逐字稿，同時輸出純文字（`.txt`）與字幕（`.srt`）兩種格式。

## 功能

- 輸入來源：YouTube URL、本機 MP3、Colab 上傳
- 輸出格式：TXT（YouTube 風格時間戳記）、SRT（標準字幕）
- 支援 Google Colab（含免費 GPU 加速）與本機 Jupyter 兩種環境
- 在 Colab 以表單欄位輸入，無需修改程式碼

## 輸出範例

**TXT**
```
# 我的 Podcast EP.42
# 產生時間：2026-05-05 10:30:00

[0:00] 大家好，歡迎收聽今天的節目。
[0:12] 今天我們要討論的主題是...
[1:05] 首先來看第一個部分。
```

**SRT**
```
1
00:00:00,000 --> 00:00:04,500
大家好，歡迎收聽今天的節目。

2
00:00:04,800 --> 00:00:08,200
今天我們要討論的主題是...
```

## 使用方式

### Google Colab（建議）

1. 點擊 [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/chihchao/whisper-transcriber/blob/main/whisper-transcriber.ipynb) 開啟 notebook
2. 開啟 GPU（選用，可大幅加速）：`執行階段` → `變更執行階段類型` → `T4 GPU`
3. 在**設定** cell 填入模型與輸出參數
4. 在**輸入** cell 填入來源（YouTube URL、本機路徑，或勾選 `UPLOAD_IN_COLAB`）
5. 執行最後一個 cell（**安裝、轉錄、下載**）：
   - 若勾選 `UPLOAD_IN_COLAB`：cell 啟動後會立即顯示上傳介面，上傳完成後自動載入模型並開始轉錄
   - 若使用 YouTube URL：自動下載音訊後載入模型並開始轉錄
6. 轉錄結束後自動觸發 TXT 與 SRT 的瀏覽器下載

### 本機 Jupyter

**前置需求**

```bash
# ffmpeg（必要）
brew install ffmpeg          # macOS
sudo apt install ffmpeg      # Linux

# Python 套件（notebook 執行時自動安裝）
pip install openai-whisper yt-dlp
```

1. 在**設定** cell 填入模型與輸出參數
2. 在**輸入** cell 填入 `MP3_PATH` 或 `YOUTUBE_URL`（`UPLOAD_IN_COLAB` 請保持 `False`）
3. 執行最後一個 cell，轉錄完成後檔案會儲存至 notebook 同目錄

## 設定說明

| 參數 | 預設值 | 說明 |
| --- | --- | --- |
| `WHISPER_MODEL` | `medium` | 模型大小，見下方比較表 |
| `LANGUAGE` | 空白 | 留空自動偵測；填入如 `zh`、`en`、`ja` 可加速 |
| `MAX_CHARS_PER_LINE` | `0` | 每行最多字元數，`0` 表示不截斷 |
| `OUTPUT_FILE` | `transcript.txt` | TXT 輸出路徑，留空不儲存 |
| `OUTPUT_SRT_FILE` | `transcript.srt` | SRT 輸出路徑，留空不儲存 |

## 輸入來源選擇

| 情境 | 設定方式 |
| --- | --- |
| YouTube 影片 | 填入 `YOUTUBE_URL` |
| 本機 MP3（Jupyter） | 填入 `MP3_PATH` |
| 上傳音訊（Colab） | 勾選 `UPLOAD_IN_COLAB` |

三者同時設定時，優先順序為：**上傳 > YouTube URL > 本機路徑**。

## Whisper 模型比較

| 模型 | 速度 | 準確度 | VRAM |
| --- | --- | --- | --- |
| `tiny` | 最快 | 低 | ~1 GB |
| `base` | 快 | 普通 | ~1 GB |
| `small` | 中 | 中 | ~2 GB |
| `medium` | 中 | 好 | ~5 GB |
| `large-v3` | 慢 | 最高 | ~10 GB |

Colab 免費版 T4 GPU 建議使用 `medium`，效果與速度的平衡最佳。
