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