# Sundog CCDV-F Exam1 — 間違えた問題の日本語訳

> 元データ: `tasks/sundog-exam1-analysis/extracted/all.txt`（Attempt 2・53問中 10 incorrect）
> 試験結果: 43 correct / 10 incorrect / 0 skipped（81%）
> 翻訳方針: 技術用語・コード・パラメータ名（effort, adaptive thinking, tool_choice, cache_control, budget_tokens, prefill, max_turns 等）は英語保持。正解は all.txt の "Correct answer" に厳密に従う。

---

## Q5（D1: Applications and Integration）

大学の登録プラットフォームチームが、Claude を使った transcript review workflow をパイロットテストから本番へ移行しようとしている。この workflow は、独自の Agent Skill と再利用可能な prompt template を使う。チームには、安定したリリース、再現可能なテスト、そして長期的に安全にシステムを維持する方法が必要である。このライフサイクル目標を最もよく支える2つのアクションはどれか。

- A. 本番での Skill 使用を特定のバージョンに pin し、active development 中のみ latest を使う。 ← 正解
- B. 本番リクエストを latest の Skill バージョンに向け、ロールバックは user report に頼る。
- C. prompts と Skill files を versioned artifacts として保存し、promotion 前に変更をテストする。 ← 正解
- D. インシデント中は live の prompt を直接編集し、release overhead を減らすために templates を省く。

**解説**: Claude アプリケーションでは、prompts, Skills, tool definitions, agent configuration をすべて code と同じく管理されたシステム artifact として扱うべきである。これらは本番の挙動に直接影響するため、場当たりなその場編集ではなく、開発・検証・デプロイ・運用・保守のライフサイクルを通じて管理する。Agent Skills については、開発中は `latest`、本番では特定バージョンの pin が Anthropic の推奨パターン。prompts についても templates で固定内容と可変内容を分離し、version 管理と test を行う。`latest` に追従させたりインシデント時に直接編集すると、変更管理が弱まり「どの artifact がある結果を生んだか」を追跡できなくなる。

**参照**: Prep M5 S2「再利用のためのパッケージ化」＋ M5 S7C「要件とライフサイクル」（versioning と promotion gate）

---

## Q15（D2: Model Selection and Optimization）

あるストリーミングエンタメプラットフォームが、Claude Sonnet 4.5 の prototype をベースに Claude router を再構築している。1日数百万件の短い字幕フォーマットチェックと、地域別リリース向けの少数の nuanced な script adaptation review がある。既存の request builder は常に `thinking: {type: "enabled", budget_tokens: N}` を送る。model selection と移行の要件に最も合う2つの推奨はどれか。

- A. すべての workload を Claude Opus 4.8 に標準化する。現行の production 向け最快モデルだから。
- B. Claude Sonnet 5 のリクエストから manual thinking budget を外し、`effort` parameter で推論の深さを調整する。 ← 正解
- C. 代表的な traffic で品質・レイテンシ・コストを測ったうえで、単純な字幕チェックは Claude Haiku 4.5 に routing する。 ← 正解
- D. Claude Sonnet 5 でも `thinking: {type: "enabled", budget_tokens: N}` を維持し、Sonnet 4.5 の request shape を保つ。

**解説**: model selection は capability, latency, cost, context, feature 互換性の tradeoff であり、「最も能力の高いモデルが常に正解」ではない。Claude Haiku 4.5 は near-frontier intelligence を持つ最快モデルで、高ボリュームの単純タスクに適する。ただし eval での品質検証は必須。移行問題は capability とは別で、Claude Sonnet 5 は manual extended thinking（`budget_tokens`）をサポートせず、 adaptive thinking が default で、`effort` で品質・レイテンシ・token の tradeoff を調整する。古い `budget_tokens` を維持すると 400 error になるため、request builder の更新が必要。

**参照**: Prep M1 S3「Models & Reasoning」（adaptive thinking + effort）＋ M4 S10A「Model Selection」

---

## Q20（D3: Agents and Workflows）

再エネ grid 事業者の reliability チームが、Claude Agent SDK でインタラクティブな保守アシスタントを構築している。troubleshooting セッション中、エンジニアが Claude が既に調べたファイルに依存する follow-up 質問を投げ、UI は長い command 駆動の run を止めて新リクエストを出せる必要がある。この interaction model に最も合うのはどれか。

- A. エンジニアのメッセージごとに新しい subagent を spawn し、follow-up ごとに isolated context で動かす。
- B. follow-up ごとに別々に `query()` を呼び、毎回独立した session で始める。
- C. メッセージごとに bare mode で `claude -p` を走らせ、local 設定をスキップする。
- D. `ClaudeSDKClient` を使い、session をターン間で持続させ、UI が interruption flow を扱う。 ← 正解

**解説**: Agent SDK では `query()` は one-off task 向きで、デフォルトで毎回新しい session を生成する。一方 `ClaudeSDKClient` は、client が継続的接続と会話 lifecycle を管理する multi-turn session 向けに設計されている。インタラクティブな troubleshooting では、Claude が過去に調べたファイル・解釈したログ・立てた仮説を follow-up 質問が依存するため、context の持続が必須。さらに `ClaudeSDKClient` は進行中タスクの interrupt をサポートし、interrupted な response stream を drain してから次の query を送るフローが、エンジニアが長い run を止めて向きを変える UI に合う。subagent は context 隔離に使うもので、状態を持続させる会話の主機構ではない。

**参照**: —（Prep では `ClaudeSDKClient` vs `query()` の直接比較は未扱い。Agent SDK の loop 自体は Prep M2 S16「Agent Construction」を参照）

---

## Q22（D5: Tools and MCPs）

海運物流プロバイダーのバックエンドチームが、カスタマーサポートコンソールに Claude を追加する。エージェントは「container MSKU1234567 はどこにあって、どんな例外が active か?」といった質問をする。データはプロバイダーのアプリケーションバックエンドからしか届かない private tracking service にあり、再利用可能な MCP server ではなく1つの Messages API 統合が欲しい。最適な手法はどれか。

- A. web fetch server tool で tracking response を取得し、ページから container status を parse する。
- B. tracking service の URL に対して MCP connector を使い、Claude が REST endpoint を発見するのに任せる。
- C. tracking workflow を Agent Skill として package し、Skill から private service を呼ばせる。
- D. custom client tool を構造化 input schema で定義し、tracking lookup をアプリケーション内で実行する。 ← 正解

**解説**: private backend lookup には custom client tool が最適。Claude は `tool_use` request を出し（container 識別子などの検証済み引数付き）、アプリケーションが private service 呼び出しを実行して `tool_result` を返す。これで credential, network access, validation, error handling をすべてプロバイダーのバックエンド内に保てる。MCP connector は Messages API から遠隔 MCP server に繋ぐもので、任意の REST service を発見する汎用の仕組みではない。Agent Skill は code execution sandbox 内で動き、backend-only の service lookup には不適。web fetch は Anthropic 側の infra から URL コンテンツを取得する server tool で、private backend bridge ではない。

**参照**: Prep M2 S7「Tool-use and Schema Design」（client tool vs MCP vs server tool）＋ M3 S12「MCP Servers」

---

## Q23（D5: Tools and MCPs）

法律出版チームが、編集者向けの case summary 作成を支援する Claude API 統合を構築する。編集者は、社内 style guide の適用、document template の再利用、必要なときの packaged validation script 実行を求める。workflow に内部サービスや外部 network への live access は不要である。最適な手法はどれか。

- A. guide, templates, scripts を Agent Skill として package し、code execution と一緒に有効にする。 ← 正解
- B. 各編集ルールを custom client tool として定義し、アプリケーションから tool result を返す。
- C. すべての drafting request で server tool 経由で style guide と templates を fetch する。
- D. materials を local STDIO MCP server として Messages API の MCP connector に繋ぐ。

**解説**: どの仕組みを選ぶかは「Claude が callable action, external tool access, それとも再利用可能な task knowledge を必要としているか」で決まる。Agent Skill は instructions, metadata, scripts や templates などの resources を package でき、progressive disclosure で関連時に読み込まれる。stable な editorial workflow で live service call なしに再利用可能な guidance と bundled artifacts を提供するには Skill が自然。Claude API では Skill は code execution と併用し、sandbox 内で materials にアクセス・実行する。client tool は構造化関数の呼び出し向け、MCP connector は遠隔 HTTP server 向けで、いずれも local file の汎用 package 机制ではない（connector は STDIO を直接繋げない）。

**参照**: Prep M3 S8「Packaging Workflows」（Skill + code execution）＋ M2 S7 末尾

---

## Q33（D1: Applications and Integration）

レストラン予約プラットフォームの analytics チームが Messages API を使い、diner feedback に短い社内コードを付ける。downstream parser は、各出力が固定 marker で始まり、続いて数文字の生成コードだけが続き、説明文はないことを期待する。この統合パターンに最も合うのはどれか。

- A. 固定 prefix を top-level system field に置き、Claude に code の前にそれを繰り返させる。
- B. 固定 prefix を earlier user message として追加し、返信の最初の一致する code を parse する。
- C. 最後を partial assistant turn（固定 prefix を含む）で終わらせ、code 向けに小さな `max_tokens` を設定する。 ← 正解
- D. 最新の feedback text だけを送り、API が前の classifier state を保持していることに頼る。

**解説**: Messages API は各リクエストを自己完結した prompt として扱う。対応する model では、最後の入力メッセージを partial assistant message にして Claude にそこから続かせる prefill パターンをサポートする。parser が単一の分類コードのようなコンパクトな続きを期待する場合、これが有用で、小さな `max_tokens` と組み合わせて生成を絞れる。ただし assistant prefill は現行 Claude model で普遍的にサポートされるわけではなく、未サポート model では失敗する点に注意。system prompt や example は振る舞いの guidance であって continuation point ではない。また API は stateless なので、前の classifier state や prefix を暗黙に保持することはない。

**参照**: Prep M2 S2「Prompting Craft」（構造化出力と prefill の非互換性）

---

## Q34（D2: Model Selection and Optimization）

海運物流 startup が Claude 駆動の経路計画アシスタントを運営する。各リクエストには、大きく安定した port operations manual、続いて変わる vessel profile, weather snapshot, user question が含まれる。adaptive thinking を hard な計画要求に使っており、response の可視テキストより請求の output token が多いことに気づいている。cost visibility を改善し、反復入力コストを下げる2つのアクションはどれか。

- A. cache token fields と output thinking token details を可視テキストだけでなく追跡する。 ← 正解
- B. `cache_control` を最後の user block に移し、各 full request を再利用のため checkpoint する。
- C. thinking display を omitted に設定し、可視 response token を課金 output token として扱う。
- D. `cache_control` を、変わる request content の直前の、最後の identically な manual block に置く。 ← 正解

**解説**: cost 管理では「何が入力として再利用されるか」と「何が出力として課金されるか」を分けて扱う。prompt caching は stable prefix の最後の block に checkpoint を置くことで最も効く。operations manual は安定し、vessel/weather/user は可変なので、境界は可変コンテンツの直前に置く。usage 把握では API の usage fields を読む。caching 活性時は `cache_read_input_tokens`, `cache_creation_input_tokens`, `input_tokens` に分かれる。adaptive/extended thinking 使用時は、可視 thinking が省かれても内部 thinking token は課金 output に算入され、`usage.output_tokens_details.thinking_tokens` で見える。thinking display を omitted にするのはレイテンシ最適化であって、thinking の課金をなくすわけではない。

**参照**: Prep M4 S11「Cost & Orchestration」（prompt caching と token 計装）＋ M2 S5「Extended Thinking」

---

## Q41（D5: Tools and MCPs）

博物館アーカイブのデジタル保存チームが Claude アプリ向けの MCP server を構築する。server はアプリに archival records へのアクセスを提供し、再利用可能な curator review templates を提供し、承認された metadata 更新を実行し、遠隔 HTTP client をサポートする必要がある。MCP server 開発のベストプラクティスに合致する2つの実装選択はどれか。

- A. metadata update 操作を tools として model 化し、`tools/list` で公開し、`tools/call` で呼び出す。 ← 正解
- B. 古い HTTP+SSE split を維持する。遠隔 MCP server 向けの現行 transport だから。
- C. records と review templates を tools として公開し、model がすべての取得と template 選択を制御する。
- D. Streamable HTTP endpoint を使い、client message は POST, streaming は GET, さらに web security controls を実装する。 ← 正解

**解説**: MCP server 設計は各機能に正しい primitive を選ぶことから始まる。tools は model-controlled の実行可能関数で、承認された metadata 更新は tool model に合う。`tools/list` で公開し `tools/call` で起動する。一方 archival records は application-controlled な resources, review templates は user-controlled な prompts が適切で、MCP は実行可能 action と resources と prompts を明確に区別する。すべてを tools にすると制御モデルが崩れる。遠隔 HTTP client 向けの現行 transport は Streamable HTTP で、単一 endpoint に POST で client JSON-RPC、GET で任意の SSE stream を送る。古い HTTP+SSE は protocol version 2024-11-05 で置き換えられた。Origin の validation, loopback への安全な bind, 適切な認証も必要。

**参照**: Prep M3 S12「MCP Servers」（tools/resources/prompts と transport）

---

## Q44（D1: Applications and Integration）

航空宇宙ロボティクス研究所が、日常開発向けの default model を設定した共有 Claude Code project configuration を持つ。あるエンジニアが、configuration 変更を commit せず、チームメイトに影響を与えずに、別 model で単一の実験を走らせたい。最適な手法はどれか。

- A. session 中に user model setting を変更し、settings の hot reload に頼る。
- B. 実験用に shared project model を `.claude/settings.json` に設定する。
- C. 実験用の temporary model instruction を `CLAUDE.md` に置く。
- D. その invocation のみ、CLI の model override で実験を始める。 ← 正解

**解説**: one-off な Claude Code 実行には、共有設定の編集ではなく invocation-level の model override を使う。Claude Code の settings は階層的（managed, command-line, local project, shared project, user）で、CLI からの model override は1つの session を default から切り替める狭いユースケース向け。チームメイトの default はそのまま。`CLAUDE.md` は memory/context を読み込む層で、runtime 設定を強制する仕組みではない。shared project settings はチーム全体の persistent default 向けで、個人の実験には不適。また model setting は session 開始時に1回読まれ、`/model` など対話的仕組みを通さない限り hot reload の対象外。再現性のため pinned model ID を使うか alias かも意識する。

**参照**: Prep M3 S2「Permission Modes & Human Gates」（settings 階層と model setting の hot-reload 挙動）

---

## Q50（D7: Security and Safety）

地域住宅管理機関の digital services チームが、住宅給付の case worker 向けに適格性 note を作成し、推奨 summary を申請者に直接見せる可能性のある Claude アシスタントを立ち上げる。user は自由形式メッセージを打て、アプリのルールを迂回しようとする試みも想定される。Anthropic の safety guidance に最も合致する2つのデプロイアクションはどれか。

- A. 強化した system prompt だけを唯一の jailbreak 対策として頼る。
- B. content moderation guide を Claude とのやり取りに対する主制御として適用する。
- C. 軽量 Claude model と structured classification で user input を事前 screening する。 ← 正解
- D. 適格性推奨が確定・配信される前に、適格な人間の review を必須にする。 ← 正解

**解説**: Claude アプリの安全なデプロイは、workflow が high-risk use case に当たるかの特定から始まる。Anthropic の Usage Policy は、住宅、金融、医療、法務、雇用の意思決定など、出力が直接個人に影響する領域を追加保護対象とする。住宅給付 workflow では、確定前の人間の review が重要な制御。guardrail は単一 system prompt ではなく重ねる。Anthropic の jailbreak 対策 guidance は、軽量 model（Claude Haiku 4.5 など）による input 事前 screening, structured output による単純分類, 既知の injection pattern の filter, prompt の hardening, 反復滥用者の throttle や ban を推奨。なお Claude とのやり取りを保護するのは guardrails guidance であって、content moderation guide はアプリ内 user 生成コンテンツのモデレーション向け。

**参照**: Prep M4 S14「Security — Teaching」（layered defense と high-risk use case）

---

## サマリ

| 問題 | ドメイン | トピック | Prep 参照 |
|------|----------|----------|-----------|
| Q5 | D1 Applications & Integration | Skill/prompt の versioning と lifecycle | M5 S2 + M5 S7C |
| Q15 | D2 Model Selection & Optimization | Sonnet 5 の adaptive thinking, Haiku 4.5 routing | M1 S3 + M4 S10A |
| Q20 | D3 Agents & Workflows | `ClaudeSDKClient` vs `query()` | —（Prep では直接未扱い。M2 S16 参照） |
| Q22 | D5 Tools & MCPs | private backend 向け custom client tool | M2 S7 + M3 S12 |
| Q23 | D5 Tools & MCPs | Agent Skill + code execution | M3 S8 + M2 S7 |
| Q33 | D1 Applications & Integration | prefill（partial assistant turn） | M2 S2 |
| Q34 | D2 Model Selection & Optimization | cache_control 配置と thinking token 課金 | M4 S11 + M2 S5 |
| Q41 | D5 Tools & MCPs | MCP primitives（tools/resources/prompts）と Streamable HTTP | M3 S12 |
| Q44 | D1 Applications & Integration | Claude Code の CLI model override | M3 S2 |
| Q50 | D7 Security & Safety | jailbreak 多層防御と high-risk use case | M4 S14 |

### 弱点傾向

- **D5 Tools & MCPs（3問: Q22, Q23, Q41）** が最多。client tool, MCP connector, Agent Skill, server tool の使い分けと、MCP primitives の制御モデルの区別が曖昧。
- **D2 Model Selection & Optimization（2問: Q15, Q34）** が次点。Sonnet 5 の adaptive thinking 移行と、prompt caching の checkpoint 配置・thinking token 課金の理解。
- **D1 Applications & Integration（3問: Q5, Q33, Q44）** は lifecycle/versioning, prefill, Claude Code model override と幅広い。
- **共通パターン**: 「MCP connector は remote HTTP server 専用で STDIO/REST 発見には使えない」「prompt 指示は guidance であって enforcement ではない」「最新 model の thinking 挙動（adaptive + effort）と旧 budget_tokens の非互換性」が繰り返し現れる。
