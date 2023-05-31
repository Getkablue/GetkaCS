This assignment does not require any readings, but the computing review might help.

I assume here that you are on Windows. You do not need to provide any solutions for this assignment. Instead, focus on getting the development environment set up on your PC.

1. Go to python.org and download the most recent stable Python version. Open up a shell (like PowerShell) and run "python --version" to verify that it is properly installed. If you get an error along the lines of "python is not a recognizable command", then you probably need to select "Add to PATH" in the installer.

When Python is installed properly, copy-paste the following Python code into a new file and run that file with python.
``` python
print("Hello world!")
```

```PowerShell
python helloworld.py
```

If you get any errors, get to work googling or see me.

2. Go to Microsoft's webpage and get the Community edition of visual studio (Any version 2015 or newer is fine). It should include "MSVC Build Tools", but make sure that you get them if it doesn't. Also install the Desktop C++ environment if prompted.

When Visual Studio is installed, create a new C++ project (select command-line/terminal project, or alternatively "blank project", when given the chance -- don't make a Windows MFC app or anything like that). Paste in the following code and hit F5. The program should build and then run (depending on your environment, it might close right away, but you should know if you receive an error).

``` cpp
#include <iostream>
int main()
{
  std::cout << "Hello, World!" << std::endl;
  return 0;
}
```
You should be able to also compile a C program containing the following:

```c
#include <stdio.h>
int main() {
	printf("Hello world!\n");
	return 0;
}
```


Again, if you see any errors, get to googling or see me. One helpful resource: https://devblogs.microsoft.com/cppblog/cpp-tutorial-hello-world/

