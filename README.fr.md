# Alpine.js

![npm bundle size](https://img.shields.io/bundlephobia/minzip/alpinejs)
![npm version](https://img.shields.io/npm/v/alpinejs)
[![Chat](https://img.shields.io/badge/chat-on%20discord-7289da.svg?sanitize=true)](https://alpinejs.codewithhugo.com/chat/)

Alpine.js offre les propriétés déclaratives et réactives des grands frameworks tels que Vue ou React à un coût bien moindre.

Le DOM est préservé, et vous pouvez lui attribuer les comportements comme bon vous semble.

C'est un peu le [Tailwind](https://tailwindcss.com/) du JavaScript.

> Note: La syntaxe de cet outil est presque entièrement emprunté de [Vue](https://vuejs.org/) (et par extension [Angular](https://angularjs.org/)). Je suis à jamais reconnaissant pour ce qu'ils ont apporté au web.

## Documentations traduites

| Langue | Lien vers la documentation |
| --- | --- |
| Chinois Traditionel | [**繁體中文說明文件**](./README.zh-TW.md) |
| Allemand | [**Dokumentation in Deutsch**](./README.de.md) |
| Indonesien | [**Dokumentasi Bahasa Indonesia**](./README.id.md) |
| Japonais | [**日本語ドキュメント**](./README.ja.md) |
| Portuguais | [**Documentação em Português**](./README.pt.md) |
| Russe | [**Документация на русском**](./README.ru.md) |
| Espagnol | [**Documentación en Español**](./README.es.md) |
| français | [**Documentation en Français**](./README.fr.md) |

## Installation

**Avec CDN:** Ajoutez le script suivant à la fin de votre section `<head>`.
```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js" defer></script>
```

C'est tout. L'initialisation est automatique.

Dans les environnements de production, il est recommandé d'inscrire un numéro de version spécifique dans le lien, afin d'éviter qu'une nouvelle version ne provoque un comportement inattendu.
Par exemple, pour utiliser la version `2.7.3` (la dernière) :
```html
<script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.7.3/dist/alpine.min.js" defer></script>
```

**Avec npm:** Installer le paquet avec npm.
```js
npm i alpinejs
```

Importez-le dans votre script.
```js
import 'alpinejs'
```

**Pour la compatibilité IE11** utilisez plutôt les scripts suivants.
```html
<script type="module" src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine.min.js"></script>
<script nomodule src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.x.x/dist/alpine-ie11.min.js" defer></script>
```

Le schéma ci-dessus représente le [schéma module/nomodule](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/). Il permet  de faire en sorte que le pack soit automatiquement chargé dans les navigateurs modernes, mais aussi pour IE11 et les autres navigateurs anciens.

## Utilisation

*Menu déroulant/Modal*
```html
<div x-data="{ open: false }">
    <button @click="open = true">Ouvrir le menu</button>

    <ul
        x-show="open"
        @click.away="open = false"
    >
        Elément de menu
    </ul>
</div>
```

*Onglets*
```html
<div x-data="{ tab: 'foo' }">
    <button :class="{ 'active': tab === 'foo' }" @click="tab = 'foo'">Foo</button>
    <button :class="{ 'active': tab === 'bar' }" @click="tab = 'bar'">Bar</button>

    <div x-show="tab === 'foo'">Onglet Foo</div>
    <div x-show="tab === 'bar'">Onglet Bar</div>
</div>
```

Vous pouvez même l'utiliser pour faire des choses moins courantes:
*Précharger le HTML d'un élément de menu lors du survol.*
```html
<div x-data="{ open: false }">
    <button
        @mouseenter.once="
            fetch('/dropdown-partial.html')
                .then(response => response.text())
                .then(html => { $refs.dropdown.innerHTML = html })
        "
        @click="open = true"
    >Afficher Menu</button>

    <div x-ref="dropdown" x-show="open" @click.away="open = false">
        Chargement Spinner...
    </div>
</div>
```

## Apprentissage

Il existe 14 directives disponibles:

| Directive | Description |
| --- | --- |
| [`x-data`](#x-data) | Déclare la portée d'un nouveau composant. |
| [`x-init`](#x-init) | Exécute une expression lors de l'initialisation d'un composant. |
| [`x-show`](#x-show) | Alterne `display: none;` sur l'élément selon l'expression (true ou false). |
| [`x-bind`](#x-bind) | Fixe la valeur d'un attribut au résultat d'une expression JS. |
| [`x-on`](#x-on) | Attache un écouteur d'évènement à l'élément. Exécute une expression JS lorsque l'évènement est déclenché. |
| [`x-model`](#x-model) | Ajoute une liaison de données bidirectionnelle (two-way data binding) à un élément. Conserve la synchronisation de l'élément d'entrée avec les données du composant. |
| [`x-text`](#x-text) | Fonctionne de manière similaire à `x-bind`, mais avec mise à jour du `innerText` d'un élément. |
| [`x-html`](#x-html) | Fonctionne de manière similaire à `x-bind`, mais avec mise à jour du `innerHTML` d'un élément. |
| [`x-ref`](#x-ref) | Un moyen pratique de récupérer des éléments bruts du DOM de votre composant. |
| [`x-if`](#x-if) | Supprime totalement un élément du DOM. Doit s'utiliser avec le tag `<template>`. |
| [`x-for`](#x-for) | Crée de nouveaux noeuds DOM pour chaque élément d'un tableau. Doit s'utiliser avec le tag `<template>`. |
| [`x-transition`](#x-transition) | Directives pour renseigner les classes aux différentes étapes de transition d'un élément. |
| [`x-spread`](#x-spread) | Permet de lier l'objet de directives d'Alpine à un élément pour une meilleure réutilisation. |
| [`x-cloak`](#x-cloak) | Cet attribut est supprimé lorsque Alpine s'initialise. Utile pour masquer les DOM pré-initialisés. |

Et 6 propriétés magiques:

| Propriétés magiques | Description |
| --- | --- |
| [`$el`](#el) |  Récupère le nœud DOM du composant racine. |
| [`$refs`](#refs) | Récupère les éléments du DOM marqués par `x-ref` à l'intérieur du composant. |
| [`$event`](#event) | Récupère l'objet natif "Event" du navigateur dans un écouteur d'évènements.  |
| [`$dispatch`](#dispatch) | Crée un `CustomEvent` et le distribue à l'aide de `.dispatchEvent()` en interne. |
| [`$nextTick`](#nexttick) | Exécute une expression donnée APRES qu'Alpine ait fait ses mises à jour réactives du DOM. |
| [`$watch`](#watch) | Effectue un rappel (callback) préalablement défini lorsque la propriété d'un composant "surveillé" (watch) est modifiée. |


## Sponsors

<img width="33%" src="https://refactoringui.nyc3.cdn.digitaloceanspaces.com/tailwind-logo.svg" alt="Tailwind CSS">

**Votre logo ici ? [DM sur Twitter](https://twitter.com/calebporzio)**

## Projets Communautaires

* [AlpineJS Weekly Newsletter](https://alpinejs.codewithhugo.com/newsletter/)
* [Spruce (State Management)](https://github.com/ryangjchandler/spruce)
* [Turbolinks Adapter](https://github.com/SimoTod/alpine-turbolinks-adapter)
* [Alpine Magic Helpers](https://github.com/KevinBatdorf/alpine-magic-helpers)
* [Awesome Alpine](https://github.com/ryangjchandler/awesome-alpine)

### Directives

---

### `x-data`

**Exemple:** `<div x-data="{ foo: 'bar' }">...</div>`

**Structure:** `<div x-data="[object literal]">...</div>`

`x-data` déclare la portée d'un nouveau composant. Indique au framework d'initialiser un nouveau composant avec le prochain objet de données.

Il faut voir cela comme la propriété de `données` d'un composant Vue.

**Extraction de la Logique des Composants**

Vous pouvez extraire les données (et le comportement) en fonctions réutilisables :

```html
<div x-data="dropdown()">
    <button x-on:click="open">Ouvrir</button>

    <div x-show="isOpen()" x-on:click.away="close">
        // Menu déroulant
    </div>
</div>

<script>
    function dropdown() {
        return {
            show: false,
            open() { this.show = true },
            close() { this.show = false },
            isOpen() { return this.show === true },
        }
    }
</script>
```

> **Pour les utilisateurs de modules bundler**, notez que Alpine.js accède à des fonctions qui sont dans la portée globale (`window`). Vous devrez explicitement assigner vos fonctions à `window` pour les utiliser avec `x-data`. Par exemple `window.dropdown = function () {}` ( c'est parce qu'avec Webpack, Rollup, Parcel etc. les fonctions que vous écrivez sont par défaut dans la portée du module et non dans celle de la page - `window`).


Vous pouvez également mélanger plusieurs objets de données en utilisant la décomposition d'objet :

```html
<div x-data="{...dropdown(), ...tabs()}">
```

---

### `x-init`
**Exemple:** `<div x-data="{ foo: 'bar' }" x-init="foo = 'baz'"></div>`

**Structure:** `<div x-data="..." x-init="[expression]"></div>`

`x-init` exécute une expression lorsqu'un composant est initialisé.

Si vous souhaitez exécuter du code APRES qu'Alpine ait effectué ses mises à jour initiales dans le DOM (un peu comme le hook `mounted()` de VueJS), vous pouvez retourner un callback depuis `x-init`, et il sera ensuite exécuté :

`x-init="() => { // on a ici accès à l'état du DOM post-initialisation // }"`

---

### `x-show`
**Exemple:** `<div x-show="open"></div>`

**Structure:** `<div x-show="[expression]"></div>`

`x-show` alterne le style `display: none;` sur l'élément selon que l'expression retourne `true` ou `false`.

**x-show.transition**

`x-show.transition` est une API de commodité pour rendre vos `x-show` plus agréables en utilisant des transitions CSS.

```html
<div x-show.transition="open">
    Le contenu ici fera l'objet de transitions "in" et "out".
</div>
```

| Directive | Description |
| --- | --- |
| `x-show.transition` | Fondu et échelle simultanés. (opacity, scale: 0.95, timing-function: cubic-bezier(0.4, 0.0, 0.2, 1), duration-in: 150ms, duration-out: 75ms)
| `x-show.transition.in` | Transition `in` seule. |
| `x-show.transition.out` | Transition `out` seule. |
| `x-show.transition.opacity` |Fondu seul. |
| `x-show.transition.scale` | Echelle seule. |
| `x-show.transition.scale.75` | Personnalise la modification CSS de l'échelle `transform: scale(.75)`. |
| `x-show.transition.duration.200ms` | Fixe la transition "in" à 200 ms. La transition "out" sera fixée à la moitié de cette valeur (100 ms). |
| `x-show.transition.origin.top.right` | Personnalise l'origine de la transformation CSS `transform-origin: top right`. |
| `x-show.transition.in.duration.200ms.out.duration.50ms` | Durées différentes pour "in" et "out". |

> Note: Tous ces modificateurs de transition peuvent être utilisés conjointement les uns avec les autres. Il est même possible de faire ceci (bien que ridicule lol): `x-show.transition.in.duration.100ms.origin.top.right.opacity.scale.85.out.duration.200ms.origin.bottom.left.opacity.scale.95`

> Note: `x-show` attendra que les objets enfants aient terminé leur transition. Si vous voulez contourner ce comportement, ajoutez le modificateur `.immediate` :
```html
<div x-show.immediate="open">
    <div x-show.transition="open">
</div>
```
---

### `x-bind`

> Note: vous êtes libre d'utiliser la syntaxe ":" plus courte: `:type="..."`.

**Exemple:** `<input x-bind:type="inputType">`

**Structure:** `<input x-bind:[attribute]="[expression]">`

`x-bind` fixe la valeur d'un attribut au résultat d'une expression JavaScript. Cette expression a accès à toutes les clés de l'objet de données du composant, et se met à jour à chaque fois que ses données changent.

> Note: les liaisons d'attributs (attribute bindings) ne se mettent à jour QUE lorsque leurs dépendances changent. Le framework est suffisamment intelligent pour observer les changements de données et détecter les liens qui les concernent.

**`x-bind` pour les attributs de classe**

`x-bind` se comporte un peu différemment lorsqu'il est lié à l'attribut `class`.

En ce qui concerne les classes, vous passez un objet dont les clés sont des noms de classe, et les valeurs sont des expressions booléennes pour déterminer si ces noms de classe sont appliqués ou non.

Par exemple:
`<div x-bind:class="{ 'hidden': foo }"></div>`

Dans cet exemple, la classe "hidden" ne sera appliquée que si la valeur de l'attribut de données `foo` est `true`.

**`x-bind` pour les attributs booléens**

`x-bind` supporte les attributs booléens de la même manière que les attributs de valeur, en utilisant une variable comme condition ou toute expression JavaScript qui se résout en `true` ou `false`.

Par exemple:
```html
<!-- Soit: -->
<button x-bind:disabled="myVar">Cliquez moi</button>

<!-- Lorsque myVar == true: -->
<button disabled="disabled">Cliquez moi</button>

<!-- Lorsque myVar == false: -->
<button>Cliquez moi</button>
```

Cela ajoute ou supprime l'attribut `disabled` lorsque `myVar` est respectivement vrai ou faux.

Les attributs booléens sont pris en charge conformément à la [spécification HTML](https://html.spec.whatwg.org/multipage/indices.html#attributes-3:boolean-attribute), par exemple `disabled`, `readonly`, `required`, `checked`, `hidden`, `selected`, `open`, etc.

> Note: Si vous avez besoin d'un état `false` pour afficher un attribut, comme par exemple `aria-*`, chainez `.toString()` à la valeur tout en liant (bind) l'attribut. Par exemple: `:aria-expanded="isOpen.toString()"` va persister, que `isOpen` soit `true` ou `false`.

**Modificateur `.camel`**
**Exemple:** `<svg x-bind:view-box.camel="viewBox">`

The `camel` modifier will bind to the camel case equivalent of the attribute name. In the example above, the value of `viewBox` will be bound the `viewBox` attribute as opposed to the `view-box` attribute.

---

### `x-on`

> Note: You are free to use the shorter "@" syntax: `@click="..."`.

**Example:** `<button x-on:click="foo = 'bar'"></button>`

**Structure:** `<button x-on:[event]="[expression]"></button>`

`x-on` attaches an event listener to the element it's declared on. When that event is emitted, the JavaScript expression set as its value is executed. You can use `x-on` with any event available for the element you're adding the directive on, for a full list of events, see [the Event reference on MDN](https://developer.mozilla.org/en-US/docs/Web/Events) for a list of possible values.

If any data is modified in the expression, other element attributes "bound" to this data, will be updated.

> Note: You can also specify a JavaScript function name.

**Example:** `<button x-on:click="myFunction"></button>`

This is equivalent to: `<button x-on:click="myFunction($event)"></button>`

**`keydown` modifiers**

**Example:** `<input type="text" x-on:keydown.escape="open = false">`

You can specify specific keys to listen for using keydown modifiers appended to the `x-on:keydown` directive. Note that the modifiers are kebab-cased versions of `Event.key` values.

Examples: `enter`, `escape`, `arrow-up`, `arrow-down`

> Note: You can also listen for system-modifier key combinations like: `x-on:keydown.cmd.enter="foo"`

**`.away` modifier**

**Example:** `<div x-on:click.away="showModal = false"></div>`

When the `.away` modifier is present, the event handler will only be executed when the event originates from a source other than itself, or its children.

This is useful for hiding dropdowns and modals when a user clicks away from them.

**`.prevent` modifier**
**Example:** `<input type="checkbox" x-on:click.prevent>`

Adding `.prevent` to an event listener will call `preventDefault` on the triggered event. In the above example, this means the checkbox wouldn't actually get checked when a user clicks on it.

**`.stop` modifier**
**Example:** `<div x-on:click="foo = 'bar'"><button x-on:click.stop></button></div>`

Adding `.stop` to an event listener will call `stopPropagation` on the triggered event. In the above example, this means the "click" event won't bubble from the button to the outer `<div>`. Or in other words, when a user clicks the button, `foo` won't be set to `'bar'`.

**`.self` modifier**
**Example:** `<div x-on:click.self="foo = 'bar'"><button></button></div>`

Adding `.self` to an event listener will only trigger the handler if the `$event.target` is the element itself. In the above example, this means the "click" event that bubbles from the button to the outer `<div>` will **not** run the handler.

**`.window` modifier**
**Example:** `<div x-on:resize.window="isOpen = window.outerWidth > 768 ? false : open"></div>`

Adding `.window` to an event listener will install the listener on the global window object instead of the DOM node on which it is declared. This is useful for when you want to modify component state when something changes with the window, like the resize event. In this example, when the window grows larger than 768 pixels wide, we will close the modal/dropdown, otherwise maintain the same state.

>Note: You can also use the `.document` modifier to attach listeners to `document` instead of `window`

**`.once` modifier**
**Example:** `<button x-on:mouseenter.once="fetchSomething()"></button>`

Adding the `.once` modifier to an event listener will ensure that the listener will only be handled once. This is useful for things you only want to do once, like fetching HTML partials and such.

**`.passive` modifier**
**Example:** `<button x-on:mousedown.passive="interactive = true"></button>`

Adding the `.passive` modifier to an event listener will make the listener a passive one, which means `preventDefault()` will not work on any events being processed, this can help, for example with scroll performance on touch devices.

**`.debounce` modifier**
**Example:** `<input x-on:input.debounce="fetchSomething()">`

The `debounce` modifier allows you to "debounce" an event handler. In other words, the event handler will NOT run until a certain amount of time has elapsed since the last event that fired. When the handler is ready to be called, the last handler call will execute.

The default debounce "wait" time is 250 milliseconds.

If you wish to customize this, you can specify a custom wait time like so:

```
<input x-on:input.debounce.750="fetchSomething()">
<input x-on:input.debounce.750ms="fetchSomething()">
```

**`.camel` modifier**
**Example:** `<input x-on:event-name.camel="doSomething()">`

The `camel` modifier will attach an event listener for the camel case equivalent event name. In the example above, the expression will be evaluated when the `eventName` event is fired on the element.

---

### `x-model`
**Example:** `<input type="text" x-model="foo">`

**Structure:** `<input type="text" x-model="[data item]">`

`x-model` adds "two-way data binding" to an element. In other words, the value of the input element will be kept in sync with the value of the data item of the component.

> Note: `x-model` is smart enough to detect changes on text inputs, checkboxes, radio buttons, textareas, selects, and multiple selects. It should behave [how Vue would](https://vuejs.org/v2/guide/forms.html) in those scenarios.

**`.number` modifier**
**Example:** `<input x-model.number="age">`

The `number` modifier will convert the input's value to a number. If the value cannot be parsed as a valid number, the original value is returned.

**`.debounce` modifier**
**Example:** `<input x-model.debounce="search">`

The `debounce` modifier allows you to add a "debounce" to a value update. In other words, the event handler will NOT run until a certain amount of time has elapsed since the last event that fired. When the handler is ready to be called, the last handler call will execute.

The default debounce "wait" time is 250 milliseconds.

If you wish to customize this, you can specifiy a custom wait time like so:

```
<input x-model.debounce.750="search">
<input x-model.debounce.750ms="search">
```

---

### `x-text`
**Example:** `<span x-text="foo"></span>`

**Structure:** `<span x-text="[expression]"`

`x-text` works similarly to `x-bind`, except instead of updating the value of an attribute, it will update the `innerText` of an element.

---

### `x-html`
**Example:** `<span x-html="foo"></span>`

**Structure:** `<span x-html="[expression]"`

`x-html` works similarly to `x-bind`, except instead of updating the value of an attribute, it will update the `innerHTML` of an element.

> :warning: **Only use on trusted content and never on user-provided content.** :warning:
>
> Dynamically rendering HTML from third parties can easily lead to [XSS](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) vulnerabilities.

---

### `x-ref`
**Example:** `<div x-ref="foo"></div><button x-on:click="$refs.foo.innerText = 'bar'"></button>`

**Structure:** `<div x-ref="[ref name]"></div><button x-on:click="$refs.[ref name].innerText = 'bar'"></button>`

`x-ref` provides a convenient way to retrieve raw DOM elements out of your component. By setting an `x-ref` attribute on an element, you are making it available to all event handlers inside an object called `$refs`.

This is a helpful alternative to setting ids and using `document.querySelector` all over the place.

> Note: you can also bind dynamic values for x-ref: `<span :x-ref="item.id"></span>` if you need to.

---

### `x-if`
**Example:** `<template x-if="true"><div>Some Element</div></template>`

**Structure:** `<template x-if="[expression]"><div>Some Element</div></template>`

For cases where `x-show` isn't sufficient (`x-show` sets an element to `display: none` if it's false), `x-if` can be used to  actually remove an element completely from the DOM.

It's important that `x-if` is used on a `<template></template>` tag because Alpine doesn't use a virtual DOM. This implementation allows Alpine to stay rugged and use the real DOM to work its magic.

> Note: `x-if` must have a single element root inside the `<template></template>` tag.

> Note: When using `template` in a `svg` tag, you need to add a [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) that should be run before Alpine.js is initialized.

---

### `x-for`
**Example:**
```html
<template x-for="item in items" :key="item">
    <div x-text="item"></div>
</template>
```

> Note: the `:key` binding is optional, but HIGHLY recommended.

`x-for` is available for cases when you want to create new DOM nodes for each item in an array. This should appear similar to `v-for` in Vue, with one exception of needing to exist on a `template` tag, and not a regular DOM element.

If you want to access the current index of the iteration, use the following syntax:

```html
<template x-for="(item, index) in items" :key="index">
    <!-- You can also reference "index" inside the iteration if you need. -->
    <div x-text="index"></div>
</template>
```

If you want to access the array object (collection) of the iteration, use the following syntax:

```html
<template x-for="(item, index, collection) in items" :key="index">
    <!-- You can also reference "collection" inside the iteration if you need. -->
    <!-- Current item. -->
    <div x-text="item"></div>
    <!-- Same as above. -->
    <div x-text="collection[index]"></div>
    <!-- Previous item. -->
    <div x-text="collection[index - 1]"></div>
</template>
```

> Note: `x-for` must have a single element root inside of the `<template></template>` tag.

> Note: When using `template` in a `svg` tag, you need to add a [polyfill](https://github.com/alpinejs/alpine/issues/637#issuecomment-654856538) that should be run before Alpine.js is initialized.

#### Nesting `x-for`s
You can nest `x-for` loops, but you MUST wrap each loop in an element. For example:

```html
<template x-for="item in items">
    <div>
        <template x-for="subItem in item.subItems">
            <div x-text="subItem"></div>
        </template>
    </div>
</template>
```

#### Iterating over a range

Alpine supports the `i in n` syntax, where `n` is an integer, allowing you to iterate over a fixed range of elements.

```html
<template x-for="i in 10">
    <span x-text="i"></span>
</template>
```

---

### `x-transition`
**Example:**
```html
<div
    x-show="open"
    x-transition:enter="transition ease-out duration-300"
    x-transition:enter-start="opacity-0 transform scale-90"
    x-transition:enter-end="opacity-100 transform scale-100"
    x-transition:leave="transition ease-in duration-300"
    x-transition:leave-start="opacity-100 transform scale-100"
    x-transition:leave-end="opacity-0 transform scale-90"
>...</div>
```

```html
<template x-if="open">
    <div
        x-transition:enter="transition ease-out duration-300"
        x-transition:enter-start="opacity-0 transform scale-90"
        x-transition:enter-end="opacity-100 transform scale-100"
        x-transition:leave="transition ease-in duration-300"
        x-transition:leave-start="opacity-100 transform scale-100"
        x-transition:leave-end="opacity-0 transform scale-90"
    >...</div>
</template>
```

> The example above uses classes from [Tailwind CSS](https://tailwindcss.com).

Alpine offers 6 different transition directives for applying classes to various stages of an element's transition between "hidden" and "shown" states. These directives work both with `x-show` AND `x-if`.

These behave exactly like VueJS's transition directives, except they have different, more sensible names:

| Directive | Description |
| --- | --- |
| `:enter` | Applied during the entire entering phase. |
| `:enter-start` | Added before element is inserted, removed one frame after element is inserted. |
| `:enter-end` | Added one frame after element is inserted (at the same time `enter-start` is removed), removed when transition/animation finishes.
| `:leave` | Applied during the entire leaving phase. |
| `:leave-start` | Added immediately when a leaving transition is triggered, removed after one frame. |
| `:leave-end` | Added one frame after a leaving transition is triggered (at the same time `leave-start` is removed), removed when the transition/animation finishes.

---

### `x-spread`
**Example:**
```html
<div x-data="dropdown()">
    <button x-spread="trigger">Open Dropdown</button>

    <span x-spread="dialogue">Dropdown Contents</span>
</div>

<script>
    function dropdown() {
        return {
            open: false,
            trigger: {
                ['@click']() {
                    this.open = true
                },
            },
            dialogue: {
                ['x-show']() {
                    return this.open
                },
                ['@click.away']() {
                    this.open = false
                },
            }
        }
    }
</script>
```

`x-spread` allows you to extract an element's Alpine bindings into a reusable object.

The object keys are the directives (Can be any directive including modifiers), and the values are callbacks to be evaluated by Alpine.

> Note: There are a couple of caveats to x-spread:
> - When the directive being "spread" is `x-for`, you should return a normal expression string from the callback. For example: `['x-for']() { return 'item in items' }`.
> - `x-data` and `x-init` can't be used inside a "spread" object.

---

### `x-cloak`
**Example:** `<div x-data="{}" x-cloak></div>`

`x-cloak` attributes are removed from elements when Alpine initializes. This is useful for hiding pre-initialized DOM. It's typical to add the following global style for this to work:

```html
<style>
    [x-cloak] { display: none; }
</style>
```

### Magic Properties

> With the exception of `$el`, magic properties are **not available within `x-data`** as the component isn't initialized yet.

---

### `$el`
**Example:**
```html
<div x-data>
    <button @click="$el.innerHTML = 'foo'">Replace me with "foo"</button>
</div>
```

`$el` is a magic property that can be used to retrieve the root component DOM node.

### `$refs`
**Example:**
```html
<span x-ref="foo"></span>

<button x-on:click="$refs.foo.innerText = 'bar'"></button>
```

`$refs` is a magic property that can be used to retrieve DOM elements marked with `x-ref` inside the component. This is useful when you need to manually manipulate DOM elements.

---

### `$event`
**Example:**
```html
<input x-on:input="alert($event.target.value)">
```

`$event` is a magic property that can be used within an event listener to retrieve the native browser "Event" object.

> Note: The $event property is only available in DOM expressions.

If you need to access $event inside of a JavaScript function you can pass it in directly:

`<button x-on:click="myFunction($event)"></button>`

---

### `$dispatch`
**Example:**
```html
<div @custom-event="console.log($event.detail.foo)">
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
    <!-- When clicked, will console.log "bar" -->
</div>
```

**Note on Event Propagation**

Notice that, because of [event bubbling](https://en.wikipedia.org/wiki/Event_bubbling), when you need to capture events dispatched from nodes that are under the same nesting hierarchy, you'll need to use the [`.window`](https://github.com/alpinejs/alpine#x-on) modifier:

**Example:**

```html
<div x-data>
    <span @custom-event="console.log($event.detail.foo)"></span>
    <button @click="$dispatch('custom-event', { foo: 'bar' })">
<div>
```

> This won't work because when `custom-event` is dispatched, it'll propagate to its common ancestor, the `div`.

**Dispatching to Components**

You can also take advantage of the previous technique to make your components talk to each other:

**Example:**

```html
<div x-data @custom-event.window="console.log($event.detail)"></div>

<button x-data @click="$dispatch('custom-event', 'Hello World!')">
<!-- When clicked, will console.log "Hello World!". -->
```

`$dispatch` is a shortcut for creating a `CustomEvent` and dispatching it using `.dispatchEvent()` internally. There are lots of good use cases for passing data around and between components using custom events. [Read here](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events) for more information on the underlying `CustomEvent` system in browsers.

You will notice that any data passed as the second parameter to `$dispatch('some-event', { some: 'data' })`, becomes available through the new events "detail" property: `$event.detail.some`. Attaching custom event data to the `.detail` property is standard practice for `CustomEvent`s in browsers. [Read here](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent/detail) for more info.

You can also use `$dispatch()` to trigger data updates for `x-model` bindings. For example:

```html
<div x-data="{ foo: 'bar' }">
    <span x-model="foo">
        <button @click="$dispatch('input', 'baz')">
        <!-- After the button is clicked, `x-model` will catch the bubbling "input" event, and update foo to "baz". -->
    </span>
</div>
```

> Note: The $dispatch property is only available in DOM expressions.

If you need to access $dispatch inside of a JavaScript function you can pass it in directly:

`<button x-on:click="myFunction($dispatch)"></button>`

---

### `$nextTick`
**Example:**
```html
<div x-data="{ fruit: 'apple' }">
    <button
        x-on:click="
            fruit = 'pear';
            $nextTick(() => { console.log($event.target.innerText) });
        "
        x-text="fruit"
    ></button>
</div>
```

`$nextTick` is a magic property that allows you to only execute a given expression AFTER Alpine has made its reactive DOM updates. This is useful for times you want to interact with the DOM state AFTER it's reflected any data updates you've made.

---

### `$watch`
**Example:**
```html
<div x-data="{ open: false }" x-init="$watch('open', value => console.log(value))">
    <button @click="open = ! open">Toggle Open</button>
</div>
```

You can "watch" a component property with the `$watch` magic method. In the above example, when the button is clicked and `open` is changed, the provided callback will fire and `console.log` the new value.

## Security
If you find a security vulnerability, please send an email to [calebporzio@gmail.com]().

Alpine relies on a custom implementation using the `Function` object to evaluate its directives. Despite being more secure then `eval()`, its use is prohibited in some environments, such as Google Chrome App, using restrictive Content Security Policy (CSP).

If you use Alpine in a website dealing with sensitive data and requiring [CSP](https://csp.withgoogle.com/docs/strict-csp.html), you need to include `unsafe-eval` in your policy. A robust policy correctly configured will help protecting your users when using personal or financial data.

Since a policy applies to all scripts in your page, it's important that other external libraries included in the website are carefully reviewed to ensure that they are trustworthy and they won't introduce any Cross Site Scripting vulnerability either using the `eval()` function or manipulating the DOM to inject malicious code in your page.

## V3 Roadmap
* Move from `x-ref` to `ref` for Vue parity?
* Add `Alpine.directive()`
* Add `Alpine.component('foo', {...})` (With magic `__init()` method)
* Dispatch Alpine events for "loaded", "transition-start", etc... ([#299](https://github.com/alpinejs/alpine/pull/299)) ?
* Remove "object" (and array) syntax from `x-bind:class="{ 'foo': true }"` ([#236](https://github.com/alpinejs/alpine/pull/236) to add support for object syntax for the `style` attribute)
* Improve `x-for` mutation reactivity ([#165](https://github.com/alpinejs/alpine/pull/165))
* Add "deep watching" support in V3 ([#294](https://github.com/alpinejs/alpine/pull/294))
* Add `$el` shortcut
* Change `@click.away` to `@click.outside`?

## License

Copyright © 2019-2020 Caleb Porzio and contributors

Licensed under the MIT license, see [LICENSE.md](LICENSE.md) for details.
