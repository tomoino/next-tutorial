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

# プリレンダリングとデータフェッチング
## プリレンダリング
Next.js はデフォルトで全ページをプリレンダリングする。あらかじめ各ページのHTMLを生成しておくことでパフォーマンスとSEOを向上させる。
* Nextでは、プレレンダリングにより初期HTMLが生成され、ページが読み込まれるとJSによりアプリがインタラクティブになる(ハイドレーション)。
* プレーンなReactではクライアントのJSによってレンダリングされる。

## プリレンダリングの種類
Nextでは以下の方法をページ単位で指定できる。
* 静的生成：ビルド時にHTMLを生成する。
* SSR：毎回のリクエストごとにHTMLを生成する。

## 静的生成とSSRの使い分け
* 静的生成の方が高速（一度ビルドされたら CDN によって提供されるため）。
* ユーザーのリクエストに先立ってプリレンダリングできないページはSSR。
  * 頻繁に更新されるデータを表示するページ
  * 毎回のリクエストごとに内容が変わるページ

## 静的生成とデータ取得
静的生成は外部データの有無にかかわらず使用できる。
* 外部データ無しの場合：ビルド時に外部データを取得しないで生成できるページの場合。
* 外部データ有りの場合：ビルド時に外部データを取得しないと生成できないページの場合。

## データ有り静的生成とgetStaticProps
Next.js では、ページコンポーネントを export するとき、getStaticProps という async 関数も export することができる。
* getStaticPropsはビルド時に実行される
* 関数内部では、外部データを取得して、props としてページに渡すことができる。
```js
export default function Home(props) { ... }

export async function getStaticProps() {
    // ファイルシステムや API、DB などから外部データを取得する
    cons
```

# 動的ルーティング
Nextでは、外部データに依存するパスを持ったページを静的に生成することができる。
## 実装手順
1. pages/以下に[id].jsのような括弧で囲まれたファイル名のjsファイルを作る。(動的なページ)
2. id として とりうる値 のリストを返す getStaticPaths という async 関数を export する。
3. getStaticProps で受け取った id に基づいて必要なデータを取得する。
```js
import Layout from '../../components/layout'

export default function Post() {
  return <Layout>...</Layout>
}

export async function getStaticPaths() {
  // id としてとりうる値のリストを返す
}

export async function getStaticProps({ params }) {
  // params.id を使用して、ブログの投稿に必要なデータを取得する
}
```

## markdownのレンダー
remarkライブラリを使う。
```
npm install remark remark-html
```
```js
import remark from 'remark'
import html from 'remark-html'
```

# APIルート
Next.js アプリの中に API エンドポイントを作成することができる。
pages/api下にファイルを作る。

```js
// req = リクエストデータ, res = レスポンスデータ
export default (req, res) => {
  // ...
}
```

# デプロイ
Vercelにデプロイするのが簡単。
* [Sign up](https://vercel.com/signup)
* [リポジトリをimportする](https://vercel.com/import/git)

# TypeScript
```
touch tsconfig.json
yarn add --dev typescript @types/react @types/node
yarn dev
touch next-env.d.ts
```
