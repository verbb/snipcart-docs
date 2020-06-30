---
meta:
    - name: description
      content: Plugin setup guide, examples, and reference.
---

# Snipcart Plugin for Craft CMS

![](../resources/hero.svg)

One of the best things about [Snipcart](https://snipcart.com/) is how quickly it can be used to turn _any_ site into a working store. The Snipcart plugin will help you get your store running even faster with Craft CMS and integrate more deeply, even if you've never used Snipcart.

[[toc]]

## Plugin Features

### Super fast store setup.

Use the included _Product Details_ field type and automatically handle things like unit conversion and quantity adjustment.

![Product Details Field](../resources/field-type.png)

Add the cart system to your frontend, a cart link with an item count, and [simple or complex _Buy Now_ buttons](/templating/fields.md) with included Twig tags.

```twig
{# include Snipcart JS #}
{{ craft.snipcart.cartSnippet }}

{# View Cart #}
{{ craft.snipcart.cartLink }}

{# Buy Now #}
{{ entry.productDetails.getBuyNowButton() | raw }}

{# Buy Now button with custom options #}
{{ entry.productDetails.getBuyNowButton({
   'customOptions': [
       {
           name: 'Color',
           required: true,
           options: [ 'Blue', 'Green', 'Red' ]
       }
   ]
}) | raw }}

```

### Browse store details from the control panel.

Control panel section with sales stats, orders, customers, discounts, abandoned carts, and subscriptions.

![Orders](../resources/overview.png)

Customizable Dashboard widget.

![Dashboard Widget](../resources/widget.png)

### Create discounts and refund orders from the control panel.

![Refund Order](../resources/refund.png)

### Email custom order notifications.

Use included store [admin and customer order notifications](/setup/notifications.md), optionally using your own Twig templates. You can also [hook into events](/dev/events.md) to send notifications (email, Slack, etc.) for whatever you want!

![Admin Order Email](../resources/order-email.png)

### Add custom functionality with powerful webhooks.

Integrate your own store logic with [more than ten different events](/dev/events.md). Manage shipping rates, inventory, and email notifications, and more.

```php
Event::on(
    Shipments::class,
    Shipments::EVENT_BEFORE_RETURN_SHIPPING_RATES,
    function(WebhookEvent $event) {
        $event->rates = $this->modifyShippingRates(
            $event->rates,
            $event->order,
            $event->packaging
        );
    }
);
```

## Commerce as a Service

![Snipcart order flow.](../resources/order-flow.png)

Snipcart's focus is on quick site integration, so a lot of complex store functionality is handled by a hosted service. The Snipcart plugin relies on Craft and Snipcart's REST API to more tightly integrate the two and reduce your setup and development time without sacrificing the ability to have a full-featured store.

## Craft Commerce Comparison

The Snipcart plugin is great for stores that don't require the full complexity of Commerce Pro, but would be too limited by Commerce Lite.

![Snipcart vs. Craft Commerce](../resources/commerce-comparison.png)

### How’s this different from Commerce?

If you’ve worked with [Craft Commerce](https://craftcms.com/commerce) before, there are some key differences to be aware of:

1. **Snipcart is focused on the front end.**  
Snipcart (the service) gets product details from buy buttons on the front end, so you’re free to model products in Craft however you’d like. There’s no purchasable or custom element type. The included Product Details field conveniently stores common product information in a field type.
2. **Heavy lifting is handled by Snipcart’s SaaS.**  
Your Craft project’s role is to deliver a front end and optionally use event hooks for customizing shipping options and order flow. A number of integrations and options are easily configured from [app.snipcart.com](https://app.snipcart.com/).
3. **You don’t need to create checkout or email templates;** you can quickly start with Snipcart’s base templates and modify them as you’d like. (Email templates can be edited from Snipcart’s dashboard, and the cart can be modified with CSS and a JavaScript API.)
4. **Snipcart users != Craft users.**  
Snipcart customers can check out as guests or create an account, but the plugin doesn’t provide any automatic syncing of Snipcart accounts and Craft users. With Commerce, every customer is a Craft user and that could be important for your shop.