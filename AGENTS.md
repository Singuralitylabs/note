# AGENTS.md

This file provides guidance to coding agents when working with code in this repository.

## リポジトリ概要

Web系技術記事を管理するリポジトリ。記事はWebサービス「note」で公開予定。

**対象トピック:**
- GAS（Google Apps Script）の文法・アプリ紹介
- 生成AIを使った開発
- HTML/CSS
- React / Next.js
- その他Web技術全般

## 記事作成ルール

### noteで使用可能なマークダウン記法

**見出し**
- `## 見出し` - 大見出し（h2）
- `### 見出し` - 小見出し（h3）
- h1、h4以降は使用不可（h4以降はh3に変換される）

**テキスト装飾**
- `**太字**` または `__太字__`
- `~~取り消し線~~`

**その他**
- `> ` - 引用
- `---` - 区切り線（必ず3文字で入力）。ただし記事本文中のセクション間には使用しないこと。区切り線は末尾の「Web技術体験会のご案内」セクションの直前にのみ使用する
- `[リンクテキスト](URL)` - リンク
- ` ``` ` - コードブロック（言語指定でシンタックスハイライト可能）

### noteで使用不可の記法

以下の記法は使用しないこと:

- テーブル（表形式）
- インラインコード（`` `code` ``）
- 画像埋め込み（`![alt](url)`形式）
- 番号付きリスト（`1. `形式）
- 箇条書きリスト（`- `形式）のネスト

### コードブロックの書き方

言語を指定してシンタックスハイライトを有効にする:

```javascript
// このように言語名を指定する
const example = "code";
```

対応言語例: javascript, python, html, css, typescript など

## 必ず入れるメッセージ

下記のメッセージは、すべての記事の末尾に必ず追加すること

```
---

## Web技術体験会のご案内
毎月数回、生成AIやプログラミング、その他様々なWeb技術を学べるオンライン講座を開催しております。直接学びたい方はぜひご参加ください。
申し込みフォームは[こちら](https://docs.google.com/forms/d/e/1FAIpQLScCLBSCJvZEl7R15tCDTajcKa7INCTSOKPEXyfIEX69Q_xtEg/viewform)
過去のWeb技術体験会の紹介は[こちら](https://sinlab.future-tech-association.org/school/)

## シンギュラリティ・ラボのご案内
オンラインサロン「シンギュラリティ・ラボ」（通称シンラボ）では、AI駆動開発やプログラミング等のWeb技術やチーム開発について学び、実践する場を準備しております。 初心者から経験者まで、どなたでも参加可能です。
少しでも興味がございましたらお気軽にお越しください。
シンギュラリティ・ラボHPは[こちら](https://sinlab.future-tech-association.org/join/)
お問い合わせ先 sinlab-recruit@future-tech-association.org

## AIでWeb技術を学ぶサービス「Sinlab Study」のご案内
読むだけで終わらせず、手を動かして身につけたい方へ。「Sinlab Study」では、動画で学び、演習を解き、AIレビューで確認するところまで一続きで学べます（ナビゲーターはシンラビ）。詳細は[解説サイト](https://sinlab.future-tech-association.org/sinlab-study)を、お試しは[こちら](https://study.future-tech-association.org)をご参照ください。
```