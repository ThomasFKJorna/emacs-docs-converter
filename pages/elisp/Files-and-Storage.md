

### 25.8 Files and Secondary Storage

After Emacs changes a file, there are two reasons the changes might not survive later failures of power or media, both having to do with efficiency. First, the operating system might alias written data with data already stored elsewhere on secondary storage until one file or the other is later modified; this will lose both files if the only copy on secondary storage is lost due to media failure. Second, the operating system might not write data to secondary storage immediately, which will lose the data if power is lost.

Although both sorts of failures can largely be avoided by a suitably configured file system, such systems are typically more expensive or less efficient. In more-typical systems, to survive media failure you can copy the file to a different device, and to survive a power failure you can use the `write-region` function with the `write-region-inhibit-fsync` variable set to `nil`. See [Writing to Files](Writing-to-Files.html).
