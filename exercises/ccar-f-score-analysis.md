# CCAR-F スコア分析 & CCDV-F 活用

## 試験結果
- 日付: 2026-07-25（土）10:00〜 / テストセンター（徒歩5分）
- 所要: 90分（75分一周+15分レビュー・フラグ25題・最後まで勘は3題）
- スコア: **904/1000**（合格ライン720・**+184pt**）
- 結果: **Pass**

## 問題形式（※重要訂正）
- **全問4択1問選択・複数選択なし**
- CCAO-F（複数選択あり5択）とは別形式
- → **SkillCertPro / Tutorials Dojo の「複数選択なし」形式が本番と合致**（前回の Claude 評価「Purcell の複数選択が本番形式」は CCAR-F では誤り）
- Purcell の複数選択は過剰（本番出題なし）。ただし**内容の深さ**は本番代表

## シナリオ（4つ）
1. Customer Support Resolution Agent
2. Multi-Agent Research System（うろ覚え）
3. Structured Data Extraction
4. **Code Generation with Claude Code**（一番難しい・完全所見多数・**勘3題は全部ここ**）

## ドメイン別スコア

### ❌ 0%（完全脱落）
1. **orchestration safeguards**: agent session がどう終わっても「完了 or human escalation」にする safeguard
2. **MCP server scope**: project-level（共有）vs user-level（個人/実験）の使い分け

### △ 50%
3. refinement workflow（input-output例・targeted feedback・batched issues）
4. tool selection reliability（ツール説明拡充・disambiguation）

### 75%
5. MCP integration（スコープ・認証env var・検証）

### ✅ 100%（強み・省略）
handoff packages / session resumption / task decomposition / PreToolUse+PostToolUse hooks / stop_reason / slash commands / plan mode / CLAUDE.md vs settings / codebase exploration(Grep/Glob/Read) / context management / API stateless / escalation criteria / review routing / API processing mode(sync vs Batches) / structured output(tool use+JSON schema) / extraction schemas(optional/nullable/enum) / tool_choice / extraction accuracy / feedback loop metadata / MCP error handling / tool_choice sequencing

## 教材の有効性（質問2 回答）
- **🏆 SkillCertPro が本番に一番近い**（CCAR-F 本番後の実感・形式＋内容とも）
- Purcell: 96.7%（事前）→ 本番904反映。内容の深さは本番代表。複数選択は過剰
- Tutorials Dojo: 形式合致（複数選択なし）
- Prep / Provorov: 体系学習の基盤

## CCDV-F 教材の詳細（subagent 解析）
- **Maarek Exam2/3/4 は Exam1 より質が上**: "Reviewers would evaluate" テンプレ句が **Exam2/3/4 ではゼロ**（Exam1 は88回）。コード問題も 13→20-22 に増加。実装寄り。**前回の Maarek AI 痕跡評価は Exam1 のみ**
- **Maarek Exam4 が実装詳細で最厚**: retry 24回・turns 26回・tool_choice 6回。adaptive thinking×temp×tool_choice の Q7 は本番核心公算大
- **Sundog Exam3 は model modes に厚い**: Q34(fast mode)/Q36(adaptive+effort)/Q39(Haiku)/Q40(disabled)/Q41(temp0 非サポート)。ただし streaming 実装詳細は薄い
- **3教材とも複数選択あり**: Sundog(4択2選)・Maarek(5択2選)・Purcell(5択2選) → CCDV-F 複数選択ありを支持

## CCAO-F（薄対策）vs CCAR-F（分厚い対策）の対比
| | CCAO-F | CCAR-F |
|---|---|---|
| 対策 | Udemy 1コースのみ（2セット9割）| Prep/Provorov/SkillCertPro/TD/Purcell/quiz |
| 本番時間 | **1時間48分**（ギリギリ）| **90分**（余裕）|
| スコア | 752（ギリギリ）| **904**（+184pt）|

**教訓: 対策の分厚さが本番の時間短縮＋スコア余裕に直結。** CCDV-F は model modes/SDK gap を埋めないと「CCAO-F 型のギリギリ」リスク。

## Code Gen シナリオ難所（質問1 回答より）
- **hooks（Pre/PostToolUse）vs settings.json deny**: どちらで業務ルールを強制するか
- **CLAUDE.md vs .claude/rules/**: ガイダンスの配置先
- plan mode vs 直接実行の判断
- → CCAR-F 対応セクションは100%取れたが、**所見では紛らわしく迷った**
- 教材で練習済みだから解けた。完全所見だと苦戦（勘3題の温床）

## CCDV-F への示唆（質問で更新）

### 形式（質問3）
- **CCDV-F は複数選択を想定して対策**。CCAR-F は「旧 CCA-F 時代の名残で4択（4試験中で特異）」の可能性。CCAO-F（複数選択あり）・CCAR-P（select two of five・Purcell）の傾向から CCDV-F も複数選択の公算大
- → **Purcell CCDV-F（複数選択・合格者・970点）を本番直前シミュレーションとして使用**。4択練習だけだと複数選択で「部分点なし（半正解=誤答）」失点リスク

### 内容
- CCAR-F の「設定メカニズム使い分け」（hooks/settings/CLAUDE.md/rules）は CCDV-F D3 Claude Code（3.1%）で浅く出る。基本確認
- **CCDV-F の核心は model modes/SDK/API**（Purcell 実体験）→ こちら優先
- （質問ラウンド継続で追記）
