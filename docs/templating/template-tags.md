---
meta:
  - name: description
    content: Snipcart plugin template tag reference.
---

# Template Tags

## Cart Snippet

```twig
{# include Snipcart JS #}
{{ craft.snipcart.cartSnippet }}
```

There are three optional arguments:

- `includejQuery` is `true` by default and will also render a `<script>` tag for loading jQuery. Disable to include it yourself, just be sure it’s [compatible with Snipcart](https://docs.snipcart.com/v2/).
- `onload` can be a value to set for the Snipcart tag’s `onload` property.
- `includeStyles` is `true` by default, and if `false` will not render the `<link>` tag that loads Snipcart’s base stylesheet.

```twig
{# include Snipcart JS + optional arguments #}
{{ craft.snipcart.cartSnippet(true, '', true) }}
```

## Cart Button

```twig
{# View Cart #}
{{ craft.snipcart.cartLink }}
```

There are two optional arguments:

- `text` can be used to change the button's inner text, which defaults to “Shopping Cart”.
- `showCount` is `true` by default and includes a `<span>` element classed with `snipcart-total-items` which Snipcart will populate with the number of items in the shopping cart.

```twig
{# View Cart + optional arguments #}
{{ craft.snipcart.cartLink('Shopping Cart', true) }}
```
