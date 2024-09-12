---
title: "Svelte 5 Runes はなぜ必要なのか"
emoji: "🤔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["svelte"]
published: false
---

# はじめに

`Svelte` 5 では、`Runes` という新しい概念が導入されました。
この記事では、`Runes` とは何か、なぜ必要なのかについて説明します。

https://svelte-5-preview.vercel.app/docs/runes

また、`Svelte` の書き方がどう変わるか、他のフレームワークとの比較については、以下のサイトで確認できます。
本記事と併せてご覧ください。
https://component-party.dev/compare/svelte4-vs-svelte5

# `Svelte` 3/4 における Reactivity について

`Svelte` 3/4 では、Reactivity は以下のように実現されていました。

```html
<script>
  let count = 0;
</script>

<button on:click={() => count += 1}>
  Clicked {count} times
</button>
```

`Svelte` 3/4 では、`let` で変数を top-level に宣言することで、その変数が自動的に Reactivity になります。
例えば、上のコードでは、`count` が変更されると、ボタンのテキストも自動的に更新されます。
これが良くも悪くも、`Svelte` 3/4 のわかりやすさの象徴と言えるでしょう。
(嫌いな人は嫌いらしいですが)

また、`export let` という書き方で、外部から変数を受け取ることもできます。

```html
<!-- Count.svelte -->
<script>
  export let count = 0;
</script>

<p>{count}</p>
```

```html
<!-- App.svelte -->
<script>
  import Count from './Count.svelte';
</script>

<Count count={10} />
```

個人的にはこの書き方が好きでした。
（とはいえこれも気持ち悪いと感じる人は多くいるようです）

:::details 余談: Svelte 3 の Reactivity はどこから来たのか
この `let` で変数を top-level に宣言し、それが自動的に Reactivity になるという仕組みは、`React Hooks` から着想されたものです。

以下の一連のツイートが `Svelte 3` の Reactivity 誕生の瞬間です。

https://x.com/youyuxi/status/1057148450519871489
https://x.com/Rich_Harris/status/1057226723601743872
https://x.com/Rich_Harris/status/1057290365395451905

この書き方は、`Vue 3` の `ref` へ影響を与えることになります。
そして、 `ref` は `Svelte` 5 の `Runes` へと繋がっていきます。
:::

# `Svelte` 4 までの問題点

