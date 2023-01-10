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

    this.append(div, style);
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

  /**
   * This can return several things:
   * - `TemplateResult`: the return type of `html`
   * - An instance of an Element: `return document.createElement('...');`
   * - A string
   */
  render() {
    return html`Hello world`;
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
        ? this.items.map(item => html`<li>${item.name}</li>`)
        : 'No items'
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

  <td part=${ifDefined(parts)}>

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
