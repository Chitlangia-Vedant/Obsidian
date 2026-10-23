The [PrintWriter](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html) class offers the functionality to write to files. The constructor of the `PrintWriter` class receives as its parameter a string that represents the location of the target file.

```java
PrintWriter writer = new PrintWriter("file.txt");
writer.println("Hello file!"); //writes the string "Hello file!" and line change to the file
writer.println("More text");
writer.print("And a little extra"); // writes the string "And a little extra" to the file without a line change
writer.close(); //closes the file and ensures that the written text is saved to the file
```

The constructor of the `PrintWriter` class might throw an exception that must be either handled or thrown so that it is the responsibility of the calling method.

It is also possible to handle files in a way that adds the new text after the existing content. In that case, you might want to use the [FileWriter](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html) class.