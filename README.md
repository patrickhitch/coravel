# Coravel

[![CircleCI](https://circleci.com/gh/jamesmh/coravel/tree/master.svg?style=svg)](https://circleci.com/gh/jamesmh/coravel/tree/master)

__Note: Coravel is unstable as it's in the "early" stages of development. Once version 2 is released Coravel will be considered stable. Please use with this in mind :)__

Inspired by all the awesome features that are baked into the Laravel PHP framework - coravel seeks to provide additional features that .Net Core lacks like:

- Task Scheduling
- Queuing
- Mailer [TBA]
- Command line tools integrated with coraval features [TBA]
- Caching [TBA]
- More???

## Full Docs

- [Task Scheduling](https://github.com/jamesmh/coravel/blob/master/Docs/Scheduler.md)
- [Queuing](https://github.com/jamesmh/coravel/blob/master/Docs/Queuing.md)

## Quick-Start

Add the nuget package `Coravel` to your .NET Core app. Done!

### 1. Scheduling Tasks

Tired of using cron and Windows Task Scheduler? Want to use something easy that ties into your existing code?

In `Startup.cs`, put this in `ConfigureServices()`:

```c#
services.AddScheduler(scheduler =>
    {
        scheduler.Schedule(
            () => Console.WriteLine("Run at 1pm utc during week days.")
        )
        .DailyAt(13, 00)
        .Weekday();
    }
);
```

For async tasks you may use `ScheduleAsync()`:

```c#
scheduler.ScheduleAsync(async () =>
{
    await Task.Delay(500);
    Console.WriteLine("async task");
})
.EveryMinute();
```

Easy enough? Look at the documentation to see what methods are available!

### 2. Task Queuing

Tired of having to install and configure other systems to get a queuing system up-and-running? Look no further!

In `Startup.cs`, put this in `ConfigureServices()`:

```c#
services.AddQueue();
```

Voila! Too hard?

To append to the queue, inject `IQueue` into your controller (or wherever injection occurs):

```c#
using Coravel.Queuing.Interfaces; // Don't forget this!

// Now inside your MVC Controller...
IQueue _queue;

public HomeController(IQueue queue) {
    this._queue = queue;
}
```

And then call:

```c#
this._queue.QueueTask(() =>
    Console.WriteLine("This was queued!")
);
```

Or, for async tasks:

```c#
this._queue.QueueAsyncTask(async () =>
{
    await Task.Delay(100);
    Console.WriteLine("This was queued!")
});
```

Now you have a fully functional queue!