

    creare progetto laravel sul pc
    creare repo vuota
    collegare repo con progetto
    mi sposto con il temrinale dentro la cartella di lavoro
    Pulisco la cache di npm se ci sono errori: npm cache clear --force
    lancio il comando npm i
    creare un nuovo DB
    Collegare il DB tramite il file .env
    Riavviare il server con php artisan serve
    Creare 2 cartelle nella cartella views per separare frontend da backend
    modificare il percorso delle rotte in web.php e homeController
    Modificare il file RouteServiceProvider con il path della nuova pagina di atterraggio dopo il Login
    Eliminare il file homeCOntroller di default
    Ricraere l'homeController con il terminale nella cartella Admin per separare frontend da backend nei controlli: php artisan make:controller Admin/HomeController
    Riscritto la funzione index del vecchio homeController nel nuovo HomeController
    Creato nel web.php la struttura pe ril gruppo di rotte gestite da middleware

Route::middleware('auth')
    ->name('admin.')
    ->prefix('admin')
    ->namespace('Admin')
    ->group(function(){
        Route::get('/', 'HomeController@index')->name('home');
});

    Creiamo il controller che gestisce le crud della tabella di riferimento: php artisan make:controller Admin/BookController -r
    Creiamo anche il modello, migration e seeder: php artisan make:model Models/Book -ms
    Rimosso composer remove fzaninotto/faker
    Installato faker: composer require fakerphp/faker
    Impostato la migration con le rispettive colonne dell'entità
    Impostato il seeder utilizzando faker
    creato un seeder per gestire lo user di default
    Inseriti i seeder nel file databaseSeeder, all'interno di parentesi quadre se sono più di 1

    public function run()
    {
        $this->call(
            [
                UserSeeder::class,
                BookSeeder::class
            ]
        );
    }

    Rilanciato da zero la migration: php artisan migrate:refresh --seed
    Aggiornato il file web.php per inserire il collegamento al controller Book:

    Route::middleware('auth')
        ->name('admin.')
        ->prefix('admin')
        ->namespace('Admin')
        ->group(function(){
            Route::get('/', 'HomeController@index')->name('home');
            Route::resource('books', 'BookController');
    });

    Aggiornare index del bookController:

    public function index()
    {
        //Ricordiamoci di importare il modello App\Models\Book
        $books = Book::All();
        return view('admin.books.index', compact('books'));
    }

    Creare il file della view nella cartella admin nella cartella books
    Estendiamo il layout nella view
    Richiamiamo i dati nella view con un ciclo forElse
    Inserito il bottone nella action per ogni record per collegarci alla funzione show
    impostata la funzione show nelle CRUD di book:

    public function show(Book $book)
    {
        return view( 'admin.books.show', compact('book') );
    }

    Creato il file per la show nelle view strutturandone l'aspetto
    creato bottone per la create nella index
    impostato la funzione di create nelle CRUD
    Inserito nuovo file create.blade.php nella view con all'interno il form per creare il nuovo book
    IMpostata la funzione di create e store

    public function create()
    {
        return view('admin.books.create');
    }


       public function store(Request $request)
    {
        $data = $request->All();

        $book = new Book();
        $book->fill($data);
        $book->save();

        return redirect()->route('admin.books.index')->with('message', "Hai creato il nuovo libbro $book->title");
    }

    imposta la edit insieme al bottone nella index per ottenere la pagina con il form di modifica
    impostato la funzione update nelle CRUD


        public function edit(Book $book)
    {
        return view( 'admin.books.edit', compact('book') );
    }

       public function update(Request $request, Book $book)
    {
        $data = $request->All();

        $book->update($data);

        return redirect()->route('admin.books.show', compact('book'))->with('message', "Hai modificato il libbro $book->title correttamente");
    }

    Nella edit abbiamo inserito li stessi campi del form della create abbinando la funzione old() nel value per ottenere i dati pregressi del DB di ogni campo

