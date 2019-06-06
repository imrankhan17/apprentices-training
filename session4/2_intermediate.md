## More command line

Let's start by navigating to the project directory.
```bash
pwd

cd Documents/other_projects/apprentices/

pwd

ls -la
```

**Tip:** if you accidentally change directory and want to go back to where you are, use `cd -`.

#### Editing files using a text editor

The terminal has an in-built text editor called Vim (or Vi).
```bash
# create and open a new file
vim new_file.txt
```

Vim has a few basic modes.  To edit the file, use `i`.  You should see `-- INSERT --` at the bottom to indicate you are in insert mode.

To exit insert mode, hit `esc`.

Type `:q` to quit or `:wq` to save the file and quit.

```bash
cat new_file.txt
```

#### Getting help

We can use the `man` command followed by any other command to bring up a detailed manual.
```bash
man ls

man tail
```

#### Aliases

You can create shortcuts for commonly used commands.
```bash
alias ll='ls -laht'

ll

alias ll
```

To persist your aliases, you can save them in a file called `.bash_profile`.  This is basically your terminal start-up script.
```bash
vim .bash_profile
```

#### Searching for files

```bash
man find

# recursively list every directory and file in the current directory
find .

find . -type d

find . -type f -name "1_intro.md"

find . -type f -name "*.md"

find . -type f -name "*.csv"

find . -type f -name "*.jpeg"

# case sensitive
find . -type f -name "readme.md"

# case insensitive
find . -type f -iname "readme.md"

# search for files by modification time
find . -mtime -1

# search for files by file size
find . -size +200k

find . -type f -size +50M
```

**Tip:** use the `open` command to open files in its default program e.g. `open data.csv`.

#### Searching within files

```bash
man grep

cd session4/files

head data.csv

grep "Taffy" data.csv

grep "Michael" data.csv

grep "Dar" data.csv

# match whole words only
grep -w "Dar" data.csv

# case insensitive
grep -i "dar" data.csv

# also include lines before and after the match
grep -B 4 "Taffy" data.csv

grep -A 4 "Taffy" data.csv

grep -C 4 "Taffy" data.csv

# search in multiple files
grep -i "sit" .

grep -i "sit" *

cd ../..

grep -ir "sit" *

grep -irl "sit" *

grep -irc "sit" *
```

#### Piping

We can send the output of one command into the input of another command using the pipe, `|` operator.
```bash
wc -l data.csv

grep "Female" data.csv | wc -l
```

### Manipulating data

So far we've used `cat`, `head`, `tail` and `grep` to view and filter the contents of files.  We can `cut` to select columns from a file.
```bash
man cut

# select the 1st column
cut -d , -f 1 data.csv

# select multiple columns
cut -d , -f 2,3 data.csv

cut -d , -f 2,3 data.csv | head

cut -d , -f 2-4 data.csv | head
```

We can use `sort` to order the file.
```bash
sort data.csv| head

cut -d , -f 2 data.csv | sort

cut -d , -f 2 data.csv | sort -f

cut -d , -f 2 data.csv | sort -r

cut -d , -f 5 data.csv | sort

cut -d , -f 5 data.csv | sort -u

cut -d , -f 5 data.csv | sort | uniq -c

cut -d , -f 4 data.csv | grep ".co.uk" | wc -l

cut -d , -f 2 data.csv | sort > names.csv
```

**Tip:** you can skip the first line of a file with e.g. `tail -n +2 data.csv`.

There are many other file manipulation commands - check out `awk`, `sed`, `tr`, `paste` , `join` etc.

#### File permissions

Every file and directory has a set of different permissions - read, write and execute and there are three types of users - owner, group and other.

The total permissions of each file can be summarised by a single 3 digit code.

| permission | shortcode | digit |
|------------|-----------|-------|
| read       | r         | 4     |
| write      | w         | 2     |
| execute    | x         | 1     |

```bash
ls -l

stat -f "%OLp" 0_installation.md

# change permissions of a file
chmod 600 0_installation.md

stat -f "%OLp" 0_installation.md
```

#### Other stuff

```bash
# current timestamp
date

# sleep for some time
sleep 2

# run one command after the other if it doesn't fails
sleep 2 && ls

# get list of current processes
ps -ef

# kill a running process
kill {id}

# explore your files in the browser
python -m SimpleHTTPServer
# -> go to localhost:8000 in the browser
```

#### Writing scripts

We can string together many shell commands into a script.  These commands are written in a file with a `.sh` extension.
```bash
cd session4 

vim new_script.sh

echo "Starting script..."
cd files
wc -l data.csv
echo "Script has finished."
```

To run the script you need to reference the file path.
```bash
./new_script.sh

stat -f "%OLp" new_script.sh

chmod 744 new_script.sh

./new_script.sh
```
