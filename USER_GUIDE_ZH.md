# Type on Strap - 繁體中文使用手冊 🎨

本手冊將引導您如何安裝、部屬及使用 `Type on Strap` Jekyll 主題。

---

## 1. 環境建置 🛠

在開始之前，請確保您的系統已安裝 **Ruby**。

### 安裝步驟：
1.  **複製專案**：
    ```bash
    git clone https://github.com/Sylhare/Type-on-Strap.git
    cd Type-on-Strap
    ```
2.  **安裝 Bundler 與依賴套件**：
    ```bash
    gem install bundler
    bundle install
    ```

---

## 2. 本地預覽 🖥

您可以在本地端啟動 Jekyll 伺服器，以便即時預覽修改內容。

```bash
bundle exec jekyll serve
```
啟動後，開啟瀏覽器並訪問 `http://localhost:4000` 即可看到您的部落格。

---

## 3. 基本配置 ⚙️

主要的設定皆位於專案根目錄的 `_config.yml` 檔案中。

### 重要設定項目：
*   `title`: 部落格名稱。
*   `description`: 部落格簡介（影響 SEO）。
*   `avatar`: 大頭貼路徑（預設為 `assets/img/avatar.png`）。
*   `color_theme`: 設定主題顏色（建議使用 `dark`，或選用 `auto`, `light`）。
*   `baseurl`: 若是在 GitHub Pages 的子目錄（如 `username.github.io/blog`），需設定為 `"/blog"`。

---

## 4. 發佈內容 📝

### 發佈文章 (Blog Posts)
所有文章檔案都應放在 `_posts` 資料夾中。

1.  **檔案命名規則**：`YYYY-MM-DD-您的標題.md`
2.  **文章 Front Matter**：
    ```markdown
    ---
    layout: post
    title: "這是我的第一篇文章"
    feature-img: "assets/img/sample.png"
    tags: [生活, 程式]
    ---
    ```

### 發佈作品集 (Portfolio)
作品集檔案放在 `_portfolio` 資料夾中。

*   **檔案命名規則**：`YYYY-MM-DD-標題.md`
*   **作品集屬性**：
    ```markdown
    ---
    layout: post
    title: "專案名稱"
    img: "assets/img/portfolio/project-thumbnail.png"
    ---
    ```

---

## 5. 後台管理 (Netlify CMS) 🖥️

本部落格已整合 **Netlify CMS**，提供圖形化介面來管理與發佈文章，無需手動編輯 Markdown 檔案。

### 進入後台
部署完成後，在瀏覽器訪問：
```
https://您的網域/admin/
```
系統會引導您透過 **GitHub 帳號**登入。

### 發佈流程
本專案啟用了 **Editorial Workflow（編輯工作流）**，文章的生命週期為：

1.  **草稿 (Draft)**：在後台點擊「New Blog」建立新文章，填寫標題、標籤、內文等。
2.  **審閱 (In Review)**：文章完成後，將狀態改為「In Review」（此步驟會在 GitHub 建立一個 Pull Request）。
3.  **發佈 (Ready / Publish)**：確認無誤後，點擊「Publish」即可正式上線（PR 會自動合併至 `master` 分支）。

### 後台可編輯欄位
| 欄位 | 說明 |
|---|---|
| **Title** | 文章標題 |
| **Publish Date** | 發佈日期 |
| **Tags** | 標籤（以清單方式輸入） |
| **Color** | 文章主題色（選填） |
| **Featured Image** | 精選圖片（選填） |
| **Thumbnail Image** | 縮圖（選填） |
| **Layout** | 版面配置（`post`, `page`, `search`） |
| **Hide** | 是否隱藏於導覽列 |
| **Bootstrap** | 是否啟用 Bootstrap |
| **Body** | 文章內文（Markdown 編輯器） |

### 媒體上傳
透過後台上傳的圖片會自動儲存至 `assets/post-imgs` 資料夾。

---

## 6. 進階功能 💡

### 標籤 (Tags)
在文章的 Front Matter 中新增 `tags: [標籤1, 標籤2]`，即可在 `tags.md` 頁面自動分類。

### 數學公式與圖表
*   **數學 (KateX)**：在 `_config.yml` 開啟 `katex: true` 即可使用 `$$` 包裹公式。
*   **圖表 (Mermaid)**：在 `_config.yml` 開啟 `mermaid: default` 即可在文章中使用 Mermaid 渲染圖表。

---

## 7. 自動化工具 (Gulp) 🚀

本主題內建了 Gulp 工具來簡化流程：
*   **快速建立文章**：
    ```bash
    cd assets/
    gulp post -n '文章標題'
    ```
*   **壓縮圖片與 JS/CSS**：
    ```bash
    gulp default
    ```

---

## 授權
本主題採用 [MIT 授權碼](https://github.com/Sylhare/Type-on-Strap/blob/master/LICENSE)。
