# CCDV-F 準備タスク（CCAR-F 教訓反映）

## 試験
- **日曜 2026-07-26 10:00** / 53問・120分 / テストセンター（徒歩5分）

## CCAR-F からの教訓（活かす）
- [ ] 複数選択なし（4択1問選択）想定で対策（CCAR-F が4択だったため・CCDV-F も要確認）
- [ ] 消去法＋フラグ運用が有効（CCAR-F で機能・理由付けして選択肢を消す）
- [ ] 時間配分: 75分一周+15分レビュー（CCAR-F 実績）を CCDV-F（53問）に調整
- [ ] 完全所見問題への対応: Code Generation シナリオで所見に苦戦→CCDV-F の Claude Code/SDK 領域を強化

## CCDV-F 重点（Purcell 実体験・970点より）
- [ ] **model modes**（fast/effort/adaptive thinking）暗記
- [ ] **SDK 挙動**（retry/max_retries/streaming/partial_json）
- [ ] **API surface**（real-time/Batches/streaming の使い分け）

## CCAR-F 弱点の CCDV-F 共通項（要再確認）
- [ ] **MCP server scope**（project vs user）= CCDV-F D8 Tools/MCP で関連
- [ ] **tool selection / ツール説明拡充** = CCDV-F D8 で関連
- [ ] **agent session safeguard / stop_reason** = CCDV-F D1 Agents で関連
- [ ] refinement workflow / feedback loop = CCDV-F D4 Eval で関連

## 質問で収集中（※埋まる）
1. Code Generation シナリオの中身（どの機能を問われたか）
2. 教材の本番との近さ（Purcell/SkillCertPro/TD/Prep/Provorov）
3. 複数選択なし→CCDV-F での扱い
4. 時間配分の感想

## 時間配分プラン（質問4 回答）
- **100分一周+20分レビュー**（CCAO-F 型・ギリギリ警戒。複数選択と model modes/SDK gap で時間かかる想定）

## 戦略（厳しさの現実認識）
- **厳しい戦い**: SDK・API 直接経験なし・自力コード薄い・対策は CCAO-F 程ではないが薄い
- しかし **CCAR-F 904点の基盤**（tool use/MCP/Claude Code/API 共通）がある。これは大きい
- gap は「SDK/API 実装詳細」。**概念は CCAR-F で証明済み**（tool_choice/structured output/API processing mode 100%）
- problem-driven で実装知識を詰め、概念と結びつければ点になる
- 合格ライン720。CCAR-F 共通分野で点を取り、SDK gap で落としても 720 は射圏内
- **最悪ダメでも再受験の学び**。CCDV-F は1回目の挑戦・腹くくる

## 教材マップ（subagent 解析結果）
| セット | 強み | 優先 |
|--------|------|------|
| **Maarek Exam4** | 実装詳細最厚・**adaptive thinking×temperature×tool_choice Q7（本番核心）**・prompt caching Q53・Batches Q3 | **★最優先** |
| **Sundog Exam3** | model modes 直接（Q34 fast/ Q36 adaptive+effort/ Q39 Haiku/ Q40 disabled/ Q41 temp0）・API surface（stateless/caching） | **★必須** |
| **Maarek Exam3** | effort 厚い・prompt caching Q13（prefix破壊） | 時間あれば |
| Maarek Exam2 | 入門寄り・async アンチパターン | スキップ可 |

## スケジュール（解析結果反映・土曜丸1日）
- **土曜午後（13-18時）**:
  - **Maarek Exam4 を解く**（120分）→ 解説復習：**Q7（adaptive×temp×tool_choice）・Q53（cache破壊）・Q3（Batches）**
- **土曜夜前半（19-22時）**:
  - **Sundog Exam3 を解く**（120分）→ **model modes 5問**（Q34/36/39/40/41）・API surface（Q5/38/29）
- **土曜夜後半（22-深夜）**:
  - Maarek Exam3（prompt caching Q13・effort）※時間あれば
  - model modes 暗記の確認（fast/effort/adaptive・Haiku/Sonnet/Opus）
- **日曜朝**:
  - **Purcell CCDV-F**（複数選択・合格者・970点・本番シミュ）＋ model modes 最終確認
- **10時受験**（100分一周+20分レビュー）

## 互补関係（gap を分散補強）
- model modes gap → **Sundog Exam3**（直接問題）＋ Maarek Exam4 Q7（統合）
- SDK 実装 gap → **Maarek Exam4**（retry/turns/tool_choice・async アンチパターン）
- API surface gap → **Sundog Exam3**（stateless/caching）＋ Maarek Exam4 Q53
- streaming 実装 → Maarek コード問題（streaming≠WebSocket・partial_json 系）

## Sundog Exam1/2 弱点（復習マークダウンより・計19問）
出力:
- `tasks/sundog-exam1-analysis/exam1-wrong-ja.md`（10問）
- `tasks/sundog-exam2-analysis/exam2-wrong-ja.md`（9問）

統合弱点（CCDV-F gap と完全一致）:
- **D3 Agents & Workflows（5問・最大弱点）**: Agent SDK の位置づけ・workflow vs agent・サブエージェント・`ClaudeSDKClient` vs `query()`・Workflow tool・orchestrator-workers
- **D5 Tools/MCP（3問）**: client tool / MCP connector / Agent Skill / server tool の区別
- **D2 model modes/caching（2問）**: Sonnet 5 `budget_tokens` 拒否（adaptive+effort）・prompt caching の checkpoint 配置（安定 prefix 末尾）

★ **一貫誤答パターン**: 「要件の一部だけ見て、統合・比較・履歴保持を取りこぼす」（CCAR-F の「直接・単純を選びがち」と同じ）→ **設問の全要件を洗ってから選ぶ癖**

**Prep 範囲外（5問）**: context editing・`claude -p` フラグ・Claude 4.6+ dateless ID・Workflow tool・git worktree → 公式 doc 直接補完

## 難易度調査（外部 firsthand レポート・2026-07）
- **CCDV-F は ★★☆（中位）**: CCAO-F ★☆☆ < **CCDV-F ★★☆** ≒ CCAR-F ★★☆ < CCAR-P ★★★（7/22 LinkedIn）
- Purcell 評価（技術2位）と一致・CCAO-F より難しく CCAR-P より易しい
- **内容（複数ソース一致）**: Messages API vs Batches・**agent loop vs deterministic workflow**・MCP・Claude Code・**current-API gotchas**（最新 API に注意・古い blog 情報は不可）
- **英語速度注意**: 翻訳ツール禁止・シナリオベースで読む量多い → 実戦ペース（100分一周）練習が効く
- **ユーザー位置づけ**: 「devs building agents」に近い（Claude Code 経由）・CCAR-F 904 基盤あり・SDK/API 直接が gap
- **CCDV-F vs CCAR-F**: 絶対難易度でなく「日常業務との一致」。CCDV-F は実装ディテールで狭く深い

## 進捗
- [x] CCAR-F 受験（904点 Pass）
- [ ] CCAR-F 分析完了
- [ ] CCDV-F 重点復習
- [ ] CCDV-F 受験
