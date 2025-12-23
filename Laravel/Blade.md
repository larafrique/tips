## Blade
___

### Passer facilement des props à un composant

Vous pouvez plus facilement  passer un `props` à un composant blade avec cette syntaxe `:$variable` si la variable et la `props` ont un même nom

```blade
@php
    $user = User::first();
    $message = "Astuce avec les props d'un composant blade";
@endphp

{{-- Méthode traditionnelle 1 🫠🫠🫠 --}}
<x-user-card :user="$user" />
<x-toast :message="$message" />


{{-- Méthode optimisée 2 => plus rapide et optimisée 🚀🚀🚀 --}}
<x-user-card :$user />
<x-toast :$message />
```