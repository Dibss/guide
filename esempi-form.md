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