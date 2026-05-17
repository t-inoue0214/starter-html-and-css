# 09. アクセシビリティの基礎

Web ページは、マウスが使えない人・弱視の人・スクリーンリーダーを使う人など、さまざまなユーザーが利用します。アクセシビリティとは、そのような多様な人々が等しく使えるように Web を設計・実装することです。

この章では、**業務コードレビューで必ず指摘されるレベル**のアクセシビリティを習得します。WCAG（Web Content Accessibility Guidelines）50 基準への完全対応は専門家による監査が必要な領域のため、この章では扱いません。

---

## 1. 色のコントラスト比

### なぜコントラスト比が必要か

弱視のユーザーや色覚多様性（いわゆる「色盲」）を持つユーザーは、文字と背景の明暗差が小さいと読めなくなります。「デザインが美しい」と思って選んだ薄いグレーのテキストが、実際には多くのユーザーにとって読めない色になっていることがあります。

参考: [MDN: 色のコントラスト](https://developer.mozilla.org/ja/docs/Web/Accessibility/Understanding_WCAG/Perceivable/Color_contrast)

### WCAG AA 基準

| テキストの種類 | 必要なコントラスト比 |
|:---|:---|
| 通常テキスト（18px 未満 かつ 太字 14px 未満） | **4.5:1 以上** |
| 大きいテキスト（18px 以上、または太字 14px 以上） | **3:1 以上** |

数値は「文字色と背景色の明暗差」を表す比率です。白地に黒の文字は 21:1（最大）、白地に薄いグレー（`#aaa`）は約 2:1 で基準を大きく下回ります。

### DevTools でのコントラスト比確認

1. DevTools（F12）を開く
2. Elements パネルでテキスト要素を選択する
3. Styles パネルの `color` プロパティの色付き四角をクリックする
4. カラーピッカーの下部にコントラスト比が表示される（「4.5:1」基準のチェックマークで合否が分かる）

または、Elements パネルの「Accessibility」タブでも確認できます。

> **実務メモ**: Figma の Contrast Checker プラグインや Chrome DevTools のコントラスト比表示で、実装前に事前確認できます。デザイナーからの色指定でも基準を下回っていた場合は、確認を取ってから修正することが重要です。

---

## 2. キーボード操作とフォーカス管理

マウスが使えないユーザー（運動機能の障害・キーボードのみでの操作を好む開発者など）は、Tab キーでページを操作します。「フォーカス」とは、今キーボードで操作対象になっている要素のことです。

### `:focus` 疑似クラス

[`:focus`](https://developer.mozilla.org/ja/docs/Web/CSS/:focus) は、キーボードでフォーカスが当たった要素にスタイルを適用する疑似クラスです。07章で少し触れましたが、ここで深掘りします。

```css
input:focus {
    border-color: #3498db;
    outline: 3px solid #3498db;
}
```

ブラウザはデフォルトで青い枠線（`outline`）を表示しますが、CSS でデザインを整える際に消してしまうことがあります。

### `outline: none` を安易に消してはいけない

```css
/* NG: キーボードユーザーがどの要素にいるか分からなくなる */
* {
    outline: none;
}
```

これを書くと、Tab キーでページを操作しているユーザーは「今どこにいるか」が分からなくなります。視覚的に一番困るのは、リンク・ボタン・フォーム入力欄のすべてでフォーカスが消えることです。

> **実務メモ**: Tab キーを押してページをナビゲートし、フォーカスが常に見えるか確認するのが、コードレビュー前の基本確認です。

### `:focus-visible` — キーボード操作時だけフォーカスを表示する

[`:focus-visible`](https://developer.mozilla.org/ja/docs/Web/CSS/:focus-visible) は、`:focus` のうち「キーボード操作のとき」だけスタイルを適用する疑似クラスです。マウスでボタンをクリックしたときはフォーカスリングを非表示にし、Tab キーで操作したときだけ表示できます。

```css
/* キーボード操作時だけフォーカスを表示する */
:focus-visible {
    outline: 3px solid #3498db;
    outline-offset: 2px;
}

/* マウスクリック時はフォーカスリングを非表示にする */
:focus:not(:focus-visible) {
    outline: none;
}
```

| 疑似クラス | マウスクリック時 | Tab キー操作時 |
|:---|:---|:---|
| `:focus` | 表示 | 表示 |
| `:focus-visible` | 非表示 | 表示 |

### `tabindex` の基本

[`tabindex`](https://developer.mozilla.org/ja/docs/Web/HTML/Global_attributes/tabindex) 属性を使うと、Tab キーによるフォーカスの順序と対象を制御できます。

| 値 | 説明 | 用途 |
|:---|:---|:---|
| `tabindex="0"` | Tab の順序に組み込む | `<div>` など元からフォーカスできない要素をフォーカス可能にする |
| `tabindex="-1"` | Tab では当たらないが JavaScript からはフォーカスできる | モーダルを開いたとき JS でフォーカスを移動させる |

> **実務メモ**: `tabindex` に `1` 以上の正の値を設定すると Tab の順序を強制的に変えられますが、DOM の順序と Tab 順序がずれて混乱を招くため、実務では使わないことが推奨されています。

---

## 3. 主要な ARIA 属性とセマンティック HTML

ARIA（Accessible Rich Internet Applications）は、HTML だけでは伝えにくい「意味」をスクリーンリーダーなどの支援技術に伝えるための属性群です。

参考: [MDN: ARIA](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA)

### セマンティック HTML が最初のアクセシビリティ対策

ARIA の前に、まずセマンティックな HTML タグを使うことが重要です。なぜなら、ブラウザは HTML タグの意味を支援技術に自動的に伝えるからです。

```html
<!-- NG: div はキーボード操作もスクリーンリーダーの読み上げも機能しない -->
<div class="btn" onclick="submit()">送信する</div>

<!-- OK: button はキーボード操作・スクリーンリーダー対応が標準で含まれる -->
<button type="submit">送信する</button>
```

`<div onclick>` でボタンを作ると、Tab キーでフォーカスが当たらない・Enter キーで実行できない・スクリーンリーダーが「ボタン」と読み上げないなど、多くの問題が発生します。

### `alt` 属性（画像の代替テキスト）

[`alt`](https://developer.mozilla.org/ja/docs/Web/HTML/Element/img#alt) 属性は、画像が読み込めない場合や、スクリーンリーダーが読み上げる際の代替テキストです。

```html
<!-- NG: alt なしはスクリーンリーダーがファイル名を読み上げる -->
<img src="profile.jpg" />

<!-- OK: 内容を説明するテキスト -->
<img src="profile.jpg" alt="山田太郎のプロフィール写真" />

<!-- 装飾目的の画像は alt="" で「読み上げ不要」を伝える -->
<img src="decorative-line.png" alt="" />
```

### `aria-label`（ラベルテキストが画面に表示されない場合）

[`aria-label`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Attributes/aria-label) は、要素のラベルを画面に表示せずにスクリーンリーダーだけに伝えたい場合に使います。

```html
<!-- アイコンだけのボタンに意味を持たせる -->
<button aria-label="メニューを開く">
    <span class="icon-hamburger"></span>
</button>

<!-- ナビゲーションが複数ある場合に区別する -->
<nav aria-label="メインナビゲーション">...</nav>
<nav aria-label="フッターナビゲーション">...</nav>
```

### `aria-describedby`（補足説明を紐付ける）

[`aria-describedby`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Attributes/aria-describedby) は、要素の補足説明を別の要素の `id` で紐付けます。フォームの入力ヒントなどで使います。

```html
<input type="text" id="name" aria-describedby="name-hint" />
<p id="name-hint">フルネームで入力してください</p>
```

スクリーンリーダーは入力欄にフォーカスが当たったとき、ラベルに加えてヒントテキストも読み上げます。

| 属性 | 伝えること | 読み上げのタイミング |
|:---|:---|:---|
| `aria-label` | 要素のラベル（名前） | フォーカスが当たった直後 |
| `aria-describedby` | 要素の補足説明 | ラベルの後に続けて |

### `role` 属性

[`role`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Roles) 属性は、HTML タグだけでは意味が伝わらない場合に要素の役割を補足します。

```html
<!-- <nav> タグがない環境で作られたナビゲーションを補足する -->
<ul role="navigation" aria-label="メインナビゲーション">...</ul>
```

ただし、セマンティックな HTML タグ（`<nav>`, `<main>`, `<header>` など）が使える場合は `role` を後付けするより先に、適切なタグを選ぶことが推奨されています。

### `aria-current`（現在地を伝える）

[`aria-current`](https://developer.mozilla.org/ja/docs/Web/Accessibility/ARIA/Attributes/aria-current) は、ナビゲーション内の「現在地」をスクリーンリーダーに伝えます。

```html
<nav aria-label="メインナビゲーション">
    <ul>
        <li><a href="/">ホーム</a></li>
        <li><a href="/contact" aria-current="page">お問い合わせ</a></li>
    </ul>
</nav>
```

スクリーンリーダーは「現在のページ、お問い合わせ」のように読み上げます。

> **実務メモ**: macOS の VoiceOver は `Cmd + F5` で起動します。Tab キーを押しながら操作すると、スクリーンリーダーが何を読み上げているか確認できます。

---

## 演習課題：お問い合わせフォームをアクセシブルにしよう

`index.html` にはアクセシビリティに配慮したお問い合わせフォームが用意されています。`style.css` を編集して、4 つの演習を完成させてください。

### 手順 1: コントラスト比を確認・修正する

ヒントテキスト（`.hint-text`）の色が `#aaa`（薄いグレー）に設定されています。これはコントラスト比が約 2:1 しかなく、WCAG AA 基準（4.5:1）を大きく下回ります。

1. ブラウザでページを開き、「フルネームで入力してください」というテキストを確認します。
2. DevTools（F12）を開き、Elements パネルでそのテキスト要素を選択します。
3. Styles パネルの `color: #aaa;` の色付き四角をクリックします。
4. カラーピッカーにコントラスト比が表示されます（2:1 前後で基準違反の表示が出るはずです）。
5. `style.css` の `.hint-text` の `color: #aaa;` を `color: #666;` に修正してください。
6. 再度 DevTools でコントラスト比を確認します。

DevTools のカラーピッカーに「4.5:1」のチェックマーク（✓）が表示されれば正解です。

---

### 手順 2: フォーカスインジケーターを設定する

Tab キーでフォームを操作したとき、「今どこにいるか」が分かるように `:focus-visible` を設定します。

まず、フォーカスが消えた状態を体験します。

1. `style.css` の「手順 2」のコメントのすぐ上にある以下のコメントを外してください：
   ```css
   /* :focus {
       outline: none;
   } */
   ```
   → コメントを外した後、Tab キーでページを操作してみてください。フォーカスが見えなくなります。

2. 体験できたら、今度は逆にコメントに戻してから、`:focus-visible` にスタイルを追加してください：
   ```css
   :focus-visible {
       outline: 3px solid #3498db;
       outline-offset: 2px;
   }
   ```

Tab キーでフォームを操作したとき、フォーカスが青い枠線で見えれば正解です。

---

### 手順 3: マウス操作ではフォーカスリングを表示しない

手順 2 だけだと、マウスでフィールドをクリックしたときも枠線が表示されます。`:focus-visible` と `:focus:not(:focus-visible)` の組み合わせで制御します。

1. `style.css` の「手順 3」のコメントを外してください：
   ```css
   :focus:not(:focus-visible) {
       outline: none;
   }
   ```

マウスでフィールドをクリックしたときはフォーカスリングが表示されず、Tab キーで操作したときだけ表示されれば正解です。

---

### 手順 4: 現在のページを示すスタイルを設定する

ナビゲーションの「お問い合わせ」リンクに `aria-current="page"` が付いています。このリンクが現在地であることが一目で分かるように、CSS でスタイルを設定します。

1. `style.css` の `.nav-list a[aria-current="page"]` に、現在地を示すスタイルを追加してください。例えば：
   ```css
   .nav-list a[aria-current="page"] {
       color: #fff;
       border-bottom: 2px solid #fff;
   }
   ```

ナビゲーションの「お問い合わせ」だけ白い下線がついて現在地が分かれば正解です。

---

## 覚えなくて良い・後回しにする内容

| 内容 | 理由 |
|---|---|
| WCAG 2.1 AA の全 50 基準への完全対応 | 専門家による監査が必要な領域。実務では QA チームや外部機関が担う |
| WAI-ARIA のすべてのロールと属性 | 主要な数個を押さえれば現場で対応できる。全仕様は MDN で確認する |
| アクセシビリティツリーの詳細 | DevTools の Accessibility タブで確認できるが、深掘りはブラウザ仕様の範囲 |
| `aria-live` によるライブリージョン | JavaScript で動的にコンテンツを変更する場面で使う。JS を学んでから |
| `prefers-reduced-motion` | アニメーションを抑制するメディアクエリ。10 章のレスポンシブと合わせて学ぶ |

---

## この章のまとめ：できるか確認しよう

- [ ] DevTools でテキストのコントラスト比を確認し、WCAG AA 基準（4.5:1）を満たしているか調べられる
- [ ] Tab キーだけでページを操作でき、フォーカスが常に見える状態にできる
- [ ] `outline: none` を書くとキーボードユーザーに影響することを説明できる
- [ ] `<label>` と `<input>` を `for`/`id` で正しく紐付けられる
- [ ] `aria-label` と `aria-describedby` の違いを説明できる

全部できていれば、次の章へ進む準備ができています。

---

| [← 第08章: デザインの基礎](../08_css_design_basics/README.md) | [全章目次](../README.md) | [第10章: レスポンシブデザイン →](../10_css_responsive/README.md) |
|:---|:---:|---:|
