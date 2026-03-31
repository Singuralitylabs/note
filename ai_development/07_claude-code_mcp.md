# Claude CodeにおけるMCP活用

## はじめに

前回の記事では、Claude Codeのサブエージェント機能について解説しました。今回は、その次のステップとして、Claude Codeの実務活用を大きく広げるMCP（Model Context Protocol）を紹介します。  
MCPを使うと、Claude Codeはコード生成だけでなく、外部サービスと連携しながら作業できるようになります。たとえば、ブラウザ操作、ドキュメント参照、GitHub操作、社内API連携などを、同じ作業フローの中で扱えるようになります。

## MCPとは何か

MCP（Model Context Protocol）は、AIモデルと外部ツールをつなぐためのオープンプロトコルです。Claude CodeはMCPを通じて、さまざまなサービスと標準化された方法で連携できます。  
ポイントは「個別連携の寄せ集め」ではなく、「共通ルールで連携できる基盤」が用意されることです。これにより、開発チームとして再利用しやすく、運用しやすい形でAI活用を進められます。

## MCPの3つの柱

MCPを理解するうえで重要なのが、次の3つの柱です。

### Tools（ツール）

Claude Codeが呼び出せる外部機能です。

- データベース問い合わせ
- Slack送信
- GitHub操作
- ブラウザ自動操作

のような実行系タスクを、関数のように扱えます。

### Resources（リソース）

Claude Codeが参照する外部データです。

- APIレスポンス
- 設計資料
- スキーマ情報
- プロジェクトメモ

などをコンテキストとして読み込めるため、より状況に即した判断が可能になります。

### Prompts（プロンプト）

MCPサーバー側で定義されたプロンプトテンプレートです。定型作業の指示をテンプレート化することで、実行品質のばらつきを抑えられます。

## 設定スコープの使い分け

MCPサーバーの設定方法は、大きく2つあります。

- JSONファイルに直接記述する
- Claude Codeのコマンドラインから設定する

### JSONファイルに記述する方法

設定ファイルは用途に応じて使い分けます。

- .mcp.json（プロジェクト単位）
- ~/.claude/settings.json（ユーザー単位）

**.mcp.json** はプロジェクトルートに置き、チームで共有する設定を記述します。Gitで管理しやすく、プロジェクトに必要なMCPサーバーをそろえられます。

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**~/.claude/settings.json** は個人環境向けの設定です。APIキーなどの機密情報は、環境変数参照を使ってここで扱うのが安全です。

設定ファイルを書いたら、**Claude Codeを起動（または再起動）することで読み込まれます**。起動時にファイルを読み込む仕組みのため、設定変更後は再起動が必要です。

### Claude Codeのコマンドラインから設定する方法

JSONファイルを直接編集する以外に、`claude mcp add` コマンドでMCPサーバーを登録することもできます。

```bash
# ユーザー設定に追加する（~/.claude/settings.json に反映される）
claude mcp add playwright npx @playwright/mcp@latest

# 環境変数を指定して追加する
claude mcp add github -e GITHUB_PERSONAL_ACCESS_TOKEN=your_token npx -y @modelcontextprotocol/server-github

# プロジェクト設定に追加する（.mcp.json に反映される）
claude mcp add playwright --scope project npx @playwright/mcp@latest
```

登録後の確認や削除は次のコマンドで行えます。

```bash
# 登録済みサーバーの一覧を確認する
claude mcp list

# サーバーを削除する
claude mcp remove playwright
```

コマンドラインからの設定は、JSONの記法を気にせずに手軽に追加・削除できる点が利点です。チームで統一する設定は `.mcp.json` に書いておき、個人の試用や一時的な追加はコマンドで行う、という使い分けが実用的です。

## /mcpコマンドで状態を確認する

設定が完了したら、Claude Code上で `/mcp` コマンドを使って接続状態を確認できます。

```
/mcp
```

実行すると、現在接続されているMCPサーバーの一覧と、各サーバーが提供するツールが表示されます。

### 表示される主な情報

- サーバー名と接続ステータス（connected / disconnected）
- 各サーバーで利用可能なツールの一覧

### 活用のポイント

設定ファイルを書いた後に `/mcp` を実行することで、サーバーが正しく起動しているかをすぐに確認できます。ツール名が一覧に表示されていれば、Claude Codeからその機能を呼び出せる状態です。接続できていない場合は、コマンド名やパッケージ名の誤りを疑うとよいでしょう。

## 実務での活用イメージ

Claude CodeのMCP活用は、次のような流れで効果を発揮します。

- 要件整理と実装計画を作る
- 必要な外部データをResourcesで読み込む
- 実装・検証・報告をToolsで実行する
- 定型タスクはPromptsで再利用する

たとえば、

- Context7で公式ドキュメントを確認しながら設計
- Playwrightで実ブラウザ検証
- GitHubでPR作成やレビュー補助

のように、複数工程を一つの会話フローでつなげられます。

## 代表的なMCPサーバー

実務で利用頻度が高い例として、次のようなサーバーがあります。

- Playwright（ブラウザ自動化）
- Context7（公式ドキュメント参照）
- Serena（コード理解）
- Sequential（多段階推論）
- GitHub（Issue/PR運用）

これらを目的別に組み合わせることで、調査・実装・検証・共有までの流れを短くできます。

## まとめ

Claude CodeのMCP活用を理解すると、AIを「コード生成ツール」から「外部サービスと連携して仕事を進める実務パートナー」へ引き上げられます。

- MCPはAIと外部サービスをつなぐ共通基盤
- Tools / Resources / Prompts の3本柱で実務を構成できる
- 設定スコープの分離で、共有性と安全性を両立できる

---

## プログラミングイベントのご案内
毎月数回、AIを活用したプログラミングを学べるオンライン講座を開催しております。直接学びたい方はぜひご参加ください。
申し込みフォームは[こちら](https://docs.google.com/forms/d/e/1FAIpQLScCLBSCJvZEl7R15tCDTajcKa7INCTSOKPEXyfIEX69Q_xtEg/viewform)
過去のプログラミングイベントの紹介は[こちら](https://sinlab.future-tech-association.org/school/)

## シンギュラリティ・ラボのご案内
オンラインサロン「シンギュラリティ・ラボ」（通称シンラボ）では、GASも含めたプログラミングをはじめ、さまざまなITスキルやチーム開発について学び、実践する場を準備しております。 初心者から経験者まで、どなたでも参加可能です。
少しでも興味がございましたらお気軽にお越しください。
シンギュラリティ・ラボHPは[こちら](https://sinlab.future-tech-association.org/join/)
お問い合わせ先 sinlab-recruit@future-tech-association.org

## GASアプリ開発サービスのお知らせ
シンギュラリティ・ラボでは、GASを中心としたWebアプリ開発のご相談を受け付けております。
普段の作業のちょっとした自動化から自分やチーム専用のカスタムアプリまで、ぜひお気軽にお問い合わせください。
詳細は[こちら](https://appdev.future-tech-association.org/)
