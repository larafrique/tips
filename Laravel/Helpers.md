## Helpers
___


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
echo fake ()—>numerify ("###-###-###") ;
// 995-399-340

echo fake()—>numerify ("### ### ###");
// 918 201 142

echo fake()—numerify ("+243 ### ### ###");
// +243 489 525 424

echo fake()—numerify ("+243 9## ### ###");
// +243 917 704 960
```
___



### Formater un nombre ne version abrégée avec `Number::abbreviate()`

Découvrez la méthode `Number::abbreviate()` qui formate un nombre en version abrégée avec un suffixe (K, M, B, T, ...). Utile pour afficher des nombres longs de façon compacte et plus humaine.

```php
Number::abbreviate(950);
// "950" (pas assez grand pour étre abrésgé)

Number::abbreviate(1_200);
// "1.2K"

Number::abbreviate(15_000);
// "15K"

Number::abbreviate(2_530_000);
// "2.53M"

Number::abbreviate(7_800_000_000) ;
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

