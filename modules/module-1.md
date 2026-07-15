# Module 1: MSO Foundations

> **訳出元:** Anthropic Partner Academy「Claude Certified Developer - Foundations Prep Course」Module 1
> **原典:** https://anthropic-partners.skilljar.com/path/claude-certified-developer-foundations/mso-foundations
> **構成:** 9画面・6セクション・59分・2チェックポイント
> **翻訳方針:** 本文は自然な技術書の日本語。図・画面遷移・ボタン・コード・API名・設定値は機能を維持して英語のまま。

---

## Screen 1 — Orientation（オリエンテーション）· 2 min

### このモジュールを終えるとできること

Claude に向けてコードを書き始める前に、まず言葉の意味を知っておくと役立つ。

このモジュールは、Developer コースの残りが「すでに身についている」と前提する、モデルの基礎と技術的な土台を導入する。

このモジュールを終えると、次のことができるようになる。

1. **トークン**（token）とは何か、**コンテキストウィンドウ**（context window）が固定の予算としてどう働くか、**サンプリング**（sampling）がなぜ出力を変えるのか、そして**非決定性**（non-determinism）がテストや eval で何を意味するのかを説明する。
2. Claude のモデルファミリーと能力階層を説明し、モデルの選択と **extended thinking** のような推論モード（reasoning mode）の有効化を区別する。
3. zero-shot、one-shot、multi-shot のプロンプトモードから選び、例を追加することのコストと品質のトレードオフを評価する。
4. 開発者が Claude にどうアクセスするかを説明する。SDK か生の REST か、同期かストリーミングか、大量処理向けの非同期パターン。

> **DISCLAIMER / NOTICE FOR EDUCATIONAL CONTENT**
>
> We built this Developer course Module 1: MSO Foundations to help you get real work done with Claude. Treat it as educational content. It doesn't constitute legal, financial, or other professional advice, so adapt what you learn to your own situation. Our products and services evolve quickly, so certain content may contain errors or be outdated; remember to verify on Anthropic's website or docs. Examples and scenarios used in the course are illustrative and often fictitious. If the course material mentions a company or product, it doesn't mean Anthropic endorses them, they endorse Anthropic, or that we're affiliated. Also note your use of Anthropic products and services is covered by our terms, policies and documentation; if anything in this course conflicts with them, they control.

---

## Screen 2 — Teaching: How LLMs Behave（LLM の振る舞い）· 12 min

**取り上げる主題:** トークン、コンテキスト、サンプリング、非決定性

**タブ:** `Tokens` | `Context Window` | `Sampling` | `Non-determinism`

### Tokens: the unit of input, output, and cost（トークン：入力・出力・コストの単位）

Claude は文字や単語を直接は読まない。トークンを読む。トークンあたりの平均文字数は対象モデルのトークナイザに依存し、モデルの世代で異なる。文字数／トークンの目安はモデル依存と考え、ビルド時に現在のトークナイザの振る舞いを確認すること。

モデルが処理するものはすべてトークンでカウントされる。プロンプト、会話履歴、ツール定義、ツール結果、そしてモデルが生成する応答。トークンは価格と予算の両方の単位なので、ある機能のコストを見積もるときも、入力が収まるかを判断するときも、単語ではなくトークンを数える。API の課金とコンテキストウィンドウの計測がトークン単位だから、トークンで考える習慣が役立つ。

### The context window: a fixed budget（コンテキストウィンドウ：固定の予算）

コンテキストウィンドウは、1回のリクエストでモデルが取り込めるトークンの総量である。システムプロンプト、それまでの会話全体、注入するドキュメント、すべてのツール結果、モデル出力を一度に保持する。固定の予算であり、2つの異なる境界挙動がある。

入力がすでにウィンドウより大きいリクエストは、生成が始まる前にバリデーションエラーで拒否される。一方、入力時には収まるリクエストでも、生成の途中で上限に達することがある。現在のモデルはそのとき停止し、エラーを送出する代わりに、それまでに生成した出力を `model_context_window_exceeded` という stop reason とともに返す。

どちらの場合も、長いセッションを維持するには、アプリケーションが呼び出しごとに履歴を切り詰めるか要約する必要がある。開発時はテスト入力が短いので、ウィンドウが埋まることはまれである。プロダクションでは入力が長くターン数が増えるため、ウィンドウが早く埋まる。この失敗について Module 2 で詳しく扱う。

### Sampling: why the same prompt can give different answers（サンプリング：同じプロンプトで違う答えが返る理由）

言語モデルは、次のトークンを1つに固定して選ぶわけではない。各ステップで、取りうる次トークンの確率分布を生成し、そこからサンプリングする。**temperature** のような設定がその分布を形作る。温度を低くすると確率が最も高いトークンに集中し、出力が再現しやすくなる。温度を高くすると分布が広がり、出力が多様になる。

選択が固定ではなくサンプリングで決まるため、同じプロンプトを2回実行すると、両方の答えが正しくても違う表現が返ることがある。これはモデルの生成方法の性質である。

サンプリングの制御はモデル依存であることに注意したい。最新の Claude モデルは、デフォルト以外のサンプリングパラメータを受け付けない。`temperature`、`top_p`、`top_k` を設定すると 400 エラーが返り、それらのモデルではプロンプトで振る舞いを制御する。`temperature` を受け付けるモデルでも、`temperature` 0 は出力を再現しやすくするだけで、呼び出し間で同一の出力を保証しない。ビルド時に API リファレンスで現在のパラメータ対応状況を確認すること。

### Non-determinism: what it means for testing and evals（非決定性：テストと eval が受ける影響）

非決定性はサンプリングの主な帰結である。同じ入力でも同じ出力を保証しない。これが Claude の機能のテスト方法を変える。

応答の厳密なテキストを assert するテストは不安定になる。モデルは同じ正しい答えを複数の言い回しで表現できるからだ。代わりに、成り立つべき性質を assert する。必須フィールドが存在する、値が範囲内にある、構造がパースできる、といったことである。構造ではなく意味を判断したいときは、モデル採点のジャッジ（model-graded judge）を使う eval を使う。

だから本コースは eval を「機能が正しいことを知る」ための標準とし、Module 3 でその能力を構築する。

---

## Screen 3 — Teaching: Models & Reasoning（モデルと推論）· 10 min

**取り上げる主題:** モデルの選択肢と推論モード

**タブ:** `The Model Family` | `Reasoning Modes` | `How They Work Together`

### The Claude model family（Claude のモデルファミリー）

Claude は現在4つの階層にまたがるモデルファミリーである。**Fable**、**Opus**、**Sonnet**、**Haiku**。各モデルは、コスト・レイテンシ・能力の異なるトレードオフを表す。

Sonnet は、ほとんどのプロダクションワークロード向けのバランスの取れたデフォルトである。Haiku は、自身の能力範囲に収まるタスク向けに、スピードとコスト効率を作り込んだ。Opus は Sonnet の範囲を超える要求の高い作業を扱う。Fable は最も能力の高い階層で、最大の知性が優先される、最も要求の高い推論・コーディング・エージェント作業向けである。

実用的なデフォルトは、Sonnet から始めること。eval が現在の階層では品質基準に届かないことを示したときだけ1階層上げ、eval が品質の低下を許容できることを示したときだけ Haiku に下げる。Claude ファミリーは進化しているため、ビルド時に platform.claude.com/docs で現在のモデルラインナップと識別子を確認すること。

### Reasoning modes are a separate setting from model choice（推論モードはモデル選択とは別の設定）

どのモデルを実行するかは1つの決定である。モデルが答える前に推論するかどうかは、呼び出しごとに決める別の決定である。

現在のモデルでは、推論モードは **adaptive thinking** である。モデルがいつ・どれだけ考えるかを決め、固定のトークン予算ではなく **effort** 設定で深さを調整する（古い `budget_tokens` 制御は非推奨で、最新のモデル世代では 400 エラーを返す）。thinking の内容は、最新モデルではデフォルトで応答から省かれる。表示が必要なときは、要約された表示をリクエストする。推論は難しく複数ステップの問題でコストに見合い、参照や分類では無駄になる。

このモジュールの要点は、2つのレバーが組み合わさるということである。モデル選択がファミリーのメンバーを選び、推論モードはリクエストごとに設定する。モデルごとのデフォルトは異なる（最新モデルの一部は、デフォルトで、あるいは常に適応的に考える）。ビルド時に、対象モデルの現在の thinking デフォルトを確認すること。

### How the two work together（2つの設定をどう組み合わせるか）

モデル選択と推論モードは独立なので、それぞれ別々に設定できる。推論をオフにした能力の高いモデルは速く直接的である。推論をオンにした小さなモデルは、考えるために多くのトークンを消費する。最も要求の大きいタスクは、能力の高いモデルとより高い effort 設定を組み合わせる。

Module 2 は推論を有効にし、それが返す thinking ブロックを扱う方法を教える。コスト・レイテンシ・品質と天秤にかけてどのモデルを実行するかは、Module 4 で扱う。

---

## Screen 4 — Teaching: Prompting Modes（プロンプトモード）· 8 min

**取り上げる主題:** zero-shot、one-shot、multi-shot

**タブ:** `The Three Modes` | `Cost & Quality Trade-off` | `Mode & Model Choice`

### The three modes（3つのモード）

プロンプトの言葉選びとは別に、プロンプトの中にいくつ実例を与えるかがある。

**zero-shot** は指示を与え、例を与えない。タスクを記述し、結果を求める。**one-shot** は、入力と望ましい出力のペアの例を1つ加える。**multi-shot**（few-shot とも呼ぶ）は、そのような例を複数含む。これらの例は訓練データではない。プロンプトの中に置かれ、求める答えの正確な形をモデルに示す。記述だけでは、その形は決まらないことが多い。

### The cost and quality trade-off（コストと品質のトレードオフ）

追加する例は、それぞれ毎回の呼び出しでトークンを消費し、コンテキスト予算を食う。つまり品質とコストをトレードオフする選択である。

タスクが単純で出力の形が明らかなら、zero-shot で済む。出力に、記述では拾いきれない特定の構造・大文字小文字の区別・エッジケースがあるとき、one-shot や multi-shot に移る。多くの場合、正しい例を1つか2つ追加するだけで、指示をもう1段落足すより早く問題が解決する。Module 2 が強化する一般的な規律は、信頼できる結果を生む最小限のプロンプトを追加することである。

### Mode choice interacts with model choice（モード選択とモデル選択の相互作用）

プロンプトモードとモデル選択は、関連するレバーである。より能力の高いモデルは、小さなモデルが構造を合わせるために例を必要とするようなタスクで、zero-shot で成功することが多い。つまり例を追加することで、より安いモデルに仕事を任せられる。

2つの決定は一緒に検討する価値がある。eval を満たす、最もシンプルなモデルと最少の例を試し、eval が必要だと言うところだけ、能力や例を追加する。

---

## Screen 5 — Teaching: Technical Substrate（技術的土台）· 12 min

**取り上げる主題:** SDK、REST、ストリーミング、非同期

**タブ:** `SDK vs. REST` | `Sync, Streaming & Real-time` | `Async for High-Volume Work`

### How a developer reaches Claude: SDK versus raw REST（開発者が Claude に届く経路：SDK か生の REST か）

Claude は本質的に HTTP REST API 経由でアクセスする。コードは、API キーと JSON ボディを伴ったリクエストをエンドポイントに送り、JSON 応答を読み取る。そのエンドポイントは任意の HTTP クライアントで直接呼べる。

より一般的なのは公式 SDK（Python や TypeScript 向けなど）を使うことである。SDK は同じ REST API の上の薄い便利レイヤーで、認証・リクエスト構築・リトライ・応答パースを処理するため、定型文を書かずに済む。SDK と生の REST は同じ API・同じモデルに届く。SDK はリクエントの手組みを省く。Module 2 は、この同じ土台の上にある SDK と Messages API に対して構築する。

### Synchronous, streaming, and real-time responses（同期・ストリーミング・リアルタイムの応答）

同期リクエスト（synchronous）は最もシンプルなパターンである。リクエストを送り、完全な応答が1つのまとまりで戻ってくるのを待ってから処理する。これは、短い応答や、誰も待っていないバックエンドジョブに適する。

応答が長い、あるいはユーザーが見ている場合、ストリーミングはモデルが生成するのに応じて応答を小片で送る。出力は、画面が空白のまま待たされた後ではなく、すぐに現れる。コードは小片を最終メッセージに再組み立てする。Claude は Server-Sent Events を使い、同じ HTTP 接続上でストリーミングを公開する。Module 2 は、ストリームを安全に消費し、中断時に復旧する方法を教える。

### Asynchronous patterns for high-volume work（大量処理向けの非同期パターン）

大量処理に対応するパターンは2つあり、異なる問題を解く。

Python SDK は非同期クライアント（`AsyncAnthropic`）を公開し、ノンブロッキングの async/await で、アプリケーションのスレッドを占有せずに API 呼び出しをする。TypeScript SDK では、標準の Anthropic クライアントが Promise ベースなので、呼び出しを直接 await する。非同期クライアントクラスは別にない。いずれにしてもリクエストはリアルタイムで戻るが、待っているあいだアプリケーションは他の処理を扱える。ブロックせずに並行性が必要なときに適するパターンである。

Message Batches API は、バルクのオフラインワークロード向けのもう1つのパターンである。大量のリクエストを1回の呼び出しで送信し、識別子を受け取り、完了をポーリングする。バッチジョブは完了まで最大24時間かかり、そのレイテンシと引き換えにトークン単価が下がる。ユーザーが各結果を待っておらず、ターンアラウンドよりコストが重要な、オフラインパイプライン・評価実行・バルクジョブに適する。

---

## Screen 6 — Module 1 Quiz（モジュール1のクイズ）· 5 min

> Try it now. Here are some multiple-choice questions to test your understanding of the course so far.

**ボタン:** `Submit quiz` | `Skip for now`

**Question 1.** チームメートが「同一のプロンプト2つは、同一のテキストを返さなければならない」と言う。最も正確な応答はどれか。
- A. そのとおり。モデルは決定論的（deterministic）である。
- B. 必ずしもそうではない。モデルは次トークンを確率分布からサンプリングするため、両方の答えが正しくても表現が変わることがある。
- C. ストリーミングをオフにした場合のみ成り立つ。
- D. 最も大きいモデルの場合のみ成り立つ。

**Question 2.** モデル選択と推論モードを最もよく区別する記述はどれか。
- A. 同じ設定である。
- B. extended thinking は別のモデルである。
- C. モデル選択はファミリーのどのメンバーを実行するかを選び、extended thinking は、対応する任意のモデルがオン・オフで実行できる呼び出しごとの設定である。
- D. 推論モードはアカウントごとに固定である。

**Question 3.** 短く仕様が明確な分類タスクが、zero-shot で正しい答えを返す。例を3つ追加すると、最も可能性が高い結果はどれか。
- A. 精度が大幅に向上する。
- B. 毎回の呼び出しでトークンコストを追加する割に、得るものはほぼ、あるいは全くない。
- C. 使われるモデルが変わる。
- D. サンプリングが無効になる。

**Question 4.** 何千もの入力を、最低コストでオフライン処理しなければならない。適する形態はどれか。
- A. ループでの同期呼び出し。
- B. ストリーミング。
- C. ポーリングを伴うバッチ送信。
- D. より大きいコンテキストウィンドウ。

> **訳注:** 正解は原文の `Submit quiz` で採点される。この和訳では選択肢構造を維持し、正解は明示しない。

---

## Screen 7 — Exercise: Predict the Behavior（演習：振る舞いを予測する）· 6 min

> Each scenario below presents a configuration drawn from one of this module's four foundations, sampling, prompting mode, request shape, and the context budget. For each one, select the answer that predicts the correct behavior and identifies the reason why. Partial credit is available when you answer three of four correctly.

**ボタン:** `Submit` | `Skip for now`

**Scenario 1.** 分類タスクを `temperature` 0 で実行した場合と、同じタスクを高い `temperature` で実行した場合を考える。繰り返し実行したとき、出力がどう違うかを予測せよ。
- A. `temperature` が低いと、モデルは確率を最も高いトークンに集中させる。そのため繰り返し実行しても同じラベルがはるかに一貫して返る。ただし `temperature` 0 でも決定性は保証されない。`temperature` が高いと分布が広がり、表現ばかりか選ばれるラベルまで変わることがある。分類器には、低い温度の再現しやすい振る舞いが望ましい。
- B. どちらの設定でも毎回同一の出力が返る。`temperature` は選ばれるトークンではなく、応答の長さにだけ影響するからである。
- C. `temperature` が高いほうが精度が高い。分布を広げることで、モデルはより多くの正しい答えを考慮できるからである。
- D. 分類タスクに `temperature` は影響しない。分類はサンプリングに関わらず常に固定ラベルを返すからである。

**Scenario 2.** zero-shot プロンプトで構造が誤った出力が返り続けるタスクを考える。multi-shot に切り替えると何が変わるかを予測せよ。
- A. multi-shot への切り替えは、新しい構造でモデルを再訓練する。例を送れば、以後のすべての呼び出しでその変更が永続する。
- B. 正しい入出力の例を2つか3つ追加すると、モデルに合わせるべき正確な構造を示せる。多くの場合、これで指示文を足しても解決しなかった構造の問題が直る。コストは毎回の呼び出しで余分にトークンがかかることなので、出力を信頼できるようにする最少の例を追加する。
- C. multi-shot は構造の問題に効かない。出力の形を変えるには `temperature` を上げるしかない。
- D. multi-shot は呼び出しあたりのトークンコストを下げる。例のおかげでモデルはより短い応答を生成できるからである。

**Scenario 3.** ユーザーを待たせずに、1晩で50,000件のドキュメントを処理しなければならないパイプラインを考える。どのリクエスト形態が適するか、そしてその理由を予測せよ。
- A. 同期のループが最も適する。ドキュメントごとに1回 API を呼ぶのが最もシンプルなパターンで、バッチ送信のオーバーヘッドを避けられるからである。
- B. ストリーミングが最も適する。応答を小片で送ることで、パイプラインが各ドキュメントの処理を早く始められるからである。
- C. バッチパターンが適する。リクエストをバッチで送信し、完了をポーリングする。レイテンシが長くなることを受け入れて、トークン単価を下げる。同期ループはレートリミットに当たり、アプリケーションを占有する。ストリーミングは、誰も見ていないので何の得もない。
- D. より大きいコンテキストウィンドウが最も適する。50,000件すべてを1リクエストに収めれば、繰り返し呼ばずに済むからである。

**Scenario 4.** 長いマルチターンのエージェントセッションで、コンテキストウィンドウが埋まり続ける場合を考える。症状を予測し、該当する予算の名前を答えよ。
- A. モデルは、場所を作るために最も古いターンを黙って捨てる。セッションは続くが、エラーなしに早期のコンテキストを静かに失う。
- B. コンテキストウィンドウは固定のトークン予算である。履歴とツール結果が蓄積するにつれて埋まる。すでに大きすぎる入力は、生成前にエラーで拒否される。入力時には収まるが生成中に上限に達したリクエストは、切り詰められた出力と `model_context_window_exceeded` という stop reason で戻る。症状は、テストでは問題なく動いたセッションが、入力が大きくなると失敗することである。だからアプリケーションが履歴を切り詰めるか要約する。
- C. 症状はサンプリングの遅延である。該当する予算は `temperature` 設定で、セッションが長くなるにつれて下げなければならない。
- D. 固定の予算はない。ウィンドウは蓄積する履歴をすべて保持するため自動的に広がり、長いセッションがこの理由で失敗することはない。

---

## Screen 8 — Recap: Five Takeaways（ふりかえり：5つの要点）· 2 min

1. **トークンは入力・出力・コストの単位である。** 単語ではなくトークンで考え、予算を立てる。API が計量し、コンテキストウィンドウが測るのはトークンだからである。
2. **コンテキストウィンドウは、リクエスト全体を一度に保持する固定のトークン予算である。** 大きすぎる入力は生成前にエラーになり、生成中に上限に達すると `model_context_window_exceeded` という stop reason で切り詰められた出力が返る。だから履歴の管理はアプリケーションの仕事である。
3. **サンプリングは生成を非決定的にする。** 同じプロンプトでも、実行のたびに違う表現が返りうる。だから厳密なテキストでのテストは当てにならない。これが eval のためにある仕組みである。
4. **モデル選択と推論モードは、別々に組み合わせられるレバーである。** eval を満たす、最も小さなモデルと最もシンプルな推論・プロンプトを選び、eval が必要だと言うところだけ能力を足す。
5. **開発者は REST API 経由で、ふつう SDK を通じて Claude に届く。** 同期・ストリーミング・async/await・バッチのうちどれを選ぶかは、ユーザーが待っているかどうかと、ワークロードがリアルタイムかバルクのオフラインかで決まる。

**次に扱うこと:** Module 2 は、この土台をプロンプトの工夫・ツールスキーマ・ストリーミング・コンテキストエンジニアリング・エージェント構築に応用する。

**出典:** Claude 101（Skilljar）、Building with the Claude API（Skilljar）、AI Fluency: Framework & Foundations（Skilljar）、platform.claude.com/docs。公開時点で製品の詳細を確認すること。

ここまでで、Developer コースの共通語彙を話せる。トークン・コンテキスト・サンプリング・モデル階層・プロンプトモード・API の転送の仕組みに名前がついた。残りのコースは、この土台の上に直接積み上げる。

---

## Screen 9 — Module Complete（モジュール完了）· 2 min

> Congrats! You've successfully completed this module.

トークン、コンテキストウィンドウ、サンプリング、非決定性を説明し、モデル選択と推論モードを区別し、目的に合うプロンプトモードを選び、開発者が SDK・REST・ストリーミング・非同期パターン経由で Claude にどうアクセスするかを記述できるようになった。これらの土台は、Developer コースの残りが積み上がる共通語彙である。

**0 of 2 checkpoints passed**

**ボタン:** `Review module` | `Start over` | `Start Module 2 →` | `Return to course home`

### Developer Path の各モジュール

- **M1 — MSO Foundations（You Are Here）:** トークン、コンテキスト、サンプリング、モデル階層、プロンプトモード、技術的土台。
- **M2 — Production-Grade Prompting, Agents & Tool-use（Up Next）:** プロンプトの工夫、extended thinking、ツールスキーマ、ストリーミング、コンテキストエンジニアリング、エージェント構築。
- **M3 — Claude Code, MCP & Integration:** パーミッションモード、永続的なプロジェクトコンテキスト、プラグインのパッケージ化、クレデンシャルを漏らさない MCP 統合。
- **M4 — Production Engineering, Evals, and Security:** eval、トレーシング、失敗処理、コストとオーケストレーションの予算、プロダクションで保つセキュリティ境界。
- **M5 — Accelerators and IP Contribution:** アクセラレータのパッケージ化、検証可能なコントリビューションの準備、デプロイ先プラットフォームの選択、信頼境界の明示。

*Module 1 complete.*
