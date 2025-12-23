### Imbrication native du CSS

```css
.container {
    width: 95%;
    max-width: 900px;
    margin: auto;

    /* Utilisation de '&' pour cibler '.container.full' */
    &.full {
        max-width: 1200px;

        /* Styles appliqués aux enfants '.children' uniquement quand le parent est '.full' */
        .children {
            display: flex;
            flex-wrap: wrap;
            margin: -0.5rem;
        }
    }
}

input {
    border: 1px solid #ccc;
    padding: 8px;
    transition: border-color 0.3s ease;

    /* '&' fait référence à 'input', donc 'input:focus' */
    &:focus {
        border-color: #007bff;
        outline: none;
    }
}
```
___



