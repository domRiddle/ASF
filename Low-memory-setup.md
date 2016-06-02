# Low-memory setup

***

ASF as an application tries to be as much optimized and efficient as possible, which also takes in mind resources being used during execution. When it comes to memory, ASF prefers performance over memory consumption, which can result in temporary memory "spikes", which can be noticed e.g. with accounts having 3+ badge pages, as ASF will fetch and parse first page, read from it total number of pages, then launch fetch task for every page, which results in concurrent fetching and parsing of remaining pages. This speeds up execution, for cost of increased memory usage. Similar thing is happening e.g. with parsing active trade offers, ASF is also parsing them all concurrently.

At the moment ASF doesn't offer extra config property that would prefer to focus on keeping memory low for cost of performance, but it's entirely possible to add such property in future. If you'd be interested, make sure to add a comment.

***

## ASF

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of ```ShutdownOnFarmingFinished```. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You need only one active bot to keep ASF instance running, and be able to bring up other bots as needed.
- Keep your bots number low. Not ```Enabled``` bot instance doesn't take any resources, as ASF immediately drops state of the bot after reading it's config. Of course, you won't be able to ```!start``` disabled bot instance, so I suggest to disable only bots that you rarely start, and wouldn't mind to restart the ASF process to enable it.
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing ```AccountLimiterDelay``` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time.

***

## Mono

If you're running ASF on Windows, then there is nothing more that can help you, as Windows is not optimized by definition, so instead of looking at ASF, start looking at your OS.

Mono is highly customizable and you can use many switches and parameters to keep memory low. I strongly suggest to check out ```man mono``` 