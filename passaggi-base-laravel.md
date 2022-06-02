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

## Sintassi Create

```
public function create()
  {
      return view('comics.create');
  }
```

## Sintassi Store

Mettiamo i dati inseriti dall'utente($request) in una variabile $data, filliamo una nuova row con i $data, la salviamo e poi reindirizziamo alla pagina precedente

```
  public function store(Request $request)
  {
      $data = $request->all();

      $new_comic = new Comic();
      $new_comic->fill($data);
      $new_comic->save();

      return redirect()->route('comics.show', $new_comic);
  }
```

## Comandi git terminale

``` git checkout . ``` <br>
``` git status ``` <br>
``` git sreset --hard ``` <br>
``` git clean -f -d ``` <br>
``` git pull ``` <br>



