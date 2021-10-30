# Java Native Interface Study
 
## How to use JNI

Take JNI_c as an example:
-  Write a Java Class HelloJNI.java that uses C Codes
```java
public class HelloJNI {  // Save as HelloJNI.java
   static {
      System.loadLibrary("hello"); // Load native library hello.dll (Windows) or libhello.so (Unixes)
                                   //  at runtime
                                   // This library contains a native method called sayHello()
   }
 
   // Declare an instance native method sayHello() which receives no parameter and returns void
   private native void sayHello();
 
   // Test Driver
   public static void main(String[] args) {
      new HelloJNI().sayHello();  // Create an instance and invoke the native method
   }
}
```
-  Compile the Java Program HelloJNI.java & Generate the C/C++ Header File HelloJNI.h.Starting from JDK 8, you should use "javac -h" to compile the Java program AND generate C/C++ header file called HelloJNI.h as follows:
> javac -h . HelloJNI.java
-  Implementing the C Program HelloJNI.c
```c
// Save as "HelloJNI.c"
#include <jni.h>        // JNI header provided by JDK
#include <stdio.h>      // C Standard IO Header
#include "HelloJNI.h"   // Generated
 
// Implementation of the native method sayHello()
JNIEXPORT void JNICALL Java_HelloJNI_sayHello(JNIEnv *env, jobject thisObj) {
   printf("Hello World!\n");
   return;
}
```
- Compile the C program HelloJNI.c
  - (Windows) 64-bit JDK
  > x86_64-w64-mingw32-gcc -I"%JAVA_HOME%\include" -I"%JAVA_HOME%\include\win32" -shared -o hello.dll HelloJNI.c
  
  - (Windows) 32-bit JDK [Obsolete?]
  > i886-w64-mingw32-gcc -Wl,--add-stdcall-alias -I"%JAVA_HOME%\include" -I"%JAVA_HOME%\include\win32" -shared -o hello.dll HelloJNI.c
  
  - (Ubuntu) 64-bit JDK
  ```shell
   export JAVA_HOME=/your/java/installed/dir
   echo $JAVA_HOME
   gcc -fPIC -I"$JAVA_HOME/include" -I"$JAVA_HOME/include/linux" -shared -o libhello.so HelloJNI.c
   java -Djava.library.path=. HelloJNI
  ```
  
  - (Mac OS X) 64-Bit JDK
  ```shell
  export JAVA_HOME=/your/java/installed/dir
  echo $JAVA_HOME
  gcc -I"$JAVA_HOME/include" -I"$JAVA_HOME/include/darwin" -dynamiclib -o libhello.dylib HelloJNI.c
  java -Djava.library.path=. HelloJNI
  ```
