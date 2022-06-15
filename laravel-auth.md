# Laravel Authentication

1. ```composer require laravel/ui:^2.4```
2. Creiamo lo scaffolding con auth (solo uno dei 3 comandi):
```php artisan ui vue --auth``` ||  ```php artisan ui bootstrap --auth``` || ```php artisan ui react --auth```
3. ```npm install && npm run dev```
4. collegare un database nel .env
5. ```php artisan migrate```
6. creare cartella admin in views e spostarci home.blade.php
7. creare cartella guest in views e spostarci welcome.blade.php
8. nelle rotte cambiare:
  1. da welcome a guest.welcome
  2. da /home a /admin
  3. da name->home a name->admin.home
9. nell'HomeController nella funzione index() cambiare da home a admin.home
10. in app/providers/RouteSeerviceProvider cambiare in cost HOME da /home a /admin
11. ```php artisan make:controller Admin/HomeController```no
12. tagliare la funzione index() dall'HomeController e spostarla nell'HomeController della cartella admin
13. in web.php cambiare la route dell'HomeController:
```php
Route::get('/admin', 'Admin\HomeController@index')->middleware('auth')->name('admin.home');
```
il middleware indica a laravel che la rotta sarà adibita all'autenticazione
14. se tutto funziona eliminare l'HomeController al di fuori della cartella admin
15. ```php artisan make:controller Admin/PostController -r```
meno era sarebbe --resource abbreviato
16. Aquesto punto web.php deve diventare cosi:
```php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('guest.welcome');
});

Auth::routes();

Route::middleware('auth')
->prefix('admin')
->name('admin.')
->namespace('Admin')
->group( function(){
    Route::get('/', 'HomeController@index')->middleware('auth')->name('home');
    Route::resource('posts', 'PostController');
});
```
prefix evita nella rotta|yuri /admin
name evita in name -> admin.home
namespace evita Admin\HomeController
17. cambiare in views/guest il nome da welcome a home
18. cambiare la view in web.php in guest.home
19. in view/guest/home.blade.php cancellare il contenuto nel div .content e metterci un altro div con id="id" per avviare l'app vuejs
```php
<div class="content">
    <!-- Pagina in costruzione -->
    <div id='root'>

    </div>
</div>
```
20. copiare tutto resouces/js/app.js in un altro file front.js sempre nella stessa cartella
21. in app.ja lasciare solo ```require('./bootstrap');```
22. in front.js:
```javascript
window.Vue = require('vue');

import Vue from 'vue';
import App from "./components/App.vue";

const root = new Vue({
  el: '#root',
  render: h => h(app),
})
```
23. in webpack.mix.js:
```javascript
mix.js('resources/js/app.js', 'public/js')
    .js('resources/js/front.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css');
```
24. ```npm run dev```
25. ```npm run watch```
26. Aggiungere nell'head di view/guest/home.blade.php:
```html
<!-- app.css -->
<link rel="stylesheet" href="{{ asset('css/app.css') }}">
<!-- script -->
<script src="{{ asset('js/front.js') }}" defer></script>
```
27. ora web.php deve essere cosi: 
```php
use Illuminate\Support\Facades\Route;

Auth::routes();

//localhost:8000/admin
Route::middleware('auth')
->prefix('admin')
->name('admin.')
->namespace('Admin')
->group( function(){
    Route::get('/', 'HomeController@index')->middleware('auth')->name('home');
    Route::resource('posts', 'PostController');
});

Route::get('{any?}', function(){
    return view('guest.home');
})->where('any', '.*');
```
28. in app.vue cancellare il contenuto dello script e metterci name:"App":
```vue
<script>
export default{
    name: "App",
    components: {
    }
}
</script>
```
29. ```npm run dev```
30. ```npm run watch```
31. template nuovo componente vue:
```vue
<template>
  <div>

  </div>
</template>

<script>
export default{
    name: "NomeComponente",
    components: {
    }
}
</script>

<style language='scss'>

</style>
```
32. in App.vue, sintassi:
```vue
<template>
  <div>
    <NomeComponente/>
  </div>
</template>

<script>
import NomeComponente from '../components/NomeComponente.vue'
export default{
    name: "App",
    components: {
      NomeComponente
    }
}
</script>
```