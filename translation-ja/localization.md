# 多言語化

- [イントロダクション](#introduction)
    - [ロケールの設定](#configuring-the-locale)
- [翻訳文字列の定義](#defining-translation-strings)
    - [短縮キーの使用](#using-short-keys)
    - [翻訳文字列をキーとして使用](#using-translation-strings-as-keys)
- [翻訳文字列の取得](#retrieving-translation-strings)
    - [翻訳文字列中のパラメータの置換](#replacing-parameters-in-translation-strings)
    - [複数形](#pluralization)
- [パッケージ言語ファイルのオーバーライド](#overriding-package-language-files)

<a name="introduction"></a>
## イントロダクション

Laravelの多言語機能は、さまざまな言語の文字列を取得する便利な方法を提供し、アプリケーション内で複数の言語を簡単にサポートできるようにしています。

Laravel provides two ways to manage translation strings. First, language strings may be stored in files within the `lang` directory. Within this directory, there may be subdirectories for each language supported by the application. This is the approach Laravel uses to manage translation strings for built-in Laravel features such as validation error messages:

    /lang
        /en
            messages.php
        /es
            messages.php

Or, translation strings may be defined within JSON files that are placed within the `lang` directory. When taking this approach, each language supported by your application would have a corresponding JSON file within this directory. This approach is recommended for application's that have a large number of translatable strings:

    /lang
        en.json
        es.json

このドキュメントでは、翻訳文字列を管理する各アプローチについて説明します。

<a name="configuring-the-locale"></a>
### ロケールの設定

アプリケーションのデフォルト言語は、`config/app.php`設定ファイルの`locale`設定オプションに保存します。アプリケーションのニーズに合わせて、この値を自由に変更してください。

`App`ファサードが提供する`setLocale`メソッドを使用して、単一のHTTPリクエストの間のデフォルト言語を実行時に変更できます。

    use Illuminate\Support\Facades\App;

    Route::get('/greeting/{locale}', function ($locale) {
        if (! in_array($locale, ['en', 'es', 'fr'])) {
            abort(400);
        }

        App::setLocale($locale);

        //
    });

「フォールバック言語」も設定できます。これは、アクティブな言語に対応する特定の翻訳文字列が含まれていない場合に使用されます。デフォルト言語と同様に、フォールバック言語も`config/app.php`設定ファイルで指定します。

    'fallback_locale' => 'en',

<a name="determining-the-current-locale"></a>
#### 現在のロケールの決定

`App`ファサードの`currentLocale`メソッドと`isLocale`メソッドを使用して、現在のロケールを判定したり、ロケールが指定する値であるかどうかを確認したりできます。

    use Illuminate\Support\Facades\App;

    $locale = App::currentLocale();

    if (App::isLocale('en')) {
        //
    }

<a name="defining-translation-strings"></a>
## 翻訳文字列の定義

<a name="using-short-keys"></a>
### 短縮キーの使用

Typically, translation strings are stored in files within the `lang` directory. Within this directory, there should be a subdirectory for each language supported by your application. This is the approach Laravel uses to manage translation strings for built-in Laravel features such as validation error messages:

    /lang
        /en
            messages.php
        /es
            messages.php

すべての言語ファイルは、キー付き文字列の配列を返します。例えば:

    <?php

    // lang/en/messages.php

    return [
        'welcome' => 'Welcome to our application!',
    ];

> {note} 地域によって異なる言語の場合、ISO15897に従って言語ディレクトリに名前を付ける必要があります。たとえば、「en-gb」ではなく「en_GB」をイギリス英語に使用する必要があります。

<a name="using-translation-strings-as-keys"></a>
### 翻訳文字列をキーとして使用

翻訳可能な文字列が多数あるアプリケーションの場合、「短縮キー」ですべての文字列を定義すると、ビューでキーを参照するときに混乱する可能性があり、アプリケーションがサポートするすべての翻訳文字列のキーを継続的に作成するのは面倒です。

For this reason, Laravel also provides support for defining translation strings using the "default" translation of the string as the key. Translation files that use translation strings as keys are stored as JSON files in the `lang` directory. For example, if your application has a Spanish translation, you should create a `lang/es.json` file:

```json
{
    "I love programming.": "Me encanta programar."
}
```

#### キー／ファイルの競合

他の翻訳ファイル名と競合する翻訳文字列キーを定義しないでください。たとえば、`nl/action.php`ファイルは存在するが`nl.json`ファイルは存在しないときに"NL"ロケールの`__('Action')`を翻訳すると、トランスレータは`nl/action.php`の内容を返します。

<a name="retrieving-translation-strings"></a>
## 翻訳文字列の取得

You may retrieve translation strings from your language files using the `__` helper function. If you are using "short keys" to define your translation strings, you should pass the file that contains the key and the key itself to the `__` function using "dot" syntax. For example, let's retrieve the `welcome` translation string from the `lang/en/messages.php` language file:

    echo __('messages.welcome');

指定する翻訳文字列が存在しない場合、`__`関数は翻訳文字列キーを返します。したがって、上記の例を使用すると、翻訳文字列が存在しない場合、`__`関数は`messages.welcome`を返します。

[デフォルトの翻訳文字列を翻訳キー](#using-translation-strings-as-keys)として使用している場合は、文字列のデフォルトの翻訳を`__`関数に渡す必要があります。

    echo __('I love programming.');

この場合も、翻訳文字列が存在しない場合、`__`関数は指定する翻訳文字列キーを返します。

[Bladeテンプレートエンジン](/docs/{{version}}/Blade)を使用している場合は、`{{}}`エコー構文を使用して翻訳文字列を表示できます。

    {{ __('messages.welcome') }}

<a name="replacing-parameters-in-translation-strings"></a>
### 翻訳文字列中のパラメータの置換

必要に応じて、翻訳文字列にプレースホルダーを定義できます。すべてのプレースホルダーには接頭辞`:`が付いています。たとえば、プレースホルダー名を使用してウェルカムメッセージを定義できます。

    'welcome' => 'Welcome, :name',

翻訳文字列を取得するときにプレースホルダーを置き換えるには、置換の配列を２番目の引数として`__`関数に渡します。

    echo __('messages.welcome', ['name' => 'dayle']);

プレースホルダーにすべて大文字が含まれている場合、または最初の文字のみが大文字になっている場合、変換された値はそれに応じて大文字になります。

    'welcome' => 'Welcome, :NAME', // Welcome, DAYLE
    'goodbye' => 'Goodbye, :Name', // Goodbye, Dayle

<a name="pluralization"></a>
### 複数形

別々の言語には複数形のさまざまな複雑なルールがあるため、複数形化は複雑な問題です。ただし、Laravelは、定義した複数化ルールに基づいて文字列を異なる方法で翻訳する手助けができます。`|`文字を使用すると、文字列の単数形と複数形を区別できます。

    'apples' => 'There is one apple|There are many apples',

もちろん、[翻訳文字列をキー](#using-translation-strings-as-keys)として使用する場合でも、複数形化をサポートしています。

```json
{
    "There is one apple|There are many apples": "Hay una manzana|Hay muchas manzanas"
}
```

複数の値の範囲の変換文字列を指定する、より複雑な複数化ルールを作成することもできます。

    'apples' => '{0} There are none|[1,19] There are some|[20,*] There are many',

複数化オプションのある翻訳文字列を定義した後、`trans_choice`関数を使用して、指定する「個数」に合った行を取得できます。以下の例では、個数が１より大きいため、複数形の翻訳文字列が返されます。

    echo trans_choice('messages.apples', 10);

複数化文字列でプレースホルダー属性を定義することもできます。これらのプレースホルダーは、`trans_choice`関数の３番目の引数として配列を渡すことで置き換えられます。

    'minutes_ago' => '{1} :value minute ago|[2,*] :value minutes ago',

    echo trans_choice('time.minutes_ago', 5, ['value' => 5]);

`trans_choice`関数に渡した整数値を表示したい場合は、組み込みの`:count`プレースホルダーを使用できます。

    'apples' => '{0} There are none|{1} There is one|[2,*] There are :count',

<a name="overriding-package-language-files"></a>
## パッケージ言語ファイルのオーバーライド

Some packages may ship with their own language files. Instead of changing the package's core files to tweak these lines, you may override them by placing files in the `lang/vendor/{package}/{locale}` directory.

So, for example, if you need to override the English translation strings in `messages.php` for a package named `skyrim/hearthfire`, you should place a language file at: `lang/vendor/hearthfire/en/messages.php`. Within this file, you should only define the translation strings you wish to override. Any translation strings you don't override will still be loaded from the package's original language files.
