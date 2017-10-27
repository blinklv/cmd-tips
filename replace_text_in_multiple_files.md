# Replace text in multiple files

I will use the tool used by many programmers to do this. It's [**vim**](http://www.vim.org/), a very powerful text editor.

There are some files in the current directory, as follows:

```bash
$ ls
assert.h dirent.h expat.h  libc.h
```

Every file includes *stdlib.h* header file, but you want to replace it with *mylib.h*. Yes, you can open 
each file and do it one by one, but that's stupid. Now, I tell you an easy way to solve this problem.       


**Step 1** Open a file arbitrarily and enter command line mode (press `:`).

**Step 2** Input `arg *.h`, it means I will handle all header files in the current directory.

**Step 3** 

Input `arg`, this command can display the current arglist. You will see as follows:

```bash
[assert.h] dirent.h expat.h libc.h
```

Because I open `assert.h` file, so it's enclosed in square brackets.


**Step 4** 

Input `argdo %s/<stdlib\.h>/"mylib.h"/ge | update`. If success, the output will like this:

```bash
"assert.h" 111L, 4198C written
"dirent.h" 180L, 6689C written
"expat.h" 1047L, 41806C written
"libc.h" 75L, 2013C written
```

Check all files, you will get a surprise! :laughing:


