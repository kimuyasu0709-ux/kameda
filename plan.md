# 亀田米穀店 LP 制作プラン（plan.md）

> 本ファイルはプロジェクトの正本メモ。最初に共有された**デザイン仕様書**（配色・書体・レイアウト・全11セクション・装飾レイヤー・テキスト/画像切り分けルール）と、その後のユーザー指示・実装内容をまとめる。作業再開時はまず本ファイルを読む。

---

## 0. 概要
- **案件**：金沢の米穀店「亀田米穀店（かめだべいこくてん）」の1ページ完結ランディングページ。
- **性質**：**架空デモ**（練習・提案用）。実在店ではない。店舗情報・電話番号・実績数字・お客様の声・価格はすべて**仮値**で、本番では事実確認済み情報に差し替える前提。
- **トーン**：温かい／誠実／家庭的。老舗の信頼感を残しつつ高級店に寄りすぎない柔らかさ。重厚・格式張りは避ける。
- **土台資料**：ラフ画像2枚（プロンプト3＝ファーストビュー／プロンプト5＝トップページ全体）＋詳細デザイン仕様書。**ラフは方向の参考であり完全再現はしない。HTML本体は仕様書ベースで構築する。**

## 1. 成果物・ファイル構成
- `/Users/apple/kameda/index.html` … `<!DOCTYPE html>`〜`</html>` の**自己完結HTML**（CSSは`<style>`、JSは`<script>`でインライン。分割しない）。約50KB。
- `/Users/apple/kameda/src/` … 支給画像20点（`kameda-site-assets/img/` からコピー）。画像参照は全て `src/…`。
- `/Users/apple/kameda/kameda-site.zip` … `index.html` ＋ `src/` のバンドル（納品用・約22MB）。
- 原本素材：`/Users/apple/kameda/kameda-site-assets/img/`（＋`README.txt`）。
- プレビュー：`.claude/launch.json` の `kameda`（port 4525、`python3 -m http.server --directory /Users/apple/kameda`）。

## 2. デザイントークン（仕様書準拠）
| 用途 | 値 |
|---|---|
| メイン（ネイビー） | `#082444` … 見出し・店名・電話・実績数字・ステップ番号・Qアイコン・フッター基準 |
| サブ（ゴールド） | `#b38532`（月桂樹は明るめ `#c9a24e`）… 見出し下ライン・月桂樹・矢印・線画・FAQ開閉 |
| アクセント（オレンジ） | `#e35a1e`（hover `#c94d15`）… 主要CTA・ヘッダーボタン・最終CTA・チェック |
| 背景 | 白 `#ffffff` と 生成り `#f8f6f1` を交互。ヒーローは白基調 `#fbfaf6`。カード薄 `#fdfdfd` |
| 文字 | 本文 `#31465e` ／ 見出し `#082444` ／ 注釈 `#777777` ／ 反転白 `#ffffff` |
| 書体 | 見出し＝Noto Serif JP(500-600)、本文＝Noto Sans JP(400/700)（Google Fonts） |

**数値仕様**：通常セクション `max-width:1120px`（ヒーロー内側のみ1360）。セクション上下余白 PC 80-120／SP 56-72px。h1 PC 48-64/SP 32-40、h2 PC 32-42/SP 26-32、本文 16-18px・行間1.8。`@media(max-width:768px)` で全1カラム。`viewport` meta 必須。

## 3. ユーザー指示（実装の必須要件）
1. **素材レイヤーをCSS背景として全セクションに活用**：`position:absolute`／`background-image`で配置、**`z-index:-1`（本文の後ろ）**、`pointer-events:none`。悩み＝ドットパターンをリピート、CTA＝金の波、各生成り/白セクションにblob等を薄く。**「FVだけ豪華で下が弱い」を厳禁**（全11セクションを装飾で底上げ）。
2. **画像マスキング**：人物写真は円形（`border-radius:50%`）、実績/物系は角丸24px、**1枚だけ`clip-path`斜めカット**で単調回避。
3. **レイアウトにリズム**：全セクション中央ぞろえにしない。2カラム／カード／タイル／中央／背景色反転を交互に。
4. **ヒーローはフルブリード**：`.hero`幅100%（max-widthなし）、`.hero-inner`max1360、PCで左テキスト/右画像、`min-height` 680-760。
5. **控えめな装飾アニメ**：blob浮遊 `@keyframes float` 12s、hover `scale(1.02)`、スクロールフェードは1段のみ（`prefers-reduced-motion`で停止）。
6. **絵文字禁止**：アイコンは全て**インラインSVG**（または画像素材）で表現。
7. 主要テキスト（h1・見出し・価格・電話・FAQ・住所）は**HTMLテキスト**（画像に焼き込まない）。

## 4. 画像アセット（src/・実確認済み）
- **写真**：`fv.jpg`(家族の食卓＝ヒーロー)／`profile.jpg`(作務衣の店主・縦＝専門性)／`case-1`(店主と客の対面)／`case-2`(熨斗ギフト)／`case-3`(厨房でプロが米を扱う)／`voice-1`(母娘)／`voice-2`(夫婦)／`voice-3`(料理人)。
- **バッジ**（不透明・完成グラフィック）：`badge-1`4つの対応／`badge-2`1965年創業／`badge-3`予算に合わせて提案。→「選ばれる理由」帯にそのまま配置。
- **線画アイコン**（透過・発光）：`icon-line-1`茶碗／`icon-line-2`米袋／`icon-line-3`ギフト。※UIの細かいアイコンは統一感優先でインラインSVGを使用し、これらは**未使用**。
- **装飾**（透過）：`deco-blob-1`青／`deco-blob-2`生成り／`deco-wave-1`金波／`deco-curve-1`橙カーブ／`deco-dot-pattern`ドット。
- **ロゴ** `logo.png`（透過・ネイビー＋淡い金の発光）＝明るい背景向き。ヘッダーで使用。濃紺フッターの店名は白テキスト。
- **未使用**：`icon-line-1/2/3`・`deco-curve-1`・`case-3`（ZIPには全20点同梱）。

## 5. セクション構成（縦積み・全12ブロック）
1. **ヘッダー**（sticky・白）：ロゴ＋アンカーnav＋電話＋橙CTA。768px以下はハンバーガー→高さアンフォールドdrawer。
2. **ファーストビュー**（フルブリード）：右に`fv.jpg`（家族＋食卓全体が見える構図、`object-position:62%`）、左に白グラデ帯。h1 2行＋**金のライン**＋サブ1行＋中央寄せ電話CTA＋**月桂樹つき実績バッジ3項目**（創業1965年／北陸のお米100種類以上取扱い／お客様満足度（リピート率）90%以上）。スマホは テキスト→CTA→写真→バッジ 順。
3. **選ばれる理由**（生成り）：`badge-1/2/3`を3タイル。
4. **お悩み**（3カード・ドット背景repeat）：種類が多い／家族に合うか分からない／専門店は高そう。
5. **Before/After**（2カラム＋中央ゴールド矢印）。
6. **専門性**（白・2カラム）：`profile.jpg`**円形マスク**＋`case-1`角丸サブ／実績3数字。
7. **お客様の声**（生成り・4カード）：`voice-1/2/3`＋`case-2`（4枚目のみ`clip-path`斜めカット）。
8. **商品・価格**（白・5タイル）：家庭用/無洗米/玄米/ギフト/業務用（ゴールド円＋SVGアイコン＋価格）。
9. **流れ**（生成り・4ステップ）：ブロブ札＋濃紺丸番号＋SVGアイコン＋矢印。
10. **FAQ**（白・アコーディオン3）。
11. **最終CTA**：稲穂の写真背景（`src/cta-rice-field.png`）＋白オーバーレイ＋大型電話CTA。
12. **フッター**（濃紺）：白文字店名・店舗情報・地図iframe・サービスリンク・事業内容・デモ注記。

## 6. テキスト内容（仮値・ラフ準拠）
- h1「家族のごはんに合う一袋を、金沢の米屋が一緒に選びます。」／サブ「毎日の食べ方やご予算に合わせて、北陸のお米をご提案します。」
- 電話 `0120-766-111`（受付9:00-18:00 日祝休）。
- 実績バッジ：創業1965年／100種類以上取扱い（石川・能登・北陸中心）／お客様満足度90%以上（リピート率・ご家庭・飲食店様から）。
- 価格：家庭用2kg1,200円〜／無洗米2kg1,300円〜／玄米2kg1,100円〜／ギフト2kg2,000円〜／業務用10kg4,800円〜（税込）。
- お客様の声4・FAQ3・事業内容・フッター店舗情報（仮）。

## 7. 実装済み技術メモ
- ヒーロー：`.hero-inner{position:static}`で絶対配置mediaを`.hero`基準にしフルブリード化。z-index＝deco(-1)<media(1)<scrim(2)<copy/stats(3)。スマホはflex-column＋orderで並び替え。
- 月桂樹バッジ：数字を左右のインラインSVG月桂樹（`.laurel--r`は`scaleX(-1)`で反転）で挟む。
- JS：ハンバーガー開閉／FAQアコーディオン（`max-height`アニメ）／スクロールリベール（IntersectionObserver＋スクロール位置フォールバック＋`setTimeout`1600保険＝プレビューペインのIO不発対策）。
- 検証OK：画像404ゼロ・consoleエラー0・375pxで横スクロールなし・ハンバーガー/FAQ動作・PC/SP表示良好。
- ※プレビュー確認の注意：mobileスクショは2xスケール（375報告でも実描画750px）＝computerクリックは報告値の半分の座標。resize直後は再描画が乱れることがあるため`?v=`や再screenshotで確認。

## 8. 残タスク・要確認（仮のまま）
- [ ] フッター住所の番地（現状 `〒920-00XX ○○町2丁目15-3` はダミー）
- [ ] お客様の声③の銘柄（能登ひかり）ほか、声の内容・属性
- [ ] 電話番号・実績数字（1965/100種類/90%）の確定
- [ ] SNSリンク（Instagram/Facebook/LINE）のURL
- [ ] 地図（現状は「金沢」汎用のGoogleマップiframe）
- [ ] 本番公開時は「デモ注記」を外す／実データに差し替え

## 9. 変更履歴（要点）
- 初版：仕様書＋支給画像20点で1ページ完結HTMLを実装（全12ブロック・素材レイヤー活用・マスキング・レイアウトリズム・アニメ・絵文字ゼロ）。
- ヒーロー画像の見え方改善：表示エリアを拡大（左33%〜）・高さ680・`object-position:62%`で家族＋食卓全体が見えるように。
- **ファーストビューを参考画像に合わせて刷新**：見出し2行＋金ライン、サブ1行、CTA中央寄せ、実績バッジを月桂樹つき3項目デザイン（創業1965年／北陸のお米100種類以上／お客様満足度90%以上）に変更。
- **ヘッダーのロゴ拡大・バランス調整**：ナビ 14→15px・電話 20→22px（受付 10→11px）・`.btn--sm` 13→14px/`padding:12px 22px`。ロゴは段階的に拡大し**最終 120px**（46→60→120）、ヘッダー高さは**最終 140px**（72→88→140）。連動する `.hero` の `padding-top` とモバイルドロワー `.gnav{top}` も 140px に同期（ロゴを大きくする際はこの3か所を必ず揃える）。ロゴ原本は 1536×1024 で120px表示でも十分な解像度。PC1440・SP375で検証OK（横スクロールなし・consoleエラー0・ドロワーはヘッダー下端に整合）。
- **「ご相談からお届けまでの流れ」セクションのアイコンをイラスト画像化**：各ステップのインラインSVG（`.ico`）を支給の透過イラストに差し替え。`src/flow-icon-phone.png`（1相談する）／`flow-icon-consultation.png`（2希望を伝える・吹き出し）／`flow-icon-rice-bag.png`（3おすすめを選ぶ・米袋＋タグ）／`flow-icon-delivery-truck.png`（4受け取る・配送トラック）＝原本 `kameda-site-assets/img/flow-icon-*.png`（各1536×1024・透過・中央に小さめ配置のため alpha≥55 bbox＋26pxでトリミングして `src/` に配置）。CSS `.flow-step__ico{height:64px;width:auto}` で高さ揃え（幅はアイコンごとに自然に可変）。旧 `.flow-step .ico` は削除。ステップ4の説明に「（最短翌日）」注記を `.flow-step__note` で追加。PC1440・SP375で検証OK（JS計測：4枚ロード・高さ64・旧ico0・横スクロールなし・consoleエラー0／SPで4アイコンの描画を目視確認）。
- **「商品・サービスと価格の目安」セクションをアイコン→写真に変更**：各カード上部の金丸＋SVGアイコン（`.disc`）を、支給の商品写真に差し替え。`src/product-1-household-rice.png`（家庭用米）／`product-2-rinse-free-rice.png`（無洗米）／`product-3-brown-rice.png`（玄米）／`product-4-gift-rice.png`（贈り物・ギフト）＝原本 `kameda-site-assets/img/product-*.png`（各1600×約1020・RGB・白角丸フレーム焼込み）。**5枚目「業務用米」は専用画像がないため既存 `src/case-3.jpg`（厨房でお米を扱う）を流用**。写真は `.prod-card__ph{width:100%;height:130px;object-fit:cover;border-radius:10px}` で統一（cover croppで焼込みフレームは自然に切れ、case-3含め見た目が揃う）。旧 `.disc` CSSは削除。テキスト・価格は既存のまま（添付と一致）。PC1440・SP375で検証：JS計測で5枚ロード・寸法172×130・`.disc`ゼロ・横スクロールなし・consoleエラー0を確認（※プレビューペインはPC全体縮小表示＆スクショ同期不良のため実寸目視はSPで一部確認）。：支給の透過イラスト `src/kameda-before-woman.png`（困り顔の女性・胸像）と `kameda-after-family.png`（満足げな家族4人）を各パネルに配置（原本 `kameda-site-assets/img/kameda-*.png`、alpha≥40 bbox＋24pxでトリミング）。カードを `.ba-card__head`（タグ＋小見出しを中央寄せ）＋`.ba-card__row`（`display:flex`／リスト左・イラスト右）に再構成。イラスト幅は `clamp`（before 96〜130px／after 140〜200px）、SPは media で縮小（before 82px／after 104px・カード余白も 26/22 に）してAFTERリストの折返し過多を回避。テキスト自体は既存のまま（添付と一致）。PC1440・SP375で検証OK（横スクロールなし・404/consoleエラー0・添付カンプと一致）。
- **「お客様の声」セクションを支給写真＋横長カードに刷新**：円形サムネ（voice-1/2/3.jpg）＋4枚目 `case-2.jpg` の `clip-path` 斜めカット構成をやめ、支給の横長写真 `src/voice-1〜4.png`（1672×941・16:9・RGB／原本 `kameda-site-assets/img/voice-*.png`）を全4カード上部に**角丸長方形（`width:100%;height:148px;object-fit:cover;border-radius:12px`）**で配置。写真＝1若い夫婦／2年配夫婦／3料理人／4熨斗付き贈答品。見出しは `var(--serif)` 化、属性表記を2行（「金沢市在住　◯様（…）」＋「購入商品：…」）に、注記 `.voice__note` を右寄せに変更。旧 `.voice-card--slant` ルールは削除。PC1440・SP375で検証OK（横スクロールなし・404/consoleエラー0・添付カンプと一致）。※旧 `src/voice-1〜3.jpg`・`case-2.jpg` はこのセクションでは不使用に（ファイルは残置）。
- **「こんなお悩みはありませんか？」セクションを画像イラスト＋横並びカードに変更**：支給イラスト `src/concern-woman.png`（種類が多くて分からない）／`concern-man.png`（家族に合うお米が分からない）／`concern-wallet.png`（専門店は高そう）を採用（原本 `kameda-site-assets/img/concern-*.png`、透過余白を alpha≥40 bbox＋40pxでトリミングして `src/` に配置）。カードを従来の「アイコン上・中央寄せ」から**「イラスト左＋見出し・本文右／左寄せ」の横並び**（`.worry-card{display:flex}`／`.worry-card__thumb`固定幅104px・img `max-height:120px`／`.worry-card__body`）に変更。本文は添付に合わせ簡潔化。旧 `.ico` は不使用。PC1440・SP375で検証OK（横スクロールなし・404/consoleエラー0・添付カンプと一致）。
- **「流れ」セクションのステップ番号バッジを中央上→左角に移動**：`.flow-step .num` を `left:50%;transform:translateX(-50%);top:-18px`（中央上）から `left:14px;top:-14px`（transform削除・左角）に変更。カード上部の丸み（半径52px/SP40px）に馴染むよう左14pxに内寄せ。PC1440・SP375で検証OK（JS計測：4枚とも offsetLeft14/offsetTop-14・transform:none・横スクロールなし。※プレビューペインはスクロール制御が効かずスクショ目視不可のため計測で確認）。
- **「流れ」セクションのカード間隔を拡大**：`.flow__grid` の `gap` を `clamp(16px,2.4vw,28px)`→`clamp(28px,4vw,52px)` に拡大（1440で28→52px）。同時に gap をCSS変数 `--flow-gap` 化し、間の矢印 `.flow-step .arrow` の位置を固定 `right:-22px` から `right:calc(-1 * (var(--flow-gap)/2 + 13px))` に変更→clamp全域で矢印が隙間中央に自動追従（従来は隙間中心から-17px左寄り）。SPは既存の縦積み（`.flow__grid` gap:34px・矢印は下向き `bottom:-30px`）を維持。PC1440で検証OK（JS計測：4カラム・gap52px・カード間52px・矢印offset0・横スクロールなし／SP375も1カラム・横スクロールなし）。
- **フッターのSNSアイコン：LINE→X に差し替え**：Facebookの右のアイコン（LINE・ダミー`#`）を X(@kakekome) に変更。`href="https://x.com/kakekome"`＋`target="_blank" rel="noopener noreferrer"`＋`aria-label="X"`。SVGは公式Xロゴ（塗り形状）で、既定の `.footer__sns svg{stroke:#fff;fill:none}` に対しこのpathのみ `fill="#fff" stroke="none"` で白塗り表示。並びは Instagram・Facebook・X。※Facebookは現状ダミー`#`のまま（URL未定）。
- **820px（タブレット帯）のフッターのバランス崩れを修正**：フッターは 4カラム(≥961)→2カラム(769-960)→1カラム(≤768)。820pxの2カラムで「サービス・リンク＋事業内容」列が突出して長く（463px）、隣の「アクセス＋地図」列(199px)との高さ差で大きな空白（264px）が出ていた。修正＝**769-960帯でサービス・リンク列のリスト(`ul`)を2段組み化**（`@media(max-width:960px){.footer__grid .footer__col:last-child ul{grid-template-columns:1fr 1fr;column-gap:20px}}`＋`.footer__grid{align-items:start}`）。→ 該当列 463→299px に短縮、アクセス列との差 264→100px に縮小し均等化。モバイル(≤768)は `@media(max-width:768px)` で `ul{grid-template-columns:1fr}` に戻す（1段組み維持）。検証：820=フッター2カラム/リンク2段/列高さ均等化・820スクショで目視／961=4カラム/リンク1段(回帰なし)／375=1カラム/リンク1段(回帰なし)／横スクロールなし・consoleエラー0。
- **769〜1024px（タブレット帯）でお悩み・Before/Afterが崩れる問題を修正**：これらを1カラム化するBPが `768px` だったため、769〜960pxで お悩み(3カラム)・BA(2カラム＋矢印) が多カラムのまま詰まり、カード内の日本語が1文字ずつ縦に折返していた（781pxで worry本文40px幅×503px高・baリスト項目120px幅）。修正＝`.worry__grid{1fr}`・`.ba{1fr}`・`.ba__arrow{rotate(90deg)}` を **新設 `@media(max-width:1024px)` ブロックに移動**（768pxブロックからは削除、`.reasons__grid{1fr}`・`.ba-card` padding・`.ba-card__fig` 82/104px は768pxのまま維持）。→ ≤1024pxで1カラム（本文が全幅で自然に折返す）、≥1025pxで多カラム。検証（JS実測）：781=worry1カラム/本文377px・ba1カラム/リスト481px1行/矢印90°下向き・横スクロールなし／1000=1カラム／1025=多カラム(worry本文111px・baリスト220px1行)・崩れなし／1280=多カラム・矢印右向き・回帰なし／375=1カラム・fig82px・矢印下向き・回帰なし／consoleエラー0。※reasons(画像カード)・voice(2カラム)・products(3カラム)・flowは781pxでも文字崩れなしのため対象外（flowはやや窮屈だが1文字折返しではない）。
- **スクロール時のセクション“フワッと”表示を有効化（リビールの取りこぼし修正）**：各セクションの見出し・カード等には既に `.reveal`（`opacity:0;translateY(26px)`→`.is-in`でフェードアップ／IntersectionObserver＋スクロール位置フォールバック）が付いていたが、**JS末尾の `setTimeout(reveals→is-in, 1600)` が1.6秒後に全要素を無条件表示**していたため、実ブラウザでは読み込み直後に画面外セクションまで表示済みになり、スクロールしても何も動かなかった。修正＝この1.6秒一括表示を**削除**し、代わりに `IntersectionObserver` 非対応ブラウザのみ全表示する `else` 分岐を追加（プログレッシブエンハンスメント）。IOに `rootMargin:'0px 0px -8% 0px'` を付与し、少し画面に入ってから発火するよう調整。あわせて `@media(prefers-reduced-motion:reduce)` に `.js .reveal{opacity:1;transform:none;transition:none}` を追加（動きを減らす設定のユーザーは即時表示＝アクセシビリティ対応）。検証：フレッシュ読込で36リビール要素すべて非表示待機（opacity0）／`#voice` へスクロールすると通過セクションが順次 is-in 化（0→19）し下は非表示のまま＝スクロール発火を確認／voiceセクション表示をスクショ目視／consoleエラー0。※プレビューはscrollY表示が0のままだが実描画はスクロールしIOは発火する。
- **BAセクションのSP矢印が横向きのままだったのを下向きに修正**：`.ba__arrow` は `reveal` クラスを持ち、SP用の `@media(max-width:768px){.ba__arrow{transform:rotate(90deg)}}` が、リビール完了時の `.js .reveal.is-in{transform:none}`（詳細度0,3,0 > 0,1,0）に上書きされて回転が消え、縦積みでも右向きのままだった。修正＝矢印svgから `reveal` クラスを削除（PCはSVGネイティブで右向き、SPは rotate(90deg) が効いて下向き・中央）。検証：SP375で transform=matrix(0,1,-1,0,0,0)＝90°・reveal無・水平中央・下向き矢印をスクショ目視。
- **ヘッダーの電話番号を削除（情報過多の解消）**：`header__cta` 内の `.header__tel`（0120-766-111＋受付時間）を撤去。連絡導線は お問い合わせボタン＋ヒーロー/最終CTAの電話で担保。電話撤去で内容が狭まったため、先の折り畳みBPを **1160→1220px** に調整（電話ありきの1160では電話削除後に1161〜1180で僅かに溢れるため）。結果：≥1221pxはフルヘッダー（余裕あり・はみ出しなし）／≤1220pxはハンバーガー。検証：1240/1280=フル・overflowなし・nav/cta非重複／1200以下=ハンバーガー／横スクロールなし・1280スクショで電話が消えスッキリを確認。※`.header__tel{display:none}` ルールは残置（対象要素なしで無害）。
- **最終CTAの2ボタンのサイズを統一**：電話ボタン（アイコン＋2行で背高）と「オンラインショップで購入する」（1行で低い）で大きさが違っていたのを統一。`.finalcta__btns` を `align-items:center`→`stretch`（高さ揃え）、`.finalcta__btns .btn{flex:1 1 260px;max-width:340px}`（幅揃え）。検証：PC1280で両ボタン 340×76 で完全一致（sameW/sameH）／SP375で縦積み・330px同幅／横スクロールなし。
- **Before/After(ぴったり)セクションのイラストを新画像に差し替え**：`.ba-card__fig` の `src/kameda-before-woman.png`→`src/pittari-before-worried-woman.png`（困り顔の女性・BEFORE）、`src/kameda-after-family.png`→`src/pittari-after-family.png`（家族4人・AFTER）。支給原本（各1024×1536・透過）を `kameda-site-assets/img/` に退避し、内容範囲へトリミングして `src/` に配置（woman=端の微小ノイズ画素で全高bboxになるため alpha≥150 で実体bbox抽出＋5%マージン→531×935／family=alpha≥30 bbox＋4%→1024×1110）。CSS・サイズclampは既存のまま流用。※同時に `src/pittari-arrow.png`（矢印）も支給されているが、BAの矢印は現状インラインSVGのまま（未使用・要望あれば差し替え可）。検証：両画像loaded・描画woman113×200/family174×188・横スクロールなし・1180スクショでカンプ通り配置を確認。
- **ヘッダーのボタン/電話の折返し・狭PCはみ出しを解消（折り畳みBP引き上げ）**：`.header__tel` に `flex:none;white-space:nowrap` を追加（1180px付近で電話番号「0120-766-111」＋受付時間が3行折返していたのを1行に）。ただしボタン/電話を非圧縮化するとnav＋電話＋2ボタンが多く**約1100px未満でヘッダーが画面外にはみ出す**ため、ヘッダー折り畳み（ハンバーガー化）のブレークポイントを **768px→1160px に引き上げ**。方法：`@media(max-width:768px)` にあったヘッダー系ルール（`.gnav`ドロワー化／`.hamburger`表示／`.header__tel`・`.btn--sm`非表示／`.header__inner`gap12／`.header__logo`margin-right:auto）を新設 `@media(max-width:1160px)` ブロックへ分離（ヒーロー/グリッド等のSPルールは768pxのまま）。結果：**≥1161pxは1行フルヘッダー（余裕あり）／≤1160pxはハンバーガー**。検証：1200=フル・はみ出しなし／1040・900=ハンバーガー画面内・電話/ボタン非表示・はみ出しなし／ドロワーは top140・全幅・全8項目（ブログ/オンラインショップ含む）／1280スクショで1行表示を目視・consoleエラー0。
- **ヘッダーのボタン文字改行を修正（PC幅）**：外部リンク追加でヘッダーが混み、`.btn` に `white-space:nowrap` が無かったため「オンラインショップ」「お問い合わせ」ボタン文字がPC幅で2〜3行に折返していた。修正＝`.btn{white-space:nowrap}` 追加＋`.gnav a{white-space:nowrap}`＋`.header__cta .btn{flex:none}`（圧縮防止）。あわせて間隔を微調整して収まりを確保：`.gnav ul` gap 26→22、`.header__inner` gap 24→20、`.header__cta` gap 16→12、`.btn--sm` padding 12px22px→12px18px。検証（JS実測）：1440/1280/1100/900 いずれもボタン・nav文字が1行・折返しなし・横スクロールなし（1280以上ははみ出しゼロ、900でもnav/ボタン非重複・画面内収まり）・ヘッダー高120px維持・consoleエラー0。1440スクショで1行表示を目視確認。
- **外部サイト（ブログ／オンラインショップ）へのリンクを設置**：クライアントのブログ `https://kameda.kitemi.net/c1699.html` とBASEオンラインショップ `https://kamekome.base.shop/` を、①ヘッダー②最終CTA③フッターの推奨3箇所に配置（全て `target="_blank" rel="noopener noreferrer"`）。①ヘッダー：nav に「ブログ」リンク追加＋`header__cta` に「オンラインショップ」ボタン（新設 `.btn--shop`＝オレンジ枠白地、hoverでオレンジ塗り）を お問い合わせ の前に追加。モバイルはヘッダーボタンが非表示になるため、nav に `li.gnav__shop-m`（PCは `display:none`、SPドロワーのみ表示・オレンジ強調）で「オンラインショップ」を追加＝PC重複なし/SP到達性確保。②最終CTA：電話ボタンの横に副ボタン「オンラインショップで購入する」（`.btn--shop-lg`＝オレンジ枠）を追加、`.finalcta__btns`(flex/wrap)で包む（PC横並び・SP縦積み）。③フッター「サービス・リンク」に ブログ／オンラインショップ を追加。検証：外部リンク計6・全て target=_blank/rel=noopener・PC1180でヘッダー1行内に収まり折返しなし＋navショップ項目は非表示で重複なし・最終CTA2ボタン同一行・SP375でドロワーに両リンク表示＋ヘッダーボタン非表示＋最終CTA縦積み・PC/SPとも横スクロールなし・consoleエラー0。※ブログ/ショップは実在サイト（デモLPに実リンク）。
- **専門性(about)セクションの実績3項目アイコンをインラインSVG→支給画像に差し替え**：`.pro__stat` 内の `<svg class="ico">`（稲穂風/茶碗風/チャット風の簡易SVG）を、支給の金線画アイコン `src/about-icon-history.png`(稲穂＝創業1965年/金沢で米一筋)・`about-icon-varieties.png`(茶碗＝取扱い品種100種類以上)・`about-icon-consultations.png`(人々＝年間相談件数5,000件)に差し替え。原本 `kameda-site-assets/img/about-icon-*.png`（各2048×2048・透過・余白多め）を alpha≥20 bbox＋6%マージンでトリミングして `src/` に配置（各約1400×1450）。CSS `.pro__stat .ico`（stroke指定SVG用）を削除し、`.pro__stat__ico{height:52px;width:auto;margin:0 auto 8px;display:block}` を新設（高さ揃え・幅可変）。テキスト/数字/区切り線は既存のまま。検証：3点loaded・描画高52px・旧svg.ico 0・consoleエラー0・横スクロールなし／デスクトップ1180でカンプ通り（アイコン→数字→ラベルの縦積み・3列）を目視確認。
- **ヒーロー写真をフルブリード化（焼き込み装飾の「見切れ」解消）**：`fv.jpg` は全幅用の合成画像（右=家族写真／左下=紺＋星の帯・稲穂・生成りの波の焼き込み装飾）だが、従来は `.hero__media{inset:0 0 0 33%}`＋`object-position:62%` で右67%に押し込み＋左端トリミングしていたため、左下の角装飾が切り落とされ**33%の境目に中途半端なカケラ（灰色の塊）が残って見切れていた**（PC1440で実測：表示は元画像x313〜1480のみ）。修正＝`.hero__media` を `inset:0`（全幅）・`object-position:center` に変更し、scrim を左を塗りつぶす不透明グラデ（`#fbfaf6 0-33%`）から**軽い veil**（`rgba(251,250,246,.55→.30→0)`／左28%まで薄く掛け52%で消える）に変更。→ PC1440で画像全幅 x0〜1672 が表示され、紺＋星の帯が本来の左下角に収まる（上下19pxのみトリミング）。テキストは左の明るい面＋軽scrimで可読。モバイルは写真を別ブロック表示のままなので `.hero__media img{object-position:62%}` を SP override で追加し従来の家族フレーミングを維持。検証：PC1440フルブリード確認・見切れ解消・consoleエラー0・PC/SPとも横スクロールなし。※ヒーローの独立deco（blob-2/wave-1）は全幅写真の裏に回り不可視化するが無害のため残置。
- **あしらい（deco）が全隠れしていたバグを修正＋控えめ化＋未使用素材を追加**：既存のあしらい9点は `z-index:-1` で配置済みだったが、親セクションが `position:relative` のみで**スタッキングコンテキスト(SC)を作っておらず**、負zのあしらいがルート階層に抜けて**各セクションの不透明背景に完全に覆い隠されていた**（青blobが全く見えないことと一致）。修正＝`.section`(index.html:49) と `.hero`(:138) に **`isolation:isolate`** を1プロパティ追加（両者とも既に position:relative;overflow:hidden なので安全）。これで全セクションがSC化し、あしらいは「背景の上・本文の下」に描画される（ヒーローで青blob化した検証スクショで、見出しテキストの裏・生成り背景の上に正しく重なることを目視確認）。トーンは控えめ指定に合わせ `.deco--blob` opacity **.5→.14**。線画用に `.deco--line{opacity:.3}` を新設。**未使用素材を各所に追加**：`deco-curve-1`(橙カーブ)を worry(`.worry__deco` right:-70/top:10) と faq(`.faq__deco2` right:-80/top:0)、`deco-wave-1`(金波)を reasons(`.reasons__deco2` left:-80/bottom:-80) に配置（いずれも `deco deco--line`）。検証：deco画像 全12点200 OK・consoleエラー0・PC1440/SP375とも横スクロールなし・全decoがSC化セクションの子であることをJS確認。※プレビューペインはPC全体縮小＋スクロール固着＋スクショ同期ずれが強く、視覚確認はリロード直後のモバイル1枚で実施。
- **最終CTAセクションに稲穂の写真背景を追加**：`.finalcta` の背景を単色グラデ `linear-gradient(#faf7f0,#f2ead9)` から、支給の稲穂バナー写真＋白オーバーレイに変更＝`linear-gradient(180deg,rgba(255,255,255,.62),rgba(255,255,255,.34) 45%,rgba(248,246,241,.30)),url("src/cta-rice-field.png") center/cover no-repeat`。上を強めの白(62%)→下を薄め(30%)にして稲穂を下側ほど見せつつ見出しの可読性を確保（添付カンプ準拠）。写真と競合するため既存の装飾 `<img class="finalcta__wave">`(deco-wave-1)・`<img class="finalcta__deco">`(deco-blob-2) をこのセクションから削除し、対応するCSS `.finalcta__wave`/`.finalcta__deco` も削除。画像原本 `kameda-site-assets/img/kameda-rice-field-cta-bg.png`（2172×724・不透明・既に高キー調）を無加工で `src/cta-rice-field.png` にコピー（背景全面のためトリミング不要）。※deco-wave-1はヒーロー(`hero__deco2`)で継続使用。検証：画像200 OK・`background-size:cover`・余分なdeco img 0・h2ネイビー・横スクロールなし。SPスクショで稲穂背景＋オレンジCTA＋注記の描画を目視確認（プレビューペインはスクロール制御が効かずEndキー送りで撮影）。
- **（試行→差し戻し）ファーストビュー実績バッジの支給画像化**：一度 `hero-badge-1/2/3.png`（余白トリミング版）に差し替えたが、ユーザー指示で**元のテキスト＋インラインSVG月桂樹バッジ（白ボックス内3項目：1965年／100種類以上取扱い／90%以上）に差し戻し済み**。`.hero__stats`（白bg/影/padding）・`.hero__stat`・`.stat-*` CSS、マークアップ、モバイルCSSともに画像化前の状態に復元。`src/hero-badge-*.png` は削除（原本は `kameda-site-assets/img/hero-badge-*.png` に保持）。→ FVバッジは現状「テキスト＋SVG月桂樹」版が正。PC1440で検証OK（横スクロールなし・consoleエラー0）。
