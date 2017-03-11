# Documentation

This is an attempt to write a very detailed technical explanation how ASF works in detail - it's **definitely** not a text for typical user that just wants to make ASF "work", you have **[Setting up](https://github.com/JustArchi/ArchiSteamFarm/wiki/Setting-up)** in this case.

---

## Preface

We'll mainly focus on interesting aspects of ASF and offer explanation for more complex functions or features. This documentation won't include an explanation of every single line of code, but it will include explanation of how ASF achieves various Steam activities that might not be obvious to achieve at first glance. It's especially important for us to include information about various built-in functions, features, hacks and workarounds - as those are typically invisible to end-user and you might not know about their existance without carefully examining the source code - that's also their purpose, they just work.

ASF changes **rapidly**, therefore keeping this documentation up-to-date with code changes might be very hard and time-consuming in long-run. We will try to update it as often as possible, but we cannot guarantee that it'll always stay up-to-date in regards to latest ASF release. Current version of documentation is based on ASF **V2.2.3.7**.

---

## ASF