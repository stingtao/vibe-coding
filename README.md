# Vibe Coding 終極指南 V1.2
**作者：** [Nicolas Zullo, https://x.com/NicolasZu](https://x.com/NicolasZu)  
**建立日期：** 2025 年 3 月 12 日  
**最後更新日期：** 2025 年 10 月 06 日

---

## 快速開始
要開始 vibe coding，你只需要以下兩個工具之一：  
- **Claude Sonnet 4.5**，在 Claude Code 中
- **gpt-5-codex (high)**，在 Codex CLI 中

本指南同時適用於 CLI 版本（在終端使用）與 VSCode 擴充套件版本（Codex 與 Claude Code 都有，且介面較新）。

（註：早期版本本指南使用 **Grok 3**，之後轉用 **Gemini 2.5 Pro**。現在我們使用 **Claude 4.5**（或 **gpt-5-codex (high)**））

（註 2：若你想用 Cursor，請參考本指南的 [1.1 版](https://github.com/EnzeD/vibe-coding/tree/1.1.1)，不過我們認為它比起 Codex CLI 或 Claude Code 稍嫌弱一些）

把一切設好是關鍵。若你真心想打造一款功能完善且視覺出色的遊戲（或應用），請花時間打穩基礎。  

**關鍵原則：** 規劃即一切。不要讓 AI 自主規劃，否則你的程式碼庫會變成一團亂。

---

## 完整設定

### 1. 遊戲設計文件（GDD）
- 取用你的遊戲點子，請 **GPT-5** 或 **Sonnet 4.5** 以 Markdown 格式建立簡易的 **遊戲設計文件（GDD）**：`game-design-document.md`。  
- 審閱並微調此文件，確保其與你的願景一致。即便文件很基礎也沒關係——目標是提供 AI 關於遊戲結構與意圖的脈絡。先別過度工程化，稍後我們會漸進式迭代。

### 2. 技術棧與 `CLAUDE.md` / `Agents.md`
- 請 **GPT-5** 或 **Sonnet 4.5** 為你的遊戲推薦最合適的技術棧（例如多人 3D 遊戲可用 ThreeJS 與 WebSocket）。將建議存為 `tech-stack.md`。
  - 要求它提出「最簡單但最穩健」的技術棧。  
- 在終端開啟 **Claude Code** 或 **Codex CLI**，並使用 `/init` 指令。此指令會使用你目前建立的兩個 .md 檔，進而產生一組用來正確引導 LLM 的規則。
- **務必審查生成的規則。** 確保它們強調**模組化**（多檔案）並避免**巨石式架構**（單一巨檔）。你可能需要手動微調或新增規則，並檢查它們的觸發時機。
  - **重要：** 有些規則對維持脈絡至關重要，應設為 **「Always」**。這可確保 AI 在產生程式碼之前「一定」會參照它們。可考慮加入下面規則並標記為「Always」：
    > ```
    > # 重要：
    > # 在撰寫任何程式碼前，務必閱讀 memory-bank/@architecture.md。包含完整的資料庫結構。
    > # 在撰寫任何程式碼前，務必閱讀 memory-bank/@game-design-document.md。
    > # 在加入重大功能或完成里程碑後，請更新 memory-bank/@architecture.md。
    > ```
  - 例：確保其他（非「Always」）規則能引導 AI 遵循你的技術棧最佳實務（如網路、狀態管理等）。
  - *若你想要一個盡可能優化且維持乾淨的程式碼庫，這套規則設定至關重要。*

### 3. 實作計畫（Implementation Plan）
- 提供 **GPT-5** 或 **Sonnet 4.5**：  
  - 遊戲設計文件（`game-design-document.md`）
  - 技術棧建議（`tech-stack.md`）
- 要求它建立一份詳盡的 **實作計畫**（Markdown `.md`），內容是一組提供給 AI 開發者的逐步指示。  
  - 步驟應小而明確。  
  - 每一個步驟都必須包含用來驗證正確實作的測試。  
  - 不寫程式碼——只提供清楚、具體的指示。  
  - 聚焦於「基礎遊戲」，而非完整功能集（細節之後再加）。  

### 4. 記憶庫（Memory Bank）
- 新建你的專案資料夾，然後於 VSCode 開啟。
- 在專案資料夾內建立一個 `memory-bank` 子資料夾。  
- 在 `memory-bank` 內新增以下檔案：  
  - `game-design-document.md`  
  - `tech-stack.md`  
  - `implementation-plan.md`  
  - `progress.md`（建立此空白檔用於追蹤已完成步驟）  
  - `architecture.md`（建立此空白檔用於紀錄各檔案用途）

---

## 為基礎遊戲進行 Vibe Coding
現在有趣的部分開始了！

### 確保一切都清楚
- 在 VSCode 的擴充套件中開啟 **Codex** 或 **Claude Code**，或在專案的終端中啟動 Claude Code 或 Codex CLI。
- 提示：閱讀 `/memory-bank` 內的所有文件，`implementation-plan.md` 是否清楚？為了讓你 100% 清楚，你有哪些問題？
- 它通常會提出 9-10 個問題。請回答，並引導它編修 `implementation-plan.md`，讓文件更完善。

### 你的第一個實作提示
- 在 VSCode 擴充套件中開啟 **Codex** 或 **Claude Code**，或在專案的終端中啟動 Claude Code 或 Codex CLI。  
- 提示：閱讀 `/memory-bank` 的所有文件，並執行實作計畫的第 1 步。我會負責執行測試。在我驗證測試通過之前，請不要開始第 2 步。當我驗證通過後，開啟 `progress.md`，將你為未來開發者所做的工作記錄下來。接著，把任何關於架構的洞見新增到 `architecture.md`，解釋每個檔案的用途。
- **務必**以「詢問模式」或「計畫模式」（在 Claude Code 中按 `shift+tab`）開始；當你滿意後，再允許 AI 進行該步驟。

- **極致 vibe：** 安裝 [Superwhisper](https://superwhisper.com) 與 Claude 或 GPT-5 以輕鬆口語對話，取代打字。  

### 工作流程
- 完成第 1 步後：  
- 將你的變更提交到 Git（若不熟悉，請求你的 AI 協助）。  
- 開始新的聊天（`/new` 或 `/clear`）。  
- 提示：現在請通讀記憶庫中的所有檔案，先閱讀 `progress.md` 以理解既有工作，然後執行第 2 步。在我驗證測試前，請勿開始第 3 步。
- 重複此流程，直到整份 `implementation-plan.md` 完成。  

---

## 增添細節
恭喜，你已經完成了基礎遊戲！它可能還很粗糙、缺少功能，但現在你可以開始實驗並逐步打磨。  
- 想要霧效、後製、特效，或音效？更好的飛機／車子／城堡？更迷人的天空？
- 對於每個主要功能，建立一份新的 `feature-implementation.md`，並列出精簡步驟與測試。  
- 以漸進方式實作並測試。  

---

## 修復錯誤與卡關
- 當提示失敗或破壞了遊戲：  
- 在 Claude Code 使用 `/rewind`，微調你的提示直到可行。若使用 GPT-5，你可以常常提交到 git，必要時再重設。
- 若遇錯誤：  
    - **若是 JavaScript：** 開啟主控台（`F12`），複製錯誤，貼進 VSCode，並提供截圖以呈現視覺問題。  
    - **懶人選項：** 安裝 [BrowserTools](https://browsertools.agentdesk.ai/installation) 以免手動複製／截圖。  
- 若卡關：  
    - 回到你最後一次的 Git 提交（`git reset`），並用新的提示再嘗試。  
- 若「真的」卡住：  
    - 使用 [RepoPrompt](https://repoprompt.com/) 或 [uithub](https://uithub.com/)，把整個程式碼庫彙整成單一檔案，再請 **GPT-5 或 Claude** 協助。  

---

## Claude Code 與 Codex 小技巧
- **在終端使用 Codex CLI 或 Claude Code：** 於 VSCode 的終端中執行任一工具，以檢視差異並在不離開工作區的情況下提供更多上下文。
- **Claude Code 的 `/rewind`：** 若某次迭代未達預期，使用此指令把專案回滾至較早狀態。
- **自訂 Claude Code 指令：** 建立像 `/explain $arguments` 的小幫手，它會觸發如下提示：「深入解析程式碼並理解 $arguments 的運作。一旦理解，我會提供你要執行的任務。」讓模型在編輯前先取得豐富脈絡。
- **清理上下文：** 若仍需要先前對話的上下文，請經常使用 `/clear` 或 `/compact` 進行清理。
- **省時（風險自負）：** 使用 `claude --dangerously-skip-permissions` 或 `codex --yolo` 在永不詢問確認的模式下啟動 Claude Code 或 Codex CLI。

## 其他提示
- **小幅編修：** 使用 GPT-5（medium）
- **優質行銷文案：** 使用 Opus 4.1
- **產生優質像素圖（2D 圖片）：** 使用 ChatGPT 與 Nano Banana
- **產生音樂：** 使用 Suno
- **產生音效：** 使用 ElevenLabs
- **產生影片：** 使用 Sora 2
- **更好的提示輸出：**  
    - 添增：「think as long as needed to get this right, I am not in a hurry. What matters is that you follow precisely what I ask you and execute it perfectly. Ask me questions if I am not precise enough."（盡量思考把事情做好，我不趕時間。重點是精準遵循我的要求並完美執行。若我不夠精確，請發問。）」  
    - 在 Claude Code 中，可用特定片語觸發更深入的推理：`think` < `think hard` < `think harder` < `ultrathink`。

---

## 常見問題
**Q：我在做的是應用，不是遊戲，流程是否相同？**  
**A：** 大致相同！把 GDD（遊戲設計文件）換成 PRD（產品需求文件）即可。你也可以先使用 v0、Lovable 或 Bolt.new 進行原型製作，之後把程式碼移至 GitHub，然後在 VSCode 或終端中依照本指南繼續開發。

**Q：你空戰遊戲中的飛機很棒，但我無法用一個提示就完成！**  
**A：** 不是一個提示就能完成——大約需要 30 個提示，並由特定的 `plane-implementation.md` 文件引導。請使用銳利、具體的提示，例如「在機翼上為副翼預留空間」，而非模糊的「做一架飛機」。

**Q：為什麼現在 Claude Code 或 Codex CLI 比 Cursor 更好用？**  
**A：** 這取決於你的喜好。我們強調 Claude Code 在運用 Claude Sonnet 4.5 上表現更佳，而 Codex CLI 在運用 GPT-5 上更勝一籌，這都比 Cursor 目前的體驗更好。把它們放在終端使用可解鎖更多開發流程：在任意 IDE 工作、透過 SSH 連線到遠端伺服器，等等。還有強大的自訂選項（自訂命令、子代理、hooks），能隨時間加速品質與效率。最後，即使你只訂閱較低階的 Claude 或 ChatGPT 方案，也已足夠開始。

**Q：我不會為我的多人遊戲架設伺服器**  
**A：** 問你的 AI。

---
