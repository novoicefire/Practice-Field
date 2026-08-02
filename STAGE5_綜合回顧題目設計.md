# Stage 5「綜合回顧」新版題目設計

> 適用專案：`Practice-Field`  
> 適用檔案：單一頁面 SPA `index.html`  
> 文件定位：本文件是 Stage 5 題目、答案、回饋與通關規則的內容規格，也是後續實作的題庫來源。

---

## 1. Stage 5 的任務

Stage 5 不應再像 GitHub 細節考試，也不應重做一次 Stage 4 的逐步操作。它只需要確認學生經過前四階段後，是否已經理解以下完整協作邏輯：

> **把工作說清楚 → 分配負責人 → 在安全的工作版本修改 → 留下可追蹤紀錄 → 請隊友檢查 → 修正後再確認 → 合併成正式版本 → 確認進度已更新**

### 目標受眾

- 沒有程式設計背景的台灣管理學院學生
- 第一次接觸 GitHub、Repository、Branch、Commit、Pull Request 的學生
- 只需要先學會用 GitHub 網頁做小組分工、上傳檔案、查看修改與整合成果的學生

### 題目設計原則

1. 所有題目都放在同一個「校園市集企劃書」情境中，不突然切換成工程師情境。
2. 只考前四階段實際學過、看過或操作過的內容。
3. 不把 SHA、Push、終端機指令、分支保護細節或 Project automation 設定當成核心通關條件。
4. 不考人名記憶、按鈕位置、顏色或畫面機械點擊。
5. 每題都要能回答：「這個判斷會如何幫助小組合作？」
6. 答錯後先呈現實際後果與白話提示，不另外插入內容高度重複的補強題。
7. 最後一題必須整合完整流程，而不是再考一個零碎名詞。

---

## 2. 統一情境

你和三位組員正在完成「校園市集企劃書」。教授要求星期五前繳交，團隊已經在 GitHub 建立：

- Repository：`campus-market`
- 正式版本：`main`
- 主要檔案：`market-plan.md`
- Project 看板：`Campus Market Project`

目前團隊要補上目標客群分析、修正活動預算，並在繳交前完成隊友審查。

---

## 3. 題目路線總覽

| 題號 | 題目主題 | 核心理解 | 題型 |
|---|---|---|---|
| 1 | 正式版到底在哪裡？ | Repository 與 main 的角色 | 單選 |
| 2 | 這張工作要怎麼分？ | Issue、Assignee、Project | 單選 |
| 3 | 怎麼修改才不會弄亂正式版？ | Branch 的用途 | 單選 |
| 4 | 怎麼留下隊友看得懂的版本？ | Upload/Edit、Commit、Commit message | 單選 |
| 5 | 怎麼請隊友採用修改？ | Pull Request 的目的與內容 | 單選 |
| 6 | 發現錯誤後怎麼審查？ | Diff、Request changes、修正後重新確認、Approve | 兩步情境題 |
| 7 | 從接到任務到正式完成 | 完整協作流程 | 排序題 |

固定路線：

> `intro → q1 → q2 → q3 → q4 → q5 → q6 → q7 → report`

不再根據弱項插入其他重複題目。弱項只影響即時回饋與最後報告。

---

# 4. 完整題目內容

## Q1｜正式版到底在哪裡？

### 學習目標

確認學生理解 Repository 是共同工作區，`main` 是團隊目前認可的正式版本。

### 情境

LINE 群組裡出現：

- `企劃書_final.docx`
- `企劃書_final_v2.docx`
- `企劃書_真的最終版.docx`

組員已經不知道哪一份才要交給教授。

### 題目

> 接下來怎麼做，最能避免團隊繼續出現很多個「最終版」？

### 選項

- **A｜正確**：把共同檔案集中在 `campus-market` Repository，並把 `main` 當成目前認可的正式版本。
- **B**：繼續在 LINE 傳檔，只要每次把檔名加上新的 `final`。
- **C**：每位組員各自建立一個 Repository，最後再猜哪一份最新。
- **D**：所有人都直接修改 `main`，這樣最快。

### 正確答案

`A`

### 正確回饋

> 對。Repository 是團隊共同工作的地方，`main` 則代表目前認可的正式版本。之後的新修改應先在工作 Branch 完成，不必再靠檔名猜版本。

### 錯誤回饋

- **選 B**：檔名一直加 `final` 仍然無法知道誰改了什麼，也容易同時出現多份最終版。
- **選 C**：每人一個 Repository 會讓資料更分散，團隊仍然缺少共同正式版本。
- **選 D**：直接修改 `main` 可能讓未完成或未檢查的內容立刻進入正式版。下一題會回顧更安全的修改方式。

### 白話類比

- Repository：小組共同使用的資料室
- `main`：目前準備交給教授的正式企劃書

### English copy

**Question:** What should the team do to avoid creating more “final” files?  
**Correct option:** Keep the shared files in the `campus-market` repository and treat `main` as the currently accepted official version.

---

## Q2｜這張工作要怎麼分？

### 學習目標

確認學生知道 Issue 用來說清楚工作，Assignee 表示主要負責人，Project 用來查看整體進度。

### 情境

團隊發現企劃書缺少「18–24 歲目標客群的資料來源」，Jamie 負責在星期三前補完，目前正在處理。

### 題目

> 哪一個安排最能讓全組知道「要做什麼、誰負責、目前做到哪裡」？

### 選項

- **A｜正確**：建立 Issue「補上 18–24 歲目標客群資料來源」，Assignee 指定 Jamie，並在 Project 標為 `In progress`。
- **B**：在 LINE 留言「有人記得補資料」，不指定負責人。
- **C**：建立名稱只有「報告」的 Issue，不寫完成條件，直接放到 `Done`。
- **D**：把企劃書上傳到 Project，看板就會自動知道誰要修改。

### 正確答案

`A`

### 正確回饋

> 對。Issue 說明具體工作與完成條件，Assignee 標示主要負責人，Project 則讓全組看到這張工作目前的狀態。

### 錯誤回饋

- **選 B**：只有模糊留言，沒有人能確認誰應該負責，也不知道怎樣才算完成。
- **選 C**：名稱太模糊且狀態不符合實際進度，容易讓團隊誤以為工作已完成。
- **選 D**：Project 是追蹤工作進度的看板，不會只靠上傳檔案自動理解分工內容。

### 最低必要內容

這題只要求學生理解三者用途，不考 Due date、Label、Milestone 或 automation 設定。

### English copy

**Question:** Which setup best shows what needs to be done, who owns it, and its current status?  
**Correct option:** Create a clear Issue, assign Jamie, and place it in `In progress` on the Project.

---

## Q3｜怎麼修改才不會弄亂正式版？

### 學習目標

確認學生理解 Branch 是從正式版本分出的安全工作版本，未完成內容不應直接進入 `main`。

### 情境

你要修改 `market-plan.md` 的目標客群段落，但 `main` 是全組目前認可、準備繳交的版本。

### 題目

> 你應該先怎麼做？

### 選項

- **A｜正確**：從 `main` 建立工作 Branch，例如 `update-target-audience`，再到這個 Branch 修改或上傳檔案。
- **B**：直接在 `main` 修改，改錯再請大家想辦法恢復。
- **C**：下載檔案並改名為 `market-plan-final-v8.md`，再傳到 LINE。
- **D**：請其他組員先不要工作，等你改完再說。

### 正確答案

`A`

### 正確回饋

> 對。Branch 讓你在不影響正式版的情況下完成修改；等隊友檢查通過後，才把成果合併到 `main`。

### 錯誤回饋

- **選 B**：未完成內容會直接進入正式版，其他組員也可能同時受到影響。
- **選 C**：重新依賴檔名管理版本，會回到一開始的版本混亂。
- **選 D**：GitHub 協作的目的就是讓不同工作能安全並行，不需要讓全組停工。

### English copy

**Question:** What should you do before editing the official version?  
**Correct option:** Create a working branch from `main`, such as `update-target-audience`, and make the change there.

---

## Q4｜怎麼留下隊友看得懂的版本？

### 學習目標

確認學生理解：在正確 Branch 修改或上傳檔案後，應建立 Commit，並用具體訊息說明本次改動。

### 情境

你已切換到 `update-target-audience`，並準備上傳修改後的 `market-plan.md`。

### 題目

> 哪一個做法最能讓隊友之後查得懂這次修改？

### 選項

- **A｜正確**：確認目前在工作 Branch，上傳或修改檔案，並用「補上 18–24 歲目標客群與資料來源」作為 Commit message。
- **B**：切回 `main` 上傳檔案，Commit message 只寫 `update`。
- **C**：只在自己電腦按儲存，不在 GitHub 留下 Commit。
- **D**：把檔名改成 `market-plan-final-final.md`，不需要說明改了什麼。

### 正確答案

`A`

### 正確回饋

> 對。Commit 是一筆可追蹤的版本紀錄；具體的 Commit message 可以讓隊友快速知道這次修改的內容，而不用打開每個檔案猜測。

### 錯誤回饋

- **選 B**：直接在 `main` 修改不安全，而且 `update` 無法讓隊友理解改動內容。
- **選 C**：電腦上的普通儲存不會自動成為 GitHub 的版本紀錄，隊友也看不到這次修改。
- **選 D**：改檔名無法取代版本紀錄與修改說明，仍可能形成多個最終版。

### 本題刻意不考

- SHA 的格式或用途
- `git add`、`git commit`、`git push` 指令
- Upload files 與 Push 的術語辨識

這些不是目前 Web-only 初學路線的必要通關知識。

### English copy

**Question:** Which action leaves a version record that teammates can understand later?  
**Correct option:** Upload or edit the file on the working branch and use a specific commit message such as “Add the 18–24 target audience and sources.”

---

## Q5｜怎麼請隊友採用修改？

### 學習目標

確認學生知道 Pull Request 是把工作 Branch 的修改交給隊友查看、討論並決定是否放進 `main`。

### 情境

`update-target-audience` 上的內容已完成，你希望隊友檢查後把它放進正式版。

### 題目

> 下一步怎麼做最合適？

### 選項

- **A｜正確**：建立從 `update-target-audience` 到 `main` 的 Pull Request，寫清楚改了什麼、為什麼要改，以及希望隊友檢查什麼。
- **B**：在 LINE 說「我改好了」，請別人自行下載並複製到正式檔。
- **C**：不經過隊友檢查，直接把自己的 Branch Merge 到 `main`。
- **D**：再建立一個新的 Repository 存放修改後的檔案。

### 正確答案

`A`

### 建議出現在模擬 PR 的內容

**Title**

> 補上目標客群與資料來源

**Description**

> - 補上 18–24 歲目標客群說明  
> - 新增問卷與校內資料來源  
> - 請確認資料來源是否足以支持企劃結論

### 正確回饋

> 對。Pull Request 不是單純通知「我做完了」，而是把修改內容、原因與需要檢查的地方整理給隊友，讓團隊能在合併前討論。

### 錯誤回饋

- **選 B**：回到手動複製與傳檔流程，容易漏掉修改，也沒有集中審查紀錄。
- **選 C**：未經檢查就合併，可能讓錯誤直接進入正式版。
- **選 D**：修改應留在同一個專案的工作 Branch，不需要為每次修改建立新 Repository。

### English copy

**Question:** What is the best next step when your branch is ready for team review?  
**Correct option:** Open a Pull Request from the working branch to `main` and clearly describe the change, reason, and review request.

---

## Q6｜發現錯誤後怎麼審查？

### 學習目標

確認學生理解 Review 是檢查實際修改內容；必須修正的問題應使用 Request changes，修正後要重新查看最新變更再 Approve。

### 情境

隊友查看 Pull Request 時發現：

> 活動預算寫成「場地費 5」，但應該是「場地費 5,000 元」。這個錯誤若不修正，企劃書不能繳交。

### 第一步題目

> 審查者現在最適合怎麼做？

### 第一步選項

- **A｜正確**：在有問題的修改旁留下具體說明，並選擇 `Request changes`，要求修正後再審查。
- **B**：選擇 `Approve`，因為至少對方有完成工作。
- **C**：只在 LINE 說「怪怪的」，不在 Pull Request 留下紀錄。
- **D**：直接 Merge，之後有空再改。

### 第一步正確回饋

> 對。這是合併前必須修正的問題，應指出具體位置與正確內容，並用 Request changes 清楚表達「修正後才能通過」。

### 第一步錯誤回饋

- **選 B**：Approve 代表你認為目前修改可以採用，與「這個錯誤必須先修正」矛盾。
- **選 C**：只在 LINE 留言會讓問題和修改紀錄分散，也不容易確認是否已處理。
- **選 D**：已知錯誤不應先進入正式版。

### 情境更新

修改者新增修正，把內容改成「場地費 5,000 元」。

### 第二步題目

> 修正送出後，審查者下一步應該怎麼做？

### 第二步選項

- **A｜正確**：重新查看 Pull Request 的最新修改，確認問題真的已修正，再選擇 `Approve`。
- **B**：沿用修正前的印象直接 Approve，不必再看內容。
- **C**：因為曾經 Request changes，所以這個 Pull Request 永遠不能通過。
- **D**：只要看到新增 Commit，就代表內容一定正確，可以直接 Merge。

### 第二步正確回饋

> 對。重點不是記住 SHA，而是知道「內容改過後，要重新看最新修改」，確認原本要求修正的問題已經解決。

### 第二步錯誤回饋

- **選 B**：內容已經改變，審查結論應建立在目前最新修改上。
- **選 C**：Request changes 是要求修正，不是永久否決；問題解決後仍然可以通過。
- **選 D**：新增版本只代表內容有改動，不代表改動一定正確。

### 本題刻意不考

- 舊核准是否因特定 Branch protection 規則而自動失效
- 最新 Commit 的 SHA
- 按鈕在畫面上的固定位置

### English copy

**Step 1:** A required budget correction is missing. What should the reviewer do?  
**Correct:** Leave a specific review comment and request changes.  

**Step 2:** The author submits a fix. What should the reviewer do next?  
**Correct:** Review the latest change, verify the fix, and then approve.

---

## Q7｜從接到任務到正式完成

### 學習目標

檢查學生能否把前面分散的名詞與操作串成一套完整、可實際使用的小組協作流程。

### 情境

團隊要完成「修正活動預算」這項工作。請把下面七張卡片排成合理順序。

### 排序卡片

1. **建立 Issue，寫清楚工作與完成條件，並指定主要負責人。**
2. **從 `main` 建立工作 Branch。**
3. **在工作 Branch 修改或上傳檔案，建立有清楚說明的 Commit。**
4. **建立 Pull Request，請隊友查看這次修改。**
5. **隊友 Review；若發現問題就 Request changes，修改者修正後再重新確認。**
6. **確認修改可採用後 Approve，並 Merge 到 `main`。**
7. **確認 Issue 與 Project 顯示的進度符合實際狀況；需要時手動更新。**

### 正確順序

`1 → 2 → 3 → 4 → 5 → 6 → 7`

### 錯誤回饋規則

不要只顯示「順序錯誤」。系統應指出第一個不合理的相鄰關係，例如：

- Merge 被放在 Review 前：
  > 還沒讓隊友檢查，就把內容放進正式版了。先找出應該發生在 Merge 前的步驟。

- Commit 被放在修改前：
  > Commit 是記錄一次已完成的修改；先完成檔案變更，再留下版本紀錄。

- PR 被放在 Branch 前：
  > Pull Request 是把工作 Branch 的修改交給團隊審查，因此要先有工作 Branch 與修改內容。

- Project 更新被放在最前面：
  > 進度狀態應反映真實工作；先完成協作流程，再確認看板是否需要更新。

### 正確回饋

> 完成。你已經把 GitHub 當成一套小組協作流程，而不是一堆分散按鈕：Issue 說清楚工作，Branch 保護正式版，Commit 留下版本，Pull Request 集中討論，Review 確認品質，Merge 更新正式版，Project 反映進度。

### English copy

Arrange the workflow in this order:

> Issue and assignee → Working branch → Edit/upload and commit → Pull Request → Review and fix → Approve and merge → Confirm task and project status

---

# 5. 答錯、提示與通關規則

## 5.1 不使用重複補強題

每題固定只出現一次。答錯時在原題內提供：

1. 此選擇在小組中會造成的實際後果
2. 一個白話提示
3. 重新選擇機會

不再跳到 `version-repair`、`pr-repair`、`tracking-repair` 等高度重複頁面。

## 5.2 建議狀態判定

| 狀態 | 判定方式 | 報告用語 |
|---|---|---|
| `mastered` | 第一次即答對 | 已掌握 |
| `clarified` | 看過一次錯誤回饋後答對 | 經提示後已釐清 |
| `needs-review` | 連續答錯兩次後才在完整說明協助下完成 | 建議再回顧 |

所有學生最後都能完成 Stage 5，但報告應誠實呈現哪些概念需要回顧，不用以分數羞辱或阻擋新手。

## 5.3 第二次答錯後的處理

第二次答錯時：

- 顯示完整白話解釋
- 將正確判斷的關鍵詞加粗
- 不直接替學生自動選答案
- 仍要求學生親自選出正確答案後才能前進
- 該題在報告中標記為 `needs-review`

## 5.4 不以無意義條件通關

禁止使用以下通關條件：

- 至少輸入 20 個字
- 自己勾選「我有提到……」就算正確
- 點擊指定顏色或按鈕位置
- 記住 Alex、Jamie 等情境人名，除非題目當下明確提供負責資訊
- 背出 SHA
- 區分未實際學過的 Git CLI 指令

---

# 6. 最終回顧報告

報告不顯示百分比分數，改顯示五個能力面向：

| 能力面向 | 對應題目 |
|---|---|
| 共同工作區與正式版本 | Q1 |
| 任務分工與進度 | Q2、Q7 |
| 安全修改與版本紀錄 | Q3、Q4、Q7 |
| 提出修改與團隊審查 | Q5、Q6、Q7 |
| 完整協作流程 | Q7 |

每個面向顯示：

- 已掌握
- 經提示後已釐清
- 建議再回顧

### 報告結尾文案

> 你不需要背完所有 GitHub 英文名詞，重點是知道每一步在團隊中解決什麼問題。之後真的做小組報告時，只要記得：先說清楚工作、在安全版本修改、留下紀錄、請隊友檢查，再把確認過的內容放進正式版。

### 回顧連結

若某面向為「建議再回顧」，提供精準返回位置：

- Q1 → Stage 1 的版本混亂情境
- Q2 → Stage 4 的 Issue／Project 操作
- Q3、Q4 → Stage 4 的 Branch／Commit 操作
- Q5、Q6 → Stage 4 的 Pull Request／Review 操作
- Q7 → 回到本題重新排序

---

# 7. 驗收判準

新版題庫完成後，應同時符合：

1. 學生不需要接觸 Git CLI，也能完成所有題目。
2. 題目中沒有 SHA 記憶、Push 辨識、Project automation 設定考試。
3. 每個錯誤選項都對應一種初學者可能真的產生的錯誤心智模型。
4. 沒有只為湊選項而設計的荒謬答案。
5. 每題都能回扣「如何讓小組分工、丟檔案與整合成果更清楚」。
6. Q7 能獨立檢查完整工作流，不依賴前面按鈕操作紀錄。
7. 完成 Stage 5 後，學生能用自己的選擇證明理解，不靠自我勾選或字數門檻。
