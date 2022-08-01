1. Fare il .env
2. Creare cartella vendor (se non c'è?)
3. ``` composer install ```
4. ```composer require laravel/ui:^2.4```
5. ```npm install && npm run dev```
6. ``` npm run watch ```
8. Creare database MAMP boolbnb
7. ``` php artisan key:generate ```
8. ```php artisan ui vue --auth```

9. ```php artisan make:model Models/User -ms```
10. ``` php artisan make:controller UserController --resource ```
11. migration users
```php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('email', 100)->unique();
        $table->string('password', 30);
        $table->string('first_name', 30)->nullable();
        $table->string('last_name', 30)->nullable();
        $table->date('birth_date')->nullable();
        $table->rememberToken();
        $table->timestamps();
    });
}
```

9. ```php artisan make:model Models/Apartment -mrs```
10. ``` php artisan make:controller ApartmentController --resource ```
12. migration apartments
```php
public function up()
{
    Schema::create('apartments', function (Blueprint $table) {
        $table->id();
        $table->string('title', 100)->unique();
        $table->tinyint('rooms', 30);
        $table->tinyint('bathrooms', 30);
        $table->tinyint('beds', 30);
        $table->tinyint('mq');
        $table->string('address');
        $table->string('image');
        $table->text('description');
        $table->boolean('visibility');
        $table->boolean('sponsored')->nullable();
        $table->string('slug')->unique();

        $table->unsignedBigInteger('user_id')->nullable()->after('id');

        $table->foreign('user_id')
            ->references('id')
            ->on('users')->onDelete('set null');

        $table->rememberToken();
        $table->timestamps();
    });
}

public function down()
{
    Schema::dropIfExists('posts');

    Schema::table('apartments', function (Blueprint $table) {
        $table->dropForeign('apartments_service_id_foreign');

        $table->dropColumn('service_id');
    });
}
```
per il foreign vedere se c'è un comando a parte add_table...

```php
public function run(Faker $faker)
    {

        $user_ids = User::pluck('id')->toArray();
        $service_ids = User::pluck('id')->toArray();

        for($i = 0; $i < 10; $i++){

            $apartment = new Apartment();

            $apartment->title = $faker->text();
            $apartment->rooms = $faker->paragraph(2);
            $apartment->bathrooms = $faker->paragraph(2);
            $apartment->beds = $faker->paragraph(2);
            $apartment->mq = $faker->paragraph(2);
            $apartment->address = $faker->paragraph(2);
            $apartment->description = $faker->paragraph(2);
            $apartment->image = $faker->imageUrl(250, 250);
            $apartment->slug = $slug = Str::slug($apartment->title, '-');

            $apartment->user_id = Arr::random($user_ids);
            $apartment->service_id = Arr::random($service_ids);

            $apartment->save();
        }
    }
```

9. ```php artisan make:model Models/Service```
9. ``` php artisan make:migration create_services_table ```
10. 
```php
public function up()
{
    Schema::create('services', function (Blueprint $table) {
        $table->id();
        $table->string('label');
        $table->timestamps();
    });
}
```
13. ```php artisan make:seeder ServiceTableSeeder```
12. 
```php
public function run()
{
    $service_names = [
        'FrontEnd',
    ];

    foreach ($service_names as $service) {
        
        $newService = new Service();
        $newService->label = $service;
        $newService->save();
    }
}
```


9. ``` php artisan make:migration create_apartment_service_table ```
10. 
```php
public function up()
{
    Schema::create('apartment_service', function (Blueprint $table) {
        $table->id();

        $table->unsignedBigInteger('apartment_id');
        $table->foreign('apartment_id')->references('id')->on('apartments')->onDelete('cascade');

        $table->unsignedBigInteger('service_id');
        $table->foreign('service_id')->references('id')->on('services')->onDelete('cascade');
        $table->timestamps();
    });
}
```