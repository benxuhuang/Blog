---
layout: post
title: Hermes Kanban × Tailscale：在手機上安全使用 Dashboard 與看板
color: turquoise
feature-img: "/assets/post-imgs/hermes-kanban-cover.svg"
thumbnail: "/assets/post-imgs/hermes-kanban-cover.svg"
excerpt_separator: <!--more-->
tags:
  - hermes
  - kanban
  - tailscale
  - dashboard
  - devops
---

如果你想把 Hermes Kanban 放到手機上用，重點不是「把服務打開」，而是 **在保有安全性的前提下，讓手機只透過私人網路進來**。這篇我把整個流程整理成一篇實戰教學：從 Tailscale 安裝、Dashboard 設定，到 Kanban 的實際使用方式，最後再用一個程式開發案例示範怎麼拆卡。

<!--more-->

## 先看整體架構

{% include aligner.html images="hermes-kanban-access-flow.svg,hermes-kanban-workflow.svg" column=2 %}

左邊這張圖是存取路徑，右邊這張圖是看板流程。前者解決「手機怎麼安全連進來」，後者解決「卡片該怎麼拆才不會亂」。

## 一、先把 Hermes Dashboard 跑起來

Dashboard 預設只要綁在本機就好，這樣可以避免直接把管理介面暴露到公網。

```bash
hermes dashboard --host 127.0.0.1 --no-open
```

預設埠是 `9119`。啟動後，Dashboard 會只聽本機，外部還進不來。這是比較安全的起點。

## 二、用 Tailscale 讓手機進得來

最簡單的做法，是讓 VPS 和手機加入同一個 Tailscale 私人網路，然後只在這個私人網路裡開放 Dashboard。

### VPS 端

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
tailscale ip -4
sudo tailscale serve --bg --yes --http=9119 127.0.0.1:9119
```

### 手機端

1. 安裝 Tailscale App
2. 登入同一個帳號
3. 用瀏覽器打開 Tailscale 提供的私人網址，或直接開 VPS 的 Tailscale IP
4. 進入 Hermes Dashboard 後，切到 **Kanban** 分頁

### 這樣做的好處

- 不需要把 9119 直接開到公網
- 手機與 VPS 之間走私人網路
- 只要你有登入同一個 tailnet，就能進來

## 三、Kanban 到底怎麼用

Hermes Kanban 比較像是「會跑的工作排程」，不是單純待辦清單。它最適合處理這種工作：

- 有前後順序
- 可以平行拆開
- 需要人回覆決策
- 需要保留過程與結果

### 一張卡只做一件事

這是最重要的原則。不要把研究、實作、測試、review 全塞在同一張卡裡。那樣只會讓 worker 猜題。

比較好的寫法是：

- 卡 1：研究需求與技術方案
- 卡 2：實作後端功能
- 卡 3：實作前端 UI
- 卡 4：測試與修正
- 卡 5：Review 與收尾

這樣看板會非常清楚，而且每張卡都能獨立追蹤。

## 四、用「程式開發」當案例

假設我要做一個新功能：**讓使用者可以在手機上打開 Hermes Dashboard，並順利進入 Kanban**。這個需求看起來是一件事，但實際上應該拆成幾張卡。

### 卡片 1：先研究安全存取方案

內容可以這樣寫：

> 研究如何讓手機安全連進 Hermes Dashboard。比較直接開公網、反向代理、Tailscale 私網的差異，最後給出建議方案。

這張卡的重點不是動手改，而是先把方向弄清楚。

### 卡片 2：實作 Dashboard 啟動方式

> 將 Dashboard 綁在 localhost，確認本機可運作，避免直接暴露到外網。

這一步通常會把基礎服務先穩住。

### 卡片 3：實作手機存取路徑

> 透過 Tailscale 讓手機進入私人網路，並驗證 Dashboard 可以從手機瀏覽器打開。

這張卡只負責「能連上」，不要再混進其他功能。

### 卡片 4：驗證與修正

> 從手機實測 Dashboard、檢查 Kanban 分頁、確認 host header 與 WebSocket 都正常。

如果這步卡住，就把具體錯誤寫進 comment，再讓卡片 block。

### 卡片 5：最後 review

> 檢查整個流程是否真的安全、可重現、可說明，並整理成文件。

這張卡的角色是收尾，不是重新開發。

## 五、什麼時候要 block，什麼時候要 complete

這個判斷很重要。

### 適合 block 的情況

- 你需要使用者決定方案
- 缺少關鍵資訊
- 需要外部資料，現在無法繼續

### 適合 complete 的情況

- 任務真的做完了
- 驗證也完成了
- 沒有留下未解決的前提

如果你只是「差一步但還沒驗證」，通常不要急著 complete。先把卡片狀態說清楚，比硬收尾好。

## 六、我會怎麼寫一張好卡

我通常會讓卡片至少包含這四件事：

- **目標**：到底要完成什麼
- **限制**：哪些事情不能做
- **驗收標準**：做到什麼算完成
- **備註**：補充資訊、錯誤訊息、參考路徑

例如：

```text
目標：讓手機能安全進入 Hermes Dashboard 的 Kanban 頁面
限制：不要直接把服務暴露到公網
驗收標準：手機瀏覽器可正常開啟 Dashboard，Kanban 分頁可讀寫
備註：若出現 host header 問題，先確認是否為代理層造成
```

這種卡片，worker 幾乎不用猜。

## 七、最常見的幾個坑

1. **把所有事情塞成一張卡**
   - 這是最常見的問題，也最難追。

2. **直接把 Dashboard 開到公網**
   - 方便，但風險大。

3. **卡住了不寫原因**
   - 只寫「blocked」沒什麼幫助，最好把缺什麼寫出來。

4. **沒有先驗證手機真的能連**
   - 伺服器起來不代表手機一定可用，還是要實測。

## 結語

Hermes Kanban 最好用的地方，是它讓工作變得可拆、可追、可回收。再加上 Tailscale 這種私人網路，你就可以很安心地在手機上打開 Dashboard，直接處理看板，不用把管理介面暴露到外面。

如果你要，我下一篇可以再補一篇更實際的版本：**「我在 Hermes Kanban 上怎麼寫卡、怎麼拆卡、怎麼讓 worker 做事」**，直接給你範本。
