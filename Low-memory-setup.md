# Low-memory setup

***

ASF is extremely lightweight on resources by definition, depending on your usage even 128 MB VPS with Linux is capable of running it, although going that low is not recommended and can lead to issues. While being light, ASF is not afraid of asking OS for more memory, if such memory is needed for ASF to operate with optimal speed.
m
ASF as an application tries to be as much optimized and efficient as possible, which also takes in mind resources being used during execution. When it comes to memory, ASF prefers performance over memory consumption, which can result in temporary memory "spikes", which can be noticed e.g. with accounts having 3+ badge pages, as ASF will fetch and parse first page, read from it total number of pages, then launch fetch task for every extra page, which results in concurrent fetching and parsing of remaining pages. This speeds up execution, for cost of increased memory usage. Similar thing is happening e.g. with parsing active trade offers, ASF is also parsing them all concurrently. On top of all of that, ASF (C# runtime) doesn't return unused memory back to OS immediately. Huh? What's going on?

ASF is extremely well optimized, and makes use of available resources as much as possible. High memory usage of ASF doesn't mean that ASF actively **uses** that memory and **needs it**. Very often ASF will keep some memory allocated for some "room" for future actions, as by not asking OS for every memory chunk we're improving performance. The runtime (.NET, or Mono) should automatically release unused ASF memory back to OS when OS will **truly** need it. Remember - **[unused memory is wasted memory](http://www.howtogeek.com/128130/htg-explains-why-its-good-that-your-computers-ram-is-full/)**. You run into issues when the memory you **need** is higher than the memory that is available for you, not when ASF keeps some extra for having free space for functions that will execute in a moment. You run into problems when Linux kernel is killing Mono process due to OOM (out of memory), not when you see ASF as top memory consumer in ```htop```.

At the moment ASF doesn't offer extra config property that would prefer to focus on keeping memory low for cost of performance, but it's entirely possible to add such property in future (if enough demand). If you'd be interested, make sure to **[post an issue](https://github.com/JustArchi/ArchiSteamFarm/issues)**. I don't think it's needed as ASF as a program in best case can run some concurrent code synchronously, which is not good as it heavily affects performance, and it's not always better to keep 40 MB for 20 seconds, rather than 100 MB for 3 seconds. Therefore I prefer to focus on writing **fast** code, and leave optimal memory management for runtime, which you can read about below.

***

Below suggestions are divided into three categories, from simple ASF tricks, through fine-tuning the runtime, and finishing at full runtime recompilation.

***

## ASF (Easy)

- Never run more than one ASF instance. ASF is meant to handle unlimited number of bots all at once, and unless you're binding every ASF instance to different interface/IP address, you should have exactly **one** ASF process, with multiple bots (if needed).
- Make use of ```ShutdownOnFarmingFinished```. Active bot takes more resources than deactivated one. It's not a significant save, as the state of bot still needs to be kept, but you're saving some amount of resources, especially all resources related to networking, such as TCP sockets. You need only one active bot to keep ASF instance running, and you can always bring up other bots if needed.
- Keep your bots number low. Not ```Enabled``` bot instance doesn't take any resources, as ASF immediately drops state of the bot after reading it's config. Of course, you won't be able to ```!start``` disabled bot instance, so I suggest to disable only bots that you rarely start, and wouldn't mind to restart the ASF process to enable them.
- Fine-tune your configs. Especially global ASF config has many variables to adjust, for example by increasing ```AccountLimiterDelay``` you'll bring up your bots slower, which will allow already started instance to fetch badges in the meantime, as opposed to bringing up your bots faster, which will take more resources as more bots will do major work (such as parsing badges) at the same time. The less work that has to be done at the same time - the less memory used.

Those are a few things you can keep in mind when dealing with memory usage. However, those things don't have any "crucial" matter on memory usage, because memory usage comes mostly from things ASF has to deal with, and not from internal structures used for cards farming.

The most resources-heavy functions are:
- Badge page parsing
- Inventory parsing

Which means that memory will spike the most when ASF is dealing with reading badge pages, and when it's dealing with it's inventory (e.g. sending trade or dealing with STM). This is because ASF has to deal with really huge amount of data - the memory usage of your favourite browser launching those two pages will not be any lower than that. Sorry, that's how it works - decrease number of your badge pages, and keep number of your inventory items low, that can help :+1: 

***

## Mono Runtime (Advanced)

If you're running ASF on Windows, then there is nothing more that can help you, as Windows is not optimized by definition, so instead of looking at ASF, start looking at your OS. Squishing every megabyte out of runtime is pointless when your OS allocates minimum of 2 GB by definition.

If you're not using Windows, then you should know that Mono is highly customizable and you can use many switches and parameters to keep it's memory low. I strongly suggest to check out ```man mono``` to find out which of the recommended options are available for you, because they can differ from one version or another.

Some interesting features that could help you:
- Take a look at ```MONO_GC_PARAMS```. This is the most important thing that directly affects how GC operates, properly setting things such as ```nursery-size```, ```soft-heap-limit``` or ```evacuation-threshold``` can really help you, by tuning GC to me more aggressive and try to keep memory low, as opposed to default tuning for performance.
- Use ```--desktop``` parameter which will tune Mono for desktop usage, slightly improving garbage collector which will act more aggressively.
- Experiment with ```-O``` also known as ```--optimize```. Enabling optimizations in general should increase performance for cost of increased memory usage, so logically you should **avoid** using optimizations if you intend to keep memory low. However, there are certain optimizations that might improve memory efficiency, such as ```deadce``` - dead code elimination. If you want, go ahead and experiment, you can also turn on all optimizations with ```-O=all```, although that probably won't help with memory.

A good example of excellent ```MONO_GC_PARAMS``` for keeping memory as low as possible:
```
export MONO_GC_PARAMS="nursery-size=1m,soft-heap-limit=256m,evacuation-threshold=90,save-target-ratio=0.1,default-allowance-ratio=1.0"
```

I suggest to further tune ```soft-heap-limit``` to size that you expect from ASF to occupy at most, and also read about other variables I put in ```man mono```. **Note:** ```soft-heap-limit``` doesn't specify maximum allowed memory for ASF to use, as we can't put any hard limit on GC, we can only suggest GC how much we can expect from ASF to use, but if there will be a need, GC is free to ignore our tip to satisfy ASF needs. I suggest to set this parameter to 75-90% of free memory you expect to have.

**Also keep in mind that above ```MONO_GC_PARAMS``` will heavily affect ASF performance, you can't expect ASF to be fast if you expect from GC to be as conservative as possible at the same time. I strongly suggest to fine-tune ```MONO_GC_PARAMS``` to your needs.**

ASF by default is optimized for performance. If you use ```run.sh``` script from ASF tree, then you'll notice that Mono is started with ```--llvm --server -O=all``` arguments. Mono optimized for memory should avoid ```--llvm``` and ```-O=all```, in addition to that it should use ```--desktop``` instead of ```--server```. In the end, you should experiment a bit to find out which setup suits you best.

***

## Mono Recompilation (Expert / Developer)

Finally, you can recompile your Mono to disable some unused features and generate code that is optimized for size and not performance, which could help you even further. This is very complex process, and you should not do that in general, unless you're feeling comfortable with recompiling and using packages from source, as opposed to binary ones.

Here is a script of mine which should greatly help you with the process:
```
#!/bin/bash
set -eu

PREFIX="/opt/mono-unstable"
JOBS="$(grep -c processor /proc/cpuinfo)"

ADCFLAGS=(-Os -pipe -march=native -fdata-sections -ffunction-sections -s)
ADLDFLAGS=(-Wl,--gc-sections)
ADMONOCFLAGS=("--prefix=$PREFIX")

# Below flags are unsupported, you use them at your own risk
ADMONOFLAGS+=(--disable-boehm --disable-libraries --disable-nls --with-gc=none --with-mcs-docs=no --with-ikvm-native=no --with-shared_mono=no --with-xen-opt=no)
ADMONOFLAGS+=(--enable-minimal=profiler,pinvoke,debug,reflection_emit_save,large_code,logging,com,portability,attach,full_messages,verifier,soft_debug,perfcounters,normalization,shared_perfcounters,security,sgen_remset,sgen_marksweep_par,sgen_marksweep_fixed,sgen_marksweep_fixed_par,sgen_copying)

export PATH="$PREFIX/bin:$PATH"
mkdir -p "$PREFIX"

git pull
git submodule update --init --recursive

export CFLAGS="${ADCFLAGS[@]} -std=gnu11"
export CXXFLAGS="${ADCFLAGS[@]} -std=gnu++14 -fvisibility=hidden"
export LDFLAGS="${ADLDFLAGS[@]}"

if [[ ! -x "autogen.sh" ]]; then
        echo "ERROR: autogen.sh could not be found!"
        exit 1
fi

if [[ -f "Makefile" ]]; then
        make clean || true
fi

./autogen.sh "${ADMONOFLAGS[@]}"
make -j "$JOBS"
make install -j "$JOBS"
```

Firstly clone mono repository:
```git clone https://github.com/mono/mono --depth 100```

Then execute my script, it's possible that you will need to install some extra dependencies required by ```autogen.sh```, so don't be afraid of getting errors at first.

If your compilation succeeded, then you can find your brand new mono in ```$PREFIX``` location, which is ```/opt/mono-unstable``` in my case. Navigate there, and create one more file, I suggest to name it ```envsetup.sh```:

```
#!/bin/bash

MONO_PREFIX=/opt/mono-unstable
export DYLD_FALLBACK_LIBRARY_PATH=$MONO_PREFIX/lib:$DYLD_LIBRARY_FALLBACK_PATH
export LD_LIBRARY_PATH=$MONO_PREFIX/lib:$LD_LIBRARY_PATH
export C_INCLUDE_PATH=$MONO_PREFIX/include
export ACLOCAL_PATH=$MONO_PREFIX/share/aclocal
export PKG_CONFIG_PATH=$MONO_PREFIX/lib/pkgconfig
export PATH=$MONO_PREFIX/bin:$PATH
```

Now when you will want to run ASF with our self-compiled stripped Mono, simply execute:

```
source /opt/mono-unstable/envsetup.sh
mono ASF.exe
```

And done.

***

### So what we're exactly doing?

We're recompiling Mono with ```-Os``` - optimization for size, as opposed to default flag of ```-O2``` - optimization for speed. We use ```-march=native``` to instruct gcc into generating code designed specially for our current machine, therefore that Mono won't run on any other machine, but as an advantage we're making use of all our available CPU instructions, and also L1/L2 CPU cache sizes, which can improve performance and memory usage. Finally, we're also removing unused code via ```-ffunction-sections --fdata-sections -Wl,--gc-sections``` and stripping final binary with ```-s```.

That is GCC part, Mono part is more interesting. Basically we disable all Mono features we don't need in ASF, I suggest to do ```./configure --help``` to read about them all.

Last word, what works for me, might not work for you. This is only example of mono recompiling process, it's **unsupported** by both me and Mono development team, therefore if you decide to compile yourself and use any of unsupported flags, you should be an expert that doesn't require any support and is completely fine dealing with broken packages. If you want to make ASF "just work", don't do any of that, because it doesn't even guarantee any significant gain on performance neither memory usage. Stick with ASF and Mono runtime instructions, and let maintainers do their job with compiling packages.

Finally, optimizing for memory usage degrades performance almost always. Recompilation of Mono should be the last step, when you're extremely constrained by available memory and you don't mind much slower execution for every damn kilobyte saved. At some point you must accept the fact that you simply don't have enough memory for all tasks you expect ASF to do, therefore either decrease amount of those tasks, or add more memory :+1:.