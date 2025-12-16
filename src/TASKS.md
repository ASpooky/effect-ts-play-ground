# Effect TS Learning Roadmap 🗺️

geminiに出力させた学習課題
Effect TSを習得するための、難易度別タスクリストです。

## Level 1: 基本とエラーハンドリング 🛡️
**タスク名: 「安全な割り算電卓」**

* [ ] **Goal**: 2つの数値を受け取り割り算を行う。0で割った場合は例外（throw）せず、型安全なエラーとして処理する。
* **Concepts**:
    * `Effect.succeed` / `Effect.fail`
    * `Effect.runSync`
    * `Effect.catchTag` (エラーの分岐処理)
* **Specs**:
    1.  関数 `divide(a: number, b: number)` を作成する。
    2.  `b === 0` の場合、`DivideByZeroError` (_tag付きオブジェクト) で失敗させる。
    3.  実行時にエラーをキャッチし、コンソールに「計算できませんでした」と表示する。

---

## Level 2: Effect.gen とパイプライン ⛓️
**タスク名: 「ユーザー設定ローダー」**

* [ ] **Goal**: `async/await` 風の構文（`Effect.gen`）を使い、複数の依存する処理を直列に実行する。
* **Concepts**:
    * `Effect.gen` (ジェネレータ構文)
    * `yield*` (ステップ実行)
    * `Effect.log`
* **Specs**:
    1.  `parseConfig(json)`: JSONパース（失敗の可能性あり）。
    2.  `validateConfig(config)`: 必須キーの確認。
    3.  `dbConnection(config)`: DB接続（モック、ログ出力のみ）。
    4.  これらを `Effect.gen` でつなぎ合わせ、一連のフローとして実行する。

---

## Level 3: 依存性の注入 (Dependency Injection) 💉
**タスク名: 「挨拶サービス（多言語対応）」**

* [ ] **Goal**: インターフェース（Tag）を定義し、実行時に実装（Layer）を注入する。
* **Concepts**:
    * `Context.Tag` (識別子)
    * `Layer` (実装のコンテナ)
    * `Effect.provide` (依存解決)
* **Specs**:
    1.  `GreetingService` タグを作成（メソッド: `sayHello`）。
    2.  `EnglishService` レイヤーを作成 ("Hello!").
    3.  `JapaneseService` レイヤーを作成 ("こんにちは！").
    4.  メインロジックを変更せず、Layerを差し替えるだけで出力言語を変える。

---

## Level 4: 並行処理 (Concurrency) 🏎️
**タスク名: 「最速データ取得ダービー」**

* [ ] **Goal**: 複数の処理を同時に走らせ、結果の制御を行う。
* **Concepts**:
    * `Effect.delay` (遅延)
    * `Effect.race` (早いもの勝ち)
    * `Effect.all` (並列実行)
* **Specs**:
    1.  `ServerA` (200ms), `ServerB` (500ms) の遅延処理を定義。
    2.  `Effect.race` を使い、早い方の結果だけを取得する。
    3.  `Effect.all` を使い、両方の完了を待って配列で受け取る。

---

## Level 5: レジリエンス (回復力) 🔄
**タスク名: 「気まぐれなAPIクライアント」**

* [ ] **Goal**: 不安定な処理に対してリトライポリシーを適用する。
* **Concepts**:
    * `Effect.retry`
    * `Schedule` (exponential backoff, fixed spacing)
* **Specs**:
    1.  ランダムで80%失敗する `flakyApiCall` を作成。
    2.  `Effect.retry` を適用し、成功するまで（または最大回数まで）自動再試行させる。
    3.  リトライ間隔や回数を `Schedule` で調整する。

---

## 👑 level 6 (応用課題) ⚔️
**タスク名: 「CLI ファイル処理ツール」**

* [ ] **Goal**: `@effect/platform-node` 等を使い、Node.js標準モジュールをラップした環境でCLIツールを作る。
* **Specs**:
    1.  コマンドライン引数でファイルパスを受け取る。
    2.  ファイルを読み込む。
    3.  行数をカウントする。
    4.  結果を標準出力に出す。
    5.  ファイル不在エラー等を適切にハンドリングする。