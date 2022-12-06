# Web component lifecycle

## Custom Element API

- ❌ constructor
- ✅ connectedCallback
- ❌ adoptedCallback
- ❌ attributeChangedCallback
- ✅ disconnectedCallback

## `LitElement` API

- ✅ willUpdate
- ❌ update
- ✅ updated
- ✅ firstUpdated

## Example 1

```ts
#onClick = (event: Event): void => {
  // Do something
};

override connectedCallback(): void {
  super.connectedCallback();

  // Do stuff here
  document.addEventListener('click', this.#onClick);
}

override disconnectedCallback(): void {
  // Do stuff here
  document.removeEventListener('click', this.#onClick);

  super.disconnectedCallback();
}
```

## Example 2

```ts
firstUpdated(): void {
  this.role = 'button';

  if (!this.hasAttribute('tabindex')) {
    this.tabIndex = 0;
  }
}
```

## Example 3

```ts
override updated(changes: PropertyValues<this>): void {
  super.updated(changes);

  if (changes.has('expandable') || changes.has('expanded')) {
    if (this.expandable) {
      this.setAttribute('aria-expanded', this.expanded ? 'true' : 'false');
    } else {
      this.removeAttribute('aria-expanded');
    }
  }
}
```

## Example 4

```ts
@property() firstName?: string;
@property() lastName?: string;
@state() fullName?: string;

willUpdate(changes: PropertyValues<this>): void {
  if (changes.has('firstName') || changes.has('lastName')) {
    this.fullName = `${this.firstName} ${this.lastName}`;
  }
}

override render(): TemplateResult {
  return html`Full name: ${this.fullName}`;
}
```
