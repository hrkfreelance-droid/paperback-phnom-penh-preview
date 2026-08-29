# HANDOFF

## V4 — モーション調整（TOPページのみ）

`v4/index.html` として追加。**V2 は変更前の比較対象として手つかずで残してある。**
コピー・構成・Index の並び・Field Note の中身は一切変更なし。
記事ページ (`v2/articles/*.html`) は未着手 — 同じ原則で後から横展開できるよう、
クラス名と DOM 構造は維持している。

写真（8.9MB）と記事ページは複製せず `../v2/assets/images/` `../v2/articles/` を参照する。
V4 で変更があるのは `index.html` 1枚だけで、差分はそこに閉じている。

- V4: https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v4/index.html
- V2（変更前）: https://hrkfreelance-droid.github.io/paperback-phnom-penh-preview/v2/index.html

方針は「動きを足す」ではなく「今ある動きの質を上げる」。

### 1. Scroll to walk in（ヒーロー導入）

| 変更 | 理由 |
|---|---|
| `.hero-photo` / `.hero-line` の `transition: .1s linear` を削除 | スクロール位置が既に時計なので、その上に CSS トランジションを重ねると **100ms の遅延がそのままワンテンポ遅れとして出る**。もっさりの主因。 |
| 進行度を線形 → `easeOutQuad` に | 最初のホイール一操作で文字が動き出し、減速しながら画面外へ。写真も同じ ease-out で、後半までまだ「到着し続けている」状態を保つ。 |
| ヒーロー小文字（kicker / sub）を進行度に応じてフェードアウト | 写真が立ち上がった後もキャプションが全不透明で残り、写真の上で読みづらくなっていた。 |
| スクロールキューを `p*4` の ease-out で早めに退場、下線のパルスを 3.2s の scaleX + opacity に | 点滅ではなく「呼吸」に近い動きへ。 |

### 2. Field Note カード

| 変更 | 理由 |
|---|---|
| `.feature-plate` を切り抜き枠に、内側に `.feature-plate-img` を新設 | 画像だけを動かし、枠は固定。`articles.json` の cover 適用先も内側の要素に変更（`#featurePlateImg`）。 |
| ホバー/フォーカスで画像 `scale(1.035)`、1.1s の ease-out | 指示どおり 1.02〜1.04 の範囲。ゆっくり寄るので拡大というより「近づく」印象。 |
| `.plate::before` のグラデーション濃度を hover 時 `opacity:.58` に | 影の濃淡だけで反応を出す（外側のドロップシャドウは足さない）。 |
| CTA は下線ではなく **余白の変化**：ブロック全体が 5px 右へドリフト＋トラッキング 0.08em→0.11em、矢印はさらに 11px | 「読む」への誘導を装飾ではなく間で表現。`:focus-visible` / `:active` も同挙動。 |

### 3. The Index

| 変更 | 理由 |
|---|---|
| フィルタ切替を即時 `display:none` → **フェード＋8px ドリフトの stagger**（out 42ms 刻み → in 42ms 刻み、各 440ms） | 瞬間的な入れ替えを避ける。連打時は進行中のスワップを中断して要素を解放するので、途中で消えたまま残る行が出ない。 |
| フォーカスレンズの `transition: opacity .15s` を削除し、JS 側で毎フレーム `--t` を lerp（係数 0.34） | CSS トランジションは指の動きの後ろにもう一つ遅いバネを置くのと同じ。lerp なら opacity と transform が同位相で動き、追従は 2〜3 フレームで収束する。 |
| 減衰カーブを線形 → `smoothstep`、半径 260px → 300px | フォーカスの縁が滑らかになり、中心付近ではっきり立つ。 |
| `--t` は 0.001 単位で変化した時だけ DOM に書く | 無駄なスタイル無効化を止める。 |
| 額縁のワイプを `.65s cubic-bezier(.22,.61,.36,1)` → `.78s cubic-bezier(.19,1,.22,1)` | 立ち上がりを速く、着地を長く。 |

### 4. エリア見出し（Riverside / BKK1 / Old Quarter / Tuol Tompoung）

flex → `.row` と同じ `56px 1fr auto` グリッドに変更。
ローマ数字は 1.7rem → 0.95rem・トラッキング 0.08em で**行番号と同じ柱に落とし込み**、
地名を 1.15rem → `clamp(1.25rem, 2vw, 1.6rem)` に上げて主従を逆転。
数字が地名より大きい状態を解消し、雑誌の柱らしい静かな見出しに。モバイルは 40px 柱。

### 5. スクロールリズム

均質だった間に強弱をつけた。Field Note の前後が最も広い。

- `.feature-teaser`: 余白なし → `clamp(56px,10vh,132px)` の上下パディング（記事導線の前後を最も広く）
- `.walk`: `padding-top:64px` → `clamp(76px,12vh,156px)`
- `.divider`: `56px` → `clamp(64px,9vh,108px)`（ただし最初の1本だけは見出し直下なので詰める）
- `.list` 下端: `14vh` → `16vh`

### 6. パフォーマンス

スクロールハンドラを **read → write の 1 本の rAF ループ** に統一。
以前は `updateHero()`（同期 write）→ 別 rAF の `computeFocus()`（read/write 交互）で
レイアウトを繰り返し無効化していた。計測もすべて `measure()` に集約し、
`scrollY` の読み出しも write 後から前へ移動。額縁ワイプの `void offsetWidth`
（強制同期レイアウト）は rAF 1 フレームでの状態確定に置き換え。
transform は `translate3d` / `scale` のみ。

同一スクロール（40px × 120 フレーム）での実測：

| | Layout | Style recalc | Script |
|---|---|---|---|
| before | 30 | 403 | 38ms |
| after | **15** | **210** | **24ms** |

レンズが収束したら rAF ループは自動停止する（アイドル時は 0 コスト）。

### 動作確認済み

Chromium 1440×900 / 390×844、`prefers-reduced-motion: reduce`、フィルタ連打。
JS エラーなし。`prefers-reduced-motion` 時はスワップアニメーションを飛ばして
即時に表示を切り替える。
