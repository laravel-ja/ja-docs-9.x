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

- [Belongs To Manyの`firstOrNew`、`firstOrCreate`、`updateOrCreate`メソッド](#belongs-to-many-first-or-new)
- [カスタムキャストと`null`](#custom-casts-and-null)
- [HTTPクライアントのデフォルトタイムアウト](#http-client-default-timeout)
- [PHPの戻り値のタイプ](#php-return-types)
- [Postgresの"Schema"設定](#postgres-schema-configuration)
- [`assertDeleted`メソッド](#the-assert-deleted-method)
- [`lang`ディレクトリ](#the-lang-directory)
- [`password`ルール](#the-password-rule)
- [`when`／`unless`メソッド](#when-and-unless-methods)
- [バリデーションされていない配列キー](#unvalidated-array-keys)

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

#### Checked／Disabled／Selected Bladeディレクティブ

**影響の可能性： 低い**

新しい`@checked`、`@disabled`、`@selected` Bladeディレクティブは、同名のVueイベントと競合する可能性があります。`@@`でディレクティブをエスケープすると、この衝突を回避できます。

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

S3、FTP、SFTPドライバを使用する前に、Composerパッケージマネージャで適切なパッケージをインストールする必要があります。

- Amazon S3: `composer require -W league/flysystem-aws-s3-v3 "^3.0"`
- FTP: `composer require league/flysystem-ftp "^3.0"`
- SFTP: `composer require league/flysystem-sftp-v3 "^3.0"`

#### 既存ファイルの上書き

`put`、`write`、`writeStream`などの書き込み操作は、デフォルトで既存のファイルを上書きするようになりました。既存のファイルを上書きしたくない場合は、書き込み操作を行う前に、ファイルの存在を確認してください。

#### Writeの例外

`put`、`write`、`writeStream`など書き込み操作で、書き込みに失敗しても例外を投げなくなりました。代わりに、`false`を返します。例外を投げる以前の動作を維持する場合は、ファイルシステムのディスク設定配列で、`throw`オプションを定義してください。

```php
'public' => [
    'driver' => 'local',
    // ...
    'throw' => true,
],
```

#### 存在しないファイルからの読み込み

存在しないファイルから読み込もうとすると、`null`を返すようにしました。以前のLaravelのリリースでは、`Illuminate\Contracts\Filesystem\FileNotFoundException`を投げていました。

#### 存在しないファイルの削除

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

しかし、Laravel9.xから、`Storage::extend`メソッドへ与えられるコールバックは、直接`Illuminate\Filesystem\FilesystemAdapter`インスタンスを返さなければなりません。

```php
use Illuminate\Filesystem\FilesystemAdapter;
use Illuminate\Support\Facades\Storage;
use League\Flysystem\Filesystem;
use Spatie\Dropbox\Client as DropboxClient;
use Spatie\FlysystemDropbox\DropboxAdapter;

Storage::extend('dropbox', function ($app, $config) {
    $adapter = new DropboxAdapter(
        new DropboxClient($config['authorization_token'])
    );

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

以前は、`data_get`ヘルパを使用して、配列および`Collection`インスタンスのネストされたデータを取得できました。しかし今、このヘルパは、すべての反復可能なオブジェクトのネストされたデータを取得するようになりました。

<a name="str-function"></a>
#### `str`ヘルパ

**影響の可能性： とても低い**

Laravel9.xでは、グローバルな`str`[ヘルパ関数](/docs/{{version}}/helpers#method-str)を取り入れました。アプリケーションでグローバルな`str`ヘルパを定義している場合は、Laravel自身の`str`ヘルパと競合しないように、名前を変更するか削除する必要があります。

<a name="when-and-unless-methods"></a>
#### `when`／`unless`メソッド

**影響の可能性： 中程度**

お気づきでしょうが、`when`メソッドと`unless`メソッドは、フレームワーク全体のさまざまなクラスで提供しています。これらのメソッドは、メソッドの最初の引数のブール値が`true`、または`false`と評価された場合、条件付きでアクションを実行するために使用します。

```php
$collection->when(true, function ($collection) {
    $collection->merge([1, 2, 3]);
});
```

そのため、以前のLaravelのリリースでは、`when`または`unless`メソッドにクロージャを渡すとは、クロージャオブジェクト（または他のオブジェクト）に対し緩い比較が行われるため、常に結果は`true`に評価され、条件付き操作が常に実行されることを意味しました。これはしばしば、予想外の結果を生みました。なぜなら、開発者はクロージャの **結果** が、条件付きアクションが実行されるかどうかを決定するブール値として使用されることを期待していたからです。

そのため、Laravel9.xでは`when`や`unless`メソッドに渡されたクロージャは実行され、クロージャが返す値が`when`や`unless`メソッドが使う理論値とみなすようにしました。

```php
$collection->when(function ($collection) {
    // このクロージャは実行される
    return false;
}, function ($collection) {
    // クロージャが"false"を返しているため、実行されない
    $collection->merge([1, 2, 3]);
});
```

### HTTPクライアント

<a name="http-client-default-timeout"></a>
#### デフォルトタイムアウト

**影響の可能性： 中程度**

[HTTPクライアント](/docs/{{version}}/http-client)のデフォルトのタイムアウトを３０秒にしました。つまり、３０秒以内にサーバから応答がない場合、例外が発生していました。以前は、HTTPクライアントにデフォルトのタイムアウトの長さが設定されていなかったため、リクエストが永久に「ハングアップ」することがありました。

もし、指定するリクエストへより長いタイムアウトを指定したい場合は、`timeout`メソッドを使用して指定できます。

    $response = Http::timeout(120)->get(...);

#### HTTP Fakeとミドルウェア

**影響の可能性： 低い**

以前は、[HTTPクライアント](/docs/{{version}}/http-client)が "fake"されていても、Laravelは指定されたGuzzle HTTPミドルウェアを実行しませんでした。しかし、Laravel 9.xでは、HTTPクライアントがFakeであっても、Guzzle HTTPミドルウェアは実行されます。

#### HTTP Fakeと依存注入

**影響の可能性： 低い**

Laravelの以前のリリースでは、`Http::fake()`メソッドを起動しても、クラスコンストラクタへ注入された`Illuminate\Http\Client\Factory`インスタンスに影響を与えませんでした。しかし、Laravel9.xでは、`Http::fake()`は、依存注入により他のサービスへ注入されたHTTPクライアントがFakeレスポンスを返すことを保証します。この動作は、他のファサードやFakeの動作とより一貫性があります。

<a name="symfony-mailer"></a>
### Symfony Mailer

**影響の可能性： 高い**

Laravl9.xの最大の変更点は、２０２１年１２月をもってメンテナンスが終了したSwiftMailerから、Symfony Mailerへの移行したことです。しかし、私たちはあなたのアプリケーションで、この移行ができるだけシームレスになるように努めました。とはいえ、あなたのアプリケーションで完全に互換を保てるように、以下の変更点のリストを十分に確認してください。

#### ドライバ要求

Mailgunトランスポートを引き続き使用するには、`symfony/mailgun-mailer`と`symfony/http-client` Composerパッケージがアプリケーションに必要です。

```shell
composer require symfony/mailgun-mailer symfony/http-client
```

`wildbit/swiftmailer-postmark` Composerパッケージをアプリケーションから削除する必要があります。代わりに、`symfony/postmark-mailer`、`symfony/http-client` Composerパッケージが必要です。

```shell
composer require symfony/postmark-mailer symfony/http-client
```

#### 戻り値の変更

`Illuminate\Mail\Mailer`の`send`、` html`、`text`、`plain`メソッドは、`void`を返さなくしました。代わりに、`Illuminate\Mail\SentMessage`インスタンスを返します。このオブジェクトには、`getSymfonySentMessage`メソッドでアクセス、もしくはオブジェクトのメソッドを動的に呼び出すことでアクセスできる`Symfony\Component\Mailer\SentMessage`インスタンスが含まれています。

#### "Swift"メソッドの変更

一部は文書化されていない、さまざまなSwiftMailer関連のメソッドは、Symfony Mailerの対応するメソッドへ名前を変更しました。たとえば、`withSwiftMessage`メソッドの名前を`withSymfonyMessage`に変更しました。

    // Laravel8.x
    $this->withSwiftMessage(function ($message) {
        $message->getHeaders()->addTextHeader(
            'Custom-Header', 'Header Value'
        );
    });

    // Laravel9.x
    use Symfony\Component\Mime\Email;

    $this->withSymfonyMessage(function (Email $message) {
        $message->getHeaders()->addTextHeader(
            'Custom-Header', 'Header Value'
        );
    });

> {note} `Symfony\Component\Mime\Email`オブジェクトと関わるすべてに関して、[Symfony Mailerドキュメント](https://symfony.com/doc/6.0/mailer.html#creating-sending-messages)を徹底的に確認してください。

以下のリストには、改名されたメソッドの概要がより詳細に記載されています。これらのメソッドの多くは SwiftMailer／Symfony Mailer と直接やりとりするための低レベルのメソッドなので、ほとんどの Laravel アプリケーションの中で一般的に使われることはないと思われます。

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

#### `Illuminate\Mail\Message`メソッドのプロキシ処理

`Illuminate\Mail\Message`は通常、見つからないメソッドを裏で動作している`Swift_Message`インスタンスへプロキシしていました。しかし、見つからないメソッドは、代わりに`Symfony\Component\Mime\Email`インスタンスへプロキシするようにしました。したがって、これまで見つからないメソッドがSwiftMailerへプロキシされる動作に依存していたコードは、対応するsymfony Mailerのメソッドへ更新する必要があります。

繰り返しますが、Laravelのドキュメントに記載されていないため、多くのアプリケーションはこれらのメソッドと関わっていないでしょう。

    // Laravel8.x
    $message
        ->setFrom('taylor@laravel.com')
        ->setTo('example@example.org')
        ->setSubject('Order Shipped')
        ->setBody('<h1>HTML</h1>', 'text/html')
        ->addPart('Plain Text', 'text/plain');

    // Laravel9.x
    $message
        ->from('taylor@laravel.com')
        ->to('example@example.org')
        ->subject('Order Shipped')
        ->html('<h1>HTML</h1>')
        ->text('Plain Text');

#### 生成するメッセージID

SwiftMailerは、`mime.idgenerator.idright`設定オプションで、生成するメッセージIDに含めるカスタムドメインを定義することが可能でした。これはSymfony Mailerではサポートされていません。代わりに、Symfony Mailerは送信者に基づいてメッセージIDを自動的に生成します。

#### `MessageSent`イベントの変更

`Illuminate\Mail\Events\MessageSent`イベントの`message`プロパティは、`Swift_Message`インスタンスの代わりに、`Symfony\Component\Mime\Email`インスタンスを含むようにしました。このメッセージは、**送信前**の電子メールを表します。

さらに、`MessageSent`イベントへ、新しく`sent`プロパティを追加しました。このプロパティは`Illuminate\Mail\SentMessage`インスタンスを含み、メッセージIDなど送信したメールに関する情報を含んでいます。

#### 再接続の強制

トランスポートの再接続を強制することはできなくなりました。（例：メーラーがデーモンプロセスで動作している場合）代わりに、Symfony Mailerは自動的にトランスポートへの再接続を試み、再接続が失敗した場合は例外を投げます。

#### SMTPストリームオプション

SMTPトランスポートのストリームオプションの定義をサポートしなくなりました。代わりに、関連オプションがサポートされている場合、設定内で直接定義する必要があります。例として、TLSピア認証を無効にする場合をご覧ください。

    'smtp' => [
        // Laravel8.x
        'stream' => [
            'ssl' => [
                'verify_peer' => false,
            ],
        ],

        // Laravel9.x
        'verify_peer' => false,
    ],

利用可能な設定オプションの詳細は、[Symfony Mailer documentation](https://symfony.com/doc/6.0/mailer.html#transport-setup)を確認してください。

> {note} 上記に例として挙げましたが、SSL認証を無効にすることは、「中間者」攻撃の可能性をもたらすので、一般的にはおすすめできません。

#### SMTP `auth_mode`

SMTPの`auth_mode`を`mail`設定ファイルで定義する必要はなくなりました。認証モードは、Symfony MailerとSMTPサーバの間で自動的にネゴシエーションされます。

#### Failed Recipients

メッセージ送信後に、失敗した受信者のリストを取得できなくなりました。代わりに、メッセージの送信に失敗すると、`Symfony\Component\Mailer\Exception\TransportExceptionInterface`例外が投げられるようになりました。メッセージ送信後に無効なメールアドレスの取得する代わりに、メッセージ送信前にメールアドレスの検証を行うことを推奨します。

### パッケージ

<a name="the-lang-directory"></a>
#### `lang`ディレクトリ

**影響の可能性： 中程度**

新しいLaravelアプリケーションでは、`resources/lang`ディレクトリがプロジェクトのルートディレクトリの`lang`へ配置しました。もし、あなたのパッケージがこのディレクトリへ直接言語ファイルをリソース公開していれば、パスをハードコードせずに、`app()->langPath()`へ確実にリソース公開してください。

<a name="queue"></a>
### キュー

<a name="the-opis-closure-library"></a>
#### `opis/closure`ライブラリ

**影響の可能性： 低い**

Laravelの依存パッケージの`opis/closure`を`laravel/serializable-closure`へ置き換えました。`opis/closure`ライブラリを直接操作していない限り、これによりアプリケーションへ互換性がない変更は起きません。付け加えて、以前、非推奨にした`Illuminate\Queue\SerializableClosureFactory`と`Illuminate\Queue\SerializableClosure`クラスを削除しました。もし、`opis/closure`ライブラリを直接操作したり、削除されたクラスを使用している場合は、代わりに [Laravelシリアライズ可能クロージャ](https://github.com/laravel/serializable-closure) を使用してください。

#### 失敗したジョブプロバイダの`flush`メソッド

**影響の可能性： 低い**

`Illuminate\Queue\Failed\FailedJobProviderInterface`インターフェイスで定義した`flush`メソッドは、`$hours`引数を受けるようにしました。これは失敗したジョブを`queue:flush`コマンドでフラッシュするまで何時間待つかを決定します。`FailedJobProviderInterface`を手作業で実装している場合、この新しい引数を反映するように実装を更新する必要があります。

```php
public function flush($hours = null);
```

### セッション

#### `getSession`メソッド

**影響の可能性： 低い**

Laravel自身の`Illuminate\Http\Request`クラスへ拡張されていた`Symfony\Component\HttpFoundaton\Request`クラスは、現在のセッションストレージハンドラを取得する`getSession`メソッドを提供しています。ほとんどのLaravelアプリケーションは、Laravel自身の`session`メソッドを通してセッションと対話するので、Laravelはこのメソッドを文書化していません。

以前の`getSession`メソッドは、`Illuminate\Session\Store`インスタンスか`null`を返していましたが、Symfony6.Xは`Symfony\Component\HttpFoundation\Session\SessionInterface`タイプを返すように強制しています。そのため、`getSession`は、`SessionInterface`の実装を返すか、セッションが存在しないときは`\Symfony\Component\HttpFoundation\Exception\SessionNotFoundException`を投げるようになりました。

### テスト

<a name="the-assert-deleted-method"></a>
#### `assertDeleted`メソッド

**影響の可能性： 中程度**

`assertDeleted`メソッドのすべての呼び出しを`assertModelMissing`へ更新してください。

### 信用できるプロキシ

**影響の可能性： 低い**

Laravel8プロジェクトをLaravel9にアップグレードする際、既存のアプリケーションコードを全く新しいLaravel9アプリケーションスケルトンへインポートする場合、アプリケーションの「信用できるプロキシ」ミドルウェアを更新する必要がある場合があります。

`app/Http/Middleware/TrustProxies.php`ファイル中の、`use Fideloper\Proxy\TrustProxies as Middleware`を`use Illuminate\Http\Middleware\TrustProxies as Middleware`へ変更してください。

次に、`app/Http/Middleware/TrustProxies.php`中の`$headers`プロパティ定義を更新します。

```php
// 変更前
protected $headers = Request::HEADER_X_FORWARDED_ALL;

// 変更後
protected $headers =
    Request::HEADER_X_FORWARDED_FOR |
    Request::HEADER_X_FORWARDED_HOST |
    Request::HEADER_X_FORWARDED_PORT |
    Request::HEADER_X_FORWARDED_PROTO |
    Request::HEADER_X_FORWARDED_AWS_ELB;
```

最後に、アプリケーションから`fideloper/proxy`Composerの依存パッケージを削除します。

```shell
composer remove fideloper/proxy
```

### バリデーション

#### フォームリクエストの`validated`メソッド

**影響の可能性： 低い**

フォームリクエストが提供している`validated`メソッドは、`$key`と`$default`引数を取るようにしました。このメソッドの定義を独自に上書きしている場合は、これらの新しい引数を反映させるように更新してください。

```php
public function validated($key = null, $default = null)
```

<a name="the-password-rule"></a>
#### `password`ルール

**影響の可能性： 中程度**

与えられた入力値が認証済みユーザーの現在のパスワードと一致するかをバリデートする`password`ルールの名前を`current_password`へ変更しました。

<a name="unvalidated-array-keys"></a>
#### Unvalidated Array Keys

**影響の可能性： 中程度**

Laravelの以前のリリースでは、Laravelのバリデータが返す「バリデーション済み」データから、バリデーションしていない配列キーを除外するには、手作業で指示する必要がありました。特に、許容するキーのリストを指定しない`array`ルールと組み合わせた場合です。

しかし、Laravel9.xでは、`array`ルールで許可するキーを指定していない場合でも、バリデーションしていない配列キーは常に「バリデーション済み」データから除外されます。通常、この動作は最も期待されている動作であり、以前の`excludeUnvalidatedArrayKeys`メソッドは、Laravel8.xで下位互換性を保つための一時的な措置として追加したものでした。

推奨はしませんが、アプリケーションのサービスプロバイダの`boot`メソッド内で、新しい`includeUnvalidatedArrayKeys`メソッドを呼び出し、以前のLaravel8.x の動作を選択することもできます。

```php
use Illuminate\Support\Facades\Validator;

/**
 * アプリケーションの全サービスの登録
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

また、`laravel/laravel` [GitHubリポジトリ](https://github.com/laravel/laravel)で、変更点を確認することをおすすめします。これらの変更の多くは必須ではありませんが、これらのファイルをあなたのアプリケーションへ同期させておくとよいでしょう。こうした変更の一部は、このアップグレードガイドでカバーしますが、設定ファイルやコメントの変更のような他のものは、カバーしません。[GitHub比較ツール](https://github.com/laravel/laravel/compare/8.x...9.x)で簡単に変更点を確認し、どの更新が皆さんにとって重要か選択できます。
