# Whisper 逐字稿產生器

給定 YouTube 網址或 MP3 音訊檔，使用本機 [OpenAI Whisper](https://github.com/openai/whisper) 產出帶時間戳記的逐字稿，同時輸出純文字（`.txt`）與字幕（`.srt`）兩種格式。

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

1. 上傳 `whisper_transcriber.ipynb` 至 [Google Colab](https://colab.research.google.com/)
2. 開啟 GPU（選用，可大幅加速）：`執行階段` → `變更執行階段類型` → `T4 GPU`
3. 依序執行所有 cell
4. 在表單欄位填入設定與輸入來源，不需修改程式碼

### 本機 Jupyter

**前置需求**

```bash
# ffmpeg（必要）
brew install ffmpeg          # macOS
sudo apt install ffmpeg      # Linux

# Python 套件（notebook 第一個 cell 會自動安裝）
pip install openai-whisper yt-dlp
```

開啟 notebook 後依序執行所有 cell，在設定 cell 調整參數即可。

## 設定說明

| 參數 | 預設值 | 說明 |
|------|--------|------|
| `WHISPER_MODEL` | `medium` | 模型大小，見下方比較表 |
| `LANGUAGE` | 空白 | 留空自動偵測；填入如 `zh`、`en`、`ja` 可加速 |
| `MAX_CHARS_PER_LINE` | `0` | 每行最多字元數，`0` 表示不截斷 |
| `OUTPUT_FILE` | `transcript.txt` | TXT 輸出路徑，留空不儲存 |
| `OUTPUT_SRT_FILE` | `transcript.srt` | SRT 輸出路徑，留空不儲存 |

## Whisper 模型比較

| 模型 | 速度 | 準確度 | VRAM |
|------|------|--------|------|
| `tiny` | 最快 | 低 | ~1 GB |
| `base` | 快 | 普通 | ~1 GB |
| `small` | 中 | 中 | ~2 GB |
| `medium` | 中 | 好 | ~5 GB |
| `large-v3` | 慢 | 最高 | ~10 GB |

Colab 免費版 T4 GPU 建議使用 `medium`，效果與速度的平衡最佳。

## 輸入來源選擇

| 情境 | 設定方式 |
|------|----------|
| YouTube 影片 | 填入 `YOUTUBE_URL` |
| 本機 MP3（Jupyter）| 填入 `MP3_PATH` |
| 上傳 MP3（Colab）| 勾選 `UPLOAD_IN_COLAB` |

三者同時設定時，優先順序為：**上傳 > YouTube URL > 本機路徑**。
