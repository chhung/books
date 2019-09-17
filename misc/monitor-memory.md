# monitor memory

```java
private void monitorMemory(int loop, long millis) {
    Runtime runtime = Runtime.getRuntime();
    //runtime.gc();
    long memory = 0;
    memory = runtime.totalMemory() - runtime.freeMemory();
    System.out.println("Used memory is bytes: " + memory);
    System.out.println("Used memory is megabytes: "
            + bytesToMegabytes(memory));
}
```

```java
private static long bytesToMegabytes(long bytes) {
    return bytes / (1024L * 1024L);
}
```

