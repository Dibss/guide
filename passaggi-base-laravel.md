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

## Sintassi index
1. in HolidayController index() -> ``` $holiday = Holiday::all(); ```
2. ``` return view('index', compact('holiday')); ```
3. 
```php
public function index()
{
    $post = Post::all();

    return view('index', compact('post'));
}
```
## Sintassi Show

Con questa sintassi nei parametri la show trova direttamente l'id senza poi dover scrivere $comic = Comic::findOrFail($id);

```
public function show(Comic $comic)
{
    return view('comics.partials.comic', compact('comic'));
}
```


### Per inserire l'informazione nella show.blade.php (e non solo) senza tag html

```
{{!! $comic->title !!}}
```
## Sintassi Create

```
public function create()
  {
      return view('comics.create');
  }
```

### Per non dover riscrivere tutti i dati nel form se non va a buon fine la creazione dell'elemento:
Da mettere tra gli attributi degli input
```
value="{{ old('title') }}"
```

### Se riguarda una select:
```
<div>
  <select name='esempio0' required>
    <option @if(old('esempio0'))>esempio2</option>
    <option @if(old('esempio0'))>esempio3</option>
  </select>
</div>
```
## Sintassi Store

Mettiamo i dati inseriti dall'utente($request) in una variabile $data, filliamo una nuova row con i $data, la salviamo e poi reindirizziamo alla pagina precedente

```
  public function store(Request $request)
  {
  
      $request->validate(
            [
                "name"=>"required",
            ]
      );
  
      $data = $request->all();

      $new_comic = new Comic();
      $new_comic->fill($data);
      $new_comic->save();

      return redirect()->route('comics.show', $new_comic);
  }
```

### Se abbiamo bypassato nella create un dato che avevamo messo obbligatorio dobbiamo 'ridichiararlo' nella store tipo: (in questo caso lo slug)
```php
$request->validate(
    [
        "title"=>"required",
        "description"=>"required",
        "image"=>"required",
    ]
);

$data = $request->all();

$new_post = new Post();
$new_post->fill($data);
$post->slug = Str::slug($post->title, '-');
$new_post->save();

return redirect()->route('admin.posts.show', $new_post);
```
in questo caso aggiungere al controller anche ```use Illuminate\Support\Str;``

## Sintassi Edit

```
public function edit(Comic $comic)
{
    return view('comics.edit', compact('comic'));
}
```
## Sintassi update

```
public function update(Request $request, Comic $comic)
{
    $data = $request->all();

    $comic->fill($data);
    $comic->save();

    return redirect()->route('comics.show', $comic)->with('message', "Hai aggiornato con successo: $comic->title");;
}
```
## Sintassi delete

```
public function destroy(Comic $comic)
{
    $comic->delete();

    return redirect()->route('comics.index')->with('message', "Hai eliminato con successo: $comic->title");
}
```

### Messaggio nell'index.blade.php per l'elemento eliminato o modificato

```
<!-- messaggio di eliminazione o modifica scritto nella funzione destroy/update del controller -->
  @if(session('message'))
    <div>
      {{ session('message') }}
    </div>
  @endif
```

### Delete button in index.blade.php

```
<form action="{{ route( 'comics.destroy', $item->id) }}" method="post">
  @method('DELETE')
  @csrf
  <button type="submit">Delete</button>
</form>
```

### Esempio file deleteMessage confermare la cancellazione: (consigliato fare uno @yield('scripts') in fondo alla pagina app.blade.php nella cartella layout)

```
const deleteForms = document.querySelectorAll('.delete-form');

deleteForms.forEach(element => {
  
  const title = element.getAttribute('data-title')

  element.addEventListener('submit', (e)=>{
    // prevent default previene il refresh della pagina
    e.preventDefault();
    const accept = confirm(`Sei sicuro di voler eliminare: ${title}?`);
    if(accept) e.target.submit();
  })

});
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

## Comandi git terminale

``` git checkout . ``` <br>
``` git status ``` <br>
``` git sreset --hard ``` <br>
``` git clean -f -d ``` <br>
``` git pull ``` <br>

## Esempio form edit

```
  <div>
    <h2>Modifica il fumetto</h2>
  </div>

  @if( $errors->any() )
    <div>
      <ul>
        @foreach ( $errors->all() as $error )
          <li>{{ $error }}</li>
        @endforeach
      </ul>
    </div>
  @endif

  <form action="{{ route('comics.update', $comic->id) }}" method="post">

    @method('PUT')

    @csrf

    <div>
      <label for="title">Title:</label>
      <input type="text" name="title" value="{{ old('title', $comic->title) }}" id="title" required>
      <!-- <div class="form-text">
      </div> -->
    </div>
    <div>
      <label for="description">Description:</label>
      <input type="text" name="description" value="{{ old('description', $comic->description) }}" id="description" required>
    </div>
    <div>
      <label for="image">Image (url link):</label>
      <input type="text" name="image" value="{{ old('image', $comic->image) }}" id="image">
    </div>
    <div>
      <label for="price">Price:</label>
      <input type="number" name="price" value="{{ old('price', $comic->price) }}" id="price" required>
    </div>
    <div>
      <label for="series">Series:</label>
      <input type="text" name="series" value="{{ old('series', $comic->series) }}" id="series" required>
    </div>
    <div>
      <label for="sale_date">Sale date:</label>
      <input type="date" name="sale_date" value="{{ old('sale_date', $comic->sale_date) }}" id="sale_date" required>
    </div>
    <div>
      <button type="submit">Aggiorna</button>
    </div>
  </form>
```

### Se c'è una select:
```
<div>
  <select name='esempio' required>
    <option @if($esempio->esempio1 === esempio2) selected @endif>esempio2</option>
    <option @if($esempio->esempio1 === esempio3) selected @endif>esempio3</option>
  </select>
</div>
```

### Se c'è una textarea:
```
<textarea name="" id="" cols="30" rows="10">
  {{ $comic->description }}
</textarea>
```

## Esempio form create

```
  <div>
    <h2>Aggiungi un fumetto</h2>
  </div>

  @if ( $errors->any() )
    <div>
      <ul>
        @foreach ( $errors->all() as $error )
          <li>{{ $error }}</li>
        @endforeach
      </ul>
    </div>
  @endif

  <form action="{{ route('comics.store') }}" method="post">
  
    @csrf

    <div>
      <label for="title">Title:</label>
      <input type="text" name="title" id="title" required>
      <!-- <div class="form-text">
      </div> -->
    </div>
    <div>
      <label for="description">Description:</label>
      <input type="text" name="description" id="description" required>
    </div>
    <div>
      <label for="image">Image (url link):</label>
      <input type="text" name="image" id="image">
    </div>
    <div>
      <label for="price">Price:</label>
      <input type="number" name="price" id="price" required>
    </div>
    <div>
      <label for="series">Series:</label>
      <input type="text" name="series" id="series" required>
    </div>
    <div>
      <label for="sale_date">Sale date:</label>
      <input type="date" name="sale_date" id="sale_date" required>
    </div>
    <div>
      <button type="submit">Aggiungi</button>
    </div>
  </form>
```




