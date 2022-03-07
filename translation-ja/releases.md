# リリースノート

- [バージョニング規約](#versioning-scheme)
- [サポートポリシー](#support-policy)
- [Laravel 9](#laravel-9)

<a name="versioning-scheme"></a>
## バージョニング規約

Laravelとファーストパーティパッケージは、[セマンティックバージョニング](https://semver.org)にしたがっています。メジャーなフレームのリリースは、毎年２月にリリースします。マイナーとパッチリリースはより細かく毎週リリースします。マイナーとパッチリリースは、**決して**ブレーキングチェンジを含みません

あなたのアプリケーションやパッケージから、Laravelフレームワーク、もしくはコンポーネントを参照する場合は、Laravelのメジャーリリースには重大な変更が含まれているため、常に`^ 9.0`などのようにバージョンを指定する必要があります。ただし、私たちは１日中以内に新しいメジャーリリースへ更新できるように、常に努力しています。

<a name="named-arguments"></a>
#### 名前付き引数

[名前付き引数](https://www.php.net/manual/en/functions.arguments.php#functions.named-arguments)は、Laravelの下位互換性ガイドラインの対象外です。Laravelコードベースを改善するために、必要に応じて関数の引数の名前を変更することもできます。したがって、Laravelメソッドを呼び出すときに名前付き引数を使用する場合は、パラメーター名が将来変更される可能性があることを理解した上で、慎重に行う必要があります。

<a name="support-policy"></a>
## サポートポリシー

Laravelのすべてのリリースは、バグフィックスは１８ヶ月、セキュリティフィックスは２年です。Lumenのようなその他の追加ライブラリでは、最新リリースのみでバグフィックスを受け付けています。また、[Laravelがサポートする](/docs/{{version}}/database#introduction)データベースのサポートについても確認してください。

| バージョン | PHP (*) | リリース | バグフィックス期日 | セキュリティ修正期日 |
| -------- | -------- | ------- | ------------------ | ---------------------- |
| 6 (LTS) | 7.2 - 8.0 | ２０１９年９月３日 | ２０２２年１月２５日 | ２０２２年９月６日 |
| 7 | 7.2 - 8.0 | ２０２０年３月３日 | ２０２０年１０月６日 | ２０２１年３月３日 |
| 8 | 7.3 - 8.1 | ２０２０年９月８日 | ２０２２年７月２６日 | ２０２３年１月２４日 |
| 9 | 8.0 - 8.1 | ２０２２年２月８日 | ２０２３年８月８日 | ２０２４年２月８日 |
| 10 | 8.0 - 8.1 | ２０２３年２月７日 | ２０２４年８月７日 | ２０２５年２月７日 |

<div class="version-colors">
    <div class="end-of-life">
        <div class="color-box"></div>
        <div>End of life</div>
    </div>
    <div class="security-fixes">
        <div class="color-box"></div>
        <div>Security fixes only</div>
    </div>
</div>

(*) 対応PHPバージョン

<a name="laravel-9"></a>
## Laravel9

ご存知かもしれませんが、Laravel8のリリース時から、Laravelは年次リリースに移行しました。以前は、メジャーバージョンを６か月ごとにリリースしていました。この変更はコミュニティのメンテナンスの負担を軽減することと、開発チームが互換性を失う変更を加えることなく、驚くべき強力な新機能を出荷する試みができることを目的としています。そのため、下位互換性を損なうことなく、さまざまな堅牢な機能をLaravel8へ取り入れました。並列テストのサポート、Breezeスターターキットの改善、HTTPクライアントの改善、さらに"has one of many"を含む、新しいEloquentリレーションシップタイプなどです。

したがって、現在のリリースへ優れた新機能を導入するこの取り組みにより、将来の「メジャー」リリースが主にアップストリームの依存関係のアップグレードなど、「メンテナンス」タスクに使用されるようになります。これは、これらのリリースノートに記載されています。

Laravel9、Symfony6.0コンポーネントの動作には、PHP8.0以上が必要です。Flysystem3.0のサポート、改良した`route:list`表示、Laravel Scoutデータベースドライバ、新しいEloquentアクセサ／ミューテタ構文、Enumによる暗黙のルート結合、その他様々なバグ修正とユーザービリティ向上によりLaravel8.xで改善されたことを継続しています。

<a name="php-8"></a>
### PHP8.0

Laravel9.xの動作には、PHP8.0以上が必要です。

<a name="symfony-mailer"></a>
### Symfony Mailer

_Symfony Mailerのサポートは、[Dries Vints](https://github.com/driesvints)_、[James Brooks](https://github.com/jbrooksuk)、[Julius Kiekbusch](https://github.com/Jubeki)が貢献しました。

以前のLaravelリリースは、送信メールの送信に[Swift Mailer](https://swiftmailer.symfony.com/docs/introduction.html)ライブラリが利用していました。しかし、このライブラリはすでにメンテナンスされておらず、Symfony Mailerに継承されています。

皆さんのアプリケーションとSymfony Mailerとの互換性の詳細を確認するには、[アップグレードガイド](/docs/{{version}}/upgrade#symfony-mailer)を参照してください。

<a name="flysyste-3"></a>
### Flysystem3.x

*Flysystem3.xのサポートは、[Dries Vints](https://github.com/driesvints)が貢献しました。*

Laravel9.xは、上流のFlysystem依存パッケージをFlysystem3.xへアップグレードしています。Flysystemは、`Storage`ファサードが提供するファイルシステム操作のすべてを提供しています。

皆さんのアプリケーションが、Flysystem3.xと互換性を持つための詳細は、[アップグレードガイド](/docs/{{version}}/upgrade#flysystem-3)を参照してください。

<a name="eloquent-accessors-and-mutators"></a>
### Eloquentアクセサ／ミューテタの向上

*Eloquentアクセサ／ミューテタの改良は、[Taylor Otwell](https://github.com/taylorotwell)が貢献しました。*

Laravel9.xは、Eloquentの[アクセサとミューテタ](/docs/{{version}}/eloquent-mutators#accessors-and-mutators)を定義する新しい方法を提供しました。以前のLaravelのリリースでは、アクセサやミューテタを定義する方法は、以下のようにモデルにプレフィックス付きのメソッドを定義することのみでした。

```php
public function getNameAttribute($value)
{
    return strtoupper($value);
}

public function setNameAttribute($value)
{
    $this->attributes['name'] = $value;
}
```

しかし、Laravel9.xでは、`Illuminate\Database\Eloquent\Casts\Attribute`タイプの戻り値をタイプヒントすることで、プレフィックスを使わない単独のメソッドを使用して、アクセサとミューテタを定義することができます。

```php
use Illuminate\Database\Eloquent\Casts\Attribute;

public function name(): Attribute
{
    return new Attribute(
        get: fn ($value) => strtoupper($value),
        set: fn ($value) => $value,
    );
}
```

さらに、アクセサを定義するこの新しいアプローチにより、[カスタムキャストクラス](/docs/{{version}}/eloquent-mutators#custom-casts)のように、属性が返したオブジェクト値をキャッシュするようになります。

```php
use App\Support\Address;
use Illuminate\Database\Eloquent\Casts\Attribute;

public function address(): Attribute
{
    return new Attribute(
        get: fn ($value, $attributes) => new Address(
            $attributes['address_line_one'],
            $attributes['address_line_two'],
        ),
        set: fn (Address $value) => [
            'address_line_one' => $value->lineOne,
            'address_line_two' => $value->lineTwo,
        ],
    );
}
```

<a name="enum-casting"></a>
### Enum Eloquent属性キャスト

> {note} EnumキャストはPHP8.1以上で利用できます。

_Enumキャストは、[Mohamed Said](https://github.com/themsaid)が貢献しました。_

Eloquentは、属性値をPHPの[「値に依存した（backed）」enums](https://www.php.net/manual/ja/language.enumerations.backed.php)へキャストできるようになりました。これを使用するには、モデルの`$casts`プロパティ配列で、キャストしたい属性と列挙型を指定します。

    use App\Enums\ServerStatus;

    /**
     * キャストする属性
     *
     * @var array
     */
    protected $casts = [
        'status' => ServerStatus::class,
    ];

一度、モデルでキャストを定義したら、属性を操作するとき、指定した属性は自動的にenumへキャストされたり、enumからキャストされたりします。

    if ($server->status == ServerStatus::provisioned) {
        $server->status = ServerStatus::ready;

        $server->save();
    }

<a name="implicit-route-bindings-with-enums"></a>
### Enumによる暗黙のルート結合

*Enumによる暗黙のルート結合は、[Nuno Maduro](https://github.com/nunomaduro)が貢献しました。*

PHP 8.1では、[Enums](https://www.php.net/manual/ja/language.enumerations.backed.php)がサポートされました。Laravel9.xでは、ルート定義にEnumをタイプヒントする機能が導入され、LaravelはそのルートセグメントがURIの有効なEnum値である場合にのみルートを呼び出します。そうでない場合は、HTTP 404レスポンスを自動的に返します。例えば、次のようなEnumがあるとします。

```php
enum Category: string
{
    case Fruits = 'fruits';
    case People = 'people';
}
```

`{category}`ルートセグメントが、`fruits`または`people`である場合にのみ呼び出されるルートを定義しています。そうでない場合は、HTTP 404レスポンスを返します。

```php
Route::get('/categories/{category}', function (Category $category) {
    return $category->value;
});
```

<a name="forced-scoping-of-route-bindings"></a>
### ルート結合における制限の強制

*制限の強制付き結合は、[Claudio Dekker](https://github.com/claudiodekker)が貢献しました。*

以前のLaravelリリースで、ルート定義の２番目のEloquentモデルを前のEloquentモデルの子になるように制限したい場合があったとしましょう。例として、特定ユーザーのブログ記事をスラッグで取得するルート定義について考えてみましょう。

    use App\Models\Post;
    use App\Models\User;

    Route::get('/users/{user}/posts/{post:slug}', function (User $user, Post $post) {
        return $post;
    });

ネストしたルートパラメータとして、カスタムキー付きの暗黙の結合を使用する場合、Laravelは親のリレーション名を推測する命名規約を使用することにより、ネストしたモデルを取得するためのクエリを自動的に制限します。しかし、この動作は以前、カスタムキーが子ルートの結合に使用されたときのみ、Laravelはサポートしていました。

しかし、Laravel9.xでは、カスタムキーが提供されない場合でも、Laravelに「子」結合を限定するよう指示できるようになりました。これを行うには、ルートの定義で`scopeBindings`メソッドを呼び出します。

    use App\Models\Post;
    use App\Models\User;

    Route::get('/users/{user}/posts/{post}', function (User $user, Post $post) {
        return $post;
    })->scopeBindings();

また、ルート定義のグループ全体へ、スコープ付き結合を使用するように指示することもできます。

    Route::scopeBindings()->group(function () {
        Route::get('/users/{user}/posts/{post}', function (User $user, Post $post) {
            return $post;
        });
    });

<a name="controller-route-groups"></a>
### コントローラルートグループ

*ルートグループの改良は、[Luke Downing](https://github.com/lukeraymonddowning)が貢献しました。*

`controller` メソッドにより、グループ内のすべてのルートに共通のコントローラを定義できるようになりました。それにより、ルート定義時に呼び出すコントローラメソッドを指定するだけでよくなりました。

    use App\Http\Controllers\OrderController;

    Route::controller(OrderController::class)->group(function () {
        Route::get('/orders/{id}', 'show');
        Route::post('/orders', 'store');
    });

<a name="full-text"></a>
### フルテキストインデックス／WHERE句

*全文インデックスと"WHERE"句は、[Taylor Otwell](https://github.com/taylorotwell)と[Dries Vints](https://github.com/driesvints)の貢献です。*

MySQLかPostgreSQLを使用している場合、カラム定義で`fullText`メソッドを追加し、フルテキストインデックスを生成できるようにしました。

    $table->text('bio')->fullText();

さらに、`whereFullText`と`orWhereFullText`メソッドを使用すると、[フルテキストインデックス](/docs/{{version}}/migrations#available-index-types) を持つカラムのクエリに、フルテキストの"WHERE"節を追加できます。これらのメソッドは、裏で動作するデータベースシステムに適したSQLへ、Laraelが変換します。例えば、MySQLを利用するアプリケーションでは、`MATCH AGAINST`句を生成します。

    $users = DB::table('users')
               ->whereFullText('bio', 'web developer')
               ->get();

<a name="laravel-scout-database-engine"></a>
### Laravel Scoutデータベースエンジン

*Laravel Scoutデータベースエンジンは、[Taylor Otwell](https://github.com/taylorotwell)と[Dries Vints](https://github.com/driesvints)が、貢献しました。*

中小規模のデータベースを操作するアプリケーションや、作業負荷が軽いアプリケーションでは、AlgoliaやMeiliSearchのような専用の検索サービスではなく、Scoutで「データベース」エンジンを利用できるようにしました。データベースエンジンは、既存のデータベースから検索結果をフィルタリングするとき、"WHERE LIKE"句と全文インデックスを使用して、クエリの検索結果を決定します。

Scoutデータベースエンジンの詳細は、[Scoutのドキュメント](/docs/{{version}}/scout)を参照してください。

<a name="rendering-inline-blade-templates"></a>
### インラインBladeテンプレートのレンダ

*インラインBladeテンプレートのレンダは、[Jason Beggs](https://github.com/jasonlbeggs)が貢献しました。インラインBladeコンポーネントのレンダは、[Toby Zerner](https://github.com/tobyzerner)が貢献しました。*

素のBladeテンプレート文字列を有効なHTMLへ変換する必要が起きることもあるでしょう。このような場合は、`Blade`ファサードが提供する`render`メソッドを使用します。`render`メソッドはBladeテンプレート文字列と、テンプレートに提供するデータの配列をオプションで受け取ります。

```php
use Illuminate\Support\Facades\Blade;

return Blade::render('Hello, {{ $name }}', ['name' => 'Julian Bashir']);
```

同様に、`renderComponent`メソッドは、クラスコンポーネントのインスタンスをメソッドへ渡し、レンダするために使用します。

```php
use App\View\Components\HelloComponent;

return Blade::renderComponent(new HelloComponent('Julian Bashir'));
```

<a name="slot-name-shortcut"></a>
### スロット名の短期法

*スロット名の短期法は、[Caleb Porzio](https://github.com/calebporzio)の貢献です。*

以前のLaravelリースでは、スロット名を`x-slot`タグの`name`属性で指定していました。

```blade
<x-alert>
    <x-slot name="title">
        Server Error
    </x-slot>

    <strong>Whoops!</strong> Something went wrong!
</x-alert>
```

しかし、Laravel9.xからは、スロットの名前を便利な短期法で指定できます。

```xml
<x-slot:title>
    Server Error
</x-slot>
```

<a name="checked-selected-blade-directives"></a>
### Checked / Selected Blade Directives

*checkedとselected Bladeディレクティブは、[Ash Allen](https://github.com/ash-jc-allen)と[Taylor Otwell](https://github.com/taylorotwell)の貢献です。

利便が良いよう、指定したHTMLのチェックボックス入力が"checked"であることを簡単に表すため、`@checked`ディレクティブを使用できます。このディレクティブは、指定条件が`true`と評価された場合、`checked`をechoします。

```blade
<input type="checkbox"
        name="active"
        value="active"
        @checked(old('active', $user->active)) />
```

同様に、`@selected`ディレクティブは、特定のセレクトオプションが"selected"であることを表すために使用します。

```blade
<select name="version">
    @foreach ($product->versions as $version)
        <option value="{{ $version }}" @selected(old('version') == $version)>
            {{ $version }}
        </option>
    @endforeach
</select>
```

<a name="bootstrap-5-pagination-views"></a>
### Bootstrap5ペジネーションビュー

*Bootstrap5ペジネーションビューは、[Jared Lewis](https://github.com/jrd-lewis)の貢献です*

Laravelは、[Bootstrap5](https://getbootstrap.com/)を使用したペジネーションビューを用意しました。デフォルトのTailwindビューの代わりにこのビューを使うには、`App\Providers\AppServiceProvider`クラスの`boot`メソッド内で、ペジネタの`useBootstrapFive`メソッドを呼び出してください。

    use Illuminate\Pagination\Paginator;

    /**
     * 全アプリケーションサービスの初期起動処理
     *
     * @return void
     */
    public function boot()
    {
        Paginator::useBootstrapFive();
    }

<a name="improved-validation-of-nested-array-data"></a>
### ネストした配列データのバリデーション向上

*ネストした配列入力のバリデーションの向上は、[Steve Bauman](https://github.com/stevebauman)が貢献しました。*

バリデーションルールを属性へ割り当てるとき、ネストした配列要素の値へアクセスする必要があることがあります。このような場合には、`Rule::forEach`メソッドを使います。`forEach`メソッドは、バリデーション対象の配列属性の繰り返しごとに呼び出されるクロージャを受け取り、属性の値と完全に展開された明確な属性名を受け取ります。クロージャは、配列の要素へ割り当てるルールの配列を返す必要があります。

    use App\Rules\HasPermission;
    use Illuminate\Support\Facades\Validator;
    use Illuminate\Validation\Rule;

    $validator = Validator::make($request->all(), [
        'companies.*.id' => Rule::forEach(function ($value, $attribute) {
            return [
                Rule::exists(Company::class, 'id'),
                new HasPermission('manage-company', $value),
            ];
        }),
    ]);

<a name="laravel-breeze-api"></a>
### Laravel Breeze APIとNext.js

*Laravel Breeze APIスカフォールドとNext.jsスターターキットは、[Taylor Otwell](https://github.com/taylorotwell)と[Miguel Piedrafita](https://twitter.com/m1guelpf)の貢献です。*

[Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-next)スターターキットに「API」スカフォールドモードと賞賛すべき[Next.js](https://nextjs.org)の[フロントエンド実装](https://github.com/laravel/breeze-next)を追加しました。このスターターキットのスカフォールドはバックグラウンドとして動作するLaravelアプリケーションや、Laravel Sanctum認証APIのJavaScriptフロントエンドのジャンプスタートに利用できるでしょう。

<a name="exception-page"></a>
### Ignition例外ページの向上

*Ignitionは、[Spatie](https://spatie.be/)が開発しています。*

Spatieが開発しているオープンソースの例外デバッグページ、Ignitionが一から設計し直されました。新しくなったIgnitionはLaravel9.xに同梱され、ライト/ダークテーマ、カスタマイズ可能な「エディタで開く」機能などが用意されました。

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/483853/149235404-f7caba56-ebdf-499e-9883-cac5d5610369.png"/>
</p>

<a name="improved-route-list"></a>
### `route:list` CLI表示の向上

`route:list`のCLI出力の改良は、[Nuno Maduro](https://github.com/nunomaduro)の貢献です。

Laravel9.xリリースでは、`route:list` CLI表示を大幅に改善し、ルート定義の表示の際に素晴らしいエクスペリエンスを提供します。

<p align="center">
<img src="https://user-images.githubusercontent.com/5457236/148321982-38c8b869-f188-4f42-a3cc-a03451d5216c.png"/>
</p>

<a name="test-coverage-support-on-artisan-test-Command"></a>
### Artisan `test`コマンドを使ったテストカバレージ

*Artisan `test`コマンド使用のテストカバレッジは、[Nuno Maduro](https://github.com/nunomaduro)の貢献です。*

Artisan の `test` コマンドに、新たに`--coverage`オプションを追加し、アプリケーションに用意したテストのコードカバレッジの量を調べられるようになりました。

```shell
php artisan test --coverage
```

テストカバレッジの結果は、CLI出力内に直接表示しています。

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/150133237-440290c2-3538-4d8e-8eac-4fdd5ec7bd9e.png"/>
</p>

さらに、満たすべき最小のテストカバレッジパーセンテージの閾値を指定する場合は、`--min` オプションを使用します。指定した最小閾値を満たさない場合、テストスイートは失敗します。

```shell
php artisan test --coverage --min=80.3
```

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/149989853-a29a7629-2bfa-4bf3-bbf7-cdba339ec157.png"/>
</p>

<a name="soketi-echo-server"></a>
### Soketi サーバ

*Soketi Echoサーバは、[Alex Renoki](https://github.com/rennokki)が開発しました。*

Laravel9.x専用ではありませんが、最近LaravelはNode.js用に書かれた[Laravel Echo](/docs/{{version}}/broadcasting)対応のWeb Socketサーバ、Soketiのドキュメント作成を支援しました。Soketiは、PusherやAblyに代わる、素晴らしいオープンソースのWeb Socketサーバを提供します。

Soketiの使い方は、[ブロードキャストのドキュメント](/docs/{{version}}/broadcasting)および[Soketiのドキュメント](https://docs.soketi.app/ を参照してください。

<a name="improved-collections-ide-support"></a>
### コレクションとIDEサポートの向上

*コレクションとIDEサポートの向上は、[Nuno Maduro](https://github.com/nunomaduro)の貢献です。*

Laravel9.xは、改良した「ジェネリック」スタイルの型定義をコレクションコンポーネントへ追加し、IDEと静的解析へのサポートを向上しました。[PHPStorm](https://blog.jetbrains.com/phpstorm/2021/12/phpstorm-2021-3-release/#support_for_future_laravel_collections)などのIDEや、[PHPStan](https://phpstan.org)などの静的解析ツールは、Laravelコレクションをより自然に理解できるようになりました。

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/151783350-ed301660-1e09-44c1-b549-85c6db3f078d.gif"/>
</p>

<a name="new-helpers"></a>
### 新ヘルパ

Laravel 9.xでは、皆さん自身のアプリケーションでも使ってもらえる、便利なヘルパ関数を２つ導入しました。

<a name="new-helpers-str"></a>
#### `str`

`str`関数は、指定した文字列に対する新しい`Illuminate\Support\Stringable`インスタンスを返します。この関数は、`Str::of`メソッドと等価です。

    $string = str('Taylor')->append(' Otwell');

    // 'Taylor Otwell'

`str`関数へ、引数を指定しなかった場合、この関数は`Illuminate\Support\Str`インスタンスを返します。

    $snake = str()->snake('LaravelFramework');

    // 'laravel_framework'

<a name="new-helpers-to-route"></a>
#### `to_route`

`to_route`関数は、指定名前付きルートへのリダイレクトHTTPレスポンスを生成し、ルートやコントローラーから名前付きルートへリダイレクトするための表現力豊かな手法を提供しています。

    return to_route('users.show', ['user' => 1]);

必要に応じ、リダイレクトへ割り当てるHTTPステータスコードと追加のレスポンスヘッダを、to_routeメソッドの第３、第４引数へ渡せます。

    return to_route('users.show', ['user' => 1], 302, ['X-Framework' => 'Laravel']);
