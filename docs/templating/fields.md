---
meta:
    - name: description
      content: How to add Snipcart product purchase buttons to your Craft CMS frontend.
---

# Product Details

_Add to Cart_ buttons are critical to Snipcart because they define products behind the scenes. If you've used the _Product Details_ field, there's a `getBuyNowButton()` method that will surely save you some time. By default it will...

- Include a product's SKU, name, price, URL, quantity (of 1), and taxable+shippable status.
- Include weight and dimensions (in grams and centimeters) if the item is shippable.

It will also simplify the process of adding custom variations (color, size, etc.) whether or not they affect the product price.

:::tip
Working with your own markup? The [Snipcart plugin's internal Twig template](https://github.com/workingconcept/snipcart-craft-plugin/blob/master/src/templates/fields/front-end/buy-now.twig) may provide a helpful starting point.
:::

## Buy Button

Simplest form.

```twig
{# Buy Now #}
{{ entry.productDetails.getBuyNowButton() | raw }}
```

The default markup will look something like this without any [customization](/templating/fields.md#additional-options):

```html
<a href="#"
    class="snipcart-add-item"
    data-item-id="to-slay-a-mockingbird"
    data-item-name="To Slay a Mockingbird"
    data-item-price="12.99"
    data-item-url="https://craftcms.dev/products/to-slay-mockingbird"
    data-item-quantity="1"
    data-item-taxable="false"
    data-item-shippable="true"
    data-item-width="13"
    data-item-length="21"
    data-item-height="3"
    data-item-weight="3"
>Buy Now</a>
```

## Buy Button + Simple Options

Optionally supply custom options that don't affect pricing.

```twig
{# Buy Now button with custom options #}
{{ entry.productDetails.getBuyNowButton({
    customOptions: [{
        name: 'Size',
        required: true,
        options: [ 'Small', 'Medium', 'Large' ]
    }]
}) | raw }}
```

:::tip
While you can hardcode these values just like the examples, they could just as well come from another Entry field, like a [Table](https://docs.craftcms.com/v3/table-fields.html#settings) or a [Matrix Block](https://docs.craftcms.com/v3/matrix-fields.html#settings) that you've established for product variations. It's up to you!
:::

## Buy Button + Price-Variant Options

Custom options that each adjust the base product price. Each amount can be positive (increasing product price), negative (reducing product price), or zero (not affecting product price).

```twig
{{ entry.productDetails.getBuyNowButton({
    customOptions: [{
        name: 'Bling Type',
        required: true,
        options: [
            {
                name: 'Bedazzled',
                price: 0
            },
            {
                name: 'Bronzed',
                price: 25
            },
            {
                name: 'Diamond Studded',
                price: 500
            },
            {
                name: 'Used, Bad Shape',
                price: -50
            }
        ]
    }]
}) | raw }}
```

## Buy Button + Price Override

If you have a specific need to override the item's price in your template, passing a `price` property will do exactly that:

```twig
{{ entry.productDetails.getBuyNowButton({
    price: 100
}) | raw }}
```

A key/value JSON array can also be used to define prices in different currencies, provided that you've [configured your store](https://docs.snipcart.com/v2/configuration/multi-currency) to support multiple currencies.

```twig
{{ entry.productDetails.getBuyNowButton({
    price: { 'usd': 20, 'cad': 26.84 }
}) | raw }}
```

## All Options

| Property          | Requires                   | Description                                                                                                                                                                                              |
| ----------------- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `classes`         | array                      | Array of additional class names to be added to the anchor element. Default is `['btn']`.<br><br>`.snipcart-add-item` will automatically be added and cannot be removed since it's functionally required. |
| `text`            | string                     | Inner text, which defaults to `Buy Now`.                                                                                                                                                                 |
| `target`          | string                     | Anchor [target](https://www.w3schools.com/TAGs/att_a_target.asp).                                                                                                                                        |
| `title`           | string                     | Anchor [title](https://www.w3schools.com/tags/att_title.asp).                                                                                                                                            |
| `rel`             | string                     | Anchor [relationship](https://www.w3schools.com/TAGs/att_a_rel.asp).                                                                                                                                     |
| `quantity`        | integer                    | Initial quantity to be added to the cart. Defaults to `1`.                                                                                                                                               |
| `image`           | string                     | URL for a product thumbnail to be used in the cart. The default cart template's image is 50px square.                                                                                                    |
| `price`           | decimal or key/value array | Price override, or key/value array to define multiple currencies (`{ 'usd': 20, 'eur': 17.79 }`). Defaults to the price defined in the Product Details field.                                            |
| `additionalProps` | key/value array            | Attribute+value pairs for the anchor. Useful for supplying additional [product definition](https://docs.snipcart.com/v2/configuration/product-definition) details.                                          |

## Querying Elements by Product Details

Snipcart 1.3.4 added the ability to query elements by information stored in the Product Details field.

For example, you can grab `products` entries that have inventory:

```twig
{% set availableProducts = craft.entries()
    .section('products')
    .productDetails({ inventory: '> 0' })
    .all() %}
```

Or get items that are not shippable:

```twig
{% set availableProducts = craft.entries()
    .section('products')
    .productDetails({ shippable: false })
    .all() %}
```

Or all items more than $50 but less than $1,000:

```twig
{% set availableProducts = craft.entries()
    .section('products')
    .productDetails({ price: '> 50', price: '< 1000' })
    .all() %}
```