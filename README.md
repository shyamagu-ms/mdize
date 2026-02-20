# mdize

Office ファイル（pptx / docx / xlsx）を GitHub Copilot SDK を使ってマークダウンに変換する CLI ツールです。

- 画像が含まれる場合は詳細な説明をマークダウンで出力
- 図形オブジェクトによる図は mermaid 形式で再現
- 複雑な図は複数の mermaid 図に分割して表現

## 出力形式

| ファイル種別 | 出力 |
|---|---|
| `.pptx` | `{ファイル名}_{スライド番号}_mdize.md`（スライドごと） |
| `.docx` | `{ファイル名}_mdize.md`（原則1ファイル） |
| `.xlsx` | `{ファイル名}_{シート番号}_{シート名}_mdize.md`（シートごと） |

## 前提条件

### GitHub Copilot SDK

GitHub Copilot SDK のセットアップが必要です。詳細は公式リポジトリを参照してください。

- [github/copilot-sdk](https://github.com/github/copilot-sdk)

### Python 環境

Python 3.10 以上が必要です。

## セットアップ

```bash
# venv の作成と有効化
python -m venv .venv

# Windows
.\.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate

# 依存パッケージのインストール
pip install -r requirements.in
```

## 使い方

```bash
python main.py
```

実行すると以下の流れで動作します。

1. 変換するファイルのパスを入力（pptx / docx / xlsx）
2. 分割分析モードを選択
   - **N（一括）**: 1 セッションでファイル全体を処理
   - **Y（分割）**: スライド / セクション / シート単位で個別に Copilot セッションを作成し、より詳細に分析
3. Copilot が内容を分析しマークダウンファイルを出力

出力先はソースファイルと同じディレクトリです。

> **⚠️ 注意:** デフォルトの AI モデルは `claude-opus-4.6-fast`（Premium Request）です。分割分析モードではスライド / セクション / シートごとにセッションを作成するため、**大量の Premium Request を消費します**。必要に応じて `main.py` の `MODEL` 変数を変更し、別のモデルをご利用ください。

## 処理モード

| モード | pptx | docx | xlsx |
|---|---|---|---|
| 一括 | 1 セッションで全スライド | 1 セッションで全体 | 1 セッションで全シート |
| 分割 | スライドごとに個別セッション | Heading 1 単位で個別セッション | シートごとに個別セッション |

