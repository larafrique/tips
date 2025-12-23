## Tutoriels
___

### Automatiser la gestion du slug

Avec Laravel, on peut écouter les évènements émis  par un model au cours de son cycle de vie pour automatiser certaines opérations.
Dans cet exemple, on vous montre comment automatiser la gestion du `slug` sur un model et réutiliser la logique 🚀🚀🚀

```php
use Illuminate\Support\str;

trait HasSlug
{
    protected static function bootHasslug()
    {
        $separator = $this->slugSeparator();
        $source = $this->{$this->slugSource()};

        static::creating(function (self $model) {
            $model->slug = self::getSlug($source, $separator);
        });

        static::updating(function (self $model) {
            $model->slug = self::getSlug($source, $separator);
        });
    }

    protected static function getSlug(string $slug, string $separator): string

    {
        return Str::slug($slug, $separator);
    }

    public function slugSeparator (): string
    {
        return '-';
    }

    public function slugSource(): string
    {
        return 'title';
    }

}
//_______________________________________________________________________
use App\Models\Traits\HasSlug;

// I A chaque création d'un article et mise a jour du titre,
// le champ `slug` va automatiquement s'adapter en se basant sur le titre
// juste avec le trait `HasSlug`*

class Article extends Model
{
    use HasSlug;
}

$post = Post::create(['title' => 'Mon titre super cool', 'content' => 'Mon contenu']);
$pots->slug // mon-titre-super-cool
```
___



### Ajouter un attribut calculé à un model



```php
class User extends Model {

    // ajouter l'attribut aux méthodes `toArray()` et `toJson()`
    protected $appends = ["full_name"];

    // Ajouter l'attribut caclculé `full_name`
    public function getFullNameAttribute() {
        return "{$this->first_name} {$this->last_name}";
    }
}

$user = User::first();
// ["id" => 1, "first_name" => "John", "last_name" => "Doe"]

$user->full_name; // "John Doe".

$user->toArray();
// [
//     "id" => 1,
//     "first_name" => "John",
//     "last_name" => "Doe",
//     "full_name" => "John Doe" // # ajouté grace à $appends
// ]
```
___




