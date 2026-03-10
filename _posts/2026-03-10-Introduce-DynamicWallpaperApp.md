---
layout: post
title: 使用 .NET MAUI Blazor 與 Firebase 打造跨平台 AI 動態桌布 App (DynamicWallpaperApp)
color: blue
excerpt_separator: <!--more-->
tags:
  - C#
  - .NET MAUI
  - Blazor
  - Firebase
  - AI
---

在現今的行動應用開發中，跨平台框架帶來了極大的便利性。今天我想向大家介紹我近期開發的專案 **DynamicWallpaperApp** — 這是一款結合了人工智慧，讓使用者可以輕鬆生成並使用 AI 動態桌布的跨平台 App。

本篇將分享這個專案的開發架構、技術棧以及一些核心設計模式。

<!--more-->

## 📱 專案簡介與核心功能

**DynamicWallpaperApp** 支援 Android 和 iOS 平台。主要提供以下核心功能：

- 🎨 **AI 生成桌布**: 使用者輸入文字提示詞（如「夕陽海灘」）並選擇風格（寫實、動漫、抽象等），AI 就會自動生成高品質桌布。
- 🖼️ **桌布預覽與設定**: 即時預覽生成的桌布，並提供**一鍵設定為手機桌布**的功能。
- 🔥 **熱門桌布社群**: 可以瀏覽社群上的熱門桌布，並進行按讚、收藏與分享。
- 💰 **廣告與商業化**: 整合 AdMob 插屏廣告，並透過 Firebase Remote Config 即時控制廣告出現頻率與開關。
- 🔄 **動態 API 切換**: 支援多個 AI Provider（如 Stability AI、Replicate），可同樣透過遠端配置即時熱切換。
- 📜 **歷史與收藏快取**: 查看生成歷史，並透過 LiteDB 實現本地儲存快取。

## 🏗️ 技術架構與技術棧

為了達成一次編寫、多平台運行的目標，本專案採用了最新的 **.NET 10 MAUI Blazor Hybrid** 技術棧：

- **前端框架**: .NET 10 MAUI Blazor
- **UI 框架**: HTML 結合 Tailwind CSS 與 Blazor Components，取代傳統 XAML 畫面。
- **後端服務**: Firebase (提供 Auth 認證, Firestore 即時資料庫, Cloud Storage 雲端儲存, Remote Config 動態配置與 Cloud Functions 函數)
- **本地儲存快取**: LiteDB
- **廣告變現**: AdMob
- **AI 影像生成 API**: Stability AI / Replicate (作為備援)

使用 Blazor Hybrid 的好處在於，我們能夠使用熟悉的 C# 與 Web 語法來建構現代化且華麗的介面設計，大幅節省學習原生 UI 系統的成本。與此同時，藉助 MAUI 所包覆的原生外殼，又可以直接呼叫底層原生 API（例如 Android / iOS 的桌布設定介面與權限）。

## 📐 核心架構與設計模式

在軟體架構層面，整個應用程式圍繞著高內聚、低耦合的原則進行設計，並在 `benxu.AppPlatform.Core` 與其他核心層廣泛運用了以下設計模式與實踐：

### 1. 策略模式 (Strategy Pattern) 支援 AI Provider 動態切換
因為 AI 繪圖服務偶爾會遇到穩定性問題或 API 限制，因此系統設計了 `IImageGenerationProvider` 介面，並分別實作了 `StabilityAIProvider` 與 `ReplicateProvider` 類別。
只要透過 Firebase Remote Config 更新 `active_image_provider` 參數值，我們就可以在不需要重推 App 上架更新的情況下，動態從後台將主要產圖的提供商從 Stability AI 切換為 Replicate。

### 2. Result Pattern (統一錯誤處理)
在跨平台應用中，隨意拋出 `Exception` 不僅會導致效能下降，在不同平台上還非常難以追蹤跟 Debug。專案全面採用了 `Result Pattern` 設計，將 API 成功的回傳資料或錯誤訊息，包裝成統一個 `Result<T>` 物件回傳。這使得呼叫端可以優雅地進行判斷與處理，避免直接閃退的問題。

### 3. 依賴注入 (Dependency Injection) 與介面導向 (Interface-Oriented)
系統中包含多個核心抽象曾，並透過事件驅動架構 (Event-Driven) 進行通訊，例如：
- `IAppStateManager` - 負責全域狀態管理機制
- `IAuthService` - 封裝 Firebase Authentication 匿名認證邏輯
- `ILocalStorage` - 封裝 LiteDB 本地儲存資料的操作與查詢
- `IRemoteConfigService` - 封裝 Firebase Remote Config 配置

所有註冊的服務都在 `MauiProgram.cs` 統一進行生命週期定義，這大大提高了程式碼的可測試性、擴展性與可維護性。

## 💡 開發心得與總結

開發 **DynamicWallpaperApp** 的過程讓我更確信了 **.NET MAUI Blazor** 在跨雙平台的潛力。它不僅能夠重複使用 Web 開發的寶貴經驗，甚至可以重用我們熟悉的第三方 UI 庫 (如 Tailwind 等) 來極速刻板，而不用被困在冗長的 XAML 畫布設定。

有了 **Firebase** 當作強化的 Serverless 驅動後台，不論是 Firebase Auth、即時串接 Firestore、AdMob 或是 Remote Config，均為 App 提供了無限的擴充性與高穩定性。

如果您也對 .NET 跨平台開發或是人工智慧應用程式感興趣，期望本篇的架構分享能帶給您一點啟發！
