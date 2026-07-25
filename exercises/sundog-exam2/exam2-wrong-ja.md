# Sundog CCDV-F Exam2 — 間違えた問題 復習

> 対象: Udemy「Sundog Education (Frank Kane) CCDV-F Practice Exam Pack」Exam2
> 受験日: 2026-07-20 / 成績: 44/53 = 83%（8 incorrect + 1 skipped）
> 9 問（Q6, Q12, Q13, Q18, Q20, Q23, Q44, Q47, Q51）を 2 パート構成で収録
> 使い方: まず **Part 1** を英語で解き直す（正解は隠してある）。その後 **Part 2** で和訳・解説・正解・Prep mapping を確認

---

## Part 1: 英語で解き直し（原文・正解マークなし）

### Q6（D1 Agents & Workflows）
A video game studio's build engineering team wants to add Claude to an internal desktop utility. The utility should work on a developer's local checkout, inspect and edit files with standard coding tools, stream progress into the UI, and run deterministic validation before tool calls. The team does not want to implement the entire tool execution loop itself. Which implementation best fits?
- [ ] A. Use the Anthropic Client SDK with Messages API calls, request file edits in text, and omit tool execution code.
- [ ] B. Embed the Claude Agent SDK, enable the needed file tools, stream SDK messages, and add hooks around tool use.
- [ ] C. Set allowedTools to file tools with bypassPermissions, stream the final result, and treat other tools as unavailable.
- [ ] D. Deploy Claude Managed Agents in a cloud sandbox, copy each checkout there, and stream session events back.

（回答は Part 2 で確認）

### Q12（D1 Agents & Workflows）
A maritime insurance analytics team is adding a Claude-powered assistant to a TypeScript desktop application for developers. The assistant must inspect a local checkout, use built-in coding tools through Claude's autonomous loop, and stream progress back to the UI. The team wants to avoid building the tool execution loop and avoid requiring a separate Claude Code installation on every laptop. Which approach best fits?
- [ ] A. Install a legacy Claude Code SDK package and require Claude Code on every developer laptop.
- [ ] B. Install `@anthropic-ai/claude-agent-sdk` and use its streaming agent loop from the application.
- [ ] C. Install the Anthropic Client SDK and rely on Messages API requests to execute tools automatically.
- [ ] D. Build a custom Messages API harness and implement local file, search, and Bash tool executors.

（回答は Part 2 で確認）

### Q13（D1 Agents & Workflows）
A public defender's e-discovery team is adding Claude to review case document dumps before attorney intake. Each case must end in the same intake memo, but the needed intermediate work can differ after inspection, such as timeline extraction, witness statement comparison, police report inconsistency checks, or exhibit indexing. The worker roles are known in advance, and the team wants more predictability than an open-ended autonomous system. Which architecture best fits this requirement?
- [ ] A. Use an orchestrator-workers workflow with a supervising LLM that assigns relevant worker analyses and combines their outputs.
- [ ] B. Use a fixed prompt-chaining workflow that runs every specialist review in the same order for all cases.
- [ ] C. Use a fully autonomous agent that chooses all tools and stages without predefined worker roles or memo structure.
- [ ] D. Use a simple routing workflow that sends each case to one specialist based only on the first document.

（回答は Part 2 で確認）

### Q18（D6 Prompt & Context Engineering）
A pharmaceutical safety monitoring team runs a Claude triage agent that calls internal tools returning verbose scanner logs. After the agent extracts the findings, older tool outputs are rarely useful, but the team must keep its full audit history in its own database. The team wants the API to reduce what Claude sees on later turns without rewriting the client-side conversation history. Which approach best fits?
- [ ] A. Switch to a larger context model and append all logs unchanged.
- [ ] B. Enable prompt caching for the scanner logs in every request.
- [ ] C. Enable context editing with tool result clearing for the request.
- [ ] D. Delete older tool result messages from the audit database.

（回答は Part 2 で確認）

### Q20（D3 Claude Code）
A landscape architecture firm's platform team is creating a scheduled local script that asks Claude Code to inspect regenerated OpenAPI files. The script must run without opening the interactive terminal interface, produce output that another process can parse, and stop after a team-defined number of agent turns. Which TWO actions should the team take?
- [ ] A. Run `claude -p` or `claude --print` with the inspection prompt from the automation process.
- [ ] B. Skip the turn limit because print mode already applies a default maximum turn count.
- [ ] C. Add `--output-format json` or `--output-format stream-json` and set `--max-turns` on that invocation.
- [ ] D. Start `claude` interactively and set `--max-turns` before entering the inspection prompt.

（回答は Part 2 で確認）

### Q23（D2 Applications & Integration）— Skipped（未回答）
A marine logistics software team runs nightly Claude Code evaluation jobs for prompt regressions. They want model choice to stay stable across future releases, and the job currently sets `model` in shared project settings to `claude-sonnet-5`. A reviewer suggests replacing it with a dated-looking model string or a general alias because the current ID might float. Which approach best fits the documented configuration behavior?
- [ ] A. Set `model` to `sonnet` in the job command so the alias remains fixed over time.
- [ ] B. Use a pre-4.6 short alias so minor-version resolution preserves exact snapshots.
- [ ] C. Keep `model` set to `claude-sonnet-5` in the job's settings for stable model selection.
- [ ] D. Move the model choice into `CLAUDE.md` so startup context enforces the selection.

（回答は Part 2 で確認）

### Q44（D1 Agents & Workflows）
A clinical trials data platform team is building a Claude-based repository migration assistant. For each migration, it must run 60 independent checks across modules, compare the findings, and then produce a consolidated plan. The team wants the main conversation to stay focused on coordination rather than accumulating every search result and intermediate transcript. Which approach best fits this workflow?
- [ ] A. Load all module files into the first prompt before planning the migration.
- [ ] B. Use the Workflow tool to run the independent checks from an orchestration script.
- [ ] C. Ask the main agent to call one subagent at a time for each check.
- [ ] D. Store every intermediate check result in memory before final synthesis.

（回答は Part 2 で確認）

### Q47（D2 Applications & Integration）
An urban planning software vendor's platform team uses Claude Code to try three competing refactors of a zoning-rules engine. Each attempt will modify many files and run tests. The engineers want to compare the diffs side by side and avoid having one attempt overwrite another before a human chooses the best design. Which approach best fits?
- [ ] A. Keep one Claude Code session open and switch branches between attempts as each refactor progresses.
- [ ] B. Run several Claude Code sessions in the same checkout and ask each one to avoid touching shared files.
- [ ] C. Disable file edits until all approaches are planned, then apply every planned change in one combined pass.
- [ ] D. Create separate git worktrees for each attempt and run an independent Claude Code session in each checkout.

（回答は Part 2 で確認）

### Q51（D6 Prompt & Context Engineering）
An insurance underwriting team's service uses Claude Opus 4.8 to classify claim notes for downstream routing. The legacy queue names include `Review Needed` and `Review needed`, which differ only by capitalization but route to different teams. The team needs parseable JSON and wants to avoid misrouting caused by output handling assumptions. Which implementation best fits?
- [ ] A. Ask Claude for JSON in the prompt, parse the first object, and retry only on syntax errors.
- [ ] B. Expose the legacy queue names with `output_config.format`, then route using exact case-sensitive string matching.
- [ ] C. Prefill the assistant response with the desired label casing, parse the label, and route before stop checks.
- [ ] D. Expose stable non-casing status codes with `output_config.format`, check stop reasons, and map parsed codes to queues.

（回答は Part 2 で確認）

---

## Part 2: 日本語で復習（和訳＋解説＋正解）

> 凡例: 「← 正解」= 正解選択肢、「← あなたの誤答」= 受験時に選んだ誤り、「Silent miss」= 自信あったが誤、「Caught」= ★marked（自覚済）で誤

### Q6（D1 Agents & Workflows）— Silent miss
**問題文（和訳）**: ビデオゲームスタジオのビルドエンジニアリングチームが、内部デスクトップ utility に Claude を追加したい。この utility は開発者のローカル checkout で動作し、標準 coding tools でファイルを検査・編集し、進捗を UI に stream し、tool call の前に決定的な検証を実行する。チームは **tool 実行 loop 全体を自前実装したくない**。どの実装が最適か？
- A. Anthropic Client SDK で Messages API を呼び出し、テキストでファイル編集を要求し、tool 実行 code を省く
- B. Claude Agent SDK を組み込み、必要な file tools を有効化し、SDK messages を stream し、tool use 周りに hooks を追加 **← 正解**
- C. `allowedTools` を file tools に設定し `bypassPermissions` にし、最終結果のみ stream し、他の tools は利用不可とする
- D. Claude Managed Agents を cloud sandbox にデプロイし、各 checkout をそこにコピーし、session events を stream **← あなたの誤答**
**解説**: 3つの wiring path（M2 Screen 16）の選別。**Managed Agents** = cloud sandbox（本件はローカル直操作に不適）。**Client SDK** = tool loop を自前実装する必要。**Agent SDK** = プロセス内で Claude Code の loop・built-in tools・hooks を組み込み。「ローカル + loop 自前不要」なら Agent SDK 一択。`allowedTools` は auto-approval であって厳格な allowlist ではない点にも注意。
**参照**: Prep M2 Screen 16（3 wiring paths）・Screen 28（Agent SDK 定義）

### Q12（D1 Agents & Workflows）— Caught
**問題文（和訳）**: 海事保険 analytics チームが、TypeScript デスクトップアプリに Claude-powered assistant を追加している。assistant はローカル checkout を検査し、Claude の autonomous loop で built-in coding tools を使い、進捗を UI に stream する必要がある。チームは **tool 実行 loop の構築**も**各 laptop への Claude Code 別途インストール**も避けたい。どの手法が最適か？
- A. legacy な Claude Code SDK package をインストールし、各開発者 laptop で Claude Code を必須にする
- B. `@anthropic-ai/claude-agent-sdk` をインストールし、その streaming agent loop を application から使用 **← 正解**
- C. Anthropic Client SDK をインストールし、Messages API request で tools を自動実行 **← あなたの誤答**
- D. custom Messages API harness を構築し、local file・search・Bash tool executor を実装
**解説**: Client SDK は Messages API の**薄い wrapper**（agent loop / built-in coding tools なし・自前 harness 必要）。Agent SDK の TS 版 `@anthropic-ai/claude-agent-sdk` は **native Claude Code binary を同梱**（CC 別途不要）。旧 "Claude Code SDK" は legacy 名。
**参照**: Prep M2 Screen 28（SDK 定義）・M3 Screen 8（Managed Agents beta）

### Q13（D1 Agents & Workflows）— Caught
**問題文（和訳）**: 公選弁護人の e-discovery チームが、attorney intake 前に case 文書一式を Claude で review している。各 case は**同じ intake memo** で終わる必要があるが、中間作業は検査後に異なり得る（timeline 抽出・証人陳述比較・警察報告 inconsistency check・exhibit 索引など）。worker 役割は事前既知で、open-ended な autonomous system より**予測可能性**を重視する。どの architecture が最適か？
- A. orchestrator-workers workflow・監督 LLM が関連 worker 分析を割り当て、出力を統合 **← 正解**
- B. 固定の prompt-chaining workflow・全 case で同じ順序の専門 review を実行
- C. 完全 autonomous agent・事前の worker 役割や memo 構造なしに全 tools/stages を選択
- D. simple routing workflow・最初の文書のみで1専門家に送る **← あなたの誤答**
**解説**: workflow patterns（"Building Effective Agents"）の選別。**routing** = 1入力を1パスに分類（本件は1 case に複数分析が必要で不適）。**orchestrator-workers** = 動的に subtask 分解しつつ予測可能な最終構造を保持 = 本件の sweet spot。完全 autonomous は over-kill。
**参照**: Prep M2 Screen 27・M4 Screen 11（orchestrator-worker のコスト）

### Q18（D6 Prompt & Context Engineering）— Caught
**問題文（和訳）**: 医薬品安全監視チームが Claude triage agent を運用し、冗長な scanner logs を返す内部 tools を呼んでいる。agent が findings を抽出した後、古い tool 出力はほとんど役立たないが、チームは**完全な audit 履歴を自前 DB で保持**する必要がある。client 側の会話履歴を書き換えずに、API で Claude が後続 turn で見る情報を減らしたい。どの手法が最適か？
- A. より大きな context model に切り替え、全 logs をそのまま append
- B. 全 request で scanner logs の prompt caching を有効化
- C. request の context editing（tool result clearing）を有効化 **← 正解**
- D. audit database から古い tool result messages を削除 **← あなたの誤答**
**解説**: 3つの区別。① **context editing** = server-side で Claude に届く前に適用（client は完全履歴保持可）。② **prompt caching** = cost/latency 最適化（cached token は文脈を占有・刈り取りではない）。③ **application storage** = 監査 DB（削除は監査要件と衝突）。「DB 保持 + Claude の見える文脈だけ削る」= context editing（`clear_tool_uses_20250919`）一択。
**参照**: 公式 doc「Context windows」— **Prep 未カバー**

### Q20（D3 Claude Code）— Silent miss（複選・片方正解/片方誤答）
**問題文（和訳）**: 景観建築 firm の platform チームが、再生された OpenAPI files を Claude Code で検査する scheduled local script を作成している。script は **interactive terminal interface を開かず**、別プロセスが parse 可能な出力を生成し、**team 定義の agent turn 数で停止**する必要がある。取るべき TWO アクションはどれか？
- A. automation process から inspection prompt を渡して `claude -p` または `claude --print` を実行 **← 正解**
- B. print mode がデフォルト最大 turn count を適用するので turn limit を skip
- C. `--output-format json` または `--output-format stream-json` を追加し、`--max-turns` を設定 **← 正解**
- D. `claude` を interactive で起動し、inspection prompt 入力前に `--max-turns` を設定 **← あなたの誤答**
**解説**: **print mode**（`-p` / `--print`）が非対話・自動化専用。`--max-turns` と `--output-format` は**print mode 専用**（interactive では適用外・要件の「非対話」にも違反）。print mode は**デフォルトで turn 上限なし**なので cap が必要。
**参照**: 公式 doc「Run Claude Code programmatically」・「CLI reference」— **Prep 未カバー**

### Q23（D2 Applications & Integration）— Skipped（未回答）
**問題文（和訳）**: 海事 logistics software チームが、prompt regression 検出のため毎晩 Claude Code evaluation job を実行している。将来の release でも model choice を安定させたい。現在 job は shared project settings で `model` を `claude-sonnet-5` に設定している。reviewer が、dated な model string か general alias に置き換えるよう提案している（現在の ID は float する可能性があると）。文書化された configuration 挙動に最も合う手法はどれか？
- A. job command で `model` を `sonnet` に設定し、alias が時経過で固定されるようにする
- B. pre-4.6 short alias を使い、minor-version 解決で完全な snapshot を保持
- C. job の settings で `model` を `claude-sonnet-5` のまま保持 **← 正解（あなたは未回答 = Skipped）**
- D. model choice を `CLAUDE.md` に移し、startup context に選択を強制
**解説**: Claude 4.6 以降では `claude-sonnet-5` のような dateless model ID は **canonical pinned snapshot**（evergreen alias ではない）。`settings.json` hierarchy が model 設定の正しい configuration surface。`CLAUDE.md` は context であって強制 layer ではない。general alias（`sonnet`）は便利だが固定用としては不適。pre-4.6 short alias は最新 dated snapshot に解決されうる。
**参照**: 公式 doc「Model IDs and versions」・「Claude Code settings」・「Claude Code memory」— Prep 未カバー（model versioning 挙動）

### Q44（D1 Agents & Workflows）— Silent miss
**問題文（和訳）**: 臨床試験 data platform チームが、Claude-based repository migration assistant を構築している。各 migration で **60 の独立した check** を module 全体に実行し、findings を比較し、統合 plan を作成する必要がある。チームは main 会話が調整に集中し、**全検索結果や中間 transcript が蓄積しない**ようにしたい。どの手法が最適か？
- A. 計画前に全 module files を最初の prompt に読み込む
- B. Workflow tool で独立 check を orchestration script から実行 **← 正解**
- C. main agent に各 check ごとに1つずつ subagent を呼ばせる **← あなたの誤答**
- D. 全中間 check 結果を最終統合前に memory に保存
**解説**: **subagents**（少数・隔離 context） vs **Workflow tool**（大規模 fan-out・orchestration を script 化・会話外実行）の使い分け。数十の独立 check は Workflow tool。1個ずつ委任は会話 loop に orchestration が残り latency/coordination overhead。memory は状態保存であって orchestration 機構ではない。
**参照**: Prep M2 Screen 28（Subagent）・M3 Screen 5（Subagents）・公式「Dynamic workflows」

### Q47（D2 Applications & Integration）— Silent miss
**問題文（和訳）**: 都市計画 software vendor の platform チームが、Claude Code で zoning-rules engine の **3つの競合 refactor** を試行している。各 attempt は多数 file を修正し test を実行する。engineer は diff を side by side で比較し、human が最良設計を選ぶ前にある attempt が別を上書きしないようにしたい。どの手法が最適か？
- A. 1つの Claude Code session を開いたまま各 refactor の進行ごとに branch を切り替え
- B. 同一 checkout で複数 Claude Code session を実行し、各々に shared file に触れないよう指示
- C. 全 approach の計画が完了するまで file 編集を無効化し、1統合 pass で全計画変更を適用 **← あなたの誤答**
- D. 各 attempt ごとに別 git worktree を作成し、各 checkout で独立 Claude Code session を実行 **← 正解**
**解説**: **git worktrees** = filesystem + branch で隔離（session は directory 紐付け・各実験が自由に編集/test・diff 比較容易）。branch switch は**会話履歴が残留**し混入。same checkout は衝突。統合 pass は「比較」の目的を損なう。並列実験は worktree 一択。
**参照**: 公式 doc「Claude Code overview」・「Create custom subagents」— **Prep 未カバー**

### Q51（D6 Prompt & Context Engineering）— Caught
**問題文（和訳）**: 保険 underwriting team の service が Claude Opus 4.8 で claim notes を分類し downstream routing に使っている。legacy queue 名は `Review Needed` と `Review needed` で、**大文字小文字のみが異なるが別 team に routing** される。parse 可能な JSON が必要で、出力取扱いの前提による誤 routing を避けたい。どの実装が最適か？
- A. prompt で JSON を要求し、最初の object を parse し、syntax error のみ retry **← あなたの誤答**
- B. `output_config.format` で legacy queue 名を露出し、case-sensitive 完全一致で routing
- C. assistant 応答に desired label casing を prefill し、label を parse し、stop check 前に routing
- D. `output_config.format` で stable な non-casing status codes を露出し、stop reasons を check し、parse した codes を queues に map **← 正解**
**解説**: ① **Structured Outputs**（`output_config.format` + JSON Schema）が schema 保証の正道（prompt+parse+retry は非公式）。② ただし **enum の casing は保証されない**（大文字小文字のみ相違は誤 route）→ stable code に置換。③ **stop_reason**（`refusal`/`max_tokens`）check 必須。④ **prefill は JSON と両立不可**・Opus 4.8 は prefill 非対応。
**参照**: Prep M2 Screen 2（output constraints / `output_config.format`）

---

## 復習メモ

- **Silent miss 4 問**（Q6, Q20, Q44, Q47）: 自信があったが誤。「自明」に見える問題ほど選択肢を全つぶしする習慣が対策
- **Caught 4 問**（Q12, Q13, Q18, Q51）: ★marked で自覚済。知識の境界を補強すれば潰せる
- **Skipped 1 問**（Q23）: 時間切れ。model versioning 挙動（dateless ID = pinned snapshot）は公式 doc で要確認
- **D1 Agents が最大急所**（Q6, Q12, Q13, Q44 の4問）: Prep M2 の精読で根本解決
- **Prep 未カバートピック**（Q18 context editing / Q20 print mode / Q47 worktrees / Q23 model versioning）: 公式 doc 直読が必須
