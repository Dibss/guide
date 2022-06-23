# Laravel

1. tutti i passaggi di laravel auth

1. Fare il .env
2. Creare cartella vendor (se non c'è?)
3. ``` composer install ```
4. ``` npm i ```
5. ``` npm run dev ```
6. ``` npm run watch ```
7. ``` php artisan key:generate ```
8. Creare database MAMP es. holidays_db
9. ``` php artisan make:migration create_holiday_table ```
10. Decidere le varie colonne della table: esempio
```php
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title')->unique();
    $table->text('description');
    $table->string('image');
    $table->string('slug')->unique();
    $table->timestamps();
});
```
slug rende tutto minuscolo e-con-i-trattini-al-posto-degli-spazi
11. ```php artisan migrate```
12. ```php artisan make:model Models/Holiday``` (php artisan make:model Models/Holiday -m) - per creare model e migration insieme
13. ```php artisan make:seeder HolidayTableSeeder```
14. Add to HolidayTableSeeder -> use App\Models\Holiday
  1. guardare la guida per faker in basso
11. in DatabaseSeeder.php scommentare $this... e cambiare il nome del seeder con il nostro
15. ``` php artisan db:seed --class=HolidayTableSeeder ```
16. Se non funziona, in MOdels\Holiday -> ```protected $table = 'holiday';```

17. Se continua a non funzionare:
``` php artisan route:cache && php artisan route:clear && php artisan config:cache && php artisan config:clear && php artisan optimize```
18. ``` php artisan make:controller HolidayController --resource ```
21. Nella view:
```php
@foreach ($holiday as $key => $item)
  // codice html
  {{ $item->nome }}
  // ecc...
@endforeach
```

22. form.blade.php (?)
23. ``` Route::resource('/', 'HolidayController'); ```
24. ``` php artisan route:list ```
25. in HolidayController aggiungere ```use App\Models\Holiday;```
26. nella cartella admin creare una cartella holidays e in holidays index.blade.php, show.blade.php etc. in base alle crud
27. es -> in index.blade.php usare @section('content') facendo riferimento allo yield in app.blade.php
28. per collegare le pagine usare il lato sinistro del menu che risulterà vuoto con per esempio:
```<li><a href="{{ route('admin.posts.index') }}">Posts</a></li>```
29. esempio button per vedere il prodotto singolo -> ```<button><a href="{{ route('admin.posts.show', $post->id) }}">View</a></button>```
30. per le pagine a piè di pagina:
```php
@if($posts->hasPages())
  {{ $posts->links() }}
@endif
```
^^^ questo va messo alla fine dell'index subito prima della chiusura dell'ultimo div
```$posts = Post::orderBy('updated_at', 'DESC')->paginate(5);```
^^^ questo sta nell'index() crud

### String slug
1. use Illuminate\Support\Str;
2. 
```php
$post->slug = $slug = Str::slug($post->title, '-');
```
per rendere il titolo minuscolo con i trattini
### Per fillare con il seeder senza faker, e anche per creare un user base che non si cancella per fare i test (NON SI FA, E' SOLO PER I TEST)
```php
use Illuminate\Database\Seeder;
use App\User;

class UserSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        $user = new User();
        $user->name = 'Pippo';
        $user->email = 'test@test.it';
        $user->password = bcrypt('password');
        $user->save();
    }
}
```
### Per fillare da un db php in config il database su mamp:
1. 
```
public function run()
  {

    $comics = config('comics');
    foreach($comics as $comic){
        $new_comic = new Comic();

        $new_comic->fill($comic);
        $new_comic->save();
    }

  }
```

2. se non funziona aggiungere in Models\nomeDelFile:
``` protected $fillable = ['nomeColonna', 'e tutti gli altri nomi delle colonne separati']; ```
### Faker (solo se si vogliono usare dati falsi)
1. ```composer remove fzaninotto/faker```
2. ```php artisan migrate``` -> remove from require dev? yes
3. ```composer require fakerphp/faker```
4. Add to HolidayTableSeeder -> use Faker\Generator as Faker
5. 
```php
public function run(Faker $faker){
  
  for($i = 0; $i < 10; $i++){

    $holiday = new Holiday();
    // l'id() non serve nel faker e nemmeno timeStamps()
    $holiday->nome = $faker->state();
    $holiday->destinazione = $faker->city();
    // etc...
    $holiday->save();

  }

}
```
# Se si vuole creare un nuovo file js in resources/js:
1. creare il file nella cartella
2. in webpack.mix.js aggiungere la riga per il nuovo file: 
```
mix.js('resources/js/app.js', 'public/js')
    .js('resources/js/nuovoFile.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
```
3. lanciare di nuovo ```npm run dev``` e ```npm run watch```
4. inserire nella pagina:
```
section('delete-message')
  <script src="{{ assset('js/nuovoFile.js') }}"></script>
@endsection
```

# Comandi git terminale

``` git checkout . ``` <br>
``` git status ``` <br>
``` git sreset --hard ``` <br>
``` git clean -f -d ``` <br>
``` git pull ``` <br>