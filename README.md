# Welcome !
Hello there, I'm glad you are here ! This instruction will guide you to use MeowIce's Flags to make your Minecraft server run as it best !

# What is MeowIce's Flags ?
MeowIce's Flags is a high-performance JVM Startup Flag set designed to work with modern Minecraft and Java versions. These flags provide a significant performance uplift by taking advantage of the latest Java features and allowing the use of ZGC for better memory management.  
MeowIce's flags are available on [Docker Minecraft Server](https://deepwiki.com/itzg/docker-minecraft-server/5.2-java-and-jvm-options#meowice-flags), [Birdflop Flags Generator](https://www.birdflop.com/resources/flags/), and soon [flags.sh](https://github.com/YouHaveTrouble/flags.sh/pull/1).  
> [!NOTE]
> Hey, if my flags helped your server, consider leaving a star or donating (found at the end of this page) !
> <img width="756" height="157" alt="image" src="https://github.com/user-attachments/assets/4db8d2e9-e2fa-448e-9a52-7af7a6c7369a" />

## Compatibility
MeowIce's flags are compatible and tested with:
- Java 25+,
- GraalVM JVM,
- Linux,
- Minecraft 1.21+
- x86_64 systems.
> [!NOTE]
> On ARM64 systems, you must manually remove these architecture specific args: `-XX:+UseFastStosb -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:UseAVX=2`.
> _[Source](https://github.com/itzg/docker-minecraft-server/pull/3498/commits/651c5b6b94262020d9fe5f605deb3698a98fc3dd)_

> [!TIP]
> Using MeowIce's Flags with [Leaf](https://github.com/Winds-Studio/Leaf) as server software can get more performance !
# Why would I have to... switch ? 
Most Minecraft servers use Aikar's flags, and it is highly recommended. Then why would I have to use this ?  

In general, Aikar's flags were written for Java 8 era (2014 - 2018), when it was the only Java version we used. With Minecraft having progressed to version 1.21.x and soon Java 26 (at the time of writing this), most of Aikar's flags are obsolete or have been integrated to JVM itself. With that being said, there is no reason to only use Aikar's flags in 2026, and doing so is almost a placebo effect. "Almost" matters because his G1GC tuning is still has an effect in some specific cases.  

MeowIce's flags integrate the existing Aikar's G1GC performance tuning, while utilizing the latest advanced performance tweaks from GraalVM and furthermore optimizes the memory management _(thanks ZGC)_ to enhance smooth Minecraft server experience.  
MeowIce's flags are suitable for server admins who are looking to improve their server for large player base and high plugin count.  
_See the [Comparions & Benchmarks](https://github.com/MeowIce/meowice-flags?tab=readme-ov-file#comparison--benchmark) section._

# Why GraalVM but not others ?
GraalVM with better JIT performance and more aggressive optimizations compared to traditional `HotSpot C2` found in many JDKs (including OpenJDK and AdoptiumJDK (aka. Eclipse Temurin)). This change makes inlining, loop unrolling, escape analysis, and vectorization work better while maintaining lower CPU usage.  
Furthermore, the GraalVM compiler compiles plugins and server code faster and smarter, adapting to actual usage and environment on the fly.  
> [!TIP]
> GraalVM allows you to inspect memory usage and trace down naughty plugins using VisualVM !

# The flags set
## G1GC version (for servers below 32GB allocated heap)
> [!NOTE]
> This is recommended for servers allocated below 32GB RAM with basic resource usage. Balanced between memory usage and performance.

`--add-modules=jdk.incubator.vector -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=28 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1MixedGCCountTarget=3 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:SurvivorRatio=32 -XX:G1HeapWastePercent=5 -XX:+PerfDisableSharedMem -XX:G1SATBBufferEnqueueingThresholdPercent=30 -XX:G1ConcMarkStepDurationMillis=5 -XX:G1RSetUpdatingPauseTimePercent=0 -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:+UseCriticalJavaThreadPriority -XX:AllocatePrefetchStyle=3 -XX:+AlwaysActAsServerClassMachine -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+UseLargePages -XX:+EagerJVMCI -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+AlignVector -XX:+OptimizeFill -XX:+EnableVectorSupport -XX:+UseCharacterCompareIntrinsics -XX:+UseCopySignIntrinsic -XX:+UseVectorStubs -XX:+UseFastJNIAccessors -XX:+UseInlineCaches -XX:+SegmentedCodeCache -XX:+UseCompactObjectHeaders -Djdk.nio.maxCachedBufferSize=262144 -Djdk.graal.UsePriorityInlining=true -Djdk.graal.Vectorization=true -Djdk.graal.OptDuplication=true -Djdk.graal.DetectInvertedLoopsAsCounted=true -Djdk.graal.LoopInversion=true -Djdk.graal.VectorizeHashes=true -Djdk.graal.EnterprisePartialUnroll=true -Djdk.graal.VectorizeSIMD=true -Djdk.graal.StripMineNonCountedLoops=true -Djdk.graal.SpeculativeGuardMovement=true -Djdk.graal.TuneInlinerExploration=1 -Djdk.graal.LoopRotation=true -Djdk.graal.CompilerConfiguration=enterprise
`

## ZGC version (for large servers with 32GB+ allocated heap)
> [!NOTE]
> This is recommended for servers with more than 32GB of allocated memory and many CPU cores, allowing for minimizing the delay between each GC run with a larger heap size in an intensive environment.

> [!WARNING]
> Only use this if you have more than 10 CPU cores; using ZGC on systems with fewer than 8 cores may cause a significant performance impact !

`--add-modules=jdk.incubator.vector -XX:+UseZGC -XX:-ZProactive -XX:SoftMaxHeapSize=$((Memory - 2048))M -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:+PerfDisableSharedMem -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:+UseCriticalJavaThreadPriority -XX:AllocatePrefetchStyle=1 -XX:+AlwaysActAsServerClassMachine -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+UseLargePages -XX:+EagerJVMCI -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+AlignVector -XX:+OptimizeFill -XX:+EnableVectorSupport -XX:+UseCharacterCompareIntrinsics -XX:+UseCopySignIntrinsic -XX:+UseVectorStubs -XX:+UseFastJNIAccessors -XX:+UseInlineCaches -XX:+SegmentedCodeCache -XX:+UseCompactObjectHeaders -Djdk.nio.maxCachedBufferSize=262144 -Djdk.graal.UsePriorityInlining=true -Djdk.graal.Vectorization=true -Djdk.graal.OptDuplication=true -Djdk.graal.DetectInvertedLoopsAsCounted=true -Djdk.graal.LoopInversion=true -Djdk.graal.VectorizeHashes=true -Djdk.graal.EnterprisePartialUnroll=true -Djdk.graal.VectorizeSIMD=true -Djdk.graal.StripMineNonCountedLoops=true -Djdk.graal.SpeculativeGuardMovement=true -Djdk.graal.TuneInlinerExploration=1 -Djdk.graal.LoopRotation=true -Djdk.graal.CompilerConfiguration=enterprise`
# Comparison & Benchmark
I have made a comparison with one of my customers' servers.
## Spark
Aikar's Flags and Adoptium JDK:

<img width="1909" height="398" alt="image" src="https://github.com/user-attachments/assets/eae32edc-f162-4b33-9246-2443933621ac" />


MeowIce's Flags and GraalVM:

<img width="1920" height="423" alt="image" src="https://github.com/user-attachments/assets/dcd97360-d73e-49f3-a4b2-1869bb581686" />

## TPS
With only Aikar's flags and OpenJDK:
<img width="1405" height="202" alt="image" src="https://github.com/user-attachments/assets/42e6dab1-76ed-4c1b-b320-d4d0a3407cf1" />
With MeowIce's flags and GraalVM:
<img width="1296" height="255" alt="image" src="https://github.com/user-attachments/assets/c8280ebd-f51e-43fb-a16a-62340c8829ca" />


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


## Benchmark from user
### System specs
- 10700K @ 5.00 GHz OC
- RAM DDR4 96 GB @ 3000 MT/s
- OS Proxmox `8.4.1` (Running Ubuntu 24.04.2 LTS Pterodactyl Panel)

Version used to test is `leaf-1.21.8-52.jar`. Both servers had 6 GB of allocated RAM and 16 dedicated threads. For monitoring and measuring data [UnifiedMetrics](https://github.com/Cubxity/UnifiedMetrics) was used.

Java Docker Image for Pterodactyl used: ghcr.io/vanes430/java:graaljdk_24-debian from https://github.com/vanes430/java

For Aikar's i used 

`java -Xms6G -Xmx6G -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:+PerfDisableSharedMem -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1HeapRegionSize=8M -XX:G1HeapWastePercent=5 -XX:G1MaxNewSizePercent=40 -XX:G1MixedGCCountTarget=4 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1NewSizePercent=30 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:G1ReservePercent=20 -XX:InitiatingHeapOccupancyPercent=15 -XX:MaxGCPauseMillis=200 -XX:MaxTenuringThreshold=1 -XX:SurvivorRatio=32 --add-modules=jdk.incubator.vector -DLeaf.enableFMA=true -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar server.jar nogui`  

And for MeowIce i used 

`java -Xms6G -Xmx6G --add-modules=jdk.incubator.vector -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=28 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1MixedGCCountTarget=3 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:SurvivorRatio=32 -XX:G1HeapWastePercent=5 -XX:MaxTenuringThreshold=1 -XX:+PerfDisableSharedMem -XX:G1SATBBufferEnqueueingThresholdPercent=30 -XX:G1ConcMarkStepDurationMillis=5 -XX:G1RSetUpdatingPauseTimePercent=0 -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:AllocatePrefetchStyle=3 -XX:+AlwaysActAsServerClassMachine -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+UseLargePages -XX:+EagerJVMCI -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+AlignVector -XX:+OptimizeFill -XX:+EnableVectorSupport -XX:+UseCharacterCompareIntrinsics -XX:+UseCopySignIntrinsic -XX:+UseVectorStubs -XX:UseAVX=2 -XX:UseSSE=4 -XX:+UseFastJNIAccessors -XX:+UseInlineCaches -XX:+SegmentedCodeCache -Djdk.nio.maxCachedBufferSize=262144 -Djdk.graal.UsePriorityInlining=true -Djdk.graal.Vectorization=true -Djdk.graal.OptDuplication=true -Djdk.graal.DetectInvertedLoopsAsCounted=true -Djdk.graal.LoopInversion=true -Djdk.graal.VectorizeHashes=true -Djdk.graal.EnterprisePartialUnroll=true -Djdk.graal.VectorizeSIMD=true -Djdk.graal.StripMineNonCountedLoops=true -Djdk.graal.SpeculativeGuardMovement=true -Djdk.graal.TuneInlinerExploration=1 -Djdk.graal.LoopRotation=true -Djdk.graal.CompilerConfiguration=enterprise -DLeaf.enableFMA=true -jar server.jar nogui`


**So now with results**

Aikar's Grafana https://snapshots.raintank.io/dashboard/snapshot/2PwtSHpB6w3BT2jStlYWAbCaKoGf2WJ1?orgId=0

MeowIce's Grafana https://snapshots.raintank.io/dashboard/snapshot/xPugteW5fyKu6Mpq4Zi8J0HgTRU4KEDF?orgId=0

Data showing all of quantile's 

Aikar's: https://snapshots.raintank.io/dashboard/snapshot/KqirFpcpC4Smo6qrSjfKf6aQeee4KvJy?orgId=0

MeowIce's: https://snapshots.raintank.io/dashboard/snapshot/IpKM7ZpJOrwJTwErZucogu79Akzmx9JR?orgId=0

Some screenshots
<img width="940" height="418" alt="image" src="https://github.com/user-attachments/assets/ca24329e-5d2b-42f5-9b8e-d21c49305cc8" />

<img width="939" height="409" alt="image" src="https://github.com/user-attachments/assets/1b904c7b-6c15-4773-9bb3-622ed86b0941" />

  Overall, I observed an average performance uplift of around 20 MSPT. 
  However, the most crucial difference appeared once I reached 1k simulated players:
  
  **MeowIce’s flags consistently held around 30–35 MSPT**
  
  **Aikar’s flags spiked to and held around 90–95 MSPT**

I did my best to minimize variance in the testing process. Please disregard the [initial](https://i.imgur.com/Mw1hkXx.png) segment of the data, as it isn’t relevant due to me forgetting to kill all entities (they were removed on Aikar’s setup from the star).

This oversight does not affect the main conclusion: **MeowIce’s flags demonstrated a significant performance advantage over Aikar’s.**

You can confirm these findings by reviewing the Grafana snapshots. 

One final note: I only encountered a single G1 Old Gen event, which caused a minor spike. Aside from that, the performance was very stable and highly impressive.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Donate

Hey ! If my JVM flags help your server, or if you want to contribute, consider donating to keep me motivated to improve them !  
ETH: `0x7F4d0e0Ed6836C1558d6fEF43BB808CA2743D184`  
BTC: `bc1p7azhpa6g5jlm0ujglfj8u88aeh2hhhdxuu675afcxsvjf7gh53psc7lj3j`  
XMR: `48By4CMoPJr66b1tGgwbKXBMVZxqtPE1d1inyoGJGJM3SQSfwgTVoeQCPA9z8ioJHoeznuB2ntuNga8r1WNM1j3TSUYnSCr`  
TON: `UQCYAO9xv3QNFbYkVq0G_bHMTXf-mIGyVec0T4DgMWd8cqKi`  

### Star History

[![Star History Chart](https://api.star-history.com/svg?repos=MeowIce/meowice-flags&type=Date)](https://www.star-history.com/#MeowIce/meowice-flags&Date)

### Sources
- [Java Flags Benchmarked](https://github.com/brucethemoose/Minecraft-Performance-Flags-Benchmarks)
- [Obydux GraalVM Flags](https://github.com/Obydux/Minecraft-GraalVM-Flags/)
- [Optimize MC Server on RedHat](https://www.redhat.com/en/blog/optimizing-rhel-8-run-java-implementation-minecraft-server)
### My Optimization service:
- [Dịch vụ tối ưu, fix lag máy chủ Minecraft](https://minecraftvn.net/toi-uu-fix-lag-cuc-cang-may-chu-minecraft.t47454/)
