# Laravel

1. Fare il .env
2. Creare cartella vendor (se non c'è?)
3. ``` composer install ```
4. ``` npm i ```
5. ``` npm run dev ```
6. ``` npm run watch ```
7. ``` php artisan key:generate ```
8. Creare database MAMP es. holidays_db
9. ``` php artisan make:migration create_holiday_table ```
10. Decidere le varie colonne della table
11. ```php artisan migrate```
12. ```php artisan make:model Models/Holiday```
13. ```php artisan make:seeder HolidayTableSeeder```
14. Add to HolidayTableSeeder -> use App\Models\Holiday

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
  
  for( i = 0; i < 10; i++){

    $holiday = new Holiday();
    // l'id() non serve nel faker e nemmeno timeStamps()
    $holiday->nome = $faker->state();
    $holiday->destinazione = $faker->city();
    // etc...
    $holiday->save();

  }

}
```

15. ``` php artisan db:seed --class=HolidayTableSeeder ```
16. Se non funziona, in MOdels\Holiday -> ```protected $table = 'holiday';```

17. Se continua a non funzionare:
``` php artisan route:cache ``` <br>
``` php artisan route:clear ``` <br>
``` php artisan config:cache ``` <br>
``` php artisan config:clear ``` <br>
``` php artisan optimize ``` <br>

18. ``` php artisan make:controller HolidayController --resource ```
19. in HolidayController index() -> ``` $holiday = Holiday::all(); ```
20. ``` return view('index', compact('holiday')); ```
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

### Esempio file deleteMessage confermare la cancellazione:

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
  <script src="{{ assset('js/deleteMessage,js') }}"></script>
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
      <input type="text" name="title" value="{{ $comic->title }}" id="title" required>
      <!-- <div class="form-text">
      </div> -->
    </div>
    <div>
      <label for="description">Description:</label>
      <input type="text" name="description" value="{{ $comic->description }}" id="description" required>
    </div>
    <div>
      <label for="image">Image (url link):</label>
      <input type="text" name="image" value="{{ $comic->image }}" id="image">
    </div>
    <div>
      <label for="price">Price:</label>
      <input type="number" name="price" value="{{ $comic->price }}" id="price" required>
    </div>
    <div>
      <label for="series">Series:</label>
      <input type="text" name="series" value="{{ $comic->series }}" id="series" required>
    </div>
    <div>
      <label for="sale_date">Sale date:</label>
      <input type="date" name="sale_date" value="{{ $comic->sale_date }}" id="sale_date" required>
    </div>
    <div>
      <button type="submit">Aggiungi</button>
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




