---
title: Delayed Payment Services report data
labels: troubleshooting,payment services,adobe commerce,magento,2.4.2-p1
---

This article explains why reporting data in Payment Services may be delayed.

## Affected products and versions

* Adobe Commerce (on-premise) 2.4.2-p1
* Adobe Commerce Cloud 2.4.2-p1
* Magento Open Source 2.4.2-p1
* Payment Services for Adobe Commerce and Magento Open Source, Early Access Program (EAP)

## Issue

Payment Services reporting data, for Payout and Order payment status reports, may not sync immediately.

After you invoice (capture) an order or issue a credit memo for an order, for instance, the status of the order may not be immediately available.

<u>Steps to reproduce</u>:

Prerequisites: An order is placed using Payment Services functionality.

1. An order is [invoiced](https://docs.magento.com/user-guide/sales/invoice-create.html) (or [canceled](https://docs.magento.com/user-guide/sales/order-update.html#cancel-a-pending-order) or [refunded via credit memo](https://docs.magento.com/user-guide/sales/credit-memos.html]) ) in the [Admin](https://docs.magento.com/user-guide/stores/admin.html).
1. Navigate to the Order payment status report to see information about that order.
1. The status is shown as `AUTHORIZED`, which is the order status prior to the invoicing or other order action.

    Commerce has not synced data and sent it to Payment Services, so the new order status is not yet recognized by the report.

>![info]
>
>This is only one common use case. There may be other use cases when an [order action](https://docs.magento.com/user-guide/sales/order-actions.html) occurs and the data is not immediately available in the applicable report.

<u>Expected result</u>:
Report data is populated immediately after there is an action on an order.

<u>Actual result</u>:
There may be a delay in visible report data for just-completed order actions. Payout reports may incur a delay of 24-48 hours. Order payment status reports may incur a few hour delay.

## Cause

There are two factors that affect this delay in visible data in the Admin:

* How often you choose to sync (export and persist) data from Commerce, via [configuration in the Admin](https://devdocs-beta.magento.com/payment-services/configure-payments.html).
* Timeframe in which PayPal publishes reporting data.

## Solution

This issue of delayed reporting data for Order payment status reports will be resolved for our General Availability (GA) release in late November 2021. Payout reports may still incur a delay because of the dependency on PayPal's data publishing timeframe.
