## Bonnes pratiques
___

### Eager loading avec `with()`

Quand vous préchargez une relation, vous pouvez préciser les champs dont vous avez besoin pour un peu plus d'optimisation avec cette synthaxe :

```php
// Méthode plus complexe ❌❌❌

Comment::with([
	'user' => function (BelongsTo $query) {
		$query->select(['id', 'name', 'avatar']);
	}
])->get();

// Méthode simple ✅✅✅
// Il faut nécéssairement préciser l'ID pour que Eloquent fasse la liason avec `user_id` de la table `comment`

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



