# starter-html-and-css

HTML と CSS が学習できる環境を提供するため作成しました。

HTML と CSS の仕様は [MDN Web Docs](https://developer.mozilla.org/ja/) に全て網羅されていますが、情報量が膨大（辞書のようなもの）であるため、初学者が「まず何を覚えるべきか」を判断するのは困難です。

このリポジトリは、**「現場で頻出する機能」** に絞って実際に手を動かしながら学ぶための **スターターキット（実践ガイド）** です。

> **⚠️ 学習上の重要ルール**
> MDN の情報が「最新かつ正確」な一次情報です。
> ここで記載した内容や、もし AI から回答を得た場合であっても、**必ず MDN の記載を確認しながら実装する癖** をつけてください。

---

## 💻 1. 開発環境 (Development Environment)

この勉強会では **GitHub Codespaces** を使用します。

面倒な環境構築は不要です。ブラウザさえあれば、すぐに学習を始められます。

1. **GitHubにログイン** してください（アカウントがない場合は作成してください）。

1. このリポジトリをフォークするため、右上の`fork`をクリックします。

    ![start-fork](./assets/start-fork.png)

1. `Create fork`ボタンをクリックして、フォーク（自分のアカウントにコピーして新しいリポジトリを作成）します。

    ![select-fork-option](./assets/select-fork-option.png)

1. `Codespace`を起動するため、`Code`タブに移動し、右上にある緑色の`code`のプルダウンメニューを開き、`Codespace`タブを開き、`Create codespace on main`をクリックします。

    ![success-setting](./assets/start-code-space.png)

1. `Codespace`の生成にはしばらく時間がかかるため、しばらく待ちます。

    ![create-now](./assets/create-now.png)

1. `VSCode`が起動しますが、画面左下が`リモートを開いています...`の間は待ちます。

    ![vscode-setup-now](./assets/vscode-setup-now.png)

1. 画面左下が`Codespace`になった場合は、`Codespace`が起動完了しました

    ![vscode-setup-finish](./assets/vscode-setup-finish.png)

環境が立ち上がったら、左側のファイル一覧から学習したい章のフォルダを開いてください。

### Codespaces利用上の注意

- `Github`の`Codespaces`を利用します。`Codespaces`は設定によってはコストがかかるものなので [Codespace の利用上の注意](./CODE_SPACES_SERICE.md) はよく確認してください。
- コストをかけないためにも、セキュリティの意味でも、使い終わったら [停止方法](./CODE_SPACES_SERICE.md#3-停止方法) に従って停止することを推奨します。

## 2. 学習の始め方

1. Codespaces を起動
    - [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/t-inoue0214/starter-html-and-css)

1. **環境の準備を待つ**
    - ブラウザでVS Codeが起動します。
    - 初回はJavaのセットアップや日本語化のために1〜2分ほどかかります。
    - 左下のステータスバーなどが落ち着くまで少し待ちましょう。

1. VS Code のターミナルを開く（VSCodeの上部にあるメニューから「ターミナル」＞「新しいターミナル」を選択するか、`Ctrl + @` または `Cmd + J`）
1. 以下のコマンドを実行する

    ```bash
    ./run_web_server.sh
    ```

1. **学習スタート！**
    - 左側のファイル一覧から `01_introduction` フォルダを開きます。
    - `README.md` をクリックして開き、解説を読みながら進めてください。
    - `README.md` を右クリックして「プレビューを開く (Open Preview)」を選ぶと読みやすくなります。

## 3. 学習カリキュラム

| フォルダ | タイトル | 学習内容 |
| -- | -- | -- |
| [01_introduction](./01_introduction/README.md) | イントロダクション | 開発者ツール（DevTools）の使い方や、Web サーバーの仕組み、HTML/CSS の概要を説明します。 |
| [02_html_tags](./02_html_tags/README.md) | HTML のタグの学習 | 文書構造を作るための主要なタグと、ブロックレベル/インライン要素の概念を学習します。 |
| [03_html_attributes](./03_html_attributes/README.md) | HTML の属性の学習 | id と class の使い分けや入力制限などの属性、将来バグらせないための記法を学習します。 |
| [04_html_forms](/04_html_forms/README.md) | HTML フォームの部品（UI） | テキストボックス、ラジオボタン、ラベルの紐付けなど、使いやすい入力フォーム（UI）の作り方を学習します。 |
| [05_css_basics](./05_css_basics/README.md) | CSS の基礎学習 | ボックスモデル（余白の構造）の理解や、実務における「CSS を書く場所」のルールを学習します。 |
| [06_css_layout](/06_css_layout/README.md) | CSS の脱初心者（レイアウト） | Flexbox による横並び配置や、Position による重ね合わせ、実務的なフォームのレイアウト手法を学習します。 |
| [07_css_advanced](./07_css_advanced/README.md) | CSS のちょっと高度な話 | ホバー時の変化やクリック時の沈み込みなど、ユーザー操作に反応する「心地よい動き（マイクロインタラクション）」を学習します。 |

## 4. フォルダ構造

```txt
starter-html-and-css/
├── .devcontainer/
│   └── devcontainer.json
├── .git/
├── 01_introduction/       <-- ここからスタート
│   ├── index.html
│   ├── style.css
│   └── README.md
|   ....
├── README.md              <-- このファイル
└── run_web_server.sh      <-- サーバー起動用スクリプト
```

---

## 💻 5. 開発環境について

この講座は以下の環境で動作するように設定されています（自動構築されます）。

- **OS:** Linux (Debian)
- **Editor:** VS Code Web (日本語化済み)
- **Extensions:**
  - Prettier - Code formatter
  - Japanese Language Pack

---

## 📝 重視する思想

このリポジトリでは、「実際に手を動かしてみる」ことを何より重視しています。

エンジニアの技術は、資料を読むだけで覚えたり、理解したりすることは難しいものです。

例えば、自動車教習所の教本を完璧に暗記したとしても、それだけで実際に車を運転できるようにはなりませんよね？

ハンドルを握り、アクセルを踏むという「実体験」がなければ、運転技術は身につきません。

ソフトウェア技術も同じです。

技術的な仕組みを知ることも大切ですが、実際に実行した経験こそが現場で役立ちます。

読むだけで終わらせず、ぜひご自身の手で実行してみてください。
