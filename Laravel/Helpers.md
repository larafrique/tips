# Helpers


### Un peu d'optimisation avec `abort_if`

```php
// Méthode 1 👎👎👎

if($post->category_id == $category->id) {
    abort (404);
}

// Méthode 2 🚀🚀🚀
abort_if($post->category_id == $category->id, 404);
```
___



### Générer des numéros de téléphone avec Faker `fake()`

✨Astuce : générer facilement des numéros de téléphone aléatoires avec `Faker` grâce au helper `fake()` de Laravel.

```php
echo fake()->numerify("###-###-###");
// 995-399-340

echo fake()->numerify("### ### ###");
// 918 201 142

echo fake()->numerify("+243 ### ### ###");
// +243 489 525 424

echo fake()->numerify("+243 9## ### ###");
// +243 917 704 960
```
___



### Formater un nombre ne version abrégée avec `Number::abbreviate()`

Découvrez la méthode `Number::abbreviate()` qui formate un nombre en version abrégée avec un suffixe (K, M, B, T, ...). Utile pour afficher des nombres longs de façon compacte et plus humaine.

```php
Number::abbreviate(950);
// "950" (pas assez grand pour être abrégé)

Number::abbreviate(1_200);
// "1.2K"

Number::abbreviate(15_000);
// "15K"

Number::abbreviate(2_530_000);
// "2.53M"

Number::abbreviate(7_800_000_000);
// "7.8B"

Number::abbreviate(1_000_000_000_000);
// "1T"

Number::abbreviate(1250000, precision: 2);
// "1.25M"
```
___


### Rediriger vers une URL

Voici quelques moyens de faire une redirection vers une route.

```php
// Méthode 👎👎👎
return redirect(route('posts.show', $post));

// Méthode 2 ✅✅✅
return redirect()->route('posts.show', $post);

// Méthode 3 🚀🚀🚀
return to_route('posts.show', $post);
```
___



### Générer un slug facilement via la façade `Str`

```php
Str::slug('Laravel Facade Str est géniale!');
// "laravel-facade-str-est-geniale"
```
___


### Afficher des dates lisibles par les humains avec `diffForHumans`

As-tu déjà voulu afficher des dates lisibles par les humains plutôt que des dates exactes ?
Par exemple : « il y a 1 jour » ou « il y a un mois » ?
Laravel te permet de faire exactement cela grâce à la méthode `diffForHumans` de `Carbon` 🚀

```php
use App\Models\Post;

$post = Post::first(); // ['id' => 1, 'created_at' => '2025-08-31 21:36]

$post->created_at->diffForHumans();
// il y a 4 semaines

$post->created_at->diffForHumans(['parts' => 2]);
// il y a 4 semaines 1 jour

$post->created_at->diffForHumans(['parts' => 4]);
// il y a 4 semaines 1 jour 21 heures 12 minutes

// ⚠️ Pour que la méthode `diffForHumans` renvoie du texte en francais,
// vous devez préciser une locale francaise dans le fichier .env “APP_LOCALE=fr"’
```
___



### Afficher un nombre... mais en toutes lettres avec `Number::spell`

Vous est-il déjà arrivé d’avoir besoin d’afficher un nombre… mais en toutes lettres ? ✨
Avec `Number::spell`, c’est possible en une seule ligne ! 
🚀🚀

```php
use Illuminate\Support\Number;

$number = Number::spell(102);
// one hundred and two

$number = Number::spell(88, locale: 'fr');
// quatre-vingt-huit

$number = Number::spell(1_000_000, locale: 'fr');
// un million

$number = Number::spell(1_147_544, locale: 'fr');
// un million cent quarante-sept mille cinq cent quarante-quatre
```
___



### Vérifier facilement si une chaîne est une URL valide

Saviez-vous que Laravel propose `Str::isUrl()` pour vérifier facilement si une chaîne est une URL valide ? Pratique et rapide !🚀

```php
use Illuminate\Support\Str;

Str::isUrl('https://larafrique.com', ['https', 'http']); // true
Str::isUrl('http://larafrique.test', ['https']); // false
Str::isUrl('ftp://larafrique.com'); // true
Str::isUrl('url-non-valide'); // false
```
___



### La jointure avancée avec `join()`

Saviez-vous que la méthode `join()` sur les collections fait bien plus qu’un simple `implode() ?`
Il est possible de définir un séparateur spécifique pour le dernier élément 🚀🚀🚀

```php
// Ceci fait un `implode()` classique 🙂🙂🙂

collect(['larafrique', 'user_1', 'user_2'])
    ->join(', '); // larafrique, user_1, user_2

// Astuce : vous pouvez définir un séparateur
// spécifique pour le dernier élément 🚀🚀🚀
collect(['larafrique', 'user_1', 'user_2'])
    ->join(', ', ' et '); // larafrique, user_1 et user_2 ✅

collect(['larafrique', 'user_1', 'user_2'])
    ->join(', ', ' ou '); // larafrique, user_1 ou user_2 ✅
```
___



### Prévisualiser des mails facilement.

Laravel permet de prévisualiser n'importe quel e-mail directement dans le navigateur, sans l'envoyer réellement.🚀🚀🚀
Il suffit de retourner une instance d'un Mailable depuis une route, comme ceci :

```php
use App\Models\User;
use App\Mail\WelcomeUserMail;

Route::get('/welcome-mail-preview', function () {

    $user = User::first();

    return new WelcomeUserMail($user);
});
```

![Prévisualation du mail](../images/laravel/mail-preview.png)
___



### Afficher la taille (d'un fichier)... en toute lettre

```php
use Illuminate\Support\Number;

$size = Number::fileSize(1024); // 1 KB

$size = Number::fileSize(1024 * 1024); // 1 MB

$size = Number::fileSize(1024, precision: 2); // 1.00 KB

```
___



### Manipuler facilement les dates avec le helper `today()`

Voici des méthodes super utiles et élégantes que vous pouvez utiliser avec le helper `today()` pour manipuler facilement les dates à partir de la date actuelle 💫

```php
today()->startOfWeek(); // Lundi de cette semaine
today()->endOfWeek(); // Dimanche
today()->startOfMonth(); // 1er du mois
today()->endOfMonth(); // 31/30/28 selon le mois

today()->isMonday(); // true/false
today()->isWeekend(); // true/false
today()->isLastOfMonth(); // true/false
```
___



### Vérifier rapidement si une chaîne est un JSON valide

Str::isJson(), pratique pour vérifier rapidement si une chaîne est un JSON valide avant de la décoder ou de l'utiliser dans la logique. Et lorsque tu passeras à PHP 8.3, Laravel utilisera automatiquement la fonction native "json_validate()" sous le capot.🚀

```php
use Illuminate\Support\Str;

Str::isJson('[1, 2, 3]');
// true

Str::isJson('{"name": "Alice", "role": "admin"}')
// true

Str::isJson('{name: "Alice", role: "admin"}');
// false

Str::isJson('Hello Laravel');
// false

Str::isJson('null');
// true
```
___



### Affichez vos durées proprement avec `CarbonInterval`

Marre d'afficher "125 secondes" à vos utilisateurs ? Transformez vos secondes en texte lisible en une seule ligne de code avec CarbonInterval.

```php
use Carbon\CarbonInterval;

$duration = 185;

echo CarbonInterval::seconds($duration)->cascade()->forHumans();
// 185 secondes -> "3 minutes 5 secondes"

echo CarbonInterval::seconds($duration)->cascade()->forHumans(['short' => true]);
// En version courte : "3m 5s"
```
___



