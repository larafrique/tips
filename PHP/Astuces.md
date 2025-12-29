### Déstructuration d'un tableau dans une boucle

Il est possible de déstructurer directement un tableau dans une boucle en PHP.

```php
$utilisateurs = [
    [
        'id' => 1,
        'nom' => 'Alice'
    ],
    [
        'id' => 2,
        'nom' => 'Bob'
    ],
    [
        'id' => 3,
        'nom' => 'Charlie'
    ],
];

// Boucle à travers les utilisateurs et déstructuration de l'id et du nom 🚀🚀🚀
foreach ($utilisateurs as ['id' => $id, 'nom' => $nom]) {
    echo "ID: $id, Nom: $nom\n";
}
```
___


### Déstructuration

```php
$user = [
    'name' => 'larafrique',
    'email' => 'contacta@larafrique.com',
    'verified' => true
];

// Méthode 1 🫠🫠🫠

$name = $user['name'];
$email = $user['email'];

echo "Nom : $name, Email : $email";
// Nom : larafrique, Email : contact@larafrique.com

// Utiliser la déstructuration 🚀🚀🚀
['name' => $name, 'email' => $email] = $user;

echo "Nom : $name, Email : $email";
// Nom : larafrique, Email : contacta@larafrique.com
```
___


### Astuce avec `isset()`

```php
// Vous pouvez simplifier ceci 👎👎👎
if(isset($var1) && isset($var2) && isset($var3)) {
    // votre code
}

// En ceci 🚀🚀🚀
if(isset($var1, $var2, $var3)) {
    // votre code
}
```
___



### Fusion de deux ou plusieurs tableaux

```php
$array1 = ['Laravel', 'Symfony'];
$array2 = ['Vue.js', 'React'];

// Avant 🫠🫠
$merged = array_merge($array1, $array2);
// ['Laravel', 'Symfony', 'Vue.js', 'React']


// Maintenant 🚀🚀🚀
$merged = [...$array1, ...$array2];
// ['Laravel', 'Symfony', 'Vue.js', 'React'] ✅✅✅
```
___



### Filtrer facilement et ne garder que les valeurs "Truthy"

On peut utiliser la fonction `array_filter` sans rien passer en deuxième paramètre pour ne garder que les valeurs implicitement égales à `true`

```php
$users = ['Larafrique', '', 'John Doe', null];

// Avant 🫠🫠
$clean = array_filter($users, fn ($user) => !!$user);

// Maintenant 🚀🚀🚀
$clean = array_filter($users);
// ["Larafrique", "John Doe"]
```



### Transformer facilement en passant le nom de la fonction avec `array_map`

```php
$users = ['Larafrique', 'John Doe', 'Jane Doe'];

// Avant 🫠🫠
$clean = array_map(fn (string $user) => strtoupper($user), $users);

// Maintenant 🚀🚀🚀
$clean = array_map('strtoupper', $users);
// ["LARAFRIQUE", "JOHN DOE", "JANE DOE"]
```