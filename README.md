# Welcome !
Glad you are here ! In this repository, I'll give you the Java flags I use to run my Minecraft server (with 80-90 players) at its best.
## Compatibility
These flags are only compatible with:
- Java 17+,
- GraalVM JVM,
- Linux.
# The flags

`--add-modules=jdk.incubator.vector -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=28 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1MixedGCCountTarget=3 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:SurvivorRatio=32 -XX:G1HeapWastePercent=5 -XX:MaxTenuringThreshold=1 -XX:+PerfDisableSharedMem -XX:G1SATBBufferEnqueueingThresholdPercent=30 -XX:G1ConcMarkStepDurationMillis=5 -XX:G1RSetUpdatingPauseTimePercent=0 -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:NmethodSweepActivity=1 -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:AllocatePrefetchStyle=3 -XX:+AlwaysActAsServerClassMachine -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+UseLargePages -XX:+EagerJVMCI -XX:+UseStringDeduplication -XX:+UseAES -XX:+UseAESIntrinsics -XX:+UseFMA -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+AlignVector -XX:+OptimizeFill -XX:+EnableVectorSupport -XX:+UseCharacterCompareIntrinsics -XX:+UseCopySignIntrinsic -XX:+UseVectorStubs -XX:UseAVX=2 -XX:UseSSE=4 -XX:+UseFastJNIAccessors -XX:+UseInlineCaches -XX:+SegmentedCodeCache -Djdk.nio.maxCachedBufferSize=262144 -Dgraal.UsePriorityInlining=true -Dgraal.Vectorization=true -Dgraal.OptDuplication=true -Dgraal.DetectInvertedLoopsAsCounted=true  -Dgraal.LoopInversion=true -Dgraal.VectorizeHashes=true -Dgraal.EnterprisePartialUnroll=true -Dgraal.VectorizeSIMD=true -Dgraal.StripMineNonCountedLoops=true  -Dgraal.SpeculativeGuardMovement=true -Dgraal.TuneInlinerExploration=1 -Dgraal.LoopRotation=true -Dgraal.OptWriteMotion=true -Dgraal.CompilerConfiguration=enterprise
`
# Why would I have to... switch ?
Almost 99% of Minecraft servers use Aikar's Flags and it is recommended by many people (some use nothing !). But why use other flags ?
The answer is that we have now developed to Minecraft 1.21.x and Java 22 (at the time of writing this) and Aikar's Flags **was written mainly to optimize Minecraft's Garbage Collector** and Java at that time (around 2018 - 2020). Later Java versions (from Java 17 and above) have added many optimization items for JVM. So I wrote a separate flag to support optimizations and integrates new features of Java 17+ while still keeping Aikar's flags to optimize GC. <ins>There's no risk in trying my flags !</ins>
# Explainations
- `--add-modules=jdk.incubator.vector` Enables advanced vector computation capabilities for Pufferfish/Purpur 1.18+. Useful for optimizing chunk loading and rendering.
- ` -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=28 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1MixedGCCountTarget=3 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:SurvivorRatio=32 -XX:G1HeapWastePercent=5 -XX:MaxTenuringThreshold=1 -XX:+PerfDisableSharedMem -XX:G1SATBBufferEnqueueingThresholdPercent=30 -XX:G1ConcMarkStepDurationMillis=5 -XX:G1RSetUpdatingPauseTimePercent=0` These are very generic G1GC flags, based on [Aikar's Flags](https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/).
- `-XX:+UseNUMA` Enables NUMA or (Non Uniform Memory Access) for supported architecture.
- `-XX:-DontCompileHugeMethods` Allows the JVM to compile larger methods to optimize performance.
- `-XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000` Sets the maximum number of nodes for the compiler's intermediate representation.
- `-XX:ReservedCodeCacheSize=400M` Sets the cache size for compiled codes. This will ensure the JIT have enough space for compiling.
- `-XX:NonNMethodCodeHeapSize=12M` Sets the code heap size for non-method codes.
- `-XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M` Same as above but for profiled and non profiled methods.
- `-XX:NmethodSweepActivity=1` Helps managing code cache more efficiently by keeping "cold" code in the cache for a longer time. There is no risk of "filling up" the code cache either, as cold code is more aggressively removed as it fills up.
## Advanced Performance Tuning
- `-XX:+UseFastUnorderedTimeStamps` This option enables the usage of fast unordered timestamps for time-related operations in the JVM.
- `-XX:+UseCriticalJavaThreadPriority` This option assigns a higher priority to critical Java threads, which can improve responsiveness and reduce latency in critical parts of the application.
- `-XX:AllocatePrefetchStyle=3` This will instruct JVM to use code style 3 for faster memory allocation. A value of 3 enables the most aggressive prefetching. More aggressive prefetching is generally useful on newer CPUs with large caches.
- `-XX:+AlwaysActAsServerClassMachine` Forces the JVM to act as if it's running on a server-class machine.
- `-XX:+UseTransparentHugePages` As it name said.
- `-XX:LargePageSizeInBytes=2M` Size of huge (large) pages.
- `-XX:+UseLargePages` Enables the usage of large pages for the JVM's memory allocation. Large pages can improve performance by reducing Translation Lookaside Buffer misses.
- `-XX:+EagerJVMCI` Enables the JVM Compiler Interface as early as needed, especially with GraalVM.
- `-XX:+UseStringDeduplication` Reduces memory ussage by deduping identical strings. Might reduce frequent GCs.
- `-XX:+UseAES -XX:+UseAESIntrinsics` Enables Hardware accelerated Advanced Encryption Standart intrinsics.
- `-XX:+UseFMA` Enables Fused Multiply Add instructions.
- `-XX:+UseLoopPredicate` Enables loop predicate optimization.
- `-XX:+RangeCheckElimination` Enables the elimination of range checks.
- `-XX:+OptimizeStringConcat` Enables optimization of string concatenation in the JVM, which can lead to improved performance when working with string concatenation operations.
- `-XX:+UseCompressedOops` Enables the use of compressed object pointers (Oops) which reduces memory usage on 64-bit systems.
- `-XX:+UseThreadPriorities` Enables the use of thread priorities.
- `-XX:+OmitStackTraceInFastThrow` Omits the stack trace for exceptions that are frequently thrown.
- `-XX:+RewriteBytecodes -XX:+RewriteFrequentPairs` Improves performance by actively rewriting byte codes and its pairs.
- `-XX:+UseFPUForSpilling` Provides a faster temporary storage for spills, so when we need just a few additional spill slots, we can avoid tripping to L1 cache-backed stack for this.
- `-XX:+UseFastStosb` Enables support for fast string operations.
- `-XX:+UseNewLongLShift` Use optimized bitwise shift left.
- `-XX:+UseVectorCmov` This option enables the usage of vectorized compare and move (cmov) instructions for better performance on CPUs that support these instructions.
- `-XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmRegToRegMoveAll` Uses XMM registers for Array Copy, int2double, int2float, register2register move operations
- `-XX:+UseXmmLoadAndClearUpper` Same as above but to load and clear upper registers.
- `-XX:+EliminateLocks` Improves single-threaded performance.
- `-XX:+DoEscapeAnalysis` Escape analysis is a technique by which the Java Hotspot Server Compiler can analyze the scope of a new object's uses and decide whether to allocate it on the Java heap.
- `-XX:+AlignVector` Aligns vectors for optimization.
- `-XX:+OptimizeFill` Enables optimization of memory fill operations.
- `-XX:+EnableVectorSupport` Its name suggested.
- `-XX:+UseCharacterCompareIntrinsics` Enables intrinsification of java.lang.Character functions.
- `-XX:+UseCopySignIntrinsic` Enables intrinsification of Math.copySign.
- `-XX:+UseVectorStubs` Optimizes vector operations.
- `-XX:UseAVX=2 -XX:UseSSE=4` Enable supported AVX, SSE instructions set on x86/x64 compatible CPUs.
- `-XX:+UseFastJNIAccessors` Use optimized versions of GetField.
- `-XX:+UseInlineCaches` Use Inline Caches for virtual calls
- `-XX:+SegmentedCodeCache` Enables the segmented code cache, which improves code cache management.
## GraalVM Compiler Specific Flags
- `-Djdk.nio.maxCachedBufferSize=262144` Sets the maximum size for cached direct buffers.
- `-Dgraal.UsePriorityInlining=true` Enables priority inlining in the Graal compiler.
- `-Dgraal.Vectorization=true` Enables vectorization in the Graal compiler.
- `-Dgraal.OptDuplication=true` Enables optimization duplication in the Graal compiler.
- `-Dgraal.DetectInvertedLoopsAsCounted=true` Enables detection of inverted loops as counted loops.
- `-Dgraal.LoopInversion=true` Enables loop inversion in the Graal compiler.
- `-Dgraal.VectorizeHashes=true` Enables vectorization of hash computations.
- `-Dgraal.VectorizeSIMD=true` Enables SIMD (Single Instruction - Multiple Data) vectorization.
- `-Dgraal.StripMineNonCountedLoops=true` Enables strip mining of non-counted loops.
- `-Dgraal.SpeculativeGuardMovement=true` Enables speculative guard movement in the Graal compiler.
- `-Dgraal.TuneInlinerExploration=1` Tunes the inliner exploration parameter in the Graal compiler.
- `-Dgraal.LoopRotation=true -Dgraal.UseCountedLoopSafepoints=true -Dgraal.EnterprisePartialUnroll=true` Loops related.
# Small test
Aikar's Flags and Adoptium JDK:

![image](https://github.com/user-attachments/assets/93e5ab90-708f-4d8a-8979-fa03e5cb6a56)

![image](https://github.com/user-attachments/assets/efb29f29-8512-4fda-a5e6-f038109c5270)

MeowIce's Flags and GraalVM:

![image](https://github.com/user-attachments/assets/8acd1d80-847d-403d-bb40-7f06c2074d02)

![image](https://github.com/user-attachments/assets/e99e1d94-4fd0-497e-a5f5-bf8c5fdba9a8)


### Sources
- [Java Flags Benchmarked](https://github.com/brucethemoose/Minecraft-Performance-Flags-Benchmarks)
- [Obydux GraalVM Flags](https://github.com/Obydux/Minecraft-GraalVM-Flags/)
- [Optimize MC Server on RedHat](https://www.redhat.com/en/blog/optimizing-rhel-8-run-java-implementation-minecraft-server)
### Read more
- [Hướng dẫn: Cấu hình để giảm lag và tối ưu server Minecraft để đạt hiệu năng tốt nhất](https://minecraftvn.net/cau-hinh-de-giam-lag-va-toi-uu-server-minecraft-de-dat-hieu-nang-tot-nhat.t46151/)
