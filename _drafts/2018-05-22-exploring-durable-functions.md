---
title: "Azure Functions - Exploring Durable Functions"
date:   2018-05-22 
categories: serverless
tags: 
  - durable functions
  - azure functions
---

After having successfully implemented a simple workflow using Azure Functions I decided to use the 'Durable Functions' extension to achieve the same effect. Will try and document some findings in this post.

The basic workflow I set out to achieve is to schedule one or more tasks triggered by an incoming message on a storage queue. These tasks do not fire off immediately but are delayed by a few minutes. 

This can be easily achieved by an Azure Function (F1) triggered by a storage queue message. For each delayed task that needs to run, F1 then posts a message to a storage queue with an appropriate visibility timeout. Another function (F2) is triggered when the message in this queue becomes visible and performs the required work. The delay is achieved by the 'visibility timeout' on the queue message. 


Microsoft recently [announced](https://azure.microsoft.com/en-us/blog/break-through-the-serverless-barriers-with-azure-functions/) GA for the 'Durable Functions' extension and that was the inspiration to try it out.

Durable Functions prescribes a certain pattern that should be followed. An 'Orchestrator' function ,as the name suggests, simply orchestrates a workflow by scheduling 'activity' functions and then collects results from these activities and process them and/or schedules further activities based on the result. The 'Orchestrator' function itself is usually triggered by a normal function.
The workflow described can be broken down into 2 activities
	- Calculating the subtasks and delay for each subtask
	- Performing the subtask

Here is the Orchestrator function that schedules the 2 activities
```c#
	    [FunctionName("Orchestrator")]
        public static async Task Orchestrator([OrchestrationTrigger] DurableOrchestrationContext context, TraceWriter log)
        {
            var results = await context.CallActivityAsync<List<Output>>("CalculateTasks", "input");

            List<Task> tasks = new List<Task>();
            var currentTime = context.CurrentUtcDateTime;
            foreach (var value in results)
            {
                TimeSpan timeDifference = currentTime - value.Origin;
                TimeSpan delay = TimeSpan.FromSeconds(value.DelayInSeconds);
                var actualDelay = timeDifference > delay ? TimeSpan.Zero : delay - timeDifference;

                var timeToStart = currentTime.Add(actualDelay);
                if (context.IsReplaying == false)
                {
                    log.Warning($"Origin time {value.Origin:o},Time to start {timeToStart:o}");
                }

                Task delayedActivityCall = context
                    .CreateTimer(timeToStart, CancellationToken.None)
                    .ContinueWith(t => context.CallActivityAsync("PerformSubtask", value),TaskContinuationOptions.ExecuteSynchronously);
                tasks.Add(delayedActivityCall);
            }

            //await Task.WhenAll(tasks);
            if (context.IsReplaying == false)
            {
                log.Warning($"All subtasks scheduled.");
            }
        }
```