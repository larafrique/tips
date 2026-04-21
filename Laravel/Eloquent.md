# Eloquent


### Economiser des lignes de code avec `updateOrCreate`

Si un enregistrement correspondant existe, il est mis à jour avec les nouvelles valeurs passées
Si aucun enregistrement ne correspond, un nouvel enregistrement est créé avec la combinaison des conditions et des valeurs données.

```php
// Méthode 1 👎👎👎

$user = User::where("email", "johndoe@example.com")->first();

if($user) {
    $user->update(["last_login_at" => now()]);
} else {
    User::create([
        "email" => "johndoe@example.com",
        "last_login_at" => now()
    ]);
}


// Méthode 2 🚀🚀🚀

$user = User::updateOrCreate(
    ['email' => 'johndoe@example.com'],
    ['last_login_at' => now()]
);
```
___




### Utilisation de la méthode `firstWhere`

Raccourci pour User::where(...)->first(). Il renvoie le premier modèle correspondant ou null si aucun enregistrement n'est trouvé. À privilégier quand on a simplement besoin du premier résultat sans construire manuellement la requête.

```php
// Méthode 1 🫠🫠🫠
$user = User::where('email', 'john@doe.test')->first();


// Méthode 2 🚀🚀🚀
$user = User::firstWhere('email', 'john@doe.test');
```
___



### Utilisation de `whereBetween`

Obtenez des résultats compris entre deux dates avec `whereBetween`

```php
use App\Models\Post;

Post::whereBetween('created_at', [
    $request->date('from') ?? '2024-01-01',
    $request->date('to') ?? now(),
])
->get();
```
La requête en sortie :

```sql
SELECT *
FROM `posts`
WHERE
    `created_at` BETWEEN '2024-01-01' AND '2025-09-10 18:03:12'
```
___


### Clauses Where dynamiques

Laravel transforme automatiquement tout ce qui suit "where" dans le nom de la fonction en nom de colonne.

```php
use App\Models\User;

User::where('nom_du_champ_en_pascal_case', 'valeur')->first();
User::whereNomDuChampEnPascalCase('valeur')->first(); // Raccourci 🚀
// SQL : SELECT * FROM `users` WHERE `nom_du_champ_en_pascal_case` = 'valeur' LIMIT 1;

User::where('full_name', 'larafrique cool')->first();
User::whereFullName('larafrique cool')->first(); // Raccourci 🚀
// SQL : SELECT * FROM `users` WHERE `full_name` = 'larafrique cool' LIMIT 1;

User::where('email', 'hello@larafrique.com')->first();
User::whereEmail('hello@larafrique.com')->first(); // Raccourci 🚀
// SQL : SELECT * FROM `users` WHERE `email` = 'hello@larafrique.com' LIMIT 1;
```
___



### Voir la requête SQL

Vous pouvez voir la vraie requête SQL générée par `Eloquent` qui sera exécutée en BDD

```php
// Méthode 1 👎👎👎

$sql = User::where('active', true)->toRawSql();
dd($sql);
// affiche : "SELECT * FROM `users` WHERE `active` = 1"


// Méthode 2 🚀🚀🚀

// utilise `dd()` et stoppe l'exécution du code
User::where('active', true)->ddRawSql();
// affiche : "SELECT * FROM `users` WHERE `active` = 1"

// utilise `dump()` et continue l'exécution du code
User::where('active', true)->dumpRawSql();
// affiche : "SELECT * FROM `users` WHERE `active` = 1"
```
___



### La méthode `WhereAny`

Utiliser la méthode `whereAny` au lieu d'utiliser des méthodes `orWhere` à la suite des autres.

```php
// Méthode 1 : Utiliser `orWhere()` 🫠🫠🫠
User::where('name', 'LIKE', "%$search%")
    ->orWhere('email', 'LIKE', "%$search%")
    ->orWhere('phonenumber', 'LIKE', "%$search%")
    ->get();


// Méthode 2 : Utiliser `whereAny()` 🚀🚀🚀
User::whereAny(['name', 'email', 'phonenumber'], 'LIKE', "%$search%")
    ->get();
```
___



### `where` dynamiques

Vous pouvez écrire des `where` dynamiques sur des noms des champs de la table 🚀🚀🚀

```php
User::whereNameAndEmail('laf', 'info@laf.com')->first();
// SELECT * FROM `users` WHERE `name` = 'laf' AND `email` = 'info@laf.com'

User::whereNameOrEmail('laf', 'info@laf.com')->first();
// SELECT * FROM `users` WHERE `name` = 'laf' OR `email` = 'info@laf.com'

User::whereIsAdmin(true)->first();
// SELECT * FROM `users` WHERE `is_admin` = 1

User::whereIsAdminAndEmail(true, 'info@laf.com')->first();
// SELECT * FROM `users` WHERE `is_admin` = 1 AND `email` = 'info@laf.com'
```
___



### Requêtes Eloquent plus élégantes

Saviez-vous que vous pouvez rendre vos requêtes Eloquent plus élégantes ?

```php
// Au lieu d’écrire 👇🏼
User::popular()->orWhere(function (Builder $query) {
    $query->active();
})->get();

// vous pouvez simplement faire 👇🏼💡✨✅ 
User ::popular()->orWhere->active()->get();
```
___



### Scopes modernes avec les attributs PHP.

Il est possible de déclarer un scope Eloquent sans respecter le préfixe `scope` grâce à l’attribut `#[Scope]`.
Le nom de la méthode devient directement le nom du scope, ce qui rend le code plus lisible. 🚀🚀

```diff
+ use Illuminate\Database\Eloquent\Attributes\Scope;

class Post extends Model
{
+    #[Scope]
-    public function scopeOnline(Builder $query) {
+    public function online(Builder $query) {
        $query->where('online', true);
    }
}
```
___


### Les global scopes directement via des attributs

Avec `#[ScopedBy]`, plus besoin de surcharger `booted()` ni d’appeler `addGlobalScope()` manuellement. Le scope est automatiquement appliqué au modèle, de façon plus lisible, déclarative et maintenable. 🚀

```diff
use App\Models\Scopes\ActiveScope;
+ use Illuminate\Database\Eloquent\Attributes\ScopedBy;

+ #[ScopedBy(ActiveScope::class)]
class User extends Authenticatable
{
-	protected static function booted()
-	{
-		static::addGlobalScope(new ActiveScope);
-	}
}
```
___



### Optimisation de la requête avec la méthode `pluck`

La méthode `pluck` est utilisée pour extraire une colonne spécifique d'une collection de modèles Eloquent.

```php
// Méthode 1 👎👎👎
$users_emails = User::all()->pluck('email');
// Récupère tous les utilisateurs en mémoire, puis extrait les emails ❌


// Méthode 2 ✅✅✅
$users_emails = User::pluck('email');
// Exécute une requête SQL optimisée qui récupère uniquement la colonne `email`
```