---
title: "Upload bulk data from on-premise to Azure SQL Database"
date:   2017-12-09 
categories: data-upload
tags: 
  - ssis
  - Azure Data Factory
  - bcp
---

When it comes to uploading mass data from an on-premise data store, be it a text file or a SQL database, to an Azure SQL database there are a few options that are at your disposal. 

 - SQL Server Integration Services (SSIS)
 - Azure Data Factory
 - Azure SQL Data Sync
 - bcp
 - SqlBulkCopy
 
Let us look into each one of them and how they can be used to upload data.

Failed messages can be handled in a couple of ways.
1. Use the  'DequeueCount' property of the CloudQueueMessage to check the number of times the message has been dequeued.If that number passes a certain threshold you can log the message with details of the failure. This threshold has to be less than or equal to 5 by default. In case you want to change the number of times the message is retried you can update the 'maxDequeueCount' property in the 'host.json' file. Using this option would avoid the message being added to a poison queue.
2. Alternatively you can let the message be added to the poison queue and add another function that is triggered whenever a message is added to the poison queue. You can do the logging in this function instead of doing it directly in the main function that processes the message. Use this approach to maintain a clear separation between the core business logic and logging/error handling code.
