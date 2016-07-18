1. 不能信任系统, new file的时候, 这个file并没有建立, 要手动去建立xxxfile.create.

```java
File f = new File(imageDir);
if (!f.exists()) {
    f.mkdirs(); //建目录
  f.createNewFile(); //建文件
}
```

> File: Instances of this class may or may not denote an actual file-system object such as a file or a directory. If it does denote such an object then that object resides in a partition. A partition is an operating system-specific portion of storage for a file system. A single storage device (e.g. a physical disk-drive, flash memory, CD-ROM) may contain multiple partitions. The object, if any, will reside on the partition named by some ancestor of the absolute form of this pathname.

