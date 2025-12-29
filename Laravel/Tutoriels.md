# Tutoriels


### Automatiser la gestion du slug

Avec Laravel, on peut écouter les événements émis par un modèle au cours de son cycle de vie pour automatiser certaines opérations.
Dans cet exemple, on vous montre comment automatiser la gestion du `slug` sur un modèle et réutiliser la logique 🚀🚀🚀

```php
use Illuminate\Support\Str;

trait HasSlug
{
    protected static function bootHasSlug()
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

    public function slugSeparator(): string
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
$post->slug; // mon-titre-super-cool
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



### Utiliser Carbon pour obtenir facilement l’âge d’un utilisateur.

```php
use Illuminate\Support\Carbon;

$date = Carbon::parse("1998-05-10");

// Obtenir facilement l'âge
echo $date->age; // 27

#_____________________________________________________________________

// Cas concret d'utilisation 🚀🚀
class User extends Model
{
    protected function casts(): array
    {
        return [
            // ⚠️ caster le champ `birthdate` en une instance de `Carbon`
            'birthdate' => 'date',
        ];
    }
}

$user = User::first(); // ['id' => 1, 'birthdate' => '2000-05-10', ...]

echo $user->birthdate->age; // 25
```
___



### Caster un attribut en `Enum`

Grâce aux backed enums de PHP et au casting de Laravel, tu peux transformer une valeur d’attribut en instance d’enum. Cela te permet d’ajouter des méthodes comme `label()` ou `description()` accessibles depuis ton modèle.✨🚀

```php
enum ReportReason: string
{
    case InappropriateContent = 'inappropriate content';
    case Spam = 'spam';
    case Harassment = 'harassment';
    case Misinformation = 'misinformation';

    public function label(): string
    {
        return match ($this) {
            self::InappropriateContent => 'Contenu inapproprié',
            self::Spam => 'Spam',
            self::Harassment => 'Harcèlement',
            self::Misinformation => 'Désinformation',
        };
    }

    public function description(): string
    {
        return match ($this) {
            self::InappropriateContent => 'Ce commentaire contient du contenu inapproprié.',
            self::Spam => 'Ce commentaire est considéré comme du spam.',
            self::Harassment => 'Ce commentaire constitue du harcèlement.',
            self::Misinformation => 'Ce commentaire contient de la désinformation.',
        };
    }
}

#___________________________________________________________________________________

class Report extends Model
{
    protected function casts()
    {
        return [
            'reason' => ReportReason::class, // Caster l'attribut ici
        ];
    }
}

$report = Report::first(); // ['id' => 1, 'reason’ => 'misinformation']

$report->reason->label(); // Désinformation
$report->reason->description(); // Ce commentaire contient de la désinformation.
```


### Déclarer plusieurs routes ressources avec `Route::resource()`

Saviez-vous qu’il est possible de déclarer plusieurs Resource Controllers en une seule ligne ?
Au lieu d’écrire plusieurs fois `Route::resource()`, vous pouvez utiliser `Route::resources([])` pour plus de clarté et moins de répétition 👌

```php
use App\Http\Controllers\PostsController;
use App\Http\Controllers\UserController;

// 🤗🤗🤗
Route::resource('posts', PostsController::class);
Route::resource('users', UserController::class);

// ✅✅✅
Route::resources([
    'posts' => PostsController::class,
    'users' => UserController::class,
]);

```
___




### Personnalisation du mail de vérification d’adresse e-mail

Personnalisez le mail de vérification d’adresse e-mail dans Laravel.
Implémentez `MustVerifyEmail`, créez une notification custom avec Markdown ✨.

```php
// On implémente l'interface `MustVerifyEmail`
class User extends Authenticatable implements MustVerifyEmail
{
    // Lorsque Laravel enverra la notification, il utilisera notre classe `CustomVerifyEmail`
    public function sendEmailVerificationNotification()
    {
        $this->notify(new CustomVerifyEmail);
    }
}
```
```php
// Notre classe étend `VerifyEmail`.
// Pour mettre la notification en file d'attente (recommandé ✨),
// on implémente l'interface `ShouldQueue` et on utilise le trait `Queueable`
class CustomVerifyEmail extends VerifyEmail implements ShouldQueue
{
    use Queueable; // Optionnel, mais requis si on implémente `ShouldQueue`

    // On redéfinit la méthode `toMail` pour personnaliser le mail
    // et envoyer par exemple un template Markdown (recommandé ✨)
    public function toMail($notifiable)
    {
        return (new MailMessage)
            ->subject('Vérification de votre adresse e-mail')
            ->markdown('mail.verify-email', [
                'user' => $notifiable,
                'verificationUrl' => $this->verificationUrl($notifiable),
            ]);
    }
}
```
```blade
{{-- fichier mail.verify-email (template Markdown avec un layout personnalisé) --}}

<x-layouts.mail-layout
    :buttons="[
        [
            'url' => $verificationUrl,
            'label' => 'Vérifier mon adresse e-mail',
            'icon' => 'heroicon-s-check-circle',
        ]
    ]"
>
    <x-slot:title>
        Vérifier l'adresse e-mail
    </x-slot>
    <x-slot:content>
        Bonjour **{{ $user->name }}**,
        Veuillez cliquer sur le bouton ci-dessous pour vérifier votre adresse e-mail.
    </x-slot>
    <x-slot:content_bottom>
        Si vous n'avez pas créé de compte, aucune action supplémentaire n'est requise.
    </x-slot>
</x-layouts.mail-layout>
```
![mail de vérification d’adresse e-mail](../images/laravel/verification-mail-customization.png)
___



