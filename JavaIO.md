## Introduction

**File**: A record within a file system that stores user and system data.
**Directory**: A record within a file system that contains files as well as other directories.

### The File Class
The most commonly used class wrt files is the `java.io.File` class. It's used to read information about existing files and directories, list the contents of a directory and create/delete files and directories. However, it cannot read or write data within a file.

<table>
<tr>
<td>
Here, we create a file object that determines if the file <code>gunjan.txt</code> exists.
</td>
<td>

```java
import java.io.File;
public class FileSample{
    public static void main(String[] args){
        File file = new File("data/gunjan.txt");
        System.out.println(file.exists());
    }
}
```

</td>
</tr>
</table>

Also, the paths can be joined like so:

```java
File parent = new File("/home/smith");
File child = new File(parent,"data/zoo.txt");
```

Some commonly used `File` methods are:
<table>
<th>Method Name</th>
<th>Description</th>
<tr>
<td><code>exists()</code></td>
<td>Returns true if the file or directory exists.</td>
</tr>
<tr>
<td><code>getName()</code></td>
<td>Returns the name of the file or directory denoted by this path</td>
</tr>
</tr>
<tr>
<td><code>getAbsolutePath()</code></td>
<td>Returns the absolute pathname string of this path.</td>
</tr>
</tr>
<tr>
<td><code>isDirectory()</code></td>
<td>Returns true if the file denoted by this path is a directory</td>
</tr>
</tr>
<tr>
<td><code>isFile()</code></td>
<td>Returns true if the file denoted by this path is a file</td>
</tr>
<tr>
<td><code>length()</code></td>
<td>Returns the number of bytes in the file.</td>
</tr>
<tr>
<td><code>lastModified()</code></td>
<td>Returns the number of milliseconds since the last epoch when the file was modified.</td>
</tr>
<tr>
<td><code>delete()</code></td>
<td>Deletes the file or directory. If the path denotes a directory, then it must be empty to be deleted</td>
</tr>
<tr>
<td><code>renameTo()</code></td>
<td>Renames the file denoted by this path.</td>
</tr>
<tr>
<td><code>mkdir()</code></td>
<td>Creates the directory named by this path.</td>
</tr>
<tr>
<td><code>mkdirs()</code></td>
<td>Creates the directory named by this path including any nonexistent parent directories.</td>
</tr>
<tr>
<td><code>getParent()</code></td>
<td>Returns the abstract pathname of this abstract pathname's parent or <code>null</code> if this pathname does not name a parent directory.</td>
</tr>
<tr>
<td><code>listFiles()</code></td>
<td>Returns a <code>File[]</code> array denoting the files in the directory.</td>
</tr>
</table>