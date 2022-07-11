# アセットの構築（Vite）

- [イントロダクション](#introduction)
- [インストールと準備](#installation)
  - [Nodeのインストール](#installing-node)
  - [ViteとLaravelプラグインのインストール](#installing-vite-and-laravel-plugin)
  - [Viteの設定](#configuring-vite)
  - [スクリプトとスタイルの読み込み](#loading-your-scripts-and-styles)
- [Viteの実行](#running-vite)
- [JavaScriptの操作](#working-with-scripts)
  - [エイリアス](#aliases)
  - [Vue](#vue)
  - [React](#react)
  - [Inertia](#inertia)
  - [URL処理](#url-processing)
- [スタイルシートの操作](#working-with-stylesheets)
- [ベースURLのカスタマイズ](#custom-base-urls)
- [環境変数](#environment-variables)
- [サーバサイドレンダリング(SSR)](#ssr)

<a name="introduction"></a>
## イントロダクション

[Vite](https://vitejs.dev)は、非常に高速な開発環境を提供してくれる、コードを本番用に構築する最新のフロントエンド・ビルド・ツールです。Laravelでアプリケーションを構築する場合、通常、Viteを使用してアプリケーションのCSSとJavaScriptファイルを本番環境用のアセットへ構築することになります。

Laravelは、開発および実働用アセットをロードするため、公式プラグインとBladeディレクティブを提供し、Viteをシームレスに統合しています。

> {tip} Laravel Mixを実行していますか？新しいLaravelのインストールでは、Laravel MixをViteへ置き換えました。Mixのドキュメントは、[Laravel Mix](https://laravel-mix.com/)のウェブサイトをご覧ください。Viteに切り替えたい場合は、[移行ガイド](https://github.com/laravel/vite-plugin/blob/main/UPGRADE.md#migrating-from-laravel-mix-to-vite)を参照してください。

<a name="vite-or-mix"></a>
#### ViteとLaravel Mixの選択

Viteへ移行する前、新しいLaravelアプリケーションは、アセットをバンドルする際に[webpack](https://webpack.js.org/)で動作する[Mix](https://laravel-mix.com/)を使用していました。Viteは、リッチなJavaScriptアプリケーションを構築する際に、より速く、より生産的な体験を提供することに重点を置いています。[Inertia](https://inertiajs.com) のようなツールで開発したものを含め、シングルページアプリケーション（SPA）を開発している場合、Viteは完璧にフィットするでしょう。

Viteは、[Livewire](https://laravel-livewire.com)を使用したものを含む、JavaScriptを「ふりかけ」程度に使った従来のサーバサイドレンダリングアプリケーションでもうまく機能します。しかし、Laravel Mixがサポートしている、JavaScriptアプリケーションで直接参照されていない任意のアセットをビルドにコピーする機能など、いくつかの機能が欠落しています。

<a name="migrating-back-to-mix"></a>
#### Mixへ戻す

Vite scaffoldingを使用して新しいLaravelアプリケーションを開始したが、Laravel Mixとwebpackへ戻る必要があるのですか？大丈夫です。[ViteからMixへの移行に関する公式ガイド](https://github.com/laravel/vite-plugin/blob/main/UPGRADE.md#migrating-from-vite-to-laravel-mix)を参照してください。

<a name="installation"></a>
## インストールと準備

> {tip} 以下のドキュメントでは、Laravel Viteプラグインを手作業でインストールし、設定する方法について説明しています。しかし、Laravelの[スターターキット](/docs/{{version}}/starter-kits)には、すでにこのスカフォールドがすべて含まれており、LaravelとViteを始める最速の方法を用意しています。

<a name="installing-node"></a>
### Nodeのインストール

ViteとLaravelプラグインを実行する前に、Node.jsとNPMを確実にインストールしてください。

```sh
node -v
npm -v
```

NodeとNPMの最新版は、[Node公式サイト](https://nodejs.org/en/download/)からグラフィカルインストーラを使って簡単にインストールできます。また、[Laravel Sail](/docs/{{version}}/sail)を使用している場合は、SailからNodeとNPMを呼び出せます。

```sh
./vendor/bin/sail node -v
./vendor/bin/sail npm -v
```

<a name="installing-vite-and-laravel-plugin"></a>
### ViteとLaravelプラグインのインストール

Laravelを新規にインストールすると、アプリケーションのディレクトリ構造のルートに`package.json`ファイルができます。デフォルトの`package.json`ファイルは、ViteとLaravelプラグインを使い始めるために必要なものを既に含んでいます。アプリケーションのフロントエンドの依存関係は、NPM経由でインストールできます。

```sh
npm install
```

<a name="configuring-vite"></a>
### Viteの設定

Viteは、プロジェクトルートの`vite.config.js`ファイルで設定します。また、アプリケーションが必要とする他のプラグイン、例えば、`@vitejs/plugin-vue`や`@vitejs/plugin-react`をインストールすることもできます。

Laravel Viteプラグインでは、アプリケーションのエントリーポイントを指定する必要があります。これらは、JavaScriptまたはCSSファイルであり、TypeScript、JSX、TSX、Sassなどのプリプロセス言語が含まれます。

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel([
            'resources/css/app.css',
            'resources/js/app.js',
        ]),
    ],
});
```

Inertiaを使用したアプリケーションを含むSPAを構築する場合、ViteはCSSエントリポイントなしで最適に動作します。

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel([
            'resources/css/app.css', // [tl! 削除]
            'resources/js/app.js',
        ]),
    ],
});
```

代わりに、JavaScriptでCSSをインポートする必要があります。通常、これはアプリケーションの `resources/js/app.js` ファイルで行います。

```js
import './bootstrap';
import '../css/app.css'; // [tl! 追加]
```

また、Laravelプラグインは複数のエントリーポイントに対応し、[SSRエントリーポイント](#ssr)などの高度な設定オプションにも対応しています。

<a name="working-with-a-secure-development-server"></a>
#### セキュアな開発サーバの取り扱い

Valetの[セキュアコマンド](/docs/{{version}}/valet#securing-sites)を含め、開発用WebサーバがHTTPSで動作している場合、Vite開発サーバへの接続に問題が発生する可能性があります。Vite設定ファイルの`vite.config.js`へ以下を追加すれば、ViteをHTTPSでも動作するように設定できます。

```js
export default defineConfig({
    // ...
    server: { // [tl! 追加]
        https: true, // [tl! 追加]
        host: 'localhost', // [tl! 追加]
    }, // [tl! 追加]
});
```

また、`npm run dev`コマンドを実行する際に、コンソールの「Local」リンクをたどり、ブラウザでViteの開発サーバの証明書の警告を受け入れる必要があります。

<a name="loading-your-scripts-and-styles"></a>
### スクリプトとスタイルの読み込み

Viteのエントリーポイントを設定したら、アプリケーションのルートテンプレートの`<head>`へ追加する、`@vite()` Bladeディレクティブで参照するだけです。

```blade
<!doctype html>
<head>
    {{-- ... --}}

    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
```

JavaScriptでCSSをインポートする場合は、JavaScriptのエントリーポイントのみを記載するだけです。

```blade
<!doctype html>
<head>
    {{-- ... --}}

    @vite('resources/js/app.js')
</head>
```

`@vite` ディレクティブは、Vite開発サーバを自動的に検出し、Viteクライアントを注入してホットモジュール置換を有効にします。ビルドモードでは、このディレクティブはインポートしたCSSを含む、コンパイル済みのバージョン管理しているアセットを読み込みます。

<a name="running-vite"></a>
## Viteの実行

Viteを起動する方法は2つあります。`dev`コマンドで開発サーバを起動するのは、ローカルで開発する際に便利です。開発サーバは、ファイルの変更を自動的に検出し、開いているブラウザウィンドウへ即座に反映させます。

もしくは、`build`コマンドを実行すると、アプリケーションのアセットをバージョン付けして構築し、本番環境にデプロイできる状態にします。

```shell
# Viteを開発サーバで実行する
npm run dev

# 実働用にアセットをバンドルし、バージョン付けする
npm run build
```

<a name="working-with-scripts"></a>
## JavaScriptの操作

<a name="aliases"></a>
### エイリアス

デフォルトでLaravelプラグインは、アプリケーションのアセットを便利にインポートできるように、共用エイリアスを提供します。

```js
{
    '@' => '/resources/js'
}
```

設定ファイル`vite.config.js`に独自のエイリアスを追加すれば、`'@'`エイリアスを上書きできます。

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel(['resources/ts/app.tsx']),
    ],
    resolve: {
        alias: {
            '@': '/resources/ts',
        },
    },
});
```

<a name="vue"></a>
### Vue

LaravelプラグインでVueプラグインを使用する場合、`vite.config.js`設定ファイルへ追加オプションをいくつか追加する必要があります。

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    plugins: [
        laravel(['resources/js/app.js']),
        vue({
            template: {
                transformAssetUrls: {
                    // Vueプラグインは、Single File Componentsで
                    // 参照する場合、アセットのURLをLaravelのWebサーバを
                    // 指すように書き換えます。
                    // これを`null`に設定すると、Laravelプラグインは
                    // アセットURLをViteサーバを指すように書き換えます。
                    base: null,

                    // Vueプラグインは、絶対URLを解析し、ディスク上のファイルへの
                    // 絶対パスとして扱います。
                    // これを`false`に設定すると、絶対URLはそのままになり、
                    // 期待通りに公開されているアセットを直接参照できるようになります。
                    includeAbsolute: false,
                },
            },
        }),
    ],
});
```

> {tip} Laravelの[スターターキット](/docs/{{version}/starter-kits)には、すでに適切なLaravel、Vue、Viteの構成が含まれています。Laravel、Vue、Viteを最速で使い始めるには、[Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-inertia)をチェックしてください。

<a name="react"></a>
### React

ViteをReactで使用する場合、JSXを含むすべてのファイルに`.jsx`または`.tsx`拡張子が付いていることを確認し、[上記](#configuring-vite)のように、必要に応じてエントリポイントを更新することを忘れないようにする必要があります。また、既存の`@vite`ディレクティブとともに追加の`@viteReactRefresh` Bladeディレクティブを含める必要もあります。

```blade
@viteReactRefresh
@vite('resources/js/app.jsx')
```

`@viteReactRefresh`ディレクティブは、`@vite`ディレクティブの前に呼び出す必要があります。

> {tip} Laravelの[スターターキット](/docs/{{version}}/starter-kits)には、すでに適切なLaravel、React、Viteの設定が含まれています。Laravel、React、Viteを最速で始めるには、[Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-inertia) をチェックしてください。

<a name="inertia"></a>
### Inertia

Laravel Viteプラグインは、Inertiaページコンポーネントを解決するのに便利な `resolvePageComponent` 関数を提供しています。以下はVue 3で使用するヘルパの例ですが、Reactなど他のフレームワークでもこの関数を利用することができます。

```js
import { createApp, h } from 'vue';
import { createInertiaApp } from '@inertiajs/inertia-vue3';
import { resolvePageComponent } from 'laravel-vite-plugin/inertia-helpers';

createInertiaApp({
  resolve: (name) => resolvePageComponent(`./Pages/${name}.vue`, import.meta.glob('./Pages/**/*.vue')),
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el)
  },
});
```

> {tip} Laravelの[スターターキット](/docs/{{version}}/starter-kits)には、すでに適切なLaravel、Inertia、Viteの構成が含まれています。Laravel、Inertia、Viteを最速で始めるには、[Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-inertia) をチェックしてください。

<a name="url-processing"></a>
### URL処理

Viteを使用して、アプリケーションのHTML、CSS、JSアセットを参照する場合、いくつか考慮すべき点があります。まず、絶対パスでアセットを参照した場合、Viteはそのアセットをビルドに含めません。したがって、そのアセットがパブリックディレクトリで利用可能であることを確認する必要があります。

相対パスで参照する場合、参照するファイルからの相対パスであることを覚えておく必要があります。相対パスで参照されたアセットは、Viteによって書き直され、バージョン付けされ、バンドルされます。

以下のようなプロジェクト構成を考えてみましょう。

```nothing
public/
  taylor.png
resources/
  js/
    Pages/
      Welcome.vue
  images/
    abigail.png
```

以下の例は、Viteが相対URLと絶対URLをどのように扱うかを示しています。

```html
<!-- このアセットをViteは扱わず、バンドルへ含まれない -->
<img src="/taylor.png">

<!-- このアセットは書き換えられ、バージョン付けし、バンドルへ含まれる -->
<img src="../../images/abigail.png">
```

<a name="working-with-stylesheets"></a>
## スタイルシートの操作

ViteのCSSサポートは、[Viteドキュメント](https://vitejs.dev/guide/features.html#css) で詳しく説明されています。[Tailwind](https://tailwindcss.com)のようなPostCSSプラグインを使用している場合、プロジェクトのルートに`postcss.config.js`ファイルを作成すると、Vite が自動的にそれを適用してくれます。

```js
module.exports = {
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    },
};
```

<a name="custom-base-urls"></a>
## ベースURLのカスタマイズ

ViteでコンパイルしたアセットをCDN経由など、アプリケーションとは別のドメインにデプロイする場合は、アプリケーションの`.env`ファイル内に`ASSET_URL`環境変数を指定する必要があります。

```env
ASSET_URL=https://cdn.example.com
```

アセットURLを設定すると、アセットを指す全ての書き換えられるURLの先頭に、設定した値が付きます。

```nothing
https://cdn.example.com/build/assets/app.9dce8d17.js
```

[絶対URLはViteで書き直されない](#url-processing)ため、プリフィックスが付かないことを覚えておいてください。

<a name="environment-variables"></a>
## 環境変数

アプリケーションの`.env`ファイルに、`VITE_` というプレフィックスを付ける環境変数を設置することで、それらの環境変数をJavaScriptへ注入できます。

```env
VITE_SENTRY_DSN_PUBLIC=http://example.com
```

注入された環境変数は、`import.meta.env`オブジェクトを介してアクセスできます。

```js
import.meta.env.VITE_SENTRY_DSN_PUBLIC
```

<a name="ssr"></a>
## サーバサイドレンダリング(SSR)

Laravel Viteプラグインを使用すると、Viteでサーバサイドレンダリングを簡単に設定できます。まず、SSRエントリーポイントを`resources/js/ssr.js`に作成し、Laravelプラグインに設定オプションを渡して、エントリーポイントを指定します。

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: 'resources/js/app.js',
            ssr: 'resources/js/ssr.js',
        }),
    ],
});
```

SSRエントリポイントの再構築を忘れないようにするために、アプリケーションの`package.json`にある"build"スクリプトを拡張して、SSRビルドを作成することをお勧めします。

```json
"scripts": {
     "dev": "vite",
     "build": "vite build" // [tl! 削除]
     "build": "vite build && vite build --ssr" // [tl! 追加]
}
```

最後に、SSRサーバの構築と起動のため、以下のコマンドを実行してください。

```sh
npm run build
node storage/ssr/ssr.js
```

> {tip} Laravelの[スターターキット](/docs/{{version}}/starter-kits)には、すでに適切なLaravel、Inertia SSR、Viteの構成が含まれています。Laravel、Inertia SSR、Viteを最速で使い始めるため、[Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-inertia) をチェックしてください。
