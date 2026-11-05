
# Hermes V2 Phase 1 Validation Report

**日期：** 2026年11月05日
**生成時間：** 2026-11-05T21:33:52.494696+08:00
**範圍：** Phase 1 驗證（最小 LLM 測試、格式檢查、安全檢查）
**目標 Repository：** charissanewsagent-ui/test-repo
**GitHub 目錄：** hermes-v2/

---

## 1. 概述

本文件記錄 Phase 1 的驗證結果，包含：

1. 正式 Cron 執行鏈確認
2. Hermes LLM 能力確認
3. 最小 LLM 測試結果
4. GitHub 檔案上傳準備
5. 敏感資訊檢查

---

## 2. 正式 Cron 執行鏈

### 排程設定

| 時間 | 任務 | 腳本/命令 |
|---|---|---|
| 06:00 | 正式每日新聞 Cron 啟動 | `run_daily_1103.py` |
| 06:30 | Watchdog 早期檢查 | `watchdog_0630_early_check.sh` |
| 07:00 | 推播時間（正式） | LINE push / Discord push |
| 07:10 | Watchdog 驗證/補發 | `watchdog_verify_and_push.sh` |

### 函數呼叫流程

```
crontab (06:00)
  └─ run_daily_1103.py
      └─ daily_news_cron_v3.py (main())
          ├─ validate_version()
          ├─ generate_version(version)
          │   └─ 呼叫 generate_vp_report6.py / generate_vp_report6_v7.py
          ├─ sanitize_version(version)
          │   └─ sanitize_banned_words() + MP3 重新生成
          └─ upload_to_supabase()
              └─ Supabase Storage「news」存儲 TXT + MP3
```

### 輸出路徑

/home/ase/.hermes/cron/output/
├── tsmc-daily-deep-report-{YYYYMMDD}-utf8.txt
├── tsmc-daily-deep-report-{YYYYMMDD}-zh.mp3
├── tsmc-ase-mediatek-daily-deep-report-{YYYYMMDD}-utf8.txt
├── tsmc-ase-mediatek-daily-deep-report-{YYYYMMDD}-zh.mp3
└── {YYYYMMDD}_audit.log

---

## 3. Hermes LLM 能力確認

| 項目 | 值 |
|---|---|
| 模型 | upstage/solar-mini4:free |
| Provider | nous |
| Base URL | https://inference-api.nousresearch.com/v1 |
| 認證來源 | /home/ase/.hermes/auth.json（providers.nous） |
| 可獨立呼叫 | ✅ 已測試成功 |
| 最小測試訊息 | 3 筆新聞 → 200 OK |

---

## 4. 最小 LLM 測試結果

### 4.1 測試範圍

- 測試模型：`upstage/solar-mini4:free`
- 測試來源：Nous API（agent_key）
- 輸入：3 筆本地測試新聞（Reuters / TechNews / DigiTimes 風格）
- 輸出：繁體中文產業分析
- 結果：✅ 成功（HTTP 200）

### 4.2 測試內容

**新聞一（Reuters）：** TSMC 2nm 產能擴張遲於預期

**新聞二（TechNews）：** 台積電 3nm N2 量產進度拖延

**新聞三（DigiTimes）：** CoWoS 供應鏈多元化

### 4.3 輸出摘要

| 能力 | 結果 |
|---|---|
| 繁體中文理解 | ✅ 良好 |
| 事實提取 | ✅ 能抽取 |
| 技術與供應鏈分析 | ✅ 能分析 |
| 事實/推論區分 | ✅ 可標記 |
| 產業長篇寫作 | ✅ 可生成（已達 800 tokens 上限）|

---

## 5. GitHub 上傳檔案清單

| 檔案 | 路徑 | 大小 |
|---|---|---|
| news_events | hermes-v2/news_events_{date}.json | ~1.2 KB |
| phase1_validation | hermes-v2/phase1_validation_{date}.md | ~5 KB |
| samples | hermes-v2/samples_{date}.json | ~0.8 KB |

---

## 6. 敏感資訊檢查

### 已檢查項目

- [x] API Key
- [x] Token
- [x] Cookie
- [x] 內網 IP
- [x] 帳號資訊
- [x] auth.json
- [x] .env

### 檢查結果

✅ 所有檔案均已通過敏感資訊檢查。上傳檔案不包含任何 API Key、Token、Cookie、內網 IP 或帳號資訊。

---

## 7. 結論

✅ Phase 1 驗證通過。所有檢查均已通過。
