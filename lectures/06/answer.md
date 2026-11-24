# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：
- STEP 2 の [tool call]：
- STEP 3 の [tool call]：
- STEP 4 の [tool call]：
- 気づき・疑問：

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎

- やったこと：
MCPサーバにツールget_lecture_summaryを公開し、エージェント側に手を加えず質問の際に当該ツールが呼び出されることを確認する。

- 結果：
○　mcp_server.pyに追記後、uv run python -m app.mcp_agent --list-toolsの実行結果

[server] glossary: コマンドで起動（uv run python -m app.mcp_server）
モデルに見えているツール: 2 個

[glossary_search_glossary]
  講座の用語集から用語を検索し、定義と、どの回で扱うかを返す。
  
  AIエンジニアリングの用語や、この講座（AIE2026）に固有の言葉について
  答える前に使うこと。query には調べたい用語を1つだけ入れる（例: "LoRA"）。
  引数: {"query": {"title": "Query", "type": "string"}}

[glossary_get_lecture_summary]
  指定した回（1始まりの整数）の講義の要点を返す。
  
  「第3回では何を学ぶ？」のように、回の内容や関係を聞かれたときに使う。
  引数: {"lecture": {"title": "Lecture", "type": "integer"}}



○ mcp_server.pyに追記後、uv run python -m app.mcp_agent --trace \
"第2回では何を学びますか？"の実行結果

[server] glossary: コマンドで起動（uv run python -m app.mcp_server）
[model] gemini-3.5-flash-lite
[tools] MCP サーバー（app/mcp_servers.json）
第2回では、**「ファインチューニングとLoRA」**をテーマに学びます。

主な内容は以下の通りです：

1. **事前学習とファインチューニング**
   - 事前学習とファインチューニングの違い、およびファインチューニングが向く場面・向かない場面の理解。
2. **SFT（教師ありファインチューニング）**
   - 入力と望ましい出力のペアを用いて学習させる手法と、データの質・形式の重要性。
3. **PEFTとLoRA**
   - 全パラメータを更新するのではなく、小さな低ランク行列だけを学習してGPUメモリや容量を大幅に削減するLoRAの仕組み。
4. **QLoRAとの関係**
   - ベースモデルを4bitに量子化してLoRAを組み合わせる手法との違い。
5. **演習**
   - Google Colab上で小さなモデル（Qwen3.5-0.8B）を使い、架空FAQのデータでLoRA学習させて出力形式の遵守率などを比較・体験する。

（関連する回：第2回）

--- trace ---
[tool call]   glossary_get_lecture_summary({'lecture': 2})
[tool return] glossary_get_lecture_summary -> # 第2回 ファインチューニングとLoRA

## この回のねらい

- 事前学習とファインチューニングの違い、ファインチューニングが向く場面と向かない場面を理解する
- LoRAの考え方を理解し、Google Colab上で小さなモデルを…
[usage] 入力 1,274 tokens / 出力 264 tokens / モデル応答 2 回

- 気づき・疑問：
意図していたとおり、MCPサーバのみに手を加え（エージェント側に手を加えず）、新たなツールglossary_get_lecture_summaryを追加しエージェントに使用させることが出来た。
複数のエージェントを使用するユースケースにおいて、コード追記箇所が減り保守性が高まると思われる。
実務においても、この手法がデファクトスタンダードとして活用されているのだろうか？
