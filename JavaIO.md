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

## Streams
### Nomenclature

<hr>
<center>Stream vs Reader/Writer</center>
<hr>

The contents of a file may be accessed or written via a stream which is a list of data elements presented sequentially. Java provides three vuilt in streams: `System.in`, `System.err` and `System.out`. The `java.io` API provides numberous classe sfor creating, accessing and manipulating streams. It defines two sets of classes for reading and writing streams: those with `Stream` in their name and those with `Reader`/`Writer` in their name.

So, what's the differences between the classes with `Stream` in their name and those with `Reader`/`Writer` in their name?

>While both are sets of classes that define a stream that reads a file, the difference is based on *how the stream is read or written*. The stream classes are used for inputting or outputting all types of binary or byte data while the reader and writer classes are used for inputtting and outputting character/string data. While it may seem like the reader/writer classes are redundant, they're **classes of convenience**. We can write to a file using reader/writer classes without worrying about the byte encoding of the file.

<hr>
<center>Input vs Output</center>
<hr>

Most input stream classes have a corresponding output stream class. Similarly, most reader classes have a corresponding writer class. However, there are some exceptions to this. For e.g., the `PrinterWriter` class has no corresponding reader class.

<hr>
<center>Low Level vs High Level</center>
<hr>

Streams can also be segmented as low level streams and high level streams. A low level stream comes directly in contact with the source of the data such as a file or an array. Alternatively, high level streams are built on top of another stream using **wrapping**. 

```java
try (
BufferedReader bufferedReader = new BufferedReader(new FileReader("zoo-data.txt"))) { 
    System.out.println(bufferedReader.readLine());
}
```

Here, the `FileReader` stream is the low level stream that's fed into the high level `BufferedReader` stream.

## Stream Base Classes
Java defines 4 abstract classes that are parents of all stream classes:
1. `InputStream`
2. `OutputStream`
3. `Reader`
4. `Writer`

Here are some examples:

```java
new BufferedInputStream(new FileReader("zoo-data.txt"));
new BufferedWriter(new FileOutputStream("zoo-data.txt"));
new ObjectInputStream(new FileOutputStream("zoo-data.txt")); 
new BufferedInputStream(new InputStream());
```

None of these will compile. Why? 
1. Feeding a `Reader` stream to an `InputStream`
2. Feeding an `OutputStream` to a `Writer` stream.
3. Feeding an `OutputStream` to an `InputStream`.
4. Trying to instantiate an object of an abstract class i.e. `InputStream`.

## Stream Operations
1. **Closing**
2. **Flushing**
3. **Marking**


