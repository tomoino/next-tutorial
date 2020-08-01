# Next.jsとは
Next.jsはReactのフレームワークの一つ。
以下のような特徴がある。

> * ページベースの直感的な**ルーティングシステム**（動的ルーティングをサポート）
> * プリレンダリングでは、静的生成（SSG）と**サーバサイドレンダリング（SSR）**の両方をページ単位でサポート
> * より速いページロードのための**自動コード分割**
> * 最適化されたプリフェッチングによるクライアントサイドルーティング
> * CSS と Sass のビルトインサポートおよびあらゆる CSS-in-JS ライブラリのサポート
> * HMR (Hot Module Replacement) をサポートする開発環境
> * サーバレス関数を用いて API エンドポイントを構築するための API ルート
> * 完全に拡張可能であるという点

# Next.jsの始め方
```bash
npm init next-app nextjs-blog
```

Defaultにするか、Templateを使うか聞かれる。
--exampleで使うtemplateを指定できる。

```bash
npm init next-app nextjs-blog --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

## 開発サーバーを動かす
Nuxtと同じ。 Hot Reloadingという特徴を持っており、更新したらすぐに反映される。
```
# npm
npm run dev

# yarn
yarn dev
```

# ルーティングとページ遷移
pages内の構成によってルーティングが決まる。
## ページ遷移
Linkタグを使う。
```js
import Link from 'next/link'

<Link href="/posts/first-post"><a>this page!</a></Link>
```
> You can learn more about the Link component [in the API reference documentation](https://nextjs.org/docs/api-reference/next/link) and routing in general [in the routing documentation](https://nextjs.org/docs/routing/introduction).

# 静的アセットの扱い
画像などはpublic配下に入れる。

# メタデータ
```js
import Head from 'next/head'

<Head>
    <title>Create Next App</title>
</Head>
```

# CSSスタイリング
Reactコンポーネント内でCSSを書くことができる。
styled-jsxというCSS-in-JSのライブラリを使用。
```html
<style jsx>{`
   ...
`}</style>
```

## Layout
すべてのページで共通して使う Layout コンポーネント。
components/layout.jsを作って、importしてもっとも外側のタグとして使う。

## CSSモジュール
React コンポーネントの中でインポートすることができる CSS ファイル。

CSS モジュールは自動的に一意のクラス名を生成するため、CSS モジュールを使っているかぎりは、クラス名の衝突を気にする必要はない。
* 拡張子は.module.cssとして、components内に作る。
* stylesとしてimportする。
```js
import styles from './layout.module.css'
```
* styles.class名 を className として使う
```js
export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

## グローバルなスタイリング：Appコンポーネントの利用
 CSS を すべての ページで読み込みたい場合、pages ディレクトリ配下に _app.js というファイルを作成する。Appコンポーネントは全ページ共通のトップレベルコンポーネント。
 ```js
export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
 ```
styles/global.cssを作成し、Appコンポーネントで読み込む。

## スタイリングに関するTips
* クラスを切り替えるために classnames ライブラリを使う。
  * [README](https://github.com/JedWatson/classnames)
```sh
npm install classnames # or yarn add classnames
```
* PostCSS
  * 設定不要でNext.js は CSS を PostCSS を使ってコンパイルする。
* Sass
  * 初期設定のまま.scss/.sass両方を使える。
  * CSS モジュールと .module.scss または .module.sass 拡張子を介して、コンポーネントレベルの Sass を使用することができる。
