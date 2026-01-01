# starter-html-and-css

HTML と CSS が学習できる環境を提供するため作成しました。

HTML と CSS なら [MDN(Mozilla Developer Network)](https://developer.mozilla.org/ja/docs) を頭からケツまで読み込めばいいだけですが、初学者からすると、どこを読めばいいのか分かりづらい可能性があるため、ある程度抜粋したものになります。

MDN の情報が最新情報であり、正確なのでここで記載した内容、および、もし AI からの解答があろうとも MDN の記載を常に確認しながら実装するようにしてください。

## クイックスタート

この勉強会では **GitHub Codespaces** を使用します。

面倒な環境構築は不要です。ブラウザさえあれば、すぐに学習を始められます。

1. Webブラウザで、このリポジトリの **[ <> Code ]** タブを開き、右上の緑色の **[ <> Code ]** ボタンをクリックします。

   ![github-code-pulldown](/assets/img/github-code-pulldown.png)

1. **[ Codespaces ]** タブを選択します。
1. **[ Create codespace on main ]** ボタンをクリックします。

   ![github-code-start](/assets/img/github-code-start.png)

環境が立ち上がったら、まずは以下の手順で Web サーバーを起動してください。

1. VS Code のターミナルを開く（`Ctrl + @` または `Cmd + J`）
1. 以下のコマンドを実行する

   ```bash
   ./run_web_server.sh
   ```

1. 右下に通知が出たら「ブラウザで開く」をクリックする

## 利用上の注意

- `GitHub Codespaces`は設定によってはコストがかかるものなので [GitHub Codespaces の利用上の注意](./CODE_SPACES_SERICE.md) はよく確認してください。
- コストをかけないためにも、セキュリティの意味でも、使い終わったら [停止方法](./CODE_SPACES_SERICE.md#3-停止方法) に従って停止することを推奨します。

## 学習カリキュラム

以下の順序で学習を進めます。 まずは `01_introduction` フォルダを開き、中の README.md を読んでください。

- `01_introduction` **イントロダクション**:

  このリポジトリの使い方に加え、Web サーバーの仕組みや開発者ツール（DevTools）の操作方法、HTML/CSS の概要を説明します。

- `02_html_tags` **HTML のタグの学習**:

  文書構造を作るための主要なタグと、ブロックレベル/インライン要素の概念を学習します。

- `03_html_attributes` **HTML の属性の学習**:

  グローバル属性、INPUT タグ固有の属性などを学習します。 `id` と `class` の使い分けや、入力制限などの属性、将来バグらせないための記法を学習します。

- `04_html_forms` **HTML の form の学習**:

  テキストボックス、ラジオボタン、ラベルの紐付けなど、使いやすい入力フォーム（UI）の作り方を学習します。

- `05_css_basics` **CSS の基礎学習**:

  ボックスモデル（余白の構造） の理解や、実務における「CSS を書く場所」のルールを学習します。

- `06_css_layout` **CSS の脱初心者**:

  Flexbox による横並び配置や、Position による重ね合わせ、実務的なフォームのレイアウト手法を学習します。

- `07_css_advanced` **CSS のちょっと高度な話**:

  ホバー時の変化やクリック時の沈み込みなど、ユーザー操作に反応する「心地よい動き（マイクロインタラクション）」を学習します。

## フォルダ構造

```txt
starter-html-and-css/
├── .devcontainer/
│   └── devcontainer.json
├── .git/
├── 01_introduction/       <-- ここからスタート
│   ├── index.html
│   ├── style.css
│   └── README.md
├── README.md              <-- このファイル
└── run_web_server.sh      <-- サーバー起動用スクリプト
```

## 次のステップ

ツールの使い方がわかったら、次は実際に自分でタグを書いてみましょう。

まずは [01. イントロダクション](./01_introduction/README.md) へ進んでください
