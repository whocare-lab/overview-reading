# overview-reading｜管理學質性研究多篇文獻綜述整合 Skill

**繁體中文** ｜ [English](README.en.md)

給管理學院做質性研究、個案研究的同學使用的 Claude skill（skill 名稱：`lit-synthesis`）。
把多篇已精讀的文獻整合成**有論證脈絡**的文獻探討，並寫出**具體的**研究缺口。

## 搭配使用：先精讀，再整合

```
英文文獻 PDF ──► close-reading（一次一篇）──► 精讀卡＋文獻矩陣 ──► lit-synthesis（多篇）──► 概念矩陣、論證大綱、研究缺口、文獻探討初稿
```

第一步的單篇精讀 skill 在另一個 repo：[whocare-lab/close-reading](https://github.com/whocare-lab/close-reading)。
lit-synthesis 的最佳輸入就是 close-reading 產出的精讀卡與 `matrix.csv`；只有 PDF 時，skill 會先請 Claude 逐篇精讀。

## 產出

| 產出 | 內容 |
|---|---|
| **A. 概念矩陣** | 文獻清單、覆蓋度檢查（年代、方法、情境、分析層次、理論、證據強度）、文獻 × 概念矩陣（每格附頁碼）、讀矩陣看到的模式 |
| **B. 論證大綱** | 漏斗式章節結構，每節一句主張、每段主題句與支持文獻、節與節的銜接；附 Mermaid 文獻地圖與建議補讀清單 |
| **C. 研究缺口陳述** | 每個缺口標出類型（混淆、忽略、應用、問題化），附矩陣證據，並說明缺什麼、為什麼重要、你的研究怎麼補 |
| **D. 文獻探討初稿**（要求才做） | 以概念為主語的學術段落、APA 7th 引用、參考文獻清單 |

觸發方式：說「整合這些文獻」「幫我寫文獻探討」「整理研究缺口」「建立概念矩陣」。

## 它和「請 AI 寫文獻探討」有什麼不同

- **以概念組織，不以作者組織**：不會寫成「A 認為……。B 認為……」的列表。
- **每個主張至少兩篇來源**，只有一篇時標「單一來源」；分歧一定要解釋原因。
- **只用你提供的文獻**：你沒讀過的文獻只會出現在「建議補讀」，不會混進正文。
- **缺口不寫空話**：「目前相關研究較少」「本研究填補此一缺口」這類句子，沒有具體證據就會被改寫。
- **文獻量不足會警告**：少於 5 篇標「初步整合」，少於 3 篇不下缺口定論。
- **引用檢查程式** `check_citations.py`：檢查文內引用與參考文獻是否一一對應、引用的文獻是否都有精讀卡（抓出憑記憶加入的文獻）、APA 7 的 et al. 寫法是否一致。中英文引用都支援。
- **引文核對** `verify_quotes.py`：可同時比對多篇 PDF，確認初稿中的直接引文逐字存在、出自哪一篇，並比對文中標註的頁碼是否正確。

## 輸出語言
預設繁體中文。用英文提問，或說「English」「英文版」，就改用英文撰寫。

## 檔案結構

```
lit-synthesis/                      ← skill 本體（上傳或複製這個資料夾）
├── SKILL.md
├── references/
│   ├── my-research.md              ← 請填你自己的研究
│   └── synthesis-methods.md        ← 整合方法、缺口類型、品質自檢表、方法參考文獻
├── scripts/
│   ├── check_citations.py          ← 引用一致性檢查
│   └── verify_quotes.py            ← 多篇 PDF 引文逐字比對
└── templates/
    ├── concept-matrix.md
    ├── review-outline.md
    └── gap-statement.md
dist/lit-synthesis.zip              ← 已打包好，可直接上傳 claude.ai
examples/input/                     ← 測試輸入：兩張精讀卡與文獻矩陣
examples/output/                    ← 測試產出：概念矩陣、論證大綱、研究缺口、初稿
```

## 安裝

### 1. 先填寫你的研究
打開 `lit-synthesis/references/my-research.md`，填入題目、研究問題、資料來源、理論視角與真實章節。若已在 close-reading 填過，複製同一份即可。
lit-synthesis 需要知道你的研究問題才能判斷缺口；沒填時它會先問你一個問題。

### 2a. claude.ai（網頁、桌面版、手機 App）
1. 下載 `dist/lit-synthesis.zip`。若修改了 `my-research.md`，請把 `lit-synthesis` 資料夾重新壓縮成 zip。
2. 到 claude.ai 設定中的 Skills 區上傳 zip。
3. 新對話中上傳多張精讀卡（或 `matrix.csv`），說「整合這些文獻，研究問題是……」。

### 2b. Claude Code
```bash
git clone https://github.com/whocare-lab/overview-reading.git
# macOS / Linux
cp -R overview-reading/lit-synthesis ~/.claude/skills/
# Windows（PowerShell）
Copy-Item -Recurse overview-reading\lit-synthesis $env:USERPROFILE\.claude\skills\
# 引文核對需要的套件
pip install pypdfium2
```
產出存到目前專案的 `literature/synthesis/`。若同時安裝 close-reading，lit-synthesis 會讀取它的 `my-research.md` 與 `literature/` 底下的精讀卡。

## 範例與測試

以 close-reading 產出的兩張精讀卡（`examples/input/`）整合，示範研究問題為「協調者如何透過治理設計，促成生態系夥伴的對齊？」，產出放在 `examples/output/`：

- 正確觸發文獻量警示（2 篇，少於 3 篇），缺口只寫成「初步觀察」。
- 覆蓋度檢查指出：實證研究 0 篇、沒有非營利或教育情境、兩篇分析單位不同。
- 找出一個分歧（治理是協調者「設計」，還是靠夥伴「協商」同意）並解釋原因；找出一個共同前提（都假設有單一焦點行動者）。
- 初稿的 8 次文內引用與參考文獻清單完全對應；2 則直接引文分別在兩篇 PDF 中逐字找到。
- 引用檢查程式另以測試檔驗證：能抓出漏列的參考文獻、未被引用的參考文獻、沒有精讀卡的文獻、三位以上作者未寫 et al.。

**兩篇只是示範**，真正的文獻探討需要更多文獻；skill 會在文獻量不足時提醒你。

另以 6 篇生態系文獻（1 篇 SMJ、1 篇 HBR、3 篇 BCG、1 篇 McKinsey）完整測試一次：先逐篇精讀，再整合成概念矩陣、論證大綱、4 個研究缺口與文獻探討初稿。
- 正確標出證據強度問題：6 篇中只有 1 篇同儕審查、實證研究 0 篇。
- 發現同一系列 BCG 文章引用的樣本說法不一致（57 個、逾 100 個、80 多個生態系）。
- 初稿 28 次文內引用全部通過引用檢查；7 張精讀卡與初稿共 77 則引文，全部逐字找到且頁碼正確。
- 測試中發現一處頁碼寫錯（PDF 第 1 頁是封面），因此為 `verify_quotes.py` 加上頁碼比對；該錯誤現在會被標為 PAGE MISMATCH。
（此測試的產出涉及個人研究內容，未放入 repo。）

自行核對：
```bash
python lit-synthesis/scripts/check_citations.py 文獻探討初稿.md literature/matrix.csv
python lit-synthesis/scripts/verify_quotes.py --offset 文獻1.pdf=0 --offset 文獻2.pdf=38 文獻1.pdf 文獻2.pdf 文獻探討初稿.md
```

## 使用時請注意
- 概念矩陣、大綱與初稿是輔助，**不是可以直接繳交的成品**。初稿請用自己的語言改寫，並逐一核對引用。
- 「尚未有研究探討」是很強的宣稱。skill 只能根據你提供的文獻判斷，定稿前請自行補檢索。
- 不要把未公開的訪談逐字稿或受訪者資料寫進 `my-research.md` 後再公開分享。

## 授權
方法依據見 `lit-synthesis/references/synthesis-methods.md`。MIT License，歡迎同學 fork 後依自己的研究調整。
