---
title: Error 404 on all pages due to Content Staging issue
labels: 404,Magento Commerce,Magento Commerce Cloud,content,log,staging,troubleshooting
---

This article provides a fix for the Magento Commerce and Commerce Cloud issue, where you get 404 error when accessing any storefront page or Magento Admin.

## Affected products and versions

* Magento Commerce 2.2.x, 2.3.x
* Magento Commerce Cloud 2.2.x, 2.3.x

## Issue

Accessing any storefront page or Magento Admin results in the 404 error (the "Whoops, our bad..." page), after performing operations with scheduled updates for store content assets using [Content Staging](http://docs.magento.com/m2/ee/user_guide/cms/content-staging.html) (updates for store content assets scheduled using the [Magento\_Staging module](http://devdocs.magento.com/guides/v2.2/mrg/ee/Staging.html) ). For example, you may have deleted a Product with a scheduled update, or removed the end date for the scheduled update.

A store content asset includes:

* Product
* Category
* Catalog Price Rule
* Cart Price Rule
* CMS Page
* CMS Block
* Widget

Some scenarios are discussed in the Cause section below.

## Cause

The `flag` table in the database (DB) contains invalid links to the `staging_update` table.

The problem is related to Content Staging. Below are two particular scenarios; please note there might be more situations that trigger the issue.

 **Scenario 1:** Deleting a store content asset which:

* has an update scheduled with Content Staging
* the update has an end date (meaning the expiry date after which the updated asset reverts to its previous version)
* the end date of the update is in the past

At the same time, the issue might not occur if a deleted asset has no end date for the scheduled update.

 **Scenario 2:** Removing the end date/time of a scheduled update.

### Identify if your issue is related

To identify, if the issue you are experiencing is the one described in this article, run the following DB query:

<pre>SELECT f.flag_data->>'$.current_version' as flag_version, (su.id IS NOT NULL) as update_exists FROM flag f LEFT JOIN staging_update su ON su.id = f.flag_data->>'$.current_version' WHERE flag_code = 'staging';</pre>

If the query returns a table where `update_exists` value is "0", then an invalid link to the `staging_update` table exists in your database and the steps described in the [Solution section](#solution) will help to solve the issue. The following is an example of the query result with `update_exists` value equal to "0":

![issue_exists.png](assets/issue_exists.png)

If the query returns a table where `update_exists` value is "1" or an empty result, it means your issue is not related to staging updates. The following is an example of the query result with `update_exists` value equal to "1":

![issue_is_different.png](assets/issue_is_different.png)

In this case you might refer to [Site Down Troubleshooter](https://support.magento.com/hc/en-us/articles/360029351531) for troubleshooting ideas.

<h2 id="solution">Solution</h2>

1. Run the following query to delete the invalid link to the `staging_update` table:    ```sql    DELETE FROM flag WHERE flag_code = 'staging';    ```    
1. Wait for the cron job to be run (runs in up to 5 minutes if set up properly) or run it manually if you do not have cron set up.

The problem should be solved straight after fixing the invalid link. If the problem persists, [submit a support ticket.](https://support.magento.com/hc/en-us/articles/360019088251) .