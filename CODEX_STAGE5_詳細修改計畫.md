# Codex 修改計畫：重做 Practice-Field Stage 5「綜合回顧」

> Repository：`novoicefire/Practice-Field`  
> 主要實作檔：`index.html`  
> 內容規格來源：`STAGE5_綜合回顧題目設計.md`  
> 核心限制：維持單一 HTML、零依賴、Web-only、繁體中文／英文雙語、不可破壞 Stage 1–4。

---

## 1. 修改目標

將目前 Stage 5 從偏技術細節、重複補強與機械操作的測驗，改成固定七個題組的「完整協作理解檢查」。

新版應確認學生是否理解：

> Issue 分工 → Branch 安全修改 → Commit 留版本 → Pull Request 請隊友檢查 → Review 與修正 → Merge 進入 main → 確認工作進度

### 必須解決的舊問題

1. 移除 Upload files 與 Push 的核心辨識題。
2. 移除 SHA 記憶與「核准必須對應某個 SHA」的硬編碼考法。
3. 移除要求學生點指定 Diff、指定綠色行、指定按鈕順序才能過關的機械流程。
4. 移除 `version-repair`、`pr-repair`、`tracking-repair` 等與原題高度重複的補強頁。
5. 移除 `Assignee === Alex` 這類人名記憶題。
6. 移除兩題「輸入至少 20 字＋自我勾選檢核」的假語意評量。
7. 降低 Project workflow／automation 細節比重；只要求學生知道合併後應確認實際進度。
8. 讓最後一題真正整合整套協作流程。

---

## 2. 不可違反的實作限制

- 不拆分 `index.html`。
- 不加入 React、Vue、npm、建置流程或第三方套件。
- 不依賴網路 API。
- 不改變 Stage 1–4 的教學內容，除非是為了修正 Stage 5 入口、名詞說明或狀態相容性。
- 保留現有整體視覺語言、Header、Course rail、Action bar、Glossary drawer 與 ALEX 測試模式。
- 所有新增文案都必須透過現有 `tx(zh, en)`、`plain(zh, en)` 或等效雙語機制輸出。
- 所有可互動控制至少 44 × 44 CSS px，並支援鍵盤與清楚焦點。
- 不要建立 Issue、Branch、Commit 或 PR，除非執行環境中的使用者另行明確要求。

---

## 3. 修改前盤點

在開始編輯前，先搜尋並記錄以下 Stage 5 相關程式：

### 路線與狀態

- `baseReview`
- `reviewWeakness`
- `buildReviewRoute`
- `ensureReviewRoute`
- `reviewPage`
- `reviewPageDomain`
- `reviewPageLabel`
- `reviewDomainStatus`
- `reviewNext`
- `reviewReset`

### 舊頁面

- `reviewFootprintView`
- `reviewVersionView`
- `reviewVersionRepairView`
- `reviewPrView`
- `reviewPrRepairView`
- `reviewTrackingView`
- `reviewTrackingRepairView`
- `reviewExplanationView`
- `reviewReportView`
- `renderReviewPage`

### 舊驗證與互動函式

- `reviewValidateVersion`
- `reviewValidateVersionRepair`
- `reviewValidatePr`
- `reviewValidatePrRepair`
- `reviewValidateTracking`
- `reviewValidateTrackingRepair`
- `reviewValidateExplanation`
- `reviewOpenPrDiff`
- `reviewSelectPrLine`
- `reviewConfirmPrFix`
- `reviewApprovePr`
- `reviewMergePr`
- 任何只服務舊 Stage 5 的 array toggle、文字檢查或 repair handler

### ALEX 測試模式

- `alexSeedReviewAnswers`
- `alexCompleteReview`
- `alexReviewAnswerKey`
- `alexReviewTools`

先用引用搜尋確認上述函式是否被 Stage 1–4 共用。只刪除 Stage 5 專用程式，不可因名稱相似誤刪共用 UI helper。

---

## 4. 新版架構決策

### 4.1 固定路線

使用固定路線，不再動態插入補強頁：

```js
const stage5Route = [
  'intro',
  'workspace',
  'task',
  'branch',
  'commit',
  'pull-request',
  'review',
  'workflow',
  'report'
];
```

畫面顯示的題號為 1–7；`intro` 與 `report` 不計入題數。

### 4.2 建議狀態模型

新增明確 schema version，避免舊 LocalStorage 資料誤套到新題庫。

```js
const STAGE5_SCHEMA_VERSION = 2;

function baseReviewV2() {
  return {
    schemaVersion: STAGE5_SCHEMA_VERSION,
    route: [...stage5Route],
    cursor: 0,
    maxVisitedCursor: 0,
    status: 'not-started',
    completed: false,
    answers: {
      workspace: '',
      task: '',
      branch: '',
      commit: '',
      pullRequest: '',
      reviewStep1: '',
      reviewStep2: '',
      workflow: []
    },
    attempts: {},
    results: {},
    feedback: null,
    report: null
  };
}
```

`results[questionId]` 僅可為：

- `mastered`
- `clarified`
- `needs-review`

### 4.3 舊狀態遷移

在 `ensureReviewRoute()` 或新的 `ensureStage5State()` 中處理：

1. 若沒有 `state.review`，建立 `baseReviewV2()`。
2. 若 `state.review.schemaVersion !== STAGE5_SCHEMA_VERSION`：
   - 只重設 Stage 5 狀態。
   - 保留 Stage 1–4 的學習紀錄、解鎖狀態與語言設定。
   - 若使用者以前已解鎖 Stage 5，仍允許進入新版 Stage 5。
   - 舊版 `completed` 不可直接視為新版已完成。
3. 儲存後重新渲染，不得進入不存在的舊 route page。

建議在第一次進入新版 Stage 5 時顯示一次非阻擋提示：

> Stage 5 已更新為新的綜合回顧，你前四階段的進度仍然保留。

---

## 5. 題庫資料化

不要再為每一頁拼接一大段難以維護的 HTML 字串。新增資料驅動題庫，例如：

```js
const stage5Questions = {
  workspace: {
    number: 1,
    domain: 'workspace',
    title: ['正式版到底在哪裡？', 'Where is the official version?'],
    type: 'single',
    correct: 'shared-repo-main',
    options: [...],
    feedback: {...}
  },
  // ...
};
```

每個題目物件至少包含：

- `number`
- `domain`
- `title`
- `scenario`
- `question`
- `type`
- `options`
- `correct` 或 `correctOrder`
- `successFeedback`
- `wrongFeedbackByOption`
- `hint`
- `termKeys`

內容必須逐字依照 `STAGE5_綜合回顧題目設計.md`，不要自行增加 SHA、Push、CLI、automation 等考點。

---

## 6. 共用渲染元件

優先保留現有可用的卡片、按鈕與 review surface 樣式；新增下列共用函式，避免七題各自寫一套：

```js
renderStage5Intro()
renderStage5Question(questionId)
renderStage5SingleChoice(question)
renderStage5ReviewScenario(question)
renderStage5OrderingQuestion(question)
renderStage5Feedback(questionId)
renderStage5Report()
renderStage5Progress()
```

### 單選題語意結構

使用：

```html
<fieldset>
  <legend>題目文字</legend>
  <!-- options -->
</fieldset>
```

選項可用 button card 或 radio，但必須：

- 可用 Tab 移動
- Enter／Space 可選擇
- 有 `aria-checked` 或原生 radio 狀態
- 不依賴顏色表示正誤

### 回饋區

回饋使用 `role="status"` 或 `aria-live="polite"`，並在送出後將焦點移至回饋標題，讓鍵盤與螢幕閱讀器使用者知道發生什麼。

---

## 7. 作答與回饋邏輯

### 7.1 單選題

建議流程：

1. 學生選擇答案。
2. 按「檢查答案」。不要在點選選項的瞬間直接跳頁。
3. 若正確：記錄結果並開放「下一題」。
4. 若錯誤：留在原題，顯示選項專屬的實際後果與提示。

### 7.2 結果判定

```js
function stage5ResultFromAttempts(attempts) {
  if (attempts === 1) return 'mastered';
  if (attempts === 2) return 'clarified';
  return 'needs-review';
}
```

實際規則：

- 第一次正確：`mastered`
- 第一次錯、第二次正確：`clarified`
- 已錯兩次以上才完成：`needs-review`

第二次錯誤後顯示完整解釋，但仍要求學生自己選出正確答案，不可自動代答。

### 7.3 Q6 兩步 Review 題

Q6 必須是同一題組中的兩個狀態：

```js
reviewStep: 1 | 2
```

- Step 1 正確後才顯示「修改者已修正」的情境更新。
- Step 2 不顯示 SHA，只顯示「最新修改」與實際內容差異。
- 完成兩步後才計算 Q6 結果；結果採兩步中較弱的狀態。

### 7.4 Q7 排序題

實作方式可採：

- 上移／下移按鈕
- 鍵盤可操作的選單排序
- HTML Drag and Drop 加上完整鍵盤替代方案

不要只做滑鼠拖曳。

建議每張卡片提供：

- 「上移」
- 「下移」
- 目前位置文字，例如「第 3 項，共 7 項」

驗證時找出第一個錯誤相鄰關係，依題目規格顯示具體回饋。不可只顯示「排序錯誤」。

---

## 8. 移除或改寫舊內容

### 8.1 必須移除的核心題目

- `Upload files 和 Push 是同一件事嗎？`
- Commit message、作者與 SHA 的多選證據題
- 舊 Diff／最新 Diff 的 SHA 切換題
- 點擊指定 `11/23` 綠色行
- `Approve 最新 SHA`
- `Assignee = Alex`
- `Merge 後 Issue 和 Project 一定更新嗎？` 的 automation 細節考試
- 關閉關鍵字＋Project workflow 選兩項的題目
- 兩個至少 20 字的自我說明題

### 8.2 可保留但改成非評量說明

- Push：若仍保留在 Glossary，只能標示為延伸名詞，說明本教材不操作、不列入 Stage 5 通關。
- Project automation：可在教師或進階補充中說明，但 Stage 5 只要求學生「確認狀態是否符合實際進度，需要時手動更新」。
- SHA：可在 GitHub 模擬畫面中自然出現，但不可要求學生辨識或背誦。

### 8.3 名詞卡更新

更新 `terms.Push.whereZh/whereEn`，不可再寫「第五階段會用回顧題比較」。建議：

```js
whereZh: '本教材採 GitHub 網頁操作，不安排 Push 指令練習。',
whereEn: 'This course uses the GitHub web interface and does not include push command practice.'
```

Stage 5 的 `reviewTermKeys()` 不應主動加入 `Push` 或 `ProjectAutomation`。

---

## 9. 最終報告

建立五個能力面向：

```js
const stage5Domains = {
  workspace: ['共同工作區與正式版本', 'Shared workspace and official version'],
  taskTracking: ['任務分工與進度', 'Task ownership and progress'],
  safeChanges: ['安全修改與版本紀錄', 'Safe changes and version records'],
  reviewMerge: ['提出修改與團隊審查', 'Proposing and reviewing changes'],
  fullWorkflow: ['完整協作流程', 'Complete collaboration workflow']
};
```

題目對應：

- `workspace`：Q1
- `taskTracking`：Q2、Q7
- `safeChanges`：Q3、Q4、Q7
- `reviewMerge`：Q5、Q6、Q7
- `fullWorkflow`：Q7

面向狀態採該面向題目中的最低結果。例如一題 `needs-review`，面向即為 `needs-review`。

### 報告禁止內容

- 不顯示 0–100 分
- 不顯示排名
- 不用「失敗」羞辱學生
- 不宣稱開放文字已由系統理解

### 精準回顧按鈕

- 共同工作區 → Stage 1
- 任務分工 → Stage 4 對應 Issue／Project step
- 安全修改 → Stage 4 對應 Branch／Commit step
- 審查合併 → Stage 4 對應 PR／Review step
- 完整流程 → 回到 Q7

若 Stage 4 目前只能按整個階段進入，可先回到 Stage 4，再將 `state.practice.step` 設為對應步驟；必須確認這不會破壞已完成狀態。

---

## 10. Stage 5 入口、進度與導覽

### Intro

將目前「自適應回顧」改為：

- 中文：`綜合回顧`
- 英文：`Integrated review`

建議 Intro 文案：

> 這次不考你背英文名詞，而是確認你能不能把 GitHub 用在一次真實的小組協作中。七個題組會從分工、修改、留下版本，一路走到審查與正式合併。

### Progress

- 題目頁顯示 `第 1 題，共 7 題`
- Q6 內顯示 `步驟 1／2`，但整體仍算第 6 題
- Intro 不顯示 `0/7`，可顯示「約 5–8 分鐘」

### Header action

更新 `headerCourseActionState()` 的 Stage 5 行為：

- Intro：開始綜合回顧
- 題目未檢查：檢查答案
- 題目正確：下一題
- Report：重看回顧報告

避免頁面內按鈕和 Header 同時觸發不同邏輯。

---

## 11. ALEX 測試模式更新

重寫 Stage 5 的 ALEX controls，至少提供：

1. `填入目前題目正確答案`
2. `填入所有新版答案`
3. `跳到指定題目`
4. `直接產生三種報告狀態`
   - 全部 mastered
   - 混合 mastered／clarified
   - 包含 needs-review
5. `完成新版 Stage 5`

移除舊 answer key 中：

- SHA
- `version-repair`
- `pr-repair`
- `tracking-repair`
- 開放題假答案

ALEX 工具只能在既有測試模式條件成立時顯示，不可出現在一般學生畫面。

---

## 12. README 同步修改

目前 README 的 Web-only 路線仍寫著 Stage 5 會比較 Push 與 Upload files。新版移除該核心題後，必須同步更新。

### 建議替換文案

舊概念：

> 本教材只在第五階段用回顧題說明 Upload files 與 Push 差異。

改為：

> 本教材專注 GitHub 網頁操作，不安排終端機或 Push 指令練習；學生先學會用 Repository、Branch、Commit、Pull Request、Review、Merge、Issue 與 Project 完成一次小組協作。

若 README 中仍使用「7 大關卡」描述舊版架構，檢查是否與目前五階段 SPA 一致；只修正明確衝突，不要在本任務中全面重寫 README。

---

## 13. 視覺與響應式要求

### Desktop

- 題目主內容寬度維持可讀，選項不要拉到整個 1600px 寬。
- 情境、題目與回饋的視覺層級明確。
- Q7 排序卡在桌面可單欄或雙欄，但順序必須容易判讀。

### Mobile（360px 起）

- 不產生水平捲動。
- 選項完整顯示，不截斷關鍵文字。
- 上移／下移按鈕不小於 44px。
- Sticky action bar 不遮住最後一個選項或回饋。

### 狀態表達

正確、錯誤與提示不可只靠綠／紅色：

- 加上文字標題
- 加上圖示或狀態 badge
- 使用 `aria-live`

---

## 14. 無障礙要求

1. 每一題有唯一且具體的 `legend` 或 heading。
2. 選項的 accessible name 必須包含完整內容，不可使用通用 `aria-label="control"`。
3. 回饋出現後移動焦點，或至少讓螢幕閱讀器即時讀取。
4. Q7 排序必須有非拖曳替代操作。
5. Modal／Glossary 的 Escape、Tab focus trap 不可退化。
6. 語言切換後，當前題目、選項、回饋與進度同步切換，不重設答案。
7. 測試 200% browser zoom，內容仍可操作。

---

## 15. 測試清單

### 15.1 基本流程

- [ ] 從 Stage 4 完成後能正常解鎖 Stage 5。
- [ ] Intro → Q1–Q7 → Report 路線正確。
- [ ] 不會進入任何舊 `*-repair` 頁。
- [ ] 每題答對後才能前進。
- [ ] 重整瀏覽器後保留目前題目、答案、attempt 與回饋狀態。
- [ ] `reviewReset()` 只重設 Stage 5，不清除 Stage 1–4。

### 15.2 錯誤回饋

- [ ] 每個錯誤選項顯示對應後果，不是通用「答錯了」。
- [ ] 第一次錯後答對標記 `clarified`。
- [ ] 兩次以上錯後答對標記 `needs-review`。
- [ ] 第二次錯誤後不會自動幫學生選正確答案。

### 15.3 Q6

- [ ] Step 1 未完成前看不到 Step 2。
- [ ] Step 2 不出現 SHA 考題。
- [ ] 兩步結果能正確合併為 Q6 狀態。

### 15.4 Q7

- [ ] 滑鼠可以排序。
- [ ] 鍵盤可以排序。
- [ ] 排序錯誤時指出第一個不合理關係。
- [ ] 正確順序為 Issue → Branch → Commit → PR → Review／修正 → Approve／Merge → 確認進度。

### 15.5 報告

- [ ] 五個能力面向計算正確。
- [ ] 沒有百分比分數與排名。
- [ ] 回顧按鈕能抵達正確 Stage 或 Q7。
- [ ] 完成後 Course rail 的 Stage 5 狀態正確。

### 15.6 相容性

- [ ] 舊版 LocalStorage 進入新版時不報錯。
- [ ] 舊版 Stage 5 route 名稱不會造成空白頁。
- [ ] Stage 1–4 所有互動仍可完成。
- [ ] 中英文切換正常。
- [ ] Chrome、Edge 最新穩定版無 console error。
- [ ] 360×800、768×1024、1440×900 皆可使用。

### 15.7 ALEX

- [ ] ALEX 可以跳至所有新題目。
- [ ] 可一鍵填入正解。
- [ ] 可產生三種不同報告狀態。
- [ ] 一般模式完全看不到 ALEX controls。

---

## 16. 完成定義

只有同時符合以下條件才算完成：

1. `index.html` 中不再存在可到達的舊 Stage 5 題目與 repair route。
2. 七題內容與 `STAGE5_綜合回顧題目設計.md` 一致。
3. Q7 能完整呈現並驗證整套協作流程。
4. 所有題目都有具體錯誤後果與提示。
5. 新版狀態可持久化並能安全處理舊狀態。
6. Stage 1–4 無回歸。
7. README 不再宣稱 Stage 5 以 Push 比較作為核心回顧。
8. 完成手動測試清單，並在最終回報中列出：
   - 修改檔案
   - 移除的舊功能
   - 新增的題目與狀態模型
   - 測試結果
   - 尚未驗證的風險

---

## 17. Codex 最終回報格式

完成修改後，請用以下格式回報，不要只說「已完成」：

```md
# Stage 5 修改結果

## 修改檔案
- index.html
- README.md

## 已移除
- ...

## 已新增
- ...

## 狀態遷移
- ...

## 測試結果
- [x] 七題完整流程
- [x] 錯誤回饋
- [x] 重新整理保存
- [x] 中英文
- [x] 鍵盤操作
- [x] 360px 手機版
- [x] Stage 1–4 回歸

## 未驗證或剩餘風險
- ...
```
