# アップグレードガイド

- [８．ｘから９．０へのアップグレード](#upgrade-9.0)

<a name="high-impact-changes"></a>
## 重要度の高い変更

<div class="content-list" markdown="1">

- [依存パッケージの更新](#updating-dependencies)
- [Flysystem3.x](#flysystem-3)
- [Symfony Mailer](#symfony-mailer)

</div>

<a name="medium-impact-changes"></a>
## 重要度が中程度の変更

<div class="content-list" markdown="1">

- [Belongs To Many `firstOrNew`, `firstOrCreate`, and `updateOrCreate` methods](#belongs-to-many-first-or-new)
- [Custom Casts & `null`](#custom-casts-and-null)
- [Default HTTP Client Timeout](#http-client-default-timeout)
- [PHP Return Types](#php-return-types)
- [Postgres "Schema" Configuration](#postgres-schema-configuration)
- [The `assertDeleted` Method](#the-assert-deleted-method)
- [The `lang` Directory](#the-lang-directory)
- [The `password` Rule](#the-password-rule)
- [The `when` / `unless` Methods](#when-and-unless-methods)
- [Unvalidated Array Keys](#unvalidated-array-keys)

</div>

<a name="upgrade-9.0"></a>
## ８．ｘから９．０へのアップグレード

<a name="estimated-upgrade-time-10-minutes"></a>
#### アップグレード時間の見積もり：３０分

> {tip} 私たちは、互換性のない変更を可能な限りすべて文書化するよう努力しています。これらの互換性のない変更の一部はフレームワークの不明瞭な部分であり、こうした変更の一部が実際にあなたのアプリケーションに影響を与える可能性があります。時間を節約したいですか？[Laravel Shift](https://laravelshift.com/) を使用すると、アプリケーションのアップグレードを自動化できます。

<a name="updating-dependencies"></a>
### 依存パッケージのアップデート

**影響の可能性： 高い**

#### PHP8.0.2が必要

今回より、Laravelを動作させるには、PHP8.0.2以上が必要になりました。

#### Composerの依存パッケージ

アプリケーションの`composer.json`ファイルにある、以下の依存パッケージを更新してください。

<div class="content-list" markdown="1">

- `laravel/framework`を`^9.0`に変更
- `nunomaduro/collision`を`^6.1`に変更

</div>

さらに、アプリケーションの`composer.json`ファイルの中の`facade/ignition`は、`"spatie/laravel-ignition": "^1.0"`へ置き換えてください。

さらに、以下のファーストパーティパッケージは、Laravel9.x をサポートするために新しくメジャーリリースしました。当てはまる場合、Laravelをアップグレードする前に、それぞれのアップグレードガイドを読んでください。

<div class="content-list" markdown="1">

- [Vonage通知チャンネル（v3.0）](https://github.com/laravel/vonage-notification-channel/blob/3.x/UPGRADE.md) (Nexmoの代替)

</div>

最後に、アプリケーションで使用している他のサードパーティパッケージを調べ、Laravel9をサポートする適切なバージョンを使用していることを確認してください。

<a name="php-return-types"></a>
#### PHPの戻り値タイプ

PHPでは、`offsetGet`や`offsetSet`などのPHPメソッドへ、戻り値のタイプの定義を義務付ける方向に移行し始めています。これを踏まえ、Laravel9ではコードベースにこれらの戻り値の型を実装しています。通常、ユーザーが書いたコードには影響しませんが、Laravelのコアクラスを拡張してこれらのメソッドをオーバーライドしている場合は、これらの戻り値の型を自分のアプリケーションやパッケージのコードに追加することが必要になります。

<div class="content-list" markdown="1">

- `count(): int`
- `getIterator(): Traversable`
- `getSize(): int`
- `jsonSerialize(): array`
- `offsetExists($key): bool`
- `offsetGet($key): mixed`
- `offsetSet($key, $value): void`
- `offsetUnset($key): void`

</div>

さらに、PHPの`SessionHandlerInterface`を実装したメソッドに戻り値のタイプが追加されました。繰り返しますが、この変更があなた自身のアプリケーションやパッケージのコードに、影響を与えることはまずないでしょう。

<div class="content-list" markdown="1">

- `open($savePath, $sessionName): bool`
- `close(): bool`
- `read($sessionId): string|false`
- `write($sessionId, $data): bool`
- `destroy($sessionId): bool`
- `gc($lifetime): int`

</div>

<a name="application"></a>
### アプリケーション

<a name="the-application-contract"></a>
#### `Application`契約

**影響の可能性： 低い**

`Illuminate\Contracts\Foundation\Application`インターフェイスの`storagePath`メソッドを更新し、`$path`引数を取るようにしました。このインターフェイスを実装している場合、それに応じて実装を更新する必要があります。

    public function storagePath($path = '');

同様に、`Illuminate\Foundation\Application`クラスの`langPath`メソッドを更新し、`$path`引数を取るようになりました。

    public function langPath($path = '');

#### 例外ハンドラの`ignore`メソッド

**影響の可能性： 低い**

例外ハンドラの`ignore`メソッドが、`protected`から`public`へ変更しました。このメソッドはデフォルトのアプリケーションスケルトンには含まれていません。しかし、もしこのメソッドを独自に定義している場合は、可視性を`public`へ更新する必要があります。

```php
public function ignore(string $class);
```

### Blade

#### レイジーコレクションと`$loop`変数

**影響の可能性： 低い**

Bladeテンプレート内の`LazyCollection`インスタンスを反復処理すると`$loop`変数は使用できなくなります。この変数にアクセスすると、`LazyCollection`全体がメモリに読み込まれるため、このシナリオではレイジーコレクションの使用が無意味になるためです。

### コレクション

#### `Enumerable`契約

**影響の可能性： 低い**

`Illuminate\Support\Enumerable`契約に、`sole`メソッドを定義しました。このインターフェイスを実装している場合は、この新しいメソッドを反映するよう実装を更新してください。

```php
public function sole($key = null, $operator = null, $value = null);
```

#### `reduceWithKeys`メソッド

`reduce`メソッドが同じ機能を提供しているため、`reduceWithKeys`メソッドを削除しました。`reduceWithKeys`の代わりに、`reduce`を呼び出すようにコードを更新するだけです。

#### `reduceMany`メソッド

`reduceMany`メソッドは、他の同様のメソッドと名前の一貫性を保つため、`reduceSpread`へ名前を変更しました。

### コンテナ

#### `Container`契約

**影響の可能性： とても低い**

`Illuminate\Contracts\Container\Container`契約は、`scoped`と`scopedIf`の２メソッド定義を受けるようにしました。この契約を実装している場合は、これらの新しいメソッドを反映するように実装を更新してください。

#### `ContextualBindingBuilder`契約

**影響の可能性： とても低い**

`Illuminate\Contracts\Container\ContextualBindingBuilder`契約で、`giveConfig`メソッドを定義しました。このインターフェイスを実装している場合は、この新しいメソッドを反映するように更新してください。

```php
public function giveConfig($key, $default = null);
```

### データベース

<a name="postgres-schema-configuration"></a>
#### Postgres "Schema"設定

**影響の可能性： 中程度**

アプリケーションの`config/database.php`設定ファイルで、Postgres接続検索パスを設定するために使用する`schema`設定オプションの名前を`search_path`へ変更してください。

<a name="schema-builder-doctrine-method"></a>
#### Schema Builder `registerCustomDoctrineType` Method

**影響の可能性： 低い**

`registerCustomDoctrineType`メソッドは、`Illuminate\Database\Schema\Builder`クラスから削除しました。代わりに、`DB`ファサードで、`registerDoctrineType`メソッドを使用するか、`config/database.php`設定ァイルで、カスタムDoctrineタイプを登録できます。

### Eloquent

<a name="custom-casts-and-null"></a>
#### カスタムキャストと`null`

**影響の可能性： 中程度**

Laravel の以前のリリースでは、カスタムキャストクラスの`set`メソッドは、キャスト属性が`null`に設定されている場合には呼び出しませんでした。しかし、この動作はLaravelのドキュメントと矛盾していました。Laravel9.xでは、キャストクラスの`set`メソッドは`$value`引数に`null`を指定しても起動されます。したがって、カスタムキャストがこのシナリオを確実に処理できるようにしてください。

```php
/**
 * 指定値を保存のために準備
 *
 * @param  \Illuminate\Database\Eloquent\Model  $model
 * @param  string  $key
 * @param  AddressModel  $value
 * @param  array  $attributes
 * @return array
 */
public function set($model, $key, $value, $attributes)
{
    if (! $value instanceof AddressModel) {
        throw new InvalidArgumentException('The given value is not an Address instance.');
    }

    return [
        'address_line_one' => $value->lineOne,
        'address_line_two' => $value->lineTwo,
    ];
}
```

<a name="belongs-to-many-first-or-new"></a>
#### Belongs To Manyの`firstOrNew`、`firstOrCreate`、`updateOrCreate`メソッド

**影響の可能性： 中程度**

`belongsToMany`リレーションシップの`firstOrNew`、`firstOrCreate`、`updateOrCreate`メソッドの最初の引数に、属性の配列が渡されます。Laravelの以前のリリースでは、この属性の配列は、既存のレコードの「ピボット」／中間テーブルと比較されていました。

しかし、この動作は期待されておらず、一般的に望まれないものでした。代わりに、これらのメソッドは関連モデルのテーブルに対して属性の配列を比較するようになりました。

```php
$user->roles()->updateOrCreate([
    'name' => 'Administrator',
]);
```

さらに、`firstOrCreate`メソッドの第２引数に、`$values`配列を取るようになりました。この配列は、関連モデルが存在していない場合に、メソッドの第１引数（`$attributes`）へマージします。この変更により、このメソッドは他のリレーションタイプで提供している`firstOrCreate`メソッドと動作が一致するようになりました。

```php
$user->roles()->firstOrCreate([
    'name' => 'Administrator',
], [
    'created_by' => $user->id,
]);
```

#### `touch`メソッド

**影響の可能性： 低い**

`touch`メソッドへ、属性を指定できるようになりました。以前このメソッドを上書きしていた場合は、この新しい引数を反映させるためにメソッドの引数指定を変更してください。

```php
public function touch($attribute = null);
```

### 暗号化

#### Encrypter契約

**影響の可能性： 低い**

`Illuminate\Contracts\Encryption\Encrypter`契約で、`getKey`メソッドを定義しました。このインターフェイスを実装している場合は、それに応じて更新してください。

```php
public function getKey();
```

### ファサード

#### `getFacadeAccessor`メソッド

**影響の可能性： 低い**

`getFacadeAccessor`メソッドは常にコンテナ結合キーを返す必要があります。Laravelの以前のリリースでは、このメソッドはオブジェクトのインスタンスを返すことができましたが、この動作はもうサポートしません。独自のファサードを記述している場合は、このメソッドがコンテナ結合文字列を返すようにしなければなりません。

```php
/**
 * コンポーネントの登録名取得
 *
 * @return string
 */
protected static function getFacadeAccessor()
{
    return Example::class;
}
```

### ファイルシステム

#### `FILESYSTEM_DRIVER`環境変数

**影響の可能性： 低い**

`FILESYSTEM_DRIVER`環境変数は、より正確にその使用目的を反映するため、`FILESYSTEM_DISK`へ名前を変更しました。この変更はアプリケーションのスケルトンにしか影響しませんが、もし望むなら、あなた自身のアプリケーションの環境変数を更新し、この変更を反映させてもかまいません。

#### "Cloud"ディスク

**影響の可能性： 低い**

ディスク設定オプションの`cloud`は、２０２０年１１月にデフォルトのアプリケーションスケルトンから削除しました。この変更は、アプリケーションのスケルトンにのみ影響します。アプリケーション内で`cloud`ディスクを使用している場合は、この設定値を自分のアプリケーションのスケルトンに残しておく必要があります。

<a name="flysystem-3"></a>
### Flysystem 3.x

**影響の可能性： 高い**

Laravel9.xは、[Flysystem](https://flysystem.thephpleague.com/v2/docs/)1.xから3.xへ移行しました。Flysystem は、`Storage`ファサードが提供するすべてのファイル操作メソッドを裏で提供しています。これにより、あなたのアプリケーションでいくらかの変更が必要になるかもしれません。しかし、私たちはこの移行を可能な限りシームレスに行えるように努めました。

#### ドライバ要件

S3、FTP、SFTPドライバーを使用する前に、Composerパッケージ・マネージャーで適切なパッケージをインストールする必要があります。

- Amazon S3: `composer require -W league/flysystem-aws-s3-v3 "^3.0"`
- FTP: `composer require league/flysystem-ftp "^3.0"`
- SFTP: `composer require league/flysystem-sftp-v3 "^3.0"`

#### 既存ファイルの上書き

`put`、`write`、`writeStream`などの書き込み操作は、デフォルトで既存のファイルを上書きするようになりました。既存のファイルを上書きしたくない場合は、書き込み操作を行う前に、ファイルの存在を確認してください。

#### 存在しないファイルからの読み込み

存在しないファイルから読み込もうとすると、`null`を返すようにしました。以前のLaravelのリリースでは、`Illuminate\Contracts\Filesystem\FileNotFoundException`を投げていました。

#### 存在しないファイルの削除

Attempting to `delete` a file that does not exist now returns `true`.
存在しないファイルを`delete`しようとすると、`true`を返すようにしました。

#### キャッシュ済みアダプタ

Flysystemは、「キャッシュ済みアダプタ」をサポートしなくなりました。そのため、Laravel から削除し、関連する設定（ディスク設定内の`cache`キーなど）もすべて削除しました。

#### カスタムファイルシステム

カスタムファイルシステムドライバの登録に必要な手順が若干変更されました。したがって、もしあなたが独自のカスタムファイルシステムドライバを定義していたり、カスタムドライバを定義しているパッケージを使用していた場合は、あなたのコードと依存を更新する必要があります。

たとえば、Laravel8.xのカスタムファイルシステムの登録は、以下のようでした。

```php
use Illuminate\Support\Facades\Storage;
use League\Flysystem\Filesystem;
use Spatie\Dropbox\Client as DropboxClient;
use Spatie\FlysystemDropbox\DropboxAdapter;

Storage::extend('dropbox', function ($app, $config) {
    $client = new DropboxClient(
        $config['authorization_token']
    );

    return new Filesystem(new DropboxAdapter($client));
});
```

しかし、Laravel9.xから、`Storage::extend`メソッドへ与えられるコールバックは、直接 `Illuminate\Filesystem\FilesystemAdapter`インスタンスを返さなければなりません。

```php
use Illuminate\Filesystem\FilesystemAdapter;
use Illuminate\Support\Facades\Storage;
use League\Flysystem\Filesystem;
use Spatie\Dropbox\Client as DropboxClient;
use Spatie\FlysystemDropbox\DropboxAdapter;

Storage::extend('dropbox', function ($app, $config) {
    $adapter = new DropboxAdapter(new DropboxClient(
        $config['authorization_token']
    ););

    return new FilesystemAdapter(
        new Filesystem($adapter, $config),
        $adapter,
        $config
    );
});
```

### ヘルパ

<a name="data-get-function"></a>
#### `data_get`ヘルパとIterableオブジェクト

**影響の可能性： とても低い**

以前は、`data_get`ヘルパーを使用して、配列および`Collection`インスタンスのネストされたデータを取得できました。しかし今、このヘルパーは、すべての反復可能なオブジェクトのネストされたデータを取得するようになりました。

<a name="str-function"></a>
#### `str`ヘルパ

**影響の可能性： とても低い**

Laravel9.xでは、グローバルな`str`[ヘルパ関数](/docs/{{version}}/helpers#method-str)を取り入れました。アプリケーションでグローバルな`str`ヘルパーを定義している場合は、Laravel自身の`str`ヘルパーと競合しないように、名前を変更するか削除する必要があります。

<a name="when-and-unless-methods"></a>
#### `when`／`unless`メソッド

**影響の可能性： 中程度**

お気づきでしょうが、`when`メソッドと`unless`メソッドは、フレームワーク全体のさまざまなクラスで提供しています。これらのメソッドは、メソッドの最初の引数のブール値が`true`、または`false`と評価された場合、条件付きでアクションを実行するために使用します。

```php
$collection->when(true, function ($collection) {
    $collection->merge([1, 2, 3]);
});
```

Therefore, in previous releases of Laravel, passing a closure to the `when` or `unless` methods meant that the conditional operation would always execute, since a loose comparison against a closure object (or any other object) always evaluates to `true`. This often led to unexpected outcomes because developers expect the **result** of the closure to be used as the boolean value that determines if the conditional action executes.

So, in Laravel 9.x, any closures passed to the `when` or `unless` methods will be executed and the value returned by the closure will be considered the boolean value used by the `when` and `unless` methods:

```php
$collection->when(function ($collection) {
    // This closure is executed...
    return false;
}, function ($collection) {
    // Not executed since first closure returned "false"...
    $collection->merge([1, 2, 3]);
});
```

### HTTP Client

<a name="http-client-default-timeout"></a>
#### Default Timeout

**影響の可能性： 中程度**

The [HTTP client](/docs/{{version}}/http-client) now has a default timeout of 30 seconds. In other words, if the server does not respond within 30 seconds, an exception will be thrown. Previously, no default timeout length was configured on the HTTP client, causing requests to sometimes "hang" indefinitely.

If you wish to specify a longer timeout for a given request, you may do so using the `timeout` method:

    $response = Http::timeout(120)->get(...);

#### HTTP Fake & Middleware

**影響の可能性： 低い**

Previously, Laravel would not execute any provided Guzzle HTTP middleware when the [HTTP client](/docs/{{version}}/http-client) was "faked". However, in Laravel 9.x, Guzzle HTTP middleware will be executed even when the HTTP client is faked.

#### HTTP Fake & Dependency Injection

**影響の可能性： 低い**

In previous releases of Laravel, invoking the `Http::fake()` method would not affect instances of the `Illuminate\Http\Client\Factory` that were injected into class constructors. However, in Laravel 9.x, `Http::fake()` will ensure fake responses are returned by HTTP clients injected into other services via dependency injection. This behavior is more consistent with the behavior of other facades and fakes.

<a name="symfony-mailer"></a>
### Symfony Mailer

**影響の可能性： 高い**

One of the largest changes in Laravel 9.x is the transition from SwiftMailer, which is no longer maintained as of December 2021, to Symfony Mailer. However, we have tried to make this transition as seamless as possible for your applications. That being said, please thoroughly review the list of changes below to ensure your application is fully compatible.

#### Driver Prerequisites

To continue using the Mailgun transport, your application should require the `symfony/mailgun-mailer` and `symfony/http-client` Composer packages:

```shell
composer require symfony/mailgun-mailer symfony/http-client
```

The `wildbit/swiftmailer-postmark` Composer package should be removed from your application. Instead, your application should require the `symfony/postmark-mailer` and `symfony/http-client` Composer packages:

```shell
composer require symfony/postmark-mailer symfony/http-client
```

#### Updated Return Types

The `send`, `html`, `text`, and `plain` methods no longer return the number of recipients that received the message. Instead, an instance of `Illuminate\Mail\SentMessage` is returned. This object contains an instance of `Symfony\Component\Mailer\SentMessage` that is accessible via the `getSymfonySentMessage` method or by dynamically invoking methods on the object.

#### Renamed "Swift" Methods

Various SwiftMailer related methods, some of which were undocumented, have been renamed to their Symfony Mailer counterparts. For example, the `withSwiftMessage` method has been renamed to `withSymfonyMessage`:

    // Laravel 8.x...
    $this->withSwiftMessage(function ($message) {
        $message->getHeaders()->addTextHeader(
            'Custom-Header', 'Header Value'
        );
    });

    // Laravel 9.x...
    use Symfony\Component\Mime\Email;

    $this->withSymfonyMessage(function (Email $message) {
        $message->getHeaders()->addTextHeader(
            'Custom-Header', 'Header Value'
        );
    });

> {note} Please thoroughly review the [Symfony Mailer documentation](https://symfony.com/doc/6.0/mailer.html#creating-sending-messages) for all possible interactions with the `Symfony\Component\Mime\Email` object.

The list below contains a more thorough overview of renamed methods. Many of these methods are low-level methods used to interact with SwiftMailer / Symfony Mailer directly, so may not be commonly used within most Laravel applications:

    Message::getSwiftMessage();
    Message::getSymfonyMessage();

    Mailable::withSwiftMessage($callback);
    Mailable::withSymfonyMessage($callback);

    MailMessage::withSwiftMessage($callback);
    MailMessage::withSymfonyMessage($callback);

    Mailer::getSwiftMailer();
    Mailer::getSymfonyTransport();

    Mailer::setSwiftMailer($swift);
    Mailer::setSymfonyTransport(TransportInterface $transport);

    MailManager::createTransport($config);
    MailManager::createSymfonyTransport($config);

#### Proxied `Illuminate\Mail\Message` Methods

The `Illuminate\Mail\Message` typically proxied missing methods to the underlying `Swift_Message` instance. However, missing methods are now proxied to an instance of `Symfony\Component\Mime\Email` instead. So, any code that was previously relying on missing methods to be proxied to SwiftMailer should be updated to their corresponding Symfony Mailer counterparts.

Again, many applications may not be interacting with these methods, as they are not documented within the Laravel documentation:

    // Laravel 8.x...
    $message
        ->setFrom('taylor@laravel.com')
        ->setTo('example@example.org')
        ->setSubject('Order Shipped')
        ->setBody('<h1>HTML</h1>', 'text/html')
        ->addPart('Plain Text', 'text/plain');

    // Laravel 9.x...
    $message
        ->from('taylor@laravel.com')
        ->to('example@example.org')
        ->subject('Order Shipped')
        ->html('<h1>HTML</h1>')
        ->text('Plain Text');

#### Generated Messages IDs

SwiftMailer offered the ability to define a custom domain to include in generated Message IDs via the `mime.idgenerator.idright` configuration option. This is not supported by Symfony Mailer. Instead, Symfony Mailer will automatically generate a Message ID based on the sender.

#### Forced Reconnections

It is no longer possible to force a transport reconnection (for example when the mailer is running via a daemon process). Instead, Symfony Mailer will attempt to reconnect to the transport automatically and throw an exception if the reconnection fails.

#### SMTP Stream Options

Defining stream options for the SMTP transport is no longer supported. Instead, you must define the relevant options directly within the configuration if they are supported. For example, to disable TLS peer verification:

    'smtp' => [
        // Laravel 8.x...
        'stream' => [
            'ssl' => [
                'verify_peer' => false,
            ],
        ],

        // Laravel 9.x...
        'verify_peer' => false,
    ],

To learn more about the available configuration options, please review the [Symfony Mailer documentation](https://symfony.com/doc/6.0/mailer.html#transport-setup).

> {note} In spite of the example above, you are not generally advised to disable SSL verification since it introduces the possibility of "man-in-the-middle" attacks.

#### SMTP `auth_mode`

Defining the SMTP `auth_mode` in the `mail` configuration file is no longer required. The authentication mode will be automatically negotiated between Symfony Mailer and the SMTP server.

#### Failed Recipients

It is no longer possible to retrieve a list of failed recipients after sending a message. Instead, a `Symfony\Component\Mailer\Exception\TransportExceptionInterface` exception will be thrown if a message fails to send. Instead of relying on retrieving invalid email addresses after sending a message, we recommend that you validate email addresses before sending the message instead.

### Packages

<a name="the-lang-directory"></a>
#### The `lang` Directory

**影響の可能性： 中程度**

In new Laravel applications, the `resources/lang` directory is now located in the root project directory (`lang`). If your package is publishing language files to this directory, you should ensure that your package is publishing to `app()->langPath()` instead of a hard-coded path.

<a name="queue"></a>
### キュー

<a name="the-opis-closure-library"></a>
#### The `opis/closure` Library

**影響の可能性： 低い**

Laravel's dependency on `opis/closure` has been replaced by `laravel/serializable-closure`. This should not cause any breaking change in your application unless you are interacting with the `opis/closure` library directly. In addition, the previously deprecated `Illuminate\Queue\SerializableClosureFactory` and `Illuminate\Queue\SerializableClosure` classes have been removed. If you are interacting with `opis/closure` library directly or using any of the removed classes, you may use [Laravel Serializable Closure](https://github.com/laravel/serializable-closure) instead.

#### The Failed Job Provider `flush` Method

**影響の可能性： 低い**

The `flush` method defined by the `Illuminate\Queue\Failed\FailedJobProviderInterface` interface now accepts an `$hours` argument which determines how old a failed job must be (in hours) before it is flushed by the `queue:flush` command. If you are manually implementing the `FailedJobProviderInterface` you should ensure that your implementation is updated to reflect this new argument:

```php
public function flush($hours = null);
```

### セッション

#### The `getSession` Method

**影響の可能性： 低い**

The `Symfony\Component\HttpFoundaton\Request` class that is extended by Laravel's own `Illuminate\Http\Request` class offers a `getSession` method to get the current session storage handler. This method is not documented by Laravel as most Laravel applications interact with the session through Laravel's own `session` method.

The `getSession` method previously returned an instance of `Illuminate\Session\Store` or `null`; however, due to the Symfony 6.x release enforcing a return type of `Symfony\Component\HttpFoundation\Session\SessionInterface`, the `getSession` now correctly returns a `SessionInterface` implementation or throws an `\Symfony\Component\HttpFoundation\Exception\SessionNotFoundException` exception when no session is available.

### テスト

<a name="the-assert-deleted-method"></a>
#### The `assertDeleted` Method

**影響の可能性： 中程度**

All calls to the `assertDeleted` method should be updated to `assertModelMissing`.

### Trusted Proxies

**影響の可能性： 低い**

If you are upgrading your Laravel 8 project to Laravel 9 by importing your existing application code into a totally new Laravel 9 application skeleton, you may need to update your application's "trusted proxy" middleware.

Within your `app/Http/Middleware/TrustProxies.php` file, update `use Fideloper\Proxy\TrustProxies as Middleware` to `use Illuminate\Http\Middleware\TrustProxies as Middleware`.

Next, within `app/Http/Middleware/TrustProxies.php`, you should update the `$headers` property definition:

```php
// Before...
protected $headers = Request::HEADER_X_FORWARDED_ALL;

// After...
protected $headers =
    Request::HEADER_X_FORWARDED_FOR |
    Request::HEADER_X_FORWARDED_HOST |
    Request::HEADER_X_FORWARDED_PORT |
    Request::HEADER_X_FORWARDED_PROTO |
    Request::HEADER_X_FORWARDED_AWS_ELB;
```

Finally, you can remove the `fideloper/proxy` Composer dependency from your application:

```shell
composer remove fideloper/proxy
```

### バリデーション

#### Form Request `validated` Method

**影響の可能性： 低い**

The `validated` method offered by form requests now accepts `$key` and `$default` arguments. If you are manually overwriting the definition of this method, you should update your method's signature to reflect these new arguments:

```php
public function validated($key = null, $default = null)
```

<a name="the-password-rule"></a>
#### The `password` Rule

**影響の可能性： 中程度**

The `password` rule, which validates that the given input value matches the authenticated user's current password, has been renamed to `current_password`.

<a name="unvalidated-array-keys"></a>
#### Unvalidated Array Keys

**影響の可能性： 中程度**

In previous releases of Laravel, you were required to manually instruct Laravel's validator to exclude unvalidated array keys from the "validated" data it returns, especially in combination with an `array` rule that does not specify a list of allowed keys.

However, in Laravel 9.x, unvalidated array keys are always excluded from the "validated" data even when no allowed keys have been specified via the `array` rule. Typically, this behavior is the most expected behavior and the previous `excludeUnvalidatedArrayKeys` method was only added to Laravel 8.x as a temporary measure in order to preserve backwards compatibility.

Although it is not recommended, you may opt-in to the previous Laravel 8.x behavior by invoking a new `includeUnvalidatedArrayKeys` method within the `boot` method of one of your application's service providers:

```php
use Illuminate\Support\Facades\Validator;

/**
 * Register any application services.
 *
 * @return void
 */
public function boot()
{
    Validator::includeUnvalidatedArrayKeys();
}
```

<a name="miscellaneous"></a>
### その他

We also encourage you to view the changes in the `laravel/laravel` [GitHub repository](https://github.com/laravel/laravel). While many of these changes are not required, you may wish to keep these files in sync with your application. Some of these changes will be covered in this upgrade guide, but others, such as changes to configuration files or comments, will not be. You can easily view the changes with the [GitHub comparison tool](https://github.com/laravel/laravel/compare/8.x...9.x) and choose which updates are important to you.
