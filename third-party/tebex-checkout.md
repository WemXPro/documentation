---
title: Tebex Checkout
description: The Tebex checkout is a multi gateway payment handler
published: true
date: 2024-01-05T18:45:32.816Z
tags: 
editor: markdown
dateCreated: 2023-10-15T18:51:17.466Z
---

# Tebex Checkout

Tebex checkout is a payment gateway service that supports a lot of global and local payment gateways. It's easy to setup and provides a robust checkout for your application.

The Tebex checkout comes in two variants.
1. Tebex Checkout (Single transactions gateway that supports all of the Tebex Gateways)
2. Tebex Subscription (Subscription based gateway limited to gateways that support Subscriptions)

## Setting up Tebex checkout

Before you can start using the Tebex checkout, you must first register your account and business on the Tebex website: https://creator.tebex.io/projects/tebex-checkout/create

1. Go to Admin Area -> Gateways -> Create Gateway (Select Tebex Checkout or Subscription)
2. Edit the newly created Tebex gateway, and add the username, password and webhook key

![tebex-credentials.png](/assets/tebex-credentials.png)

You can find the tebex username and password on the creator dashboard on Tebex https://creator.tebex.io/developers/api-keys (see screenshot)

![tebex-config.png](/assets/tebex-config.png)

You can find the webhook secret key on this page: https://creator.tebex.io/webhooks/endpoints (see screenshot)

![tebex-webhook-key.png](/assets/tebex-webhook-key.png)

Lastly, make sure you disable "test mode" for production use https://creator.tebex.io/payment-methods/settings (see screenshot)

![turn-off-test-mode.png](/assets/turn-off-test-mode.png)

3. To make the gateway functional, you need to add webhook endpoints that will let your application know when a payment is completed

## Webhook Endpoints

### Tebex Checkout
Create a webhook endpoint here: https://creator.tebex.io/webhooks/endpoints

Enter https://example.com/payment/return/tebex-checkout as the webhook endpoint url

Select the following events:
- Payment Completed
- Payment Declined
- Payment Refunded
- Payment Dispute Opened
- Payment Dispute Closed

### Tebex Subscriptions
Create a webhook endpoint here: https://creator.tebex.io/webhooks/endpoints

Enter https://example.com/payment/return/tebex-subscription as the webhook endpoint url

Select the following events:
- Recurring Payment Started
- Recurring Payment Renewed
- Recurring Payment Ended
- Recurring Payment Status Changed
- Payment Dispute Opened
- Payment Dispute Closed

