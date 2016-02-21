l2io
====
Lineage 2 client files I/O library.

Usage
-----
```java
import acmi.l2.clientmod.io.UnrealPackage;
import java.io.File;
import java.io.IOException;
import java.nio.ByteBuffer;
import static acmi.l2.clientmod.io.BufferUtil.*;


File l2Folder = new File("C:\\Lineage 2");
File pckg = new File(new File(l2Folder, "system"), "Engine.u");
String entryName = "Actor.ScriptText";

try (UnrealPackage up = new UnrealPackage(pckg, true)) {
    UnrealPackage.ExportEntry entry = up.getExportTable()
            .stream()
            .filter(e -> e.getObjectInnerFullName().equalsIgnoreCase(entryName))
            .findAny()
            .orElseThrow(() -> new IllegalStateException("Entry not found"));
    byte[] raw = entry.getObjectRawData();
    ByteBuffer buffer = ByteBuffer.wrap(raw);
    getCompactInt(buffer); //empty properties
    buffer.getInt();       //pos
    buffer.getInt();       //top
    String text = getString(buffer);
    System.out.println(text);
}
```