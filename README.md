# lightweight-java-profiler

Fork of https://code.google.com/p/lightweight-java-profiler/

This is a proof of concept implementation for a lightweight Java profiler. It is a sampling profiler that gathers stack traces asynchronously, avoiding the inaccuracies of only being able to profile code at safe points, and the overhead of having to stop the JVM to gather a stack trace. The result is a more accurate profiler that avoids the 10-20% overhead of something like hprof.

For more about why this is interesting, see

http://jeremymanson.blogspot.com/2010/07/why-many-profilers-have-serious.html

This profiler will only work with OpenJDK derived JDK/JVMs.

## Compiling

```
make clean all
```

It defaults to a 64-bit build. It will place the resulting `liblagent.so` file in the build-64 directory. If you want to have a 32-bit build, say "make BITS=32 clean all" instead.

## Usage

To use the profiler, you can invoke Java as follows:

```
java -agentpath:path/to/liblagent.so[:file=fname] <jvm flags>
```

It will spit out stack traces into traces.txt (or into the optional fname passed to the agent). The current implementation samples every 1/100th of a second. It stores the first 3000 stack traces it encounters; additional stack traces will be ignored, but duplicate stack traces will continue to be counted indefinitely (or until the counter overflows, which will take a while).

Yes, this is a horrible user interface. I imagine it would be relatively straightforward to spit out results in dot, or create any interface you want for parsing the output. Patches welcome.
