# スターターキット

- [イントロダクション](#introduction)
- [Laravel Breeze](#laravel-breeze)
    - [インストール](#laravel-breeze-installation)
    - [BreezeとInertia](#breeze-and-inertia)
    - [BreezeとNext.js／API](#breeze-and-next)
- [Laravel Jetstream](#laravel-jetstream)

<a name="introduction"></a>
## イントロダクション

新しいLaravelアプリケーションの構築をすぐに取りかかれるようするため、認証とアプリケーションのスターターキットを提供しています。これらのキットはアプリケーションのユーザーを登録および認証するために必要なルート、コントローラ、ビューを自動的にスカフォールドします。

皆さんがこうしたスターターキットを使用してくれるのは大歓迎ですが、これらは必須でありません。Laravelの真新しいコピーをインストールするだけで、自分自身のアプリケーションを自由にゼロから構築できます。いずれにせよ、みなさんが素晴らしいものを作り上げるのはわかっています！

<a name="laravel-breeze"></a>
## Laravel Breeze

[Laravel Breeze](https://github.com/laravel/breeze)へログイン、ユーザー登録、パスワードのリセット、メールの検証、パスワードの確認など、Laravelのすべての[認証機能](/docs/{{version}}/authentication)を最小限シンプルに実装しました。Laravel Breezeのデフォルトビュー層は、[Tailwind CSS](https://tailwindcss.com)でスタイルを設定したシンプルな[Bladeテンプレート](/docs/{{version}}/blade)で構成しています。

Breezeは、新しいLaravelアプリケーションを始めるための素晴らしい出発点となり、Bladeテンプレートを[Laravel Livewire](https://laravel-livewire.com)を使用し、レベルを上げる計画をしているプロジェクトにも最適な選択肢です。

<a name="laravel-breeze-installation"></a>
### インストール

まず、[新しいLaravelアプリケーションを作成](/docs/{{version}}/installation)し、データベースを設定後、[データベースのマイグレーション](/docs/{{version}}/migrations)を実行する必要があります。

```shell
curl -s https://laravel.build/example-app | bash

cd example-app

php artisan migrate
```

新しいLaravelアプリケーションを作成したら、Composerを使用してLaravel Breezeをインストールします。

```shell
composer require laravel/breeze --dev
```

ComposerでLaravel Breezeパッケージをインストールしたら、`breeze:install` Artisanコマンドを実行します。このコマンドは、認証ビュー、ルート、コントローラ、およびその他のリソースをアプリケーションにリソース公開します。Laravel Breezeは、その機能と実装を完全に制御し目に見えるようにするために、すべてのコードをアプリケーションへリソース公開します。Breezeをインストールしたら、アプリケーションのCSSファイルを使用できるようにアセットをコンパイルする必要もあります。

```shell
php artisan breeze:install

npm install
npm run dev
php artisan migrate
```

次に、Webブラウザでアプリケーションの`/login`または`/register`のURLにアクセスしてください。Breezeのすべてのルートは、`routes/auth.php'ファイル内に定義しています。

> {tip} アプリケーションのCSSとJavaScriptのコンパイルの詳細は、[Laravel Mixドキュメント](/docs/{{version}}/mix#running-mix)をご覧ください。

<a name="breeze-and-inertia"></a>
### BreezeとInertia

Laravel Breezeでは、VueやReactを使った[Inertia.js](https://inertiajs.com)のフロントエンド実装も提供しています。Inertiaスタックを使用するには、`breeze:install` Artisanコマンドを実行する際に、希望するスタックとして`vue`または`react`を指定します。

```shell
php artisan breeze:install vue

// もしくは…

php artisan breeze:install react

npm install
npm run dev
php artisan migrate
```

<a name="breeze-and-next"></a>
### BreezeとNext.js／API

Laravel Breezeは、[Next](https://nextjs.org)や[Nuxt](https://nuxtjs.org)などのモダンなJavaScriptアプリケーションで認証するAPIをスカフォールドすることもできます。使い始めるには、`breeze:install` Artisanコマンドを実行する時、`api`スタックを希望するスタックとして指定します。

```shell
php artisan breeze:install api

php artisan migrate
```

インストール中、Breezeはアプリケーションの`.env`ファイルへ、`FRONTEND_URL`環境変数を追加します。このURLは、JavaScriptアプリケーションのURLである必要があります。ローカル開発では、これは通常`http://localhost:3000`になります。さらに、`APP_URL`を`http://localhost:8000`に設定する必要があります。これは、`serve` Artisanコマンドで使用されるデフォルトのURLです。

<a name="next-reference-implementation"></a>
#### Next.jsリファレンス実装

ついに、このバックエンドとお好みのフロントエンドを組み合わせる準備ができました。BreezeフロントエンドのNextリファレンス実装は[GitHubで公開](https://github.com/laravel/breeze-next)しています。このフロントエンドはLaravelがメンテナンスし、Breezeが提供する従来のBladeスタックやInertiaスタックと同じユーザーインターフェイスを備えています。

<a name="laravel-jetstream"></a>
## Laravel Jetstream

Laravel Breezeは、Laravelアプリケーションを構築するためのシンプルで最小限の開始点を提供しますが、Jetstreamはより堅牢な機能と、追加のフロントエンドテクノロジースタックで、その機能を強化します。**Laravelを初めて使用する場合は、Laravel Jetstreamへ進む前に、Laravel Breezeで勘所を掴むことをおすめします。**

Jetstreamは、Laravelに美しく設計されたアプリケーションのスカフォールドを提供し、ログイン、ユーザー登録、メール検証、２要素認証、セッション管理、Laravel Sanctumを介したAPIサポート、およびオプションとしてチーム管理機能を含みます。Jetstreamは[TailwindCSS](https://tailwindcss.com)を使用して設計されており、[Livewire](https://laravel-livewire.com)（[日本語](/livewire/2.x/ja/quickstart.html)）もしくは[Inertia.js](https://inertiajs.com)駆動のフロントエンドスカフォールドから選択できます。

Laravel Jetstreamをインストールするための完全なドキュメントは、[公式Jetstreamドキュメント](https://jetstream.laravel.com/2.x/introduction.html)にあります。
