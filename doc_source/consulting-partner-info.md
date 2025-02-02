# Creating a private offer as a consulting partner<a name="consulting-partner-info"></a>

To create a consulting partner private offer, you must be registered through the [AWS Marketplace Management Portal](https://aws.amazon.com/marketplace/management) as an AWS Marketplace seller\. The following topics can help you get started as an AWS Marketplace seller:
+ [Getting started as a seller](user-guide-for-sellers.md)
+ [Preparing your product](product-preparation.md)
+ [Submitting your product for publication](product-submission.md)
+ [Seller reports and data feeds](reports-and-data-feed.md)

If you're new to providing products on AWS Marketplace, the following topics can help you better understand the kinds of products available:
+ [AMI\-based products](ami-products.md)
+ [Software as a service \(SaaS\)–based products](saas-products.md)

The following topics explain how to create a private offer as a consulting partner\.

**Topics**
+ [Putting an agreement in place with an ISV](#consulting-partner-isv-agreement)
+ [Extending a private offer based on an opportunity](#consulting-partner-recurring-discount)
+ [Sending a private offer to a buyer](#send-private-offer)
+ [Accepted offers](#consulting-partner-after-submitting-form)

## Putting an agreement in place with an ISV<a name="consulting-partner-isv-agreement"></a>

Before you as a consulting partner can create a private offer for a product, the ISV must authorize you to resell their product\. The ISV does this by creating an *opportunity* for you\. For more information, see [Creating a resell opportunity for a consulting partner as an ISV](consulting-partner-isv-info.md)\. To create an opportunity, the ISV must provide:
+ The product or products that they authorize you to resell\.
+ The price reduction that they want to offer you\.
+ The AWS account ID that you used to register as an AWS Marketplace seller\.

After the opportunity has been created, you will be an authorized reseller for that product\. Then, you can extend private offers that are marked up from the price given you by the ISV\.

## Extending a private offer based on an opportunity<a name="consulting-partner-recurring-discount"></a>

For recurring discount private offers, an ISV authorizes a consulting partner to resell one or more of their products on AWS Marketplace\. The discount, called the wholesale price, is an agreed\-to price or percentage discount off the product’s list price\. Consulting partners can use the discount with any number of buyers\.

The following procedure outlines how a consulting partner can extend a private offer based on a recurring discount\. 

**To extend a private offer based on a recurring discount**

1. Determine what your offer price will be\.

1. Sign into the [AWS Marketplace Management Portal](http://aws.amazon.com/marketplace/management/) with your AWS Marketplace Seller account\.
**Tip**  
Be sure that you are signed out from another AWS account before signing in with your AWS Marketplace Seller account\.

1. Choose **Partners** from the menu at the top of the screen\.

1. From the **Opportunities** list, choose the option button next to the opportunity with the product and discount you want to offer to the buyer, and then choose **Create offer**\.

   The amount shown in the **Discount** column for each opportunity is automatically populated on the **Create Private Offer** page\.

1. On the **Create Private Offer** page:

   1. For **Buyer\(s\)**, enter the one or more buyer AWS account IDs\.

      You can enter up to 25 buyer account IDs\.

   1. If your scenario is Flexible Payment Schedule \(FPS\), select the **Enable fixed units and allow buyers to pay for this product in installments** check box\.

   1. Choose **Next**\.

1. In the **Price Adjustment** section, choose one of the following options: 
   + Select the **Markup** option to increase the customer price from the wholesale cost and then enter the **Markup amount \(%\)**\. 
   + Select the **Discount** option to decrease the customer price from the public price\.

   For AMI products and SaaS contracts with consumption products, prices are expressed as a markup from the wholesale price, or a discount off the list price\.

1. If you selected the option for flexible payments, in the **Buyer Payment Schedule** section, enter the amount and invoice date for each payment the customer will make\. The amounts for **ISV Payment Schedule** and any additional information will autopopulate\.

1. In the **End User License Agreement ** section, review the EULA provided by the ISV\. You can also upload up to five documents to amend the ISV EULA\.

1. In the **Set Expiration Information** section, choose the **Offer expiry date** and **Subscription end date**\.

1. Choose **Review Offer**\.

   If you need to adjust the offer, choose **Revise Offer**\.

1. Review the offer, and then choose **Extend Offer**\.

The publishing process for this offer can take up to 45 minutes to complete\. After it's completed, the offer is visible on the **Offers**\. You can now [send the private offer to a buyer](#send-private-offer)\.

## Sending a private offer to a buyer<a name="send-private-offer"></a>

After the private offer has been published, you can send it to your buyer\.

**To send the private offer to your buyer**

1. Choose **Offers** from the menu at the top of the screen\.

1. Highlight the offer you created\.

1. Choose **Copy Offer URL**\.

1. Send the URL to your buyer\.

You can add up to 10 email addresses to receive notifications when the private offer has been accepted\. To set up email notifications, see [Manage notifications](notifications.md#manage-notifications)\.

## Accepted offers<a name="consulting-partner-after-submitting-form"></a>

After the buyer accepts the private offer from the consulting partner, the offer and any disbursement of funds occur in the same manner:

1. AWS Marketplace invoices the buyer on their existing AWS bill per the terms of the private offer\. If the private offer is extended to a linked account, the invoiced amount appears on the payer account associated with that linked account\.

1. The buyer pays their AWS bill in accordance with the net payment terms that they agreed to with AWS\. The private offer process enables custom terms for each transaction, but net payment terms aren't customizable\. 

1. After AWS receives payment from the buyer, AWS disburses payment to you and the ISV\. The ISV receives the wholesale cost minus the AWS Marketplace fee\. You receive your markup minus the AWS Marketplace processing fee\. All fees are percentages applied to the transaction amounts listed\. If you're not sure of the fee percentages and need this information for quoting purposes, contact your AWS Marketplace channel account manager\. If you don’t know who that is, send an email message to the AWS Marketplace channel team at [aws\-mp\-channel@amazon\.com](mailto://aws-mp-channel@amazon.com), and someone on the team will respond to you within 24 hours\. 

1. AWS Marketplace provides electronic reports to the ISV and to you using the [AWS Marketplace Management Portal](https://aws.amazon.com/marketplace/management)\. These reports have the following differences depending on the type of private offer:
   + For recurring discount private offers, the ISV sees you as the buyer and you see the subscriber as the buyer\.
   + For non\-recurring discount private offers, the ISV and the consulting partner see the subscriber as the buyer\.

For more information about AWS Marketplace reporting, see [Seller Reporting](https://docs.aws.amazon.com/marketplace/latest/userguide/Reporting.html)\.