# Naming Conventions & Methodologies

## OOCSS (Object Oriented CSS) [[doc]](https://github.com/stubbornella/oocss/wiki)

Make class reusable, indivisual and easy to maintain.

### Principle

- Separation of Structure from Skin
- Separation of Containers and Content

### Example: bootstrap

```html
    <form class="form-login">
        <button class="btn btn-small btn-primary"></button>
    <form>
```

## BEM (Block Element Modifier) [[doc]](http://getbem.com/naming/)

- **Block**: A standalone entity that is meaningful on its own.

    ```css
    .form
    ```

- **Element**: Parts of a block and have no standalone meaning.

    ```css
    .form__input
    .form__button
    ```

- **Modifier**: Flags on blocks or elements. Use them to change appearance, behavior or state.

    ```css
    .form--mobile-layout
    .form__input--disabled
    .form__button--primary
    ```

### Why BEM

- Avoid naming conflict and confusion.
- Providing a naming convention.
- Reduce the complexity of your selectors.
