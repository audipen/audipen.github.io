---
title: "Azure Functions - Handling messages in the poison queue"
date:   2017-10-28 
categories: serverless
tags: 
  - error handling
---

Azure Functions is Azure's serverless computing offering. Functions can be triggered by certain events such as a message being added to a storage queue, by a timer or simply by an HTTP request. Following is a sample of a Queue triggered Function written in C#.

```c#
public static void Run(CloudQueueMessage myQueueItem, 
    TraceWriter log)
{   
}
```

The message posted to the queue is available via the 'myQueueItem' parameter. If a function fails, it is retried 5 times for a given queue message and after that the message is moved to a 'poison' queue.

Failed messages can be handled in a couple of ways.
1. Use the  'DequeueCount' property of the CloudQueueMessage to check the number of times the message has been dequeued.If that number passes a certain threshold you can log the message with details of the failure. This threshold has to be less than or equal to 5 by default. In case you want to change the number of times the message is retried you can update the 'maxDequeueCount' property in the 'host.json' file. Using this option would avoid the message being added to a poison queue.
2. Alternatively you can let the message be added to the poison queue and add another function that is triggered whenever a message is added to the poison queue. You can do the logging in this function instead of doing it directly in the main function that processes the message. Use this approach to maintain a clear separation between the core business logic and logging/error handling code.
