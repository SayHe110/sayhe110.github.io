---
title: 【外文翻译】使用 Laravel 时的一些小技巧
date: 2019-04-04 10:05:09
tags:
---
> [译文地址](https://meramustaqbil.com/2019/03/23/20-un-known-gems-of-laravel/)
> 第一次翻译文章，如有翻译不好的地方还请大家指出，大家也可以直接看原文。

### 01: 触发父级的时间戳
如标题所示，在子模型更新时，可以触发父模型的时间戳。例如 `Comment` 属于 `Post`，有时更新子模型导致更新父模型时间戳非常有用。例如，当 `Comment` 模型被更新时，您要自动触发父级 `Post` 模型的 `updated_at` 时间戳的更新。`Eloquent` 让它变得简单，只需添加一个包含子模型关系名称的 `touch` 属性。
```
<?php
namespace App;
use Illuminate\Database\Eloquent\Model;
class Comment extends Model
{
    /**
     * 涉及到的所有关联关系。
     *
     * @var array
     */
    protected $touches = ['post'];
    /**
     * 获取评论所属的文章。
     */
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
```

### 02: 预加载精确的列
在使用预加载时，可以从关系中获取指定的列。
```
$users = App\Book::with('author:id,name')->get();
```

### 03: 为单个请求验证用户身份
你可以使用 `Auth::once()` 来为单个请求验证用户的身份，此方法不会使用 `Cookie` 会话。这意味着此方法可能有助于构建无状态 **API** 。
```
if (Auth::once($credentials)) {
    //
}
```

### 04: 重定向到带有参数的控制器方法中
你不仅可以将 `redirect()` 方法用于用户特定的 URL 或者路由中，还可以用于控制器中带有参数的方法中。
```
return redirect()->action('SomeController@method', ['param' => $value]);
```

### 05: 如何使用 `withDefault()` 避免在关系中出现的错误
当一个关系被调用时，如果它不存在，则会出现致命的错误，例如 `$post->user->name` ，可以使用 `withDefault()` 来避免。
```
/** 获取文章作者 */ 
public function user() 
{     
	return $this->belongsTo('App\User')->withDefault(); 
}
```

### 06: 在模版中两个平级的 `$loop` 变量
在 `blade` 的 `foreach` 中，即使在两次循环中，依然可以通过使用 `$loop` 变量来获取父级变量。
```
@foreach ($users as $user)     
	@foreach ($user->posts as $post)         
		@if ($loop->parent->first)             
			This is first iteration of the parent loop.         
		@endif     
	@endforeach 
@endforeach
```

### 07: 修改查询结果
在执行 `Eloqument` 查询后，你可以使用 `map()` 来修改行。
```
$users = User::where('role_id', 1)->get()->map(function (User $user) {
    $user->some_column = some_function($user);
    return $user;
});
```

### 08: 轻松的使用 `dd()`
在 `Eloqument` 的最后加上 `$test->dd()`，来代替 `dd($result)`。
```
// 优化前
$users = User::where('name', 'Taylor')->get();
dd($users);
// 优化后
$users = User::where('name', 'Taylor')->get()->dd();
```

### 09: Use hasMany to saveMany.
如果有 `hasMany()` 关联关系，和想要从父类对象中保存许多子类对象，可以使用 `saveMany()` 来达到你想要的效果。
```
$post = Post::find(1);
$post->comments()->saveMany([
    new Comment(['message' => 'First comment']),
    new Comment(['message' => 'Second comment']),
]);
```

### 10: 在 `Model::all()` 中指定列
当你使用 `Eloqument` 的 `Model::all()` 时，你可以指定要返回的列。
```
$users = User::all(['id', 'name', 'email']);
```

### 11: `Blade` 中的 `@auth`
你可以使用 `@auth` 指令来代替 `if` 语句来检查用户是否经过身份验证。
##### 典型的方法：
```
@if(auth()->user())     // The user is authenticated. @endif 
```
##### 简短的方法：
```
@auth    
 // The user is authenticated. 
@endauth
```

### 12: 预览邮件而不发送
如果你使用 **Mailables** 来发送你的邮件，你可以预览它们而不发送出去。
```
Route::get('/mailable', function () {
    $invoice = App\Invoice::find(1);
    return new App\Mail\InvoicePaid($invoice);
});
```

### 13: `hasMany` 的特定检查
在 `Eloquent` 的 `hasMany()` 关系中，你可以筛选出具有 n 个子记录数量的记录。
```
// Author -> hasMany(Book::class) 
$authors = Author::has('books', '>', 5)->get();
```

### 14: 恢复多个软删除
如果记录使用了软删除，那么你就可以一次恢复多条软删除记录。
```
Post::withTrashed()->where('author_id', 1)->restore();
```

### 15: 带时区的迁移列
迁移文件不仅有 `timestamps()` 时间戳，还有 `timestampsTz()` 带有时区的时间戳。
```
Schema::create('employees', function (Blueprint $table) {
	$table->increments('id');
	$table->string('name');
    $table->string('email');
    $table->timestampsTz();
});
```

### 16: 视图文件是否存在？
你知道还可以检查视图文件是否存在吗？
```
if (view()->exists('custom.page')) {
	// Load the view
}
```

### 17: 组中的路由组
在路由文件中，你可以为一个路由组创造一个组，还可以为其指定特定的中间件。
```
Route::group(['prefix' => 'account', 'as' => 'account.'], function() {
    Route::get('login', 'AccountController@login');     
    Route::get('register', 'AccountController@register');
    Route::group(['middleware' => 'auth'], function() {         
        Route::get('edit', 'AccountController@edit');     
    });
});
```

### 18: `Eloquent` 中的日期时间方法
`whereDay()` , `whereMonth()` , `whereYear()` , `whereDate()` , `whereTime()` 这些方法皆为 `Eloquent` 中检查日期的方法。
```
$products = Product::whereDate('created_at', '2018-01-31')->get(); 
$products = Product::whereMonth('created_at', '12')->get(); 
$products = Product::whereDay('created_at', '31')->get(); 
$products = Product::whereYear('created_at', date('Y'))->get(); 
$products = Product::whereTime('created_at', '=', '14:13:58')->get();
```

### 19: 在 `Eloquent` 关系中使用 `orderBy()`
你可以在 `Eloquent` 关系中直接指定 `orderBy()` 。
```
public function products()
{
    return $this->hasMany(Product::class);
}
public function productsByName()
{
    return $this->hasMany(Product::class)->orderBy('name');
}
```

### 20: 无符号整型
对于迁移的外键，不要使用 `integer()` , 而是使用 `unsignedInteger()` 或者是 `integer()->unsigned()` ，否则将会出现一系列的错误。
```
Schema::create('employees', function (Blueprint $table) {     
    $table->unsignedInteger('company_id');     
    $table->foreign('company_id')->references('id')->on('companies');     
});
```