# Sundog CCDV-F Exam1 — 間違えた問題 復習

Score: 43/53 (81%) ・Attempt 2 ・2026-07-19 ・10 incorrect

ドメイン別誤答: D1 App/Integration 3, D2 Model Selection 2, D3 Agents 1, D5 Tools/MCPs 3, D6 Security 1

---

## Part 1: 英語で解き直し（原文・正解マークなし）

> 本番形式で解き直す。優れたエンジニアは英語問題文から直接判断できる必要がある。回答は Part 2 で確認。

### Q5（D1: Applications and Integration）

A university registrar's platform team is moving a Claude-powered transcript review workflow from pilot testing into production. The workflow uses a custom Agent Skill and a reusable prompt template. The team needs stable releases, repeatable testing, and a safe way to maintain the system over time. Which two actions best support this life cycle goal?

- [ ] A. Pin production Skill usage to specific versions, and use latest only during active development.
- [ ] B. Point production requests at latest Skill versions, and rely on user reports for rollback.
- [ ] C. Store prompts and Skill files as versioned artifacts, and test changes before promotion.
- [ ] D. Edit live prompts during incidents, and skip templates to reduce release overhead.

### Q15（D2: Model Selection and Optimization）

A streaming entertainment platform is rebuilding its Claude router after a prototype on Claude Sonnet 4.5. The platform has millions of short subtitle format checks each day, plus a smaller set of nuanced script adaptation reviews for regional releases. The existing request builder always sends `thinking: {type: "enabled", budget_tokens: N}`. Which two recommendations best fit the model-selection and migration requirements?

- [ ] A. Standardize every workload on Claude Opus 4.8 because it is the fastest current model for production traffic.
- [ ] B. Remove manual thinking budgets from Claude Sonnet 5 requests and tune reasoning depth with the `effort` parameter.
- [ ] C. Route the straightforward subtitle checks to Claude Haiku 4.5 after measuring quality, latency, and cost on representative traffic.
- [ ] D. Keep `thinking: {type: "enabled", budget_tokens: N}` on Claude Sonnet 5 to preserve the Sonnet 4.5 request shape.

### Q20（D3: Agents and Workflows）

A renewable energy grid operator's reliability tooling team is building an interactive maintenance assistant with the Claude Agent SDK. During a troubleshooting session, an engineer asks follow-up questions that depend on files Claude already inspected, and the UI must let the engineer stop a long command-driven run before issuing a new request. Which approach best fits the interaction model?

- [ ] A. Spawn a new subagent for each engineer message so every follow-up runs in isolated context.
- [ ] B. Call query() separately for each follow-up so every request starts with an independent session.
- [ ] C. Run claude -p with bare mode for each message so local configuration is skipped.
- [ ] D. Use ClaudeSDKClient so the session persists across turns and the UI can handle interruption flow.

### Q22（D5: Tools and MCPs）

A maritime logistics provider's backend team is adding Claude to a customer support console. Agents ask questions such as "Where is container MSKU1234567 and what exceptions are active?" The data is in a private tracking service reachable only from the provider's application backend, and the team wants one Messages API integration rather than a reusable MCP server. Which approach best fits?

- [ ] A. Use the web fetch server tool to retrieve tracking responses, then parse the container status from the page.
- [ ] B. Use the MCP connector against the tracking service URL, then rely on Claude to discover its REST endpoints.
- [ ] C. Package the tracking workflow as an Agent Skill, then have Claude call the private service from the Skill.
- [ ] D. Define a custom client tool with a structured input schema, then execute the tracking lookup in the application.

### Q23（D5: Tools and MCPs）

A legal publishing team's platform group is building a Claude API integration to help editors draft case summaries. Editors need Claude to apply an in-house style guide, reuse document templates, and run packaged validation scripts when the task calls for them. The workflow does not require live access to an internal service or the external network. Which approach best fits?

- [ ] A. Package the guide, templates, and scripts as an Agent Skill and enable it with code execution.
- [ ] B. Define each editorial rule as a custom client tool and return tool results from the application.
- [ ] C. Fetch the style guide and templates with a server tool during every drafting request.
- [ ] D. Connect the materials as a local STDIO MCP server through the Messages API MCP connector.

### Q33（D1: Applications and Integration）

A restaurant reservation platform's analytics team uses the Messages API to label diner feedback with a short internal code. Their downstream parser expects each output to start with a fixed marker followed by only a few generated code characters, with no explanatory text. Which approach best fits this integration pattern?

- [ ] A. Put the fixed prefix in the top-level system field, and ask Claude to repeat it before the code.
- [ ] B. Add the fixed prefix as an earlier user message, and parse the first matching code in the reply.
- [ ] C. End messages with a partial assistant turn containing the fixed prefix, and set a small max_tokens for the code.
- [ ] D. Send only the latest feedback text, and rely on the API to preserve the previous classifier state.

### Q34（D2: Model Selection and Optimization）

A maritime logistics startup runs a Claude-powered route-planning assistant. Each request includes a large, stable port operations manual, then a changing vessel profile, weather snapshot, and user question. The team also uses adaptive thinking for hard planning requests and notices that invoices show more output tokens than the visible text in the response. Which two actions should the team take to improve cost visibility and reduce repeated input cost?

- [ ] A. Track usage with cache token fields and output thinking token details rather than visible text alone.
- [ ] B. Move cache_control to the final user block so each full request is checkpointed for reuse.
- [ ] C. Set thinking display to omitted and treat visible response tokens as the billed output tokens.
- [ ] D. Place cache_control on the last manual block that stays identical before the changing request content.

### Q41（D5: Tools and MCPs）

A museum archive's digital preservation team is authoring an MCP server for Claude applications. The server must let applications access archival records, provide reusable curator review templates, perform approved metadata updates, and support remote HTTP clients. Which TWO implementation choices best align with MCP server development practices?

- [ ] A. Model metadata update operations as tools exposed through tools/list and invoked through tools/call.
- [ ] B. Keep the older HTTP plus SSE split because it is the current transport for remote MCP servers.
- [ ] C. Expose records and review templates as tools so the model controls every retrieval and template selection.
- [ ] D. Use a Streamable HTTP endpoint with POST for client messages, GET for streaming, and web security controls.

### Q44（D1: Applications and Integration）

An aerospace robotics lab has a shared Claude Code project configuration that sets the default model for day-to-day development. One engineer needs to run a single experiment with a different model, without committing a configuration change or affecting teammates. Which approach best fits?

- [ ] A. Change the user model setting mid-session and rely on settings hot reload.
- [ ] B. Set the shared project model in .claude/settings.json for the experiment.
- [ ] C. Place a temporary model instruction in CLAUDE.md for the experiment.
- [ ] D. Start the experiment with a CLI model override for that invocation only.

### Q50（D6: Security and Safety）

A regional housing authority's digital services team is launching a Claude assistant that drafts eligibility notes for housing benefits caseworkers and may show recommendation summaries directly to applicants. Users can type free-form messages, and the team expects some attempts to bypass the application's rules. Which TWO deployment actions best align with Anthropic's safety guidance?

- [ ] A. Rely on a stronger system prompt as the sole jailbreak mitigation.
- [ ] B. Apply the content moderation guide as the primary control for Claude interactions.
- [ ] C. Pre-screen user inputs with a lightweight Claude model using structured classification.
- [ ] D. Require qualified human review before eligibility recommendations are finalized or disseminated.

---

## Part 2: 日本語で復習（和訳＋解説＋正解）

### Q5（D1: Applications and Integration）— 複数回答（A・C）

**問題文（和訳）**: 大学登録課のプラットフォームチームが、Claude を使った成績証明書レビューワークフローをパイロットから本番に移行している。このワークフローはカスタム Agent Skill と再利用可能なプロンプトテンプレートを使う。チームは安定したリリース・反復可能なテスト・長期的な安全な保守を必要としている。このライフサイクル目標を最もよく支えるアクション 2 つはどれか。

- A. 本番での Skill 利用を特定バージョンに pin し、active development 中のみ latest を使う ← 正解
- B. 本番リクエストを latest Skill バージョンに向け、ロールバックはユーザ報告に頼る
- C. プロンプトと Skill ファイルを versioned artifacts として保存し、昇格前に変更をテストする ← 正解
- D. インシデント中にライブのプロンプトを直接編集し、テンプレートを省いてリリースオーバーヘッドを減らす

**解説**: プロンプト・Skill・ツール定義・agent 設定は本番の挙動を決定する「管理対象アーティファクト」。コードと同じく開発→検証→デプロイ→運用→保守のライフサイクルを通すべき。Skill は本番ではバージョン pin、開発では latest が推奨パターン。プロンプトはテンプレートで固定部と可変部を分離し、バージョン管理とテストでトレーサビリティを確保。B（latest 追跡・ユーザ報告依存）は reactive で再現性がない。D（ライブ編集・テンプレート省略）は change control を弱める。

**参照**: Prep M3 S08「Packaging Workflows」（Agent Skills の API/Claude Code での使い分け・versioning）

---

### Q15（D2: Model Selection and Optimization）— 複数回答（B・C）

**問題文（和訳）**: ストリーミングエンタメプラットフォームが Claude Sonnet 4.5 のプロトタイプ後、Claude ルータを再構築している。1 日に数百万件の短い字幕フォーマットチェックと、少数の地域別脚本適応レビューがある。既存のリクエストビルダは常に `thinking: {type: "enabled", budget_tokens: N}` を送る。model-selection と移行要件に最適な推奨 2 つはどれか。

- A. すべてのワークロードを Claude Opus 4.8 に統一する（最速の本番向けモデルだから）
- B. Claude Sonnet 5 リクエストから manual thinking budget を削除し、`effort` パラメータで推論深度を調整する ← 正解
- C. 代表トラフィックで品質・レイテンシ・コストを計測したうえで、単純な字幕チェックは Claude Haiku 4.5 にルーティングする ← 正解
- D. Sonnet 4.5 のリクエスト形状を保つため Sonnet 5 でも `thinking: {type: "enabled", budget_tokens: N}` を維持する

**解説**: Haiku 4.5 は「最速・near-frontier intelligence」で高ボリューム単純タスク向け。Opus 4.8 は agentic coding/enterprise 向きで最速ではない。Sonnet 5 は **adaptive thinking がデフォルト**で manual `budget_tokens` を **400 拒否**する。そのため D は移行アンチパターン。正しい移行は B のとおり `effort` で推論深度を制御する。C の Haiku 4.5 ルーティングは計測ペア付きが推奨（eval で品質ゲートを守るため）。A は Opus 4.8 を誤って位置づけている。

**参照**: Prep M1 S03「Models & Reasoning」（model family の位置づけ）＋ Prep M2 S05「Extended Thinking」（adaptive thinking・`effort`・`budget_tokens` 非推奨化）

---

### Q20（D3: Agents and Workflows）— 単一回答（D）

**問題文（和訳）**: 再生可能エネルギー網オペレータの信頼性ツールチームが Claude Agent SDK でインタラクティブな保守アシスタントを構築している。トラブルシューティング中、エンジニアが Claude が既に調査したファイルに依存するフォローアップ質問をし、UI は新しいリクエスト前に長いコマンド駆動の実行を止められる必要がある。最適なインタラクションモデルはどれか。

- A. エンジニアの各メッセージで新しい subagent を spawn し、すべてのフォローアップを隔離されたコンテキストで実行する
- B. 各フォローアップごとに query() を別々に呼び、毎回独立したセッションで始める
- C. 各メッセージを `claude -p` の bare mode で実行し、ローカル設定をスキップする
- D. ClaudeSDKClient を使い、セッションがターン間で保持され UI が interruption flow を処理できるようにする ← 正解

**解説**: Agent SDK の核心は「一_off タスクか継続会話か」。`query()` は独立した one-off・自動化向けでデフォルトで新規セッション生成。`ClaudeSDKClient` はマルチターン・状態保持・中断(in-flight task の stop → drain → next query)をサポート。インタラクティブなトラブルシューティング（過去のファイル調査・仮説に依存するフォローアップ）には `ClaudeSDKClient` が必須。A の subagent はコンテキスト隔離に使うもので共有状態の維持には不適。C の bare `-p` は CI/スクリプト向け。

**参照**: Prep M2 S16「Agent Construction」（wiring path: Messages API / Agent SDK / Managed Agents の選択・loop の実装）

---

### Q22（D5: Tools and MCPs）— 単一回答（D）

**問題文（和訳）**: 海事物流プロバイダのバックエンドチームが顧客サポートコンソールに Claude を追加する。エージェントは「コンテナ MSKU1234567 はどこにあり、例外は何がアクティブか？」と聞く。データはプロバイダのアプリケーションバックエンドからのみ届く private tracking service にあり、再利用可能な MCP サーバではなく 1 つの Messages API 統合がしたい。最適なアプローチはどれか。

- A. web fetch server tool で追跡レスポンスを取得し、ページからコンテナステータスをパースする
- B. tracking service URL に対して MCP connector を使い、Claude に REST エンドポイントを発見させる
- C. 追跡ワークフローを Agent Skill としてパッケージし、Skill から private service を呼ばせる
- D. 構造化 input schema を持つ custom client tool を定義し、tracking lookup をアプリケーション側で実行する ← 正解

**解説**: private backend lookup には custom client tool が最適。Claude は `tool_use` を出し、アプリケーションが private service を実行して `tool_result` を返す。認証・ネットワーク・バリデーション・エラーハンドリングはすべてバックエンドに留まる。A の web fetch は Anthropic 側 URL 取得用で private service への橋ではない。B の MCP connector は remote MCP server 向きで arbitrary REST 発見機構ではない。C の Skill はバックエンド API を構造化アクションとして公開する抽象ではない（code execution sandbox で動く）。

**参照**: Prep M2 S07「Tool-use and Schema Design」（tool-use loop・client 実行モデル・schema 設計）

---

### Q23（D5: Tools and MCPs）— 単一回答（A）

**問題文（和訳）**: 法律出版チームが編集者向けのケースサマリー作成 Claude API 統合を構築している。編集者は社内スタイルガイドの適用・ドキュメントテンプレートの再利用・タスクごとのパッケージ化検証スクリプト実行を必要とする。ワークフローは内部サービスや外部ネットワークへのライブアクセスを必要としない。最適なアプローチはどれか。

- A. ガイド・テンプレート・スクリプトを Agent Skill としてパッケージし、code execution で有効化する ← 正解
- B. 各編集ルールを custom client tool として定義し、アプリケーションから tool result を返す
- C. サーバツールで毎回ドラフト要求時にスタイルガイドとテンプレートを fetch する
- D. Materials を local STDIO MCP server として Messages API MCP connector 経由で接続する

**解説**: 要件は「再利用可能なタスク知識 + バンドルされたリソース・スクリプト」であってライブ API コールではない。Agent Skill は instructions・metadata・scripts・templates をパッケージ化し、progressive disclosure で関連時にロード。Claude API では code execution と組み合わせて使い、sandbox 内でバンドル素材にアクセス・実行。B（各編集ルールを client tool 化）は不要な tool orchestration を増やす。C（毎回 fetch）は安定素材の再パッケージを怠る。D は MCP connector が remote HTTP 向けで local STDIO を Messages API に直結できない点を誤解。

**参照**: Prep M3 S08「Packaging Workflows」（Skills の API/Claude Code 統合・code execution sandbox の制約）

---

### Q33（D1: Applications and Integration）— 単一回答（C）

**問題文（和訳）**: レストラン予約プラットフォームの解析チームが Messages API で diner feedback に短い社内コードを付ける。下流パーサは各出力が固定マーカーで始まり、あとは数文字の生成コードのみ（説明文なし）を期待する。この統合パターンに最適なのはどれか。

- A. 固定 prefix を top-level system field に置き、コード前に Claude にそれを繰り返させる
- B. 固定 prefix を以前の user message として追加し、返信内の最初の一致コードをパースする
- C. メッセージを partial assistant turn（固定 prefix を含む）で終え、コード用に小さな max_tokens を設定する ← 正解
- D. 最新の feedback テキストのみを送り、API が以前の分類器状態を保持することに頼る

**解説**: Messages API は各リクエストを自己完結プロンプトとして扱う。サポートするモデルでは partial assistant message で Claude の「続き」を開始できる（**prefill** パターン）。短い分類コードのようなコンパクトな続きには `max_tokens` を小さく設定すると効果的。A・B は system/user 経由の誘導で、続きの開始点を直接形作れない。D は Messages API がステートレス（サーバ側会話状態を自動保存しない）点を誤解。ただし **prefill はモデル互換性があり**、未サポート機種では 400 になる点に注意（現行モデルの一部のみ支持）。

**参照**: Prep M2 S02「Prompting Craft」（system prompt・prefill・output constraint の使い分け）

---

### Q34（D2: Model Selection and Optimization）— 複数回答（A・D）

**問題文（和訳）**: 海事物流スタートアップが Claude 駆動のルート計画アシスタントを運用する。各リクエストには大型で安定した港運用マニュアル、続いて変化する船舶プロファイル・天気スナップショット・ユーザ質問が含まれる。adaptive thinking を使い、請求書の output token が可視テキストより多いことに気づいている。コスト可視性を改善し反復入力コストを下げるアクション 2 つはどれか。

- A. cache token fields と output thinking token details で usage を追跡する（可視テキストだけでなく） ← 正解
- B. cache_control を最終 user block に移し、各完全リクエストを再利用可能に checkpoint する
- C. thinking display を omitted に設定し、可視応答トークンを課金出力トークンとみなす
- D. cache_control を、変化する要求コンテンツの直前の最後の manual block（同一のもの）に置く ← 正解

**解説**: コスト管理は 2 つの関心を分ける。①再利用される入力: prompt caching は同一 prefix で効果を発揮。checkpoint は安定 prefix（港マニュアル）の最後の block に置き、変動コンテンツ（船舶・天気・質問）は checkpoint 後に置く（D）。②課金される出力: usage フィールドを読む。caching 活性時は入力合計が `cache_read_input_tokens`・`cache_creation_input_tokens`・`input_tokens` に分散。thinking 使用時、可視 thinking は省略されても内部 thinking token は課金出力に計上され、`usage.output_tokens_details.thinking_tokens` で可視化される（A）。B は最終 user block が毎回変化するため fresh cache write になる（アンチパターン）。C は omitted が課金免除ではなくレイテンシ最適化に過ぎない点を誤解。

**参照**: Prep M2 S05「Extended Thinking」（thinking token 課金・display 設定）＋ Prep M2 S13「Context Engineering」（cache_control 配置・cache token フィールド）

---

### Q41（D5: Tools and MCPs）— 複数回答（A・D）

**問題文（和訳）**: 博物館アーカイブのデジタル保存チームが Claude アプリ向け MCP server を作っている。サーバはアプリにアーカイブレコードへのアクセス・再利用可能な学芸員レビューテンプレート・承認されたメタデータ更新・remote HTTP クライアントサポートを提供する必要がある。MCP server 開発プラクティスに最も合致する実装選択 2 つはどれか。

- A. メタデータ更新操作を tools としてモデル化し、tools/list で公開し tools/call で起動する ← 正解
- B. 古い HTTP+SSE 分割を維持する（remote MCP server 向け現行トランスポートだから）
- C. レコードとレビューテンプレートを tools として公開し、モデルにすべての取得とテンプレート選択を制御させる
- D. Streamable HTTP endpoint を使い、client メッセージに POST・streaming に GET・web security controls を設定する ← 正解

**解説**: MCP の primitive は制御モデルが異なる。Tools = model-controlled 実行可能関数（`tools/list`・`tools/call`）、resources = application-controlled 読み取りデータ、prompts = user-controlled 指示テンプレート。承認されたメタデータ更新は tools に（A）。アーカイブレコードは resources・テンプレートは prompts が適切で、すべて tools 化する C は誤り。remote HTTP 向け現行トランスポートは **Streamable HTTP**（protocol version 2024-11-05 以降）。古い HTTP+SSE は deprecated で B は誤り。D は単一 endpoint・POST(クライアント JSON-RPC)・GET(SSE)・Origin 検証・bind 管理・認証を含む正しい実装。

**参照**: Prep M3 S12「MCP Servers」（tools/resources/prompts の使い分け・stdio/HTTP/SSE transport・Streamable HTTP）

---

### Q44（D1: Applications and Integration）— 単一回答（D）

**問題文（和訳）**: 航空宇宙ロボティクスラボが day-to-day 開発のデフォルトモデルを設定した共有 Claude Code プロジェクト設定を持つ。1 人のエンジニアが設定変更を commit せず・チームメイトに影響を与えずに、異なるモデルで 1 回限りの実験を実行したい。最適なアプローチはどれか。

- A. セッション途中で user model setting を変更し、settings hot reload に頼る
- B. 実験用に .claude/settings.json の共有 project model を設定する
- C. CLAUDE.md に一時的な model 指示を置く
- D. その invocation のみ CLI model override で実験を開始する ← 正解

**解説**: Claude Code の settings は階層的（managed > command-line > local project > shared project > user）。コマンドライン model override は単一セッションだけデフォルトから外れる仕組みで、共有設定を変えずチームメイトへの影響もない（D）。A は model 設定が session start で 1 回読まれる点（hot reload しない）と user settings 編集が永続化する点を誤解。B は共有 project settings を変えるためチームに影響。C は CLAUDE.md が memory/context であって runtime config の仕組みではない点を誤解（`/model` インタラクティブ命令は別）。

**参照**: Prep M3 S02「Permission Modes & Human Gates」（settings 階層・user/project/enterprise・hot reload の境界）

---

### Q50（D6: Security and Safety）— 複数回答（C・D）

**問題文（和訳）**: 地域住宅局のデジタルサービスチームが、住宅給付のケースワーカー向け適格性ノートを起草し、推奨サマリーを申請者に直接見せる可能性のある Claude アシスタントを立ち上げる。ユーザは自由形式メッセージを入力でき、アプリルール回避の試みが一部あると予想される。Anthropic の安全ガイダンスに最も合致するデプロイアクション 2 つはどれか。

- A. より強力な system prompt のみを jailbreak 対策として頼る
- B. Claude やり取りの主要制御として content moderation guide を適用する
- C. lightweight Claude モデルで構造化分類を使いユーザ入力を pre-screen する ← 正解
- D. 適格性推奨が確定・公開される前に qualified human review を要求する ← 正解

**解説**: 住宅給付は Anthropic Usage Policy の high-risk use case（住宅・金融・医療・法律・雇用など個人に直接影響する決定）に該当。したがって D（確定前の qualified human review）が必須制御。また申請者への直接提示時は開示要件も評価する。jailbreak 対策は layered が原則で、single system prompt だけでは不十分（A 誤り）。Anthropic は pre-screening（lightweight model・構造化分類）・既知 injection パターン filter・prompt hardening・反復滥用者の throttle/ban を推奨（C 正解）。B は content moderation guide が「アプリ内ユーザ生成コンテンツのモデレーション」向けで、Claude やり取りの保護には guardrails ガイダンスが正しい参照先である点を誤解。

**参照**: Prep M4「Production Engineering, Evals & Security」（high-risk use case・guardrails・jailbreak mitigation の layered defense）

---

## 復習メモ

- **D5 Tools/MCPs 3問誤答** が最大の弱点。client tool vs server tool vs Skill vs MCP の使い分け（Q22/Q23/Q41）を priority で潰す。
- **D2 Model Selection 2問**: adaptive thinking 移行（budget_tokens → effort）と cache_control 配置・thinking token 課金を公式 doc で再確認。
- **複数回答問題の読み違い**: Q34 で thinking display omitted を「コスト削減」と誤読。omitted はレイテンシ最適化のみ。Q50 で guardrails vs content moderation の参照先を混同。
- **次アクション**: Prep M2 S07（tool-use）・M3 S08（Skills）・M3 S12（MCP）を再読後、Exam3 で D5/D2 を再測定。
