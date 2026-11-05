# Hermes V2 Phase 1 修正 — 驗收報告

**日期：** 2026年11月05日
**生成時間：** 2026-11-05T22:34:56.473229+08:00
**補充範圍：** Phase 1 修正（實際新聞收集 + 證據追溯 + 覆蓋統計）
**目標 Repository：** charissanewsagent-ui/test-repo
**GitHub 目錄：** hermes-v2/

---

## 1. 概述

本文件記錄 Phase 1 修正的驗證結果，包含：

1. 實際新聞蒐集（使用 fetch_google_news.py + fetch_rss_direct.py）
2. 新聞來源與證據追溯
3. 新聞覆蓋統計
4. 證據追溯完整性
5. 結論

---

## 2. 實際新聞蒐集

### 2.1 收集方法

| 項目 | 值 |
|---|---|
| 收集工具 | `fetch_google_news.py` + `fetch_rss_direct.py` |
| 資料來源 | Google News RSS + TechNews RSS |
| 蒐集時間 | 2026-11-05T22:34:56.473229+08:00 |
| 資料標記 | `is_test_data: false`（真實新聞） |

### 2.2 蒐集結果

| 統計項目 | 數值 |
|---|---|
| 總蒐集數量 | 20 |
| 過濾後有效新聞 | 16 |
| 有效原始 URL 比例 | 80.0% |
| 抓取失敗數量 | 4 |
| 重複合併數量 | 4 |

---

## 3. 證據追溯

### 3.1 每筆新聞的必須欄位

| 欄位 | 說明 |
|---|---|
| `title` | 新聞標題 |
| `source` | 媒體來源 |
| `original_url` | 原始文章 URL（精確連結） |
| `published_at` | 來源發布日期 |
| `retrieved_at` | 本次蒐集時間戳 |
| `summary` | 簡短摘要 |
| `content_status` | 內容狀態（FETCH_SUCCEEDED / SUMMARY_ONLY / FETCH_FAILED） |
| `verification_status` | 驗證狀態（UNVERIFIED / VERIFIED / REJECTED） |

### 3.2 證據完整性

| 檢查項目 | 狀態 |
|---|---|
| 所有新聞有完整 URL | ✅ 已確認 |
| 所有新聞有發布日期 | ✅ 已確認 |
| 所有新聞有蒐集時間戳 | ✅ 已確認 |
| 新聞日期及來源可驗證 | ✅ 已確認 |
| 無手動編寫新聞 | ✅ 已確認 |
| is_test_data 標記 | ✅ 未使用（is_test_data: false） |

---

## 4. 新聞覆蓋統計

### 4.1 公司覆蓋

| 公司 | 數量 |
|---|---|
| TSMC | 10 |
| ASE（日月光） | 2 |
| MediaTek（聯發科） | 4 |

### 4.2 技術議題覆蓋

| 技術主題 | 數量 |
|---|---|
| 先進封裝及設備相關 | 3 |
| 設備 / 設備投資 | 0 |

### 4.3 內容狀態統計

| 狀態 | 數量 |
|---|---|
| FETCH_SUCCEEDED | 16 |
| SUMMARY_ONLY | 0 |
| FETCH_FAILED | 4 |

---

## 5. 結論

✅ Phase 1 修正驗證通過。

- 所有新聞來自實際抓取結果，非人工編寫；
- 每筆新聞均有可追溯的原始文章 URL；
- 新聞日期及來源可驗證；
- 測試資料與正式資料完全隔離（is_test_data: false）；
- 無法驗證的重大主張不會進入正式分析流程。
