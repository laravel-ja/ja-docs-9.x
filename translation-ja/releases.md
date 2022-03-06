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

[名前付き引数]（https://www.php.net/manual/en/functions.arguments.php#functions.named-arguments）は、Laravelの下位互換性ガイドラインの対象外です。Laravelコードベースを改善するために、必要に応じて関数の引数の名前を変更することもできます。したがって、Laravelメソッドを呼び出すときに名前付き引数を使用する場合は、パラメーター名が将来変更される可能性があることを理解した上で、慎重に行う必要があります。

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

Laravel9は、Symfony6.0コンポーネントの動作には、PHP8.0以上が必要です。Flysystem3.0のサポート、改良した`route:list`表示、Laravel Scoutデータベースドライバ、新しいEloquentアクセサ／ミューテタ構文、Enumによる暗黙のルート結合、その他様々なバグ修正とユーザビリティ向上によりLaravel 8.x で改善されてきたことを継続します。

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

`controller` メソッドにより、グループ内のすべてのルートに共通のコントローラーを定義できるようになりました。それにより、ルート定義時に呼び出すコントローラメソッドを指定するだけでよくなりました。

    use App\Http\Controllers\OrderController;

    Route::controller(OrderController::class)->group(function () {
        Route::get('/orders/{id}', 'show');
        Route::post('/orders', 'store');
    });

<a name="full-text"></a>
### Full Text Indexes / Where Clauses

_Full text indexes and "where" clauses were contributed by [Taylor Otwell](https://github.com/taylorotwell) and [Dries Vints](https://github.com/driesvints)_.

When using MySQL or PostgreSQL, the `fullText` method may now be added to column definitions to generate full text indexes:

    $table->text('bio')->fullText();

In addition, the `whereFullText` and `orWhereFullText` methods may be used to add full text "where" clauses to a query for columns that have [full text indexes](/docs/{{version}}/migrations#available-index-types). These methods will be transformed into the appropriate SQL for the underlying database system by Laravel. For example, a `MATCH AGAINST` clause will be generated for applications utilizing MySQL:
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
### Bootstrap 5 Pagination Views

_Bootstrap 5 pagination views were contributed by [Jared Lewis](https://github.com/jrd-lewis)_.

Laravel now includes pagination views built using [Bootstrap 5](https://getbootstrap.com/). To use these views instead of the default Tailwind views, you may call the paginator's `useBootstrapFive` method within the `boot` method of your `App\Providers\AppServiceProvider` class:

    use Illuminate\Pagination\Paginator;

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Paginator::useBootstrapFive();
    }

<a name="improved-validation-of-nested-array-data"></a>
### Improved Validation Of Nested Array Data

_Improved validation of nested array inputs was contributed by [Steve Bauman](https://github.com/stevebauman)_.

Sometimes you may need to access the value for a given nested array element when assigning validation rules to the attribute. You may now accomplish this using the `Rule::forEach` method. The `forEach` method accepts a closure that will be invoked for each iteration of the array attribute under validation and will receive the attribute's value and explicit, fully-expanded attribute name. The closure should return an array of rules to assign to the array element:

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
### Laravel Breeze API & Next.js

_The Laravel Breeze API scaffolding and Next.js starter kit was contributed by [Taylor Otwell](https://github.com/taylorotwell) and [Miguel Piedrafita](https://twitter.com/m1guelpf)_.

The [Laravel Breeze](/docs/{{version}}/starter-kits#breeze-and-next) starter kit has received an "API" scaffolding mode and complimentary [Next.js](https://nextjs.org) [frontend implementation](https://github.com/laravel/breeze-next). This starter kit scaffolding may be used to jump start your Laravel applications that are serving as a backend, Laravel Sanctum authenticated API for a JavaScript frontend.

<a name="exception-page"></a>
### Improved Ignition Exception Page

_Ignition is developed by [Spatie](https://spatie.be/)._

Ignition, the open source exception debug page created by Spatie, has been redesigned from the ground up. The new, improved Ignition ships with Laravel 9.x and includes light / dark themes, customizable "open in editor" functionality, and more.

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/483853/149235404-f7caba56-ebdf-499e-9883-cac5d5610369.png"/>
</p>

<a name="improved-route-list"></a>
### Improved `route:list` CLI Output

_Improved `route:list` CLI output was contributed by [Nuno Maduro](https://github.com/nunomaduro)_.

The `route:list` CLI output has been significantly improved for the Laravel 9.x release, offering a beautiful new experience when exploring your route definitions.

<p align="center">
<img src="https://user-images.githubusercontent.com/5457236/148321982-38c8b869-f188-4f42-a3cc-a03451d5216c.png"/>
</p>

<a name="test-coverage-support-on-artisan-test-Command"></a>
### Test Coverage Using Artisan `test` Command

_Test coverage when using the Artisan `test` command was contributed by [Nuno Maduro](https://github.com/nunomaduro)_.

The Artisan `test` command has received a new `--coverage` option that you may use to explore the amount of code coverage your tests are providing to your application:

```shell
php artisan test --coverage
```

The test coverage results will be displayed directly within the CLI output.

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/150133237-440290c2-3538-4d8e-8eac-4fdd5ec7bd9e.png"/>
</p>

In addition, if you would like to specify a minimum threshold that your test coverage percentage must meet, you may use the `--min` option. The test suite will fail if the given minimum threshold is not met:

```shell
php artisan test --coverage --min=80.3
```

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/149989853-a29a7629-2bfa-4bf3-bbf7-cdba339ec157.png"/>
</p>

<a name="soketi-echo-server"></a>
### Soketi Echo Server

_The Soketi Echo server was developed by [Alex Renoki](https://github.com/rennokki)_.

Although not exclusive to Laravel 9.x, Laravel has recently assisted with the documentation of Soketi, a [Laravel Echo](/docs/{{version}}/broadcasting) compatible Web Socket server written for Node.js. Soketi provides a great, open source alternative to Pusher and Ably for those applications that prefer to manage their own Web Socket server.

For more information on using Soketi, please consult the [broadcasting documentation](/docs/{{version}}/broadcasting) and [Soketi documentation](https://docs.soketi.app/).

<a name="improved-collections-ide-support"></a>
### Improved Collections IDE Support

_Improved collections IDE support was contributed by [Nuno Maduro](https://github.com/nunomaduro)_.

Laravel 9.x adds improved, "generic" style type definitions to the collections component, improving IDE and static analysis support. IDEs such as [PHPStorm](https://blog.jetbrains.com/phpstorm/2021/12/phpstorm-2021-3-release/#support_for_future_laravel_collections) or static analysis tools such as [PHPStan](https://phpstan.org) will now better understand Laravel collections natively.

<p align="center">
<img width="100%" src="https://user-images.githubusercontent.com/5457236/151783350-ed301660-1e09-44c1-b549-85c6db3f078d.gif"/>
</p>

<a name="new-helpers"></a>
### New Helpers

Laravel 9.x introduces two new, convenient helper functions that you may use in your own application.

<a name="new-helpers-str"></a>
#### `str`

The `str` function returns a new `Illuminate\Support\Stringable` instance for the given string. This function is equivalent to the `Str::of` method:

    $string = str('Taylor')->append(' Otwell');

    // 'Taylor Otwell'

If no argument is provided to the `str` function, the function returns an instance of `Illuminate\Support\Str`:

    $snake = str()->snake('LaravelFramework');

    // 'laravel_framework'

<a name="new-helpers-to-route"></a>
#### `to_route`

The `to_route` function generates a redirect HTTP response for a given named route, providing an expressive way to redirect to named routes from your routes and controllers:

    return to_route('users.show', ['user' => 1]);

If necessary, you may pass the HTTP status code that should be assigned to the redirect and any additional response headers as the third and fourth arguments to the to_route method:

    return to_route('users.show', ['user' => 1], 302, ['X-Framework' => 'Laravel']);
