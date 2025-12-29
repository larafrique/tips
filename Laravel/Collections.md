# Collections


### Utilisation de l' "Higher Order Message" `map`

```php
$users = User::all();

// `map` Méthode 1 🫠🫠🫠
$users->map(function (User $user) {
    return $user->makeHidden(['email']);
});

// `map` Méthode 2 🚀🚀🚀
$users->map->makeHidden(['email']);
```
___



### `dd` sur les collections

```php
use App\Models\User;

// Approche classique 😊😊😊
$users = User::all();
dd($users);

// meilleure approche ✅✅✅
User::all()->dd();
```
---



### Filtrer une collection, garder les valeurs truthy (== true)

Sur les collections, on peut utiliser la méthode `filter` sans rien en paramètre pour ne garder que les valeurs implicitement égales à `true`. Elle élimine par exemple `false`, `null`, `0`, `''`, `[]`, …

```php
// On filtre la collection pour ne garder que les valeurs 'truthy' (= true)

// Méthode 🫠🫠🫠
collect(['', null, false, 1, 2, 3, 4])
    ->filter(fn ($value) => Svalue)
    ->values(); // [1, 2, 3, 4]

// Méthode 2, Utiliser seulement `filter()` 🚀🚀🚀
collect(['', null, false, 1, 2, 3, 4])
    ->filter()
    ->values(); //[1, 2, 3, 4]
```
___



### Générer plusieurs éléments dans une collection.

La méthode `times()` te permet de répéter une action N fois et d’obtenir directement une collection en résultat.

```php
use Illuminate\Support\Collection;

// Exemple basique 🙂🙂🙂
$numbers = Collection::times(5);
// [1, 2, 3, 4, 5]

// On peut aller plus loin ✨✨✨
$users = Collection::times(5, function (int $number): array {
    return [
        'id' => $number,
        'name' => "User {$number}"
    ],
});
// [
//     ["id" => 1, "name" => "User 1"],
//     ["id" => 2, "name" => "User 2"],
//     ["id" => 3, "name" => "User 3"],
//     ["id" => 4, "name" => "User 4"],
//     ["id" => 5, "name" => "User 5"],
// ]

// Et encore plus loin 🔥🔥🔥
$dates = Collection::times(7, fn (int $i) => today()->addDays($i)->toDateString());
// [
//     0 => "2025-11-19"
//     1 => "2025-11-20"
//     2 => "2025-11-21"
//     3 => "2025-11-22"
//     4 => "2025-11-23"
//     5 => "2025-11-24"
//     6 => "2025-11-25"
// ]
```
___



