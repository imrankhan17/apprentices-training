## Shell

The shell is an efficient and incredibly powerful textual interface to your computer.  It lets you run programs and commands.

Terms used for pretty much the same thing:
* shell
* shell prompt
* terminal
* command line
* bash shell

### Basic commands

The terminal allows you work with the files on your computer.  These files are stored in a hierarchy of directories.

When we first open a terminal, our working directory (the current folder you are in) will be our "home" directory.  This is the default root directory for your user.

We can see what directory we are in by entering the `pwd` command.  This stands for **p**rint **w**orking **d**irectory.

To list the files in this directory we use the `ls` command (*list*).

To move into another directory we use the `cd` command (**c**hange **d**irectory) followed by the name of that directory.
```bash
cd Documents
```

I am now in the `Documents` directory.  We can confirm this by using `pwd` again.

At this point, let's locate the directory in which you downloaded the `apprentices-training` folder.  For me it was in `/Users/khani26/Documents/other_projects/apprentices`.
```bash
cd /Users/khani26/Documents/other_projects/apprentices

pwd

ls
```

If I wanted to go back a directory I can enter the full path or use relative directories.
```bash
cd ..

pwd
```

We can also go back to multiple stages in the directory structure.
```bash
# go back to the folder
cd apprentices

cd ../..

pwd
```

The current directory is represented by a single dot `.`
```bash
pwd

cd .

pwd
```

Running `cd` with no arguments will take you to your home directory.
```bash
pwd

cd

pwd
```

**Tip:** use the `clear` command to clear the screen.  All previous commands are still available if you scroll up.

We can add optional arguments to our shell commands using dashes (`-`) followed by particular letters.
```bash
ls -l
```

The `-l` option for `ls` lists all the files in a long format with additional details.  
The `-a` option lists all files including hidden files that start with a `.`.  

We can have multiple options by listing them one after the other.
```bash
ls -l -a

ls -la

ls -lah

ls -laht
```

We can view the contents of files using the `cat` command.
```bash
cd session4/files

ls

cat random.txt
```

We can use the `head` and `tail` commands to view portions of files.
```bash
head data.csv

tail data.csv

head -n 3 data.csv
```

**Tip:** you can use the tilde, `~`, as a shortcut for your home directory.

We can copy and move files between directories using `cp` and `mv`.
```bash
cp data.csv data_copy.csv

ls

mv data_copy.csv ~/Desktop

ls

ls ~/Desktop
```

To rename a file, we can use `mv` within the same directory.
```bash
mv random.txt random_renamed.txt

ls
```

We can create files using the `touch` command.
```bash
touch new_file.txt

ls -l

cat new_file.txt

wc new_file.txt
```

We can print text to the console using the `echo` command.
```bash
echo hello

echo this is a whole sentence

echo "this is a whole sentence"
```

We can redirect the output of a command to a file using the `>` operator.
```bash
echo "this is a whole sentence" > new_file.txt

cat new_file.txt
```

Using the `>>` operator will append the output to the file.
```bash
echo "this is some more text" >> new_file.txt

cat new_file.txt

echo "this is some more text" >> new_file.txt

cat new_file.txt
```

**Tip:** use the up and down arrow keys to view commands you ran previously.  Also try the `history` command.

To remove files we use the `rm` command.  Be careful since this deletes files permanently.
```bash
ls

rm new_file.txt

ls
```

We can create directories using the `mkdir` command.
```bash
cd ..

pwd

ls -l

mkdir my_folder

ls -l

cd my_folder

ls -l
```

To remove directories we need to use the recursive flag.
```bash
cd ..

ls -l

rm my_folder

rm -r my_folder

ls -l
```

We can calculate information about files using the `wc` command.
```bash
cd files

wc data.csv

wc -l data.csv
```

We can use the `>` operator to create new files from old files.
```bash
head -n 3 data.csv > data_top3_rows.csv

ls -l

cat data_top3_rows.csv
```

We can use wildcards to list specific files only.
```bash
ls -l

ls -l *.csv
```

We can set variables using the `$` character.
```sql
animal="Tiger"

echo $animal

echo "i have a $animal"
```

There are some in-built variables within the shell like `$USER`.  This is also the same as `whoami`.
