# Bonnes pratiques


### Eager loading avec `with()`

Quand vous préchargez une relation, vous pouvez préciser les champs dont vous avez besoin pour un peu plus d'optimisation avec cette syntaxe :

```php
// Méthode plus complexe ❌❌❌

Comment::with([
	'user' => function (BelongsTo $query) {
		$query->select(['id', 'name', 'avatar']);
	}
])->get();

// Méthode simple ✅✅✅
// Il faut nécessairement préciser l'ID pour que Eloquent fasse la liaison avec `user_id` de la table `comment`

Comment::with('user:id,name,avatar')->get();
```
---



### Attribut `#[Scope]`

Le saviez-vous ? Vous pouvez déclarer une scope plus simplement en utilisant un attribut PHP.

```php
// Méthode 1 ✅✅✅

public function scopeOnline(Builder $query) {
	$query->where('online', true);
}

// Méthode 2 🚀🚀🚀

#[Scope]
public function online (Builder $query) {
	$query->where('online', true);
}

// Dans le controller
public function index () {
	$posts = Post::online()->get();
}
```
___


### L'attribut `#[ScopedBy]`

Vous pouvez appliquer une scope globale sur un model Laravel en utilisant l' attribut `ScopedBy` plus simplement ou dans la méthode `booted` du model.

```php
// ActiveScope.php
class ActiveScope implements Scope
{
	public function apply(Builder $builder, Model $model): void
	{
		$builder->where('active', true);
	}
}

// Utilisation de la scope - Méthode 1 🫠🫠🫠
use App\Models\Scopes\ActiveScope;

class User extends Authenticatable
{
	protected static function booted()
	{
		static::addGlobalScope(new ActiveScope);
	}
}

// Utilisation de la scope - Méthode 2 🚀🚀🚀
use App\Models\Scopes\ActiveScope;

#[ScopedBy(ActiveScope::class)]
class User extends Authenticatable
{}
```
___




### Utilisez l’attribut `#[UseFactory()]` pour associer la factory au model

Laravel retrouve les 'factories' par défaut, mais si vous les placez dans un sous-dossier, par exemple pour un design pattern quelconque, utilisez l’attribut `#[UseFactory()]` plutôt que la méthode `newFactory()` pour les lier aux modèles✨🚀

```diff
namespace App\CustomDirectory\Models;

use Database\Factories\CustomDirectory\UserFactory;
use Illuminate\Database\Eloquent\Attributes\UseFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;

+ #[UseFactory(UserFactory::class)]
class User extends Authenticatable
{
	protected $fillable = [
		'name',
		'email',
		'password',
	];

-	protected static function newFactory()
-	{
-		return UserFactory::new();
-	}
}

```
___


### Simplifier les migrations avec `Schema::morphs`

Générez automatiquement les deux colonnes nécessaires pour relier un modèle à plusieurs autres modèles dans une relation polymorphique grâce à la méthode `morphs`

```diff
Schema::create('images', function (Blueprint $table) {
	$table->id();
	$table->string('path');
-	$table->string('imageable_type');
-	$table->unsignedBigInteger('imageable_id');
+	$table->morphs('imageable');
})
```
___



### Nettoyer les relations d'un model dans un job avec `#[WithoutRelations]`

Dans une job Laravel, pas besoin d’embarquer toutes les relations du modèle (parfois lourdes et inutiles).
`#[WithoutRelations]` nettoie tout avant la sérialisation. 
Résultat : Payload léger, queue plus rapide ⚡️

```diff
+ use Illuminate\Queue\Attributes\WithoutRelations;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Queue\Queueable;

class ProcessPodcast implements ShouldQueue
{
	use Queueable;

	public function __construct(
+		#[WithoutRelations]
		public Podcast $podcast
	) {
	}
}
```
___



### Simplifiez vos routes Laravel avec `Route::view()`

Au lieu de créer une closure juste pour retourner une vue, utilisez Route::view().
Plus propre, plus court, plus lisible. ✨

```php
use Illuminate\Support\Facades\Route;

// Au lieu de faire ça 🥱🥱🥱
Route::get('/welcome', function () {
	return view('welcome', ['foo' => 'bar']);
});

// Vous pouvez faire ça 😎😎😎
Route::view('/welcome', 'welcome', ['foo' => 'bar']);
```
___



### Injecter des valeurs direct dans ton constructeur avec l'attribut `#[Config]`

Tu as sûrement déjà dû remplir des propriétés avec des valeurs de config. Avec Laravel 11+, c’est beaucoup plus simple : les attributs injectent les valeurs direct dans ton constructeur, sans code inutile🚀

```diff
namespace App\Services\Github;

+ use Illuminate\Container\Attributes\Config;

class GitHubClient
{
-	private string $apiKey;

	public function __construct(
+		#[Config('services.github.api_key')]
+		private readonly string $apiKey,
	)
	{
-		$this->apiKey = config()->string('services.github.api_key');
	}

	// ...
}
```
___



