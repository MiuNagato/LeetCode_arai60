---
name: Create-PR
description: LeetCode arai60リポジトリ用にPRを自動作成するスキル。LeetCode問題URLを引数に渡すと、問題番号・タイトルを取得し、ブランチ作成・テンプレートファイル作成・コミット・push・PR作成までを一気通貫で実行する。
---

# Create-PR スキル

`MiuNagato/LeetCode_arai60` リポジトリ（ローカル: `C:\leetcode\LeetCode_arai60`）向けにPRを自動作成する。

## 入力
- LeetCode問題URL（例: `https://leetcode.com/problems/is-subsequence/description/`）

引数で渡されたURLを使用する。引数が空の場合はユーザーに問い合わせる。

## 実行手順

### 1. 問題情報の取得
WebFetchでLeetCode URLを取得し、問題番号と問題タイトルを抽出する。
- 抽出例: URLから「`392`」と「`Is Subsequence`」を取得
- LeetCodeページの問題ヘッダーは `<番号>. <タイトル>` 形式（例: `392. Is Subsequence`）

### 2. 各種文字列の生成
取得した番号とタイトルから以下を生成する。
- **ファイル名**: `{番号}. {タイトル}.md` （例: `392. Is Subsequence.md`）
- **ブランチ名**: `{番号}.-{タイトルのスペースをハイフンに置換}` （例: `392.-Is-Subsequence`）
- **PRタイトル**: `Create {ファイル名}` （例: `Create 392. Is Subsequence.md`）
- **コミット見出し**: PRタイトルと同じ
- **コミット本文**: LeetCode URL
- **PR本文**: LeetCode URL

### 3. Git操作（作業ディレクトリは `C:\leetcode\LeetCode_arai60`）
以下を順次実行する。
1. `git -C C:\leetcode\LeetCode_arai60 checkout main`
2. `git -C C:\leetcode\LeetCode_arai60 pull origin main`
3. `git -C C:\leetcode\LeetCode_arai60 checkout -b "{ブランチ名}"`
4. `C:\leetcode\LeetCode_arai60\{ファイル名}` をテンプレート内容で作成（下記テンプレート参照）
5. `git -C C:\leetcode\LeetCode_arai60 add "{ファイル名}"`
6. `git -C C:\leetcode\LeetCode_arai60 commit -m "{PRタイトル}" -m "{LeetCode URL}"`
7. `git -C C:\leetcode\LeetCode_arai60 push -u origin "{ブランチ名}"`

ファイル名・ブランチ名には空白やピリオドが含まれるため、必ずダブルクォートで囲むこと。

### 4. PR作成
```
gh pr create --repo MiuNagato/LeetCode_arai60 --base main --head "{ブランチ名}" --title "{PRタイトル}" --body "{LeetCode URL}"
```

### 5. 結果報告
作成されたPRのURLをユーザーに返す。

## ファイルテンプレート

新規作成する `{ファイル名}` の内容は以下を厳密に使用する（参考PR `fuga-98/arai60#54` のフォーマットに準拠）。各Stepの本文は空のまま作成し、後でユーザー自身が編集する。コードブロック（` ```python `）を事前に埋め込んでおくことで、ユーザーが行頭`#`コメントを書いてもMarkdown見出しと干渉しないようにする。

````markdown
# 進め方

Step1 : 3回続けてエラーが出ないように書く。

Step2 : 他の人のPRを参照し、コメントする。ドキュメントを参照する。

Step3 : 問題を解く。

# 実践

## Step1

### 3回連続で再現

```python

```

## Step2

### 同じ問題を解いた人のプルリクを見る

```python

```

## Step3

### 問題を解く

```python

```
````

## 注意事項
- ベースブランチは常に `main`
- 同名ブランチやファイルが既に存在する場合はエラーとして停止し、ユーザーに通知する（強制上書きしない）
- 一連の操作はAuto modeとして自動実行し、途中で確認を求めない
- PR作成成功後、PR URLを明示的に出力する
