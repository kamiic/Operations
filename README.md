# Operations リポジトリ 構築手順

このリポジトリは、上市星也（Ema）専用の汎用業務エージェント基盤。
「汎用業務エージェント設計書_v2.md」のOperations設計をベースにした、人力初期版。

## ローカルでのセットアップ

```bash
mkdir -p ~/Documents/agent/Operations/{rules,tools,outputs}
cd ~/Documents/agent/Operations
git init
```

このチャットで作成した以下のファイルを上記フォルダに配置：

```
Operations/
├── CLAUDE.md                     ← Claude Codeが自動読み込み
├── rules/
│   ├── MASTER_RULES.md           ← 全業務共通の再発防止ルール
│   └── inquiry_response.md       ← 汎用問い合わせ対応の業務ルール
├── tools/                        ← 今後 check_agent.py 等を配置
└── outputs/                      ← 成果物の出力先
```

## GitHubへのpush

```bash
git add .
git commit -m "Initial commit: CLAUDE.md and MASTER_RULES.md"
git remote add origin https://github.com/[あなたのGitHubアカウント]/Operations.git
git branch -M main
git push -u origin main
```

※ rootteamgit organization配下に作る場合は、澤居さんに権限付与を確認してから
  リポジトリを作成すること（設計書の「MCP・権限は中央集権管理」の方針に合わせる）。

## 今後の運用フロー

1. **ミスが起きたら、このチャット or Claude Codeで「これはミスだった」と伝える**
2. Claudeが `MASTER_RULES.md` または該当業務ファイルに新しいルールとして追記する
3. 追記内容を確認してからコミット・push
4. 次回以降、Claude Codeはこのリポジトリをcloneして `CLAUDE.md` を起点に
   ルールを読み込んでから作業する

## このチャットだけで使い続ける場合

GitHubに上げず、このチャット内で「ルール集」として運用することも可能。
その場合は、ミスを指摘するたびに以下のように伝えると、ファイルを更新できる：

> 「さっきの〇〇はミスだったから、MASTER_RULES.mdに追記して」

## 次のステップの選択肢

- `tools/check_agent.py` を実際に書く（設計書のauto_fix_add_detectionロジック）
- 問い合わせ対応の「過去のミス事例」を実際に1件ずつヒアリングして
  `inquiry_response.md` に蓄積していく
- GitHubリポジトリを実際に作成し、Claude Codeから読み込めるか動作確認する
