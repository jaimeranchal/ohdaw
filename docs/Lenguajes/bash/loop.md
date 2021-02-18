# Bucles en archivos

!!! info "Fuente"
    - [digitalocean.com](https://www.digitalocean.com/community/tutorials/workflow-loop-through-files-in-a-directory)

## Step 1 — Looping Through Files

You are going to use the `for` shell scripting loop in this tutorial. This particular control flow method is baked into the Bash or zsh command-line shell.

You can apply the following commands to any directory of your choosing, but for this tutorial, create a directory and some files to play around with.

First, create the directory:

```bash
mkdir looping
```

Then switch to the new directory:

```bash
cd looping
```

Now use the `touch` command to create a few text files:

```bash
touch file-1.txt
touch file-2.txt
touch file-3.txt
touch file-4.txt
touch file-5.txt
```

You can also create these files quickly using brace expansion and a range:

```bash
touch file-{1..5}.txt
```

To loop through a directory, and then print the name of the file, execute the following command:

```bash
for FILE in *; do echo $FILE; done
```

You’ll see the following output:

```
Outputfile-1.txt
file-2.txt
file-3.txt
file-4.txt
file-5.txt
```

You probably noticed we’re using the wild card character, `*`, in there. That tells the `for` loop to grab every single file in the directory. You could change the wild card could to `file-*` to target all of the files that started with `file-`, or to `*.txt` to grab just the text files.

Now that you know how to loop through the files in a directory and spit out the names, let’s use those files with a command.


## Step 2 — Applying a Command

The files you created in the previous section were all created with the `touch` command and are empty, so showing out how to `cat` each file wouldn’t be very useful.

Fortunately, using a similar `for` loop, you can insert text into each file with a single command.

Execute the following command to insert the file’s name, followed by a newline, followed by the text `Loops Rule!` into each file:

```bash
for FILE in *; do echo -e "$FILE\nLoops Rule\!" > $FILE; done
```

The `-e` flag on `echo` ensures that newlines are preserved.

The exclamation point needs to be escaped with a backslash so the shell doesn’t interpret the character as a shell command.

Now iterate through each file and print its contents:

```bash
for FILE in *; do cat $FILE; done
```

Now each file contains the name of the file on the first line, and a universal truth about loops on the second line.

```
Outputfile-1.txt
Loops Rule!
file-2.txt
Loops Rule!
file-3.txt
Loops Rule!
file-4.txt
Loops Rule!
file-5.txt
Loops Rule!
```

To take it a step further, you could combine these examples to first write to the file, then display its contents in a single loop:

```bash
for FILE in *; do echo -e "$FILE\nLoops Rule\!" > $FILE; cat $FILE; done
```

You’ll see the following output:

```
Outputfile-1.txt
Loops Rule!
file-2.txt
Loops Rule!
file-3.txt
Loops Rule!
file-4.txt
Loops Rule!
file-5.txt
Loops Rule!
```

By separating the commands with a semi-colon, `;`, you can string together whichever commands you need.

Now let’s look at a practical example: making backup copies of files.


## Step 3 — Making Backups of Files

Now that you know how the `for` loop works, let’s try something a bit more real world by taking a directory of files and making backups that are suffixed with the `.bak` extension.

Execute the following command in your shell which creates a backup for each file:

```bash
for FILE in *; do cp $FILE "$FILE.bak"; done;
```

Now use the `ls` command to display each file:

```bash
ls -l
```

You’ll see the following output:

```
Outputtotal 40K
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:34 file-1.txt
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:41 file-1.txt.bak
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:34 file-2.txt
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:41 file-2.txt.bak
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:34 file-3.txt
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:41 file-3.txt.bak
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:34 file-4.txt
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:41 file-4.txt.bak
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:34 file-5.txt
-rw-r--r-- 1 sammy sammy 29 Nov  7 18:41 file-5.txt.bak
```

You now have exact copies of each of your files, with an extension that indicates that they are backup files.

You’re not limited to making copies in the same directory either; you can specify a new path for your backup files. The following command stores the backups in the folder `/tmp/my-backups`, provided the directory already exists:

```bash
for FILE in *; do cp $FILE "/tmp/my-backups/$FILE.bak"; done;
```

The backups are created in the new location.
