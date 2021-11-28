# Data Science at the Command Line [E-book](https://www.datascienceatthecommandline.com/2e/chapter-3-obtaining-data.html)

## Five Types of Command-line Tools
* binary executable
* shell built-in
* interpreted script
* function
* alias (no param)

```bash
# lookup the type of something
type -a ls
```

* look for help
```bash
man cat
help cd
cd --help
```

## CURL
* get raw response
* `-s` to silence progress bar
* `-I` to print response header (useful for debugging)
* `-O` save output to a file
* `-L` follow redirect
* `-u` specify username

## Tar
* `-x` extract
* `-t` view content without extract
* `-z` gzip
* `-f` file

```bash
# view content
tar -tzf logs.tar.gz

mkdir logs

tar -xzf logs.tar.gz -C logs
```

## Send output to File
```bash
# new file
echo num > file.txt

# append to existing file
echo num >> file.txt
```

## Sed - Stream Editor
* `sed 's/t/T/'` replace lowercase t with uppercase, first instnace of every line
* `sed 's/t/T/g'` replace **all** lowercase t with uppercase
* `sed -i 's/t/T/g' text.txt` modifies original file in place
* `sed -e '/^$/d'` deletes empty line

## Awk
Treat each words (space separated) in a line as a field. Refers to as $1, $2 etc
* `awk '{print}' file.txt`
* `awk '{print $1}' file.txt`
* `awk '/^w/ {print $1}' file.txt`: print line starting with w, can do any regular expression
* `awk '/^w/ {print $1, $2/1024"K"}' file.txt`: process the second column
* `awk '/path/ && $2 > 15000 {print $1}' file.txt`: requires regex match, and second col > 1500
* `awk script.awk file.txt`: running script
* an awk script, input is one number each line, print every number no larger than the number

```awk
func printlist(n) {
  for(i=1;i<=n;i++) {
    printf("%f ",i)
  }
  printf("\n")
}

{printlist($1)}
```

## Filter by Rows
```bash
seq -f "Line %g" 10 | tee lines

# get top 3 lines
< lines head -n 3
< lines sed -n '1,3p'
< lines awk 'NR <= 3'

# get last 3 lines
< lines tail -n 3

# avoid top 3 lines
< lines tail -n +4
< lines sed '1,3d'
< lines sed -n '1,3!p'
< lines head -n -3

# get line 4, 5, 6
< lines sed -n '4,6p'
< lines awk '(NR>=4) && (NR<=6)'
< lines head -n 6 | tail -n 3

# get odd line
< lines sed -n '1~2p'
< lines awk 'NR%2'

# get even line
< lines sed -n '0~2p'
< lines awk '(NR+1)%2'
```

## Filter by Pattern
```bash
# case-insensitive
< alice.txt grep -i chapter

# use -E to interpret regex, otherwise it's used as literal string
< alice.txt grep -E '^CHAPTER (.*)\. The'

# use -v to invert
< alice.txt grep '^CHAPTER (.*)\. The'

# count non-empty lines
< alice.txt grep -Ev '^\s$' | wc -l
```

## Sample
```bash
# sample by percentage
seq -f "Line %g" 1000 | sample -r 1%
seq -f "Line %g" 1000 | sample -r 0.01

# use -d to delay
# use -s to specify running for how long
# ts add timestamp to the front
seq -f "Line %g" 1000 | sample -r 1% -d 1000 -s 5 | ts
```

## Extract Value from Text
```bash
# get first three fields splitted by space
grep -i chapter alice.txt | cut -d ' ' -f -3

# get the third field
grep -i chapter alice.txt | cut -d ' ' -f 3

# get the 3rd to the laast
grep -i chapter alice.txt | cut -d ' ' -f 3-

# get the first character to the last (idx starts with 1)
grep -i chapter alice.txt | cut -c 1-

# break every match into new line
< alice.txt grep -oE '\w{2,}'

# replace space with underscore
echo 'hello world!' | tr ' ' '_'
echo 'hello world!' | tr ' !' '_?'

# delete space and !
echo 'hello world!' | tr -d ' !'

# only keep letters
echo 'hello world!' | tr -d -c '[a-z]'

# convert to uppercase
echo 'hello world!' | tr '[a-z]' '[A-Z]'
echo 'hello world!' | tr '[:lower:]' '[:upper:]'

echo 'h ello     world!' | \
  sed -re 's/hello/bye/' | \
  sed -re 's/\s+/ /g' | \
  sed -re 's/\s+//'

csvsql --query 'SELECT i.sepal_length, i.sepal_width, i.species, m.usda_id FROM iris i JOIN irismeta m ON (i.species = m.species)' iris.csv irismeta.csv
```

## VIM
Three modes:
* `i`: insert
* `v`: visual
* `:`: command
Shorthand:
* Move curser aroound: `HJKL`
* `x`: delete character from right (in command mode)
* `r`: replace current cursor letter with something else (command mode)
* `dd`: delete entire line
* `u`: undo
* `:set number`: add line number
* `:n`: move to line N
* `+p`: paste from system clipboard
* `:w`; save
* `:!node hello.js`: run file
* `\regex`: search text
* `a`: enter edit mode, insert at right of cursor
  - `SHIFT A`: edit at end of line
* `i`: enter edit mode, insert at left of cursor
  - `SHIFT I`: edit at beginning of line

## Create Commandline Tool
1. Copy and paste the one-liner into a file.
2. Add execute permissions.
3. Define a so-called shebang.
4. Remove the fixed input part.
5. Add a parameter.
6. Optionally extend your PATH.

## Project Management with *Make*
Pros of Make
* Formalize your data workflow steps in terms of input and output dependencies.
* Run specific steps of your workflow.
* Use inline code.
* Store and retrieve data from external sources.

How it works
* search for a `Makefile` in current directory (one per project)
* directly call `make ${target}` to build the project
  - if no target is specified, the first target is executed
* a target is like a task, usually named of the output file
* `rule` specifies how a task is completed. Rule is preceded by a tab (not spaces)
* use `$@` to output to a file named after task
* if an output file exists, the task will not rerun, regardless of who generated the file, or whether the file is up-to-date!
* use `.PHONY` to mark tasks that are **not** represented by file. So these tasks will be executed regardless of whether the file exists
* dependencies (files) are specified after `:` following task name
```bash
numbers:
  seq 7 > $@
```

## Parallel Processing
* `parallel` is run for every line of input
* use placeholder `{}` to hold entire line `seq 3 | parallel cowsay {} > /dev/null`
* By default, parallel runs one job per CPU core
* common params:
  - `-j`: number of job, 1 is serial, 0 is as many as possible (not good)
  - `--results outputdir`: each job creates 3 files: job ID, stdout, stderr
  - `--keep-order`
  - `--tag`: prepends each line with the input item.
* distributed processing:
  - `parallel` doesn’t have to be installed on the remote machine
  - `ssh` submit command to remote machine
  - `~/.ssh/config.` save credential to ssh into remote
```bash
for i in {0..100..2}
do
echo "$i^2" | bc
done | trim

while read line
do
echo "Sending invitation to ${line}."
done < emails  

< emails while read line
do
echo "Sending invitation to ${line}."
done

while read line; do echo "You typed: ${line}."; done < /dev/stdin

for chapter in /data/*
do
echo "Processing Chapter ${chapter}."
done

# lists all files located under the directory /data that have csv as extension
# and are smaller than 2 kilobyte:
find /data -type f -name '*.csv' -size -2k

# save parallel job otput to one big file
seq 5 | parallel "echo Hi {}" >> one-big-file.txt
```

## Use Cases
* `sort | uniq -c`: sort lines and count **streak** duplicates.
* `wc -l file.txt`: count line in file
* `sed 's/ /\n/g' words.txt | grep -v '^$' | sort | uniq -c | sort -r | awk '{print($2 " " $1)}'`: count word frequency
  - `sed 's/ /\n/g' words.txt | sed '/^$/d' | sort | uniq -c | sort -r | awk '{print($2 " " $1)}'`
* valid phone number (leetcode): `sed -n -E '/^(\([0-9]{3}\) |[0-9]{3}-)[0-9]{3}-[0-9]{4}$/p' file.txt`
  - the `-n` does not print input
  - `grep '^\(([0-9]\{3\}) \|[0-9]\{3\}-\)[0-9]\{3\}-[0-9]\{4\}$'`: need to escape `{}`, `()`
* print PATH variable: `echo $PATH | tr ':' '\n'`
* example in chapter 4: https://www.datascienceatthecommandline.com/2e/chapter-4-creating-command-line-tools.html
```bash
curl -sL "https://www.gutenberg.org/files/11/11-0.txt" | \
tr '[:upper:]' '[:lower:]' | \
grep -oE "[a-z\']{2,}" | \
sort | \
grep -Fvwf stopwords |  \
uniq -c | \
sort -nr | \
head -n 10
```
