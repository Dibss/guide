### con -mrcs crea in automatico migration, resource controller e seeder:
es.
```php artisan make:model Models/Category -mrcs```

1. per prima cosa si fa la migration con le colonne della nuova table da collegare
```php
public function up()
{
    Schema::create('categories', function (Blueprint $table) {
        $table->id();
        $table->string('label', 30);
        $table->string('color', 30);
        $table->timestamps();
    });
}
```
2. ```php artisan migrate```
3. poi si passa al seeder
Aggiungere ```use App\Models\Category;```
```php
public function run()
{
    $categories = [
        ['label' => 'HTML', 'color' => 'blue'],
        ['label' => 'CSS', 'color' => 'tomato'],
        ['label' => 'Javascript', 'color' => 'green'],
        ['label' => 'VueJs', 'color' => 'purple'],
        ['label' => 'Laravel', 'color' => 'yellow'],
    ];

    foreach ($categories as $category) {
        
        $newCategory = new Category();
        $newCategory->label = $category['label'];
        $newCategory->color = $category['color'];
        $newCategory->save();
        
    }
}
```
4. Aggiungere il seeder a DatabaseSeeder.php
  1. deve essere cosi (con le quadre):
  ```
  $this->call([
      UserSeeder::class,
      CategorySeeder::class,
      PostSeeder::class,
  ]);
  ```
5. ```php artisan db:seed```
6. ```php artisan make:migration add _category_id_to_posts_table --table=posts```
la function down deve essere vuota
7. nella migration add category ecc (in function up()):
```
$table->unsignedBigInteger('category_id')->nullable()->after('id');

$table->foreign('category_id')
      ->references('id')
      ->on('categories')->onDelete('set null');
```
8. nella function down():
```
$table->dropForeign('posts_category_id_foreign');

$table->dropColumn('category_id');
```
9. per la relazione one to many:
 1. in category.php:
 ```
 public function posts(){
      return $this->hasMany('App\Models\Post');
  }
 ```
 2. in post.php:
 ```
  public function category(){
      return $this->belongsTo('App\Models\Category');
  }
 ```
10. in PostSeeder aggiungere la colonna category_id e ```use App\Models\Category;``` e ```use Illuminate\Support\Arr;```:
```$category_ids = Category::pluck('id')->toArray();```
^^^ questo sopra al foreach
```$post->category_id = Arr::random($category_ids);```
^^^ questo nel foreach dopo id
11. aggiungere al fillable in Post.php category_id
12. ```php artisan migrate:refresh --seed```
13. se non funziona usare questa sintrassi per i seed che non sono stati caricati
```php artisan db:seed --class=HolidayTableSeeder```
<!-- 14. aggiugere nel PostController ```use App\Models\Category;``` e la categorie nell'index():
```php
public function index()
{
    $posts = Post::all();
    $categories = Category::all();

    return view('admin.posts.index', compact('posts', 'categories'));
}
``` -->
15. aggiungere all'index.blade.php e alla show.blade.php la nuova colonna category_id (nella show anche senza l'@else):
```php
@if($post->category)
  <span style="background-color: {{$post->category->color}};">{{$post->category->label}}</span>
@else
  <span>-</span>
@endif
```
16. aggiungere a create nelle crud le label:
```php
public function create()
{
  $categories = Category::all();

  return view('admin.posts.create', compact('categories'));
}
```
17. e a create.blade.php:
```php
<div>
  <label for="category">Category</label>
  <select name="category_id" id="category">
    <option value="">Nessuna categoria</option>
    @foreach ( $categories as $category )
      <option @if(old('category_id', $post->category_id == $category->id)) selected @endif value="{{$category->id}}">{{ $category->label }}</option>
    @endforeach
  </select>
</div>
```
18. se si vogliono mettere i post dal più nuovo al più vecchio: 
```posts = Post::orderBy('updated_at', 'DESC')->get();```
19. aggiungere ```Route::resource('categories', 'CategoryController');``` a web.php
20. compilare l'index del categorycontroller, creare index.blade.php in una nuova cartella dentro admin 'categories' e compilare il file.
21. se non funziona cancellare il controller fatto dal comando e rifare solo quello manualmente: php artisan make:controller Admin\CategoryController -r


100. nella create la sintassi dell'old è:
```value="{{ old('image') }}"```
