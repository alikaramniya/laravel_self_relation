# laravel_self_relation

#### تو این مقاله قصد ارتباط یک مدل با خودش برو براتون توضیح بدم

#### تو این مثال ما یک دسته بندی اصلی و تعدادی زیر دسته بندی داریم اول از همه زمانی که داریم جدول categories رو میسازیم کلید اصلی برای زیر دسته بندی رو به صورت زیر تعریف میکنیم

```
Schema::create('categories', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->foreignId('parent_id')
        ->nullable()
        ->constrained('categories')
        ->cascadeOnDelete();
    $table->enum('state', ['active', 'unactive']);
    $table->timestamps();
});
```

#### بعد از این ما الان یک کلید اصلی داخل جدول داریم به اسم parent_id و میتونیم از اون استفاده کینیم برای self relation خودمون

#### تو قدیم بعدی میریم به مدل Category و رابطه هامون رو برقرار میکنیم

```
public function children()
{
    return $this->hasMany(Category::class, 'parent_id', 'id');
}

public function parent()
{
    return $this->belongsTo(Category::class, 'parent_id', 'id');
}
```

### متد اول children که برای رکورد ها و دسته بندی هایی به کار میره که دسته بندی اصلی هستن و میخوایم از طریق این متد به تعداد زیر دسته بندی های اون دسترسی داشته باشیم و چون هر دسته بندی اصلی تعداد زیادی زیر دسته بندی میتونه داشته باشه پس نوع رابطه میشه hasMany

### متد دوم parent هست و برای رکرود ها و دسته بندی هایی به کار میره که اون دسته بندی یک زیر دسته بندی از دسته بندی دیگه ای هست و چون هر دسته بندی فقط یک دسته بندی اصلی داره پس میشه bleongsTo نوع رابطش

#### بعد از برقرار کردن این رابطه ها ما میتونیم که بریم توی کنترلر مد نظر و از اون ها استفاده کنیم

```
public function index(Category $category)
{
    $category->parent; // گرفتن دسته بندی اصلی برای این زیر دسته بندی خاص
    // or
    $category->load('parent');
}
```

```
public function index(Category $category)
{
    $category->children; // گرفتن همه زیر دسته بندی های این دسته بندی اصلی
    // or
    $category->load('children');
}
```

