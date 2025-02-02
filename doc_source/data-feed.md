# Data feeds<a name="data-feed"></a>

AWS Marketplace provides a number of data feeds to help sellers collect and analyze information about your product sales\. Data feeds are available to all registered AWS Marketplace sellers\. Since data feeds are generated within a day, they contain the most current data available\.

This page provides an overview of data feeds, and explains how to access and use them\. Subsequent pages describe each data feed\. 

## Storage and structure of data feeds<a name="data-feed-details"></a>

Data feeds collect and deliver comma\-separated value \(CSV\) files to an encrypted Amazon S3 bucket that you provide\. The CSV files have the following characteristics:
+ They follow [4180 standards](https://tools.ietf.org/html/rfc4180)\.
+ Character encoding is UTF\-8 without BOM\.
+ Commas are used as separators between values\.
+ Fields are escaped by double quotation marks\. 
+ `\n` is the line feed character\.
+ Dates are reported in the UTC time zone, are in ISO 8601 date and time format, and are accurate within 1 second\.
+ All `*_period_start_date` and `*_period_end_date` values are inclusive, which means that `23:59:59` is the last possible timestamp for any day\. 
+ All monetary fields are preceded with a currency field\. 
+ Monetary fields use a period \(`.`\) character as a decimal separator, and don't use a comma \(,\) as a thousands separator\. 

Data feeds are generated and stored as follows:
+ Data feeds are generated within a day, and contain 24 hours of data from the previous day\. 
+ In the Amazon S3 bucket, data feeds are organized by month using the following format:

  `bucket-name/data-feed-name_version/year=YYYY/month=MM/data.csv`
+ As each daily data feed is generated, it is appended to the existing CSV file for that month\. When a new month starts, a new CSV file is generated for each data feed\. 
+ Information in data feeds is backfilled from 2010/01/01 to 2020/04/30 \(inclusive\) and is available in the [CSV file](#data-feed-details) in the `year=2010/month=01` subfolder\.

  You may notice cases where the current month's file for a given data feed contains only column headers, and no data\. This means that there were no new entries for that month for the feed\. This can happen with data feeds that are updated less frequently, like the product feed\. In these cases, data is available in the backfilled folder\. 
+ In Amazon S3, you can create an [Amazon S3 lifecycle policy](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-lifecycle.html) to manage how long to keep files in the bucket\. 
+ You can configure Amazon SNS to notify you when data is delivered to your encrypted S3 bucket\. For information on how to configure notifications, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html) in the *Amazon Simple Notification Service Developer Guide*\.

### Historization of the data<a name="data-feed-historization"></a>

Each data feed includes columns that document the history of the data\. Except for `valid_to`, these columns are common to all data feeds\. They're included as a common history schema and are useful in querying the data\. 


| Column name  | Description  | 
| --- | --- | 
| valid\_from | The first date that the value for the primary key is valid for in relation to values for other fields\. | 
| valid\_to | This column is only shown on the [Address](data-feed-address.md) data feed and is always blank\. | 
| insert\_date | The date a record was inserted into the data feed\. | 
| update\_date | The date the record was last updated\.  | 
| delete\_date | This column is always blank\. | 

The following shows an example of these columns\. 


|  valid\_from  |  valid\_to  |  insert\_date  |  update\_date  |  delete\_date  | 
| --- | --- | --- | --- | --- | 
|  2018\-12\-12T02:00:00Z  |   |  2018\-12\-12T02:00:00Z  |  2018\-12\-12T02:00:00Z  |   | 
|  2019\-03\-29T03:00:00Z  |   |  2019\-03\-29T03:00:00Z  |  2019\-03\-29T03:00:00Z  |   | 
|  2019\-03\-29T03:00:00Z  |   |  2019\-03\-29T03:00:00Z  |  2019\-04\-28T03:00:00Z  |   | 

The `valid_from` and `update_date` field together form a *bi\-temporal data model*\. The `valid_from` field, as it is named, tells you when the item is valid from\. If the item was edited, it can have multiple records in the feed, each with a different `update_date`, but the same `valid_from` date\. For example, to find the current value for an item, you would find the record with the most recent `update_date`, from the list of records with the most recent `valid_from` date\.

In the example above, the record was originally created 2018\-12\-12\. It was then changed on 2019\-03\-29 \(for example, if the address in the record changed\)\. Later, on 2019\-04\-28, the address change was corrected \(so the `valid_from` didn't change, but the `update_date` did\)\. Correcting the address \(a rare event\) retroactively changes the record from the original `valid_from` date, so that field didn't change\. A query to find the most recent `valid_from` would return two records, the one with the latest `update_date` gives you the actual current record\.

## Accessing data feeds<a name="data-feed-accessing"></a>

To access data feeds, you need to configure your environment to receive data feeds to an encrypted Amazon S3 bucket\. AWS Marketplace provides an [AWS CloudFormation template](https://s3.amazonaws.com/aws-marketplace-reports-resources/DataFeedsResources.yaml) that you can use to simplify configuration\.

**To use the AWS CloudFormation template to configure your environment to receive data feeds**

1. Open a web browser and sign into the [AWS Marketplace Management Portal](http://aws.amazon.com/marketplace/management/), then go to [Set up customer data storage](https://aws.amazon.com/marketplace/management/reports/data-feed-configuration)\.

1. Choose **Create resources with AWS CloudFormation template** to open the template in the AWS CloudFormation console in another window\.

1. In the template, specify the following and then choose **Next**:
   + Stack name – The collection of resources you're creating to enable access to data feeds\.
   + Amazon S3 bucket name – The bucket for storing data feeds\.
   + \(Optional\) Amazon SNS topic name – The topic for receiving notifications when AWS delivers new data to the Amazon S3 bucket\.

1. On the **Review** page, confirm your entries and choose **Create stack**\. This will open a new page with the CloudFormation status and details\.

1. From the **Resources** tab, copy Amazon Resource Names \(ARNs\) for the following resources from the CloudFormation page into the fields on the AWS Marketplace [Set up customer data storage](https://aws.amazon.com/marketplace/management/reports/data-feed-configuration) page:
   + Amazon S3 bucket for storing data feeds
   + AWS KMS key for encrypting the Amazon S3 bucket
   + \(Optional\) Amazon SNS topic for receiving notifications when AWS delivers new data to the Amazon S3 bucket

1. On the **Set up customer data storage** page, choose **Submit**\.

1. \(Optional\) Edit the policies created by the CloudFormation template\. See [Data feed policies](#data-feed-policies) for more details\.

You are now subscribed to data feeds\. The next time data feeds are generated, you can access the data\.

For more information about AWS CloudFormation templates, see [Working with AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html) in the *AWS CloudFormation User Guide*\.

### Data feed policies<a name="data-feed-policies"></a>

When your Amazon S3 bucket is created by the CloudFormation template, it will create policies for access attached to that bucket, the AWS KMS key, and the Amazon SNS topic\. The policies allow the AWS Marketplace reports service to write to your bucket and SNS topic with the data feed information\. Each policy will have a section like the following \(this example is from the Amazon S3 bucket\)\.

```
        {
            "Sid": "AwsMarketplaceDataFeedsAccess",
            "Effect": "Allow",
            "Principal": {
                "Service": "reports.marketplace.amazonaws.com"
            },
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketAcl",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::datafeed-bucket",
                "arn:aws:s3:::datafeed-bucket/*"
            ]
        },
```

In this policy, `reports.marketplace.amazonaws.com` is the service principal that AWS Marketplace uses to push data to the Amazon S3 bucket\. The **datafeed\-bucket** is the bucket that you specified in the CloudFormation template\.

When the AWS Marketplace reports service calls Amazon S3, AWS KMS, or Amazon SNS, it will provide the ARN of the data it is intending to write to the bucket when it does\. To ensure that the only data written to your bucket is data written on your behalf, you can specify the `aws:SourceArn` in the condition of the policy\. In the following example, you must replace the *account\-id* with the ID for your AWS account\.

```
        {
           "Sid": "AwsMarketplaceDataFeedsAccess",
           "Effect": "Allow",
           "Principal": {
                "Service": "reports.marketplace.amazonaws.com"
            },
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketAcl",
                "s3:PutObjectAcl"
            ],
            "Resource": [
                "arn:aws:s3:::datafeed-test-bucket",
                "arn:aws:s3:::datafeed-test-bucket/*"
            ,
            "Condition": {
                "StringEquals": {
                        "aws:SourceAccount": "account-id",
                        "aws:SourceArn": ["arn:aws:marketplace::account-id:AWSMarketplace/SellerDataSubscription/DataFeeds_V1",
                        "arn:aws:marketplace::account-id:AWSMarketplace/SellerDataSubscription/Example-Report"]
                        }
                }
        },
```

## Using data feeds<a name="data-feed-using"></a>

When data is available in your Amazon S3 bucket, you can use data feeds in the following ways:
+ Download the \.CSV files from the Amazon S3 bucket you created in [Accessing data feeds](#data-feed-accessing) so that you can view the data in a spreadsheet\.
+ Use ETL \(extract, transform, and load\), SQL query, business analytics tools to collect and analyze the data\.

  You can use AWS services to collect and analyze data, or any third\-party tool that can perform analysis of \.CSV\-based datasets\.

### Example: Use AWS services to collect and analyze data<a name="data-feed-using-example"></a>

The following procedure assumes that you've already configured your environment to receive data feeds to an Amazon S3 bucket and that the bucket contains data feeds\.

**To collect and analyze data from data feeds**

1. From the [AWS Glue console](https://console.aws.amazon.com/glue), [create a crawler](https://docs.aws.amazon.com/glue/latest/dg/add-crawler.html) to connect to the Amazon S3 bucket that stores the data feeds, extract the data you want, and create metadata tables in the AWS Glue Data Catalog\.

   For more information about AWS Glue, see the [https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)\.

1. From the [Athena console](https://console.aws.amazon.com/athena), [run SQL queries on the data in the AWS Glue Data Catalog](https://docs.aws.amazon.com/athena/latest/ug/querying-athena-tables.html)\.

   For more information about Athena see the [https://docs.aws.amazon.com/athena/latest/ug/what-is.html](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)\. 

1. From the [Amazon QuickSight console](http://quicksight.aws.amazon.com), [create an analysis](https://docs.aws.amazon.com/quicksight/latest/user/creating-an-analysis.html) and then [create a visual](https://docs.aws.amazon.com/quicksight/latest/user/creating-a-visual.html) of the data\.

   For more information about Amazon QuickSight, see the [https://docs.aws.amazon.com/quicksight/latest/user/welcome.html](https://docs.aws.amazon.com/quicksight/latest/user/welcome.html)\.

For a detailed example of one way to use AWS services to collect and analyze data in data feeds, see [Using Seller Data Feed Delivery Service, Amazon Athena, and Amazon QuickSight to create seller reports](http://aws.amazon.com/blogs/awsmarketplace/using-seller-data-feed-delivery-service-amazon-athena-and-amazon-quicksight-to-create-seller-reports/) at the AWS Marketplace Blog\.