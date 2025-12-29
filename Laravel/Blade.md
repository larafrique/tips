# Blade


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
___



### Utiliser `@forelse`

Utilisez `@forelse` au lieu de `@if` + `@foreach` . Moins de code, plus de clarté ✨

```blade
{{-- Au lieu de procéder ainsi 🫠🫠🫠 --}}
@if ($user->posts->count())
    @foreach ($user->posts as $post)
        <li>{{ $post->title }}</li>
    @endforeach
@else
    <p>Aucun article disponible</p>
@endif

{{-- Vous pouvez procéder de cette manière ✅🚀🚀 --}}
@forelse ($user->posts as $post)
    <li>{{ $post->title }}</li>
@empty
    <p>Aucun article disponible</p>
@endforelse
```
___




### Utilisation de la variable `$loop` dans une boucle `foreach`

Avec `$loop`, une boucle foreach ne se contente pas de parcourir les données : elle vous dit aussi si vous êtes au début, à la fin ou sur une itération paire/impaire et plus...🚀✨

```blade
@foreach ($users as $user)
    {{ $loop->index }} Renvoie l'index courant (commence à 0)
    {{ $loop->iteration }} Renvoie le numéro de l'itération (commence à 1)

    @if ($loop->first)
        C'est la première itération.
    @endif

    @if ($loop->last)
        C'est la dernière itération.
    @endif

    @if ($loop->even)
        C'est une itération paire.
    @endif

    @if ($loop->odd)
        C'est une itération impaire.
    @endif

    @if ($loop->remaining > 1)
        L'attribut "remaining" indique le nombre d'itérations restantes après celle-ci.
    @endif
@endforeach
```



### Importer une classe avec `@use`

Au lieu de multiplier les `use` en PHP dans vos fichiers Blade, Vous pouvez faire plus simple et lisible avec `@use`.

```blade
{{-- Au lieu de procéder ainsi 🫠🥱🫠 --}}
@php
    use \App\Enums\TaskResult;
    use \App\Enums\NotificationTypeEnum as NotificationType;
@endphp

{{-- Vous pouvez procéder de cette manière 🔥✨✅ --}}
@use('\App\Enums\TaskResult')
@use('\App\Enums\NotificationTypeEnum', 'NotificationType')
```
___



