# Next.jsについて
## Next.jsとは
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

## Next.jsの始め方
```bash
npm init next-app nextjs-blog
```

Defaultにするか、Templateを使うか聞かれる。
--exampleで使うtemplateを指定できる。

```bash
npm init next-app nextjs-blog --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

### 開発サーバーを動かす
Nuxtと同じ。
```
# npm
npm run dev

# yarn
yarn dev
```