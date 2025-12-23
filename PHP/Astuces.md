### Déstructuration d'un tableau dans une boucle

Il est possible de déstructurer directement un tableau dans une boucle en PHP.

```php
$utilisateurs = [
    [
        'id' => 1,
        'nom' => 'Alice'
    ]
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

echo "Nom : $name, Email : Semail";
// Nom : larafrique, Email : contact@larafrique.com

// Utiliser la déstructuration 🚀🚀🚀
['name' => $name, 'email' => $email] = $user;

echo "Nom : $name, Email : Semail";
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