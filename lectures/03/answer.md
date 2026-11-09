# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
- 1つの質問での LLM 呼び出し回数（`requests`）：
- 追加したツールと、docstring に書いた説明：

## 宿題

- 取り組んだレベル：基礎（Basic）
- やったこと：`app/agent.py` の INSTRUCTIONS から1行だけ（33行目「用語集や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。」）を削除。変更は1か所のみ。同じ質問 "What are the '3 levels' in the AIE2026 course homework?" を、変更前後で `uv run python -m app.agent ... --trace` で実行して比較した。
- 結果：変更前は search_glossary({'query': '3 levels'}) を1回呼び、用語集に見つからず「『3 levels』という用語は見つかりませんでした」と回答（requests=2, tool_calls=1, 入力700/出力79トークン）。変更後も search_glossary を1回呼んだが引数が {'query': 'levels'} に変わり、「課題の3レベル」の項目が見つかって「基礎・実装・発展」の3つを正しく回答した（requests=2, tool_calls=1, 入力730/出力155トークン）。
- 気づき・疑問：確認の1行を消してもツールは呼ばれた。質問が講座固有であることや docstring の説明で十分だった可能性がある。変わったのは呼び出しの有無ではなく検索語（'3 levels' → 'levels'）で、それが回答の正否を分けた。ただし各1回の実行で、LLM は毎回出力が変わるため、削除した1行が原因とは断定できない。次は同じ条件で複数回実行するか、docstring 側を変えて比較したい。実行と --trace の確認は AI アシスタント（Claude）の補助で行った。
