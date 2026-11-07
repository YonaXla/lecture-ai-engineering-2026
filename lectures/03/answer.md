# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：エージェントは、ツール経由で講座固有の知識を参照できる。素のLLMにはそれがない。
- `--trace` で見えたツール呼び出し（ツール名と引数）：[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
- 1つの質問での LLM 呼び出し回数（`requests`）：2回
- 追加したツールと、docstring に書いた説明：
ツール名：make_quiz
docstringに書いた説明：
  """指定した回の用語から1つ選び、確認問題と答えを返す。

    「第N回の確認問題を出して」のように、復習用の問題を求められたときに使う。
    lecture には回の番号（1始まりの整数）を入れる。
    """
動いたかどうか：[tool call] make_quiz({'lecture': 2}) が出た。
実際にでた問題例は以下の通り
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「エポック」の意味は？", "answer": "学習データ全体を1周学習することを1エポックと数える。回数を増やしすぎると過学習しやすい。"}…

## 宿題

- 取り組んだレベル：基礎
- やったこと：INSTRUCTIONS の「必ず search_glossary で確認する」を削除し、
  「LoRAとQLoRAの違いは？」を --trace 付きで変更前後に実行した
- 結果：INSTRUCTIONS の「必ず search_glossary で確認する」を消しても、
  search_glossary は2回呼ばれ（requests=2）、回答も変わらなかった。
- 気づき・疑問：予想は外れた。指示は INSTRUCTIONS だけでなく、
  ツールの docstring（「答える前に使うこと」）にも書いてあり、
  モデルはどちらも読むためだと考えた。次に docstring も消して確かめる。