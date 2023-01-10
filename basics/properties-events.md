# Properties & events

## Properties

Properties on a web component are just like properties on built-in elements (such as `<input>`).

## Example 1

```ts
@property() name?: string;
@property() name: string | undefined;
```

```html
<x-foo name="bar">
<x-foo .name=${this.name}>
```

## Example 2

```ts
@property({ type: Boolean, reflect: true, attribute: 'no-border' }) noBorder?: boolean;

this.noBorder = true;
```

```css
:host([no-border]) {
  --_border: none;
}
```

```html
<x-foo no-border>
<x-foo ?noBorder=${this.noBorder}>
```

## Example 3

```ts
@property({ attribute: false }) callback?: () => void;
```

```html
<x-foo .callback=${() => console.log('HOHOHO')}>
```

## Example 4

```ts
// foo is either an instance of Foo or null
const foo = document.querySelector<Foo>('x-foo');

if (foo) {
  foo.name = 'my name';
}
```

## Events

There is no special event mechanism in web components or Lit elements (similar to Angular and unlike React).

- `addEventListener` / `removeEventListener`
- `dispatchEvent`

## Example 1

```ts
#onDocumentClick = (event: Event): void => {
  console.log('click');
};

override connectedCallback(): void {
  super.connectedCallback();

  document.addEventListener('scroll', this.#onDocumentClick);
}

override disconnectedCallback(): void {
  document.removeEventListener('click', this.#onDocumentClick);

  super.disconnectedCallback();
}
```

## Example 2

```ts
onClick(): void {
  if (!this.disabled) {
    this.dispatchEvent(new Event('my-custom-event', { bubbles: true, composed: true }));
  }
}
```

## Example 3

```ts
import { event } from '@dna/core/common';

@event() change!: EventEmitter<boolean>;

onInput(): void {
  this.change.emit(true);
}
```

## Example 4

```ts
import { EventsController } from '@sanomalearning/slds-core/utils/controllers';

#events = new EventsController(this);

override connectedCallback(): void {
  super.connectedCallback();

  this.#events.listen(document, 'click', this.onClick);
  this.#events.listen(this, 'input', this.onInput);
}

onClick(event: Event): void {
  ...
}

onInput(event: Event): void {
  ...
}
```

## Example 5

```html
<button @click=${this.#onClick}>
```

```ts
class Foo extends LitElement {
  #onClick(event: Event): void {
    console.log('click', { event });
  }
}
```