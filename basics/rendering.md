# Rendering

Lit comes with built-in directives (different from Angular directives!): https://lit.dev/docs/templates/directives/

## Example 1

```js
class Foo extends HTMLElement {
  constructor() {
    super();

    this.attachShadow({ mode: 'open' });

    const div = document.createElement('div');
    div.textContent = 'Hello world!';

    const style = document.createElement('style');
    style.innerHtml = `
      :host {
        display: block;
        font-style: italic;
      }
    `;

    this.shadowRoot.append(div, style);
  }
}
```

vs

```ts
class Foo extends LitElement {
  static styles = css`
    :host {
      display: block;
      font-style: italic;
    }
  `;

  message = 'World';

  /**
   * This can return several things:
   * - `TemplateResult`: the return type of `html`
   * - An instance of an Element: `return document.createElement('...');`
   * - A string
   */
  render() {
    return html`<div>Hello ${this.message}</div>`;
  }
}
```

## Example 2

```ts
@property({ type: Array }) items?: string[];

override render(): TemplateResult {
  return html`
    <h1>The items</h1>
    <ul>
      ${this.items 
        ? this.items.map(item => html`<li>${item}</li>`)
        : '<li class="empty">No items</li>'
      }
    </ul>
  `;
}
```

## Example 3

```ts
override render(): TemplateResult {
  return html`
    <h1>The items</h1>
    ${this.renderList(this.items)}
  `;
}

renderList(items: string[]): TemplateResult {
  return html`
    <ul>
      ${this.items 
        ? this.items.map(item => html`<li>${item.name}</li>`)
        : 'No items'
      }
    </ul>
  `;
}
```

## Example 4

```ts
return html`
  <tr class=${classMap({ selected, [this.name]: true, foo: true })}></tr>
  <tr class="selected bar foo">

  <td part=${ifDefined(parts)}>
  <img src=${ifDefined(this.imageUrl)}>

  <span aria-hidden="true" class="direction">
    ${choose(
      this.direction,
      [
        ['asc', () => html`⬆️`],
        ['desc', () => html`⬇️`]
      ],
      () => html`↕️`
    )}
  </span>
`;
```

## Example 5

```ts
@property() data: string[];

@state() state?: string[];

willUpdate(changes: PropertyValues<this>): void {
  if (changes.has('data')) {
    this.state = [...this.state, data];
  }
}

render() {
  return html`
    ${this.state.map(...)}
  `;
}

onClick(): void {
  this.state = this.state.filter(item => !item.state);
}
```
