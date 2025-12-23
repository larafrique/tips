## Collections
___

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