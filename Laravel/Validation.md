# Validation


### Valider facilement des dates relatives

Saviez-vous que vous pouvez facilement valider des dates relatives avec les règles intégrées de Laravel ? 🚀

```php
// Vous pouvez utiliser n'importe quelle
// chaine supportée par la validation de date de `strtotime()`

$rules = [
    'start_date' => 'required|date|after:tomorrow',
    'end_date' => 'required|date|after_or_equal:start_date',
    'past_date' => 'required|date|before:yesterday',
    'deadline' => 'required|date|before_or_equal:today',
];

$validated = $request->validate($rules);
```
___



### Valider les dimensions d'une image

Tu veux t’assurer que les utilisateurs uploadent des images propres, avec la bonne taille et le bon format ? Laravel te simplifie la vie avec la règle dimensions. Elle te permet de contrôler la largeur, la hauteur et même le ratio d’une image 🚀

```php
use Illuminate\Validation\Rule;
use Illuminate\Http\Request;

public function store(Request $request): void
{
    $data = $request->validate([
        'avatar' => [
            'required',
            'image',
            Rule::dimensions()
                ->maxWidth(1000)
                ->maxHeight(500)
                ->ratio(3 / 2),
        ]
    ]);
    
    // ...traitement (ex: sauvegarder l'avatar)    
}
```
___



