# Text Searching and Manipulation

*   grep

    In a nutshell, grep56 searches text files for the occurrence of a given regular expression and outputs any line containing a match to the standard output, which is usually the terminal screen.

    ```sh
    ls -la /usr/bin | grep -i "zip" | sort**
    ```

    we listed all the files in the /usr/bin directory with ls and pipe the output into the grep command, which searches for any line containing the string “zip”. Understanding the grep tool and when to use it can prove incredibly useful.

    To use to one or more search Using `\`

    ```sh
    ls -la /usr/bin | grep -i "zip\\|ZIP\\|Zip\\|br\\|gzip"**
    ```
* split

The `split` command in Unix/Linux is used to split a file into smaller pieces. This can be particularly useful for managing large files, distributing data, or simply breaking down files into more manageable parts. Here’s an overview of how to use the `split` command along with various options:

```markdown
➜  OSCP** for i in {1..1000};do echo {$i}_Osec >> file ;done
 ➜  split 100 file new_
```

#### Basic Syntax

```bash
split [OPTION] [INPUT [PREFIX]]

```

* **OPTION:** Various options to specify how the file should be split.
* **INPUT:** The input file to split. If not specified, `split` reads from standard input.
* **PREFIX:** The prefix for the output files. The default prefix is "x".

#### Common Options

* `l LINES`: Split the file into pieces with `LINES` lines each.
* `b SIZE`: Split the file into pieces of `SIZE` bytes each. You can use suffixes like `K`, `M`, or `G` for kilobytes, megabytes, or gigabytes, respectively.
* `C SIZE`: Split the file at the specified size, but ensuring that each split part does not split any individual line.
* `d`: Use numeric suffixes instead of alphabetic. For example, `x00`, `x01`, etc.
* `a SUFFIX_LENGTH`: Use suffixes of length `SUFFIX_LENGTH`. The default is 2.

#### Examples

1.  **Split by Lines:**

    ```bash
    split -l 1000 largefile.txt

    ```

    This command splits `largefile.txt` into files each containing 1000 lines. The output files will be named `xaa`, `xab`, `xac`, etc.
2.  **Split by Bytes:**

    ```bash
    split -b 1M largefile.txt

    ```

    This splits `largefile.txt` into files each containing 1 megabyte of data. The output files will be named `xaa`, `xab`, `xac`, etc.
3.  **Split by Lines with Numeric Suffixes:**

    ```bash
    split -l 1000 -d largefile.txt

    ```

    This splits `largefile.txt` into files each containing 1000 lines, with numeric suffixes (`x00`, `x01`, `x02`, etc.).
4.  **Split with Custom Prefix:**

    ```bash
    split -l 1000 largefile.txt part_

    ```

    This splits `largefile.txt` into files each containing 1000 lines, with the prefix `part_`. The output files will be named `part_aa`, `part_ab`, `part_ac`, etc.
5.  **Split by Size with Ensuring Complete Lines:**

    ```bash
    split -C 100M largefile.txt

    ```

    This splits `largefile.txt` into files around 100 megabytes each, but ensures that no line is split across files. The output files will be named `xaa`, `xab`, `xac`, etc.
6.  **Split by Bytes with Long Suffixes:**

    ```bash
    split -b 1M -a 3 largefile.txt

    ```

    This splits `largefile.txt` into files each containing 1 megabyte of data, with suffixes of length 3 (`xaa`, `xab`, ..., `xaaa`, `xaab`, ...).

#### Practical Use Case: Splitting a Log File

Assume you have a large log file `server.log` and you want to split it into smaller parts with each part containing 500 lines:

```bash
split -l 500 server.log log_part_

```

This command creates files named `log_part_aa`, `log_part_ab`, `log_part_ac`, etc., each containing 500 lines from `server.log`.

#### Combining Split Files

To combine the split files back into the original file, you can use the `cat` command:

```bash
cat log_part_* > combined_server.log

```

#### Conclusion

The `split` command is a versatile tool for dividing large files into smaller, more manageable pieces. By using various options, you can control the size, number of lines, and naming conventions of the output files to suit your needs.



* **sed**

_**sed**_ is a powerful stream editor. It is also very complex so we will only briefly scratch its surface here. At a very high level, sed performs text editing on a stream of text, either a set of specific files or standard output. Let’s look at an example:

```sh
kali@kali:~$ echo "I need to try hard" | sed 's/hard/harder/'
I need to try harde

➜ OSCP sed 's/pass/root/' txt.txt 
root pass pass
rootwod  pass 
Pass root
```

look in this change row 1 only but can’t change Global

To Change use **`g`** ⇒ Global

```sh
➜  OSCP sed 's/pass/root/g' txt.txt 
root root root
rootwod  root 
Pass root

```

* Cut

The `cut` command is a Unix/Linux utility used to extract sections from each line of input (usually files). It's commonly used to parse and retrieve specific columns of data from text files or command output. Here's an overview of how to use the `cut` command with various options:

```markdown
echo "I hack binaries,web apps,mobile apps, and just about anything
else"| cut -f 2 -d ","
web apps
```

#### Basic Syntax

```bash
cut OPTION [FILE...]

```

If no file is specified, `cut` reads from the standard input.

#### Common Options

* `b LIST`: Select only the bytes listed in LIST.
* `c LIST`: Select only the characters listed in LIST.
* `d DELIM`: Use DELIM instead of the tab character as the field delimiter.
* `f LIST`: Select only the fields listed in LIST.
* `-complement`: Complement the selection. This option makes `cut` select all fields except the ones specified.
* `-output-delimiter=STRING`: Use STRING as the output delimiter. The default is to use the input delimiter.

#### Examples

1.  **Cut by Bytes:**

    ```bash
    echo "Hello, World!" | cut -b 1-5

    ```

    Output:

    ```
    Hello

    ```

    This extracts the first 5 bytes from the input string.
2.  **Cut by Characters:**

    ```bash
    echo "Hello, World!" | cut -c 1-5

    ```

    Output:

    ```
    Hello

    ```

    This extracts the first 5 characters from the input string.
3.  **Cut by Fields:**

    ```bash
    echo "a:b:c:d" | cut -d ':' -f 2
    ➜  OSCP echo "Hello : hi : World : ! : " | cut -f 2,3 -d ":"

    ```

    Output:

    ```
    b

    ```

    This extracts the second field from the input string, using `:` as the delimiter.
4.  **Multiple Fields:**

    ```bash
    echo "a:b:c:d" | cut -d ':' -f 1,3

    ```

    Output:

    ```
    a:c

    ```

    This extracts the first and third fields from the input string, using `:` as the delimiter.
5.  **Complement:**

    ```bash
    echo "a:b:c:d" | cut -d ':' --complement -f 2

    ```

    Output:

    ```
    a:c:d

    ```

    This extracts all fields except the second one from the input string, using `:` as the delimiter.
6.  **Change Output Delimiter:**

    ```bash
    echo "a:b:c:d" | cut -d ':' -f 1,3 --output-delimiter=','

    ```

    Output:

    ```
    a,c

    ```

    This extracts the first and third fields and changes the output delimiter to a comma.

#### Practical Use Case: Extracting Specific Columns from a CSV File

Assume you have a CSV file `data.csv` with the following content:

```
Name,Age,Gender,Location
Alice,30,Female,New York
Bob,25,Male,Los Angeles
Carol,28,Female,Chicago

```

To extract the `Name` and `Location` columns:

```bash
cut -d ',' -f 1,4 data.csv

```

Output:

```
Name,Location
Alice,New York
Bob,Los Angeles
Carol,Chicago

```

### Head

```markdown
head -n 5 file
```

### tail

```bash
tail -n 5 file
```

Output

```
{996}_Osec
{997}_Osec
{998}_Osec
{999}_Osec
{1000}_Osec
```

### awk

`awk` is a powerful programming language and command-line utility for text processing and data extraction in Unix/Linux environments. It is especially useful for working with structured text data, such as CSV files or log files. Here’s a detailed guide on how to use `awk` effectively:

#### Basic Syntax

```bash
awk 'pattern {action}' [file...]

```

* **pattern**: Specifies the condition to match.
* **action**: Specifies the commands to execute when the pattern matches.
* **file**: Specifies the input file(s) to process. If no file is provided, `awk` reads from standard input.

#### Common Usage Patterns

1.  **Print Specific Columns:**

    ```bash
    awk '{print $1, $3}' file.txt

    ```

    This command prints the first and third columns from `file.txt`.
2.  **Field Separator:**

    ```bash
    awk -F',' '{print $1, $2}' file.csv

    ```

    This command sets the field separator to a comma and prints the first and second columns from `file.csv`.
3.  **Conditional Processing:**

    ```bash
    awk '$3 > 100 {print $1, $2}' file.txt

    ```

    This command prints the first and second columns for rows where the third column is greater than 100.
4.  **Using Built-in Variables:**

    ```bash
    awk '{print NR, $0}' file.txt

    ```

    This command prints the line number (`NR`) followed by the entire line (`$0`) from `file.txt`.
5.  **Pattern Matching:**

    ```bash
    awk '/error/ {print $0}' file.txt

    ```

    This command prints all lines containing the word "error" from `file.txt`.

#### Advanced Examples

1.  **Summing a Column:**

    ```bash
    awk '{sum += $2} END {print sum}' file.txt

    ```

    This command sums the values in the second column and prints the total after processing all lines.
2.  **Average Calculation:**

    ```bash
    awk '{sum += $2; count++} END {print sum/count}' file.txt

    ```

    This command calculates and prints the average of the values in the second column.
3.  **Complex Field Separator:**

    ```bash
    awk -F '[: ]' '{print $1, $3}' file.txt

    ```

    This command sets the field separator to either a colon or a space and prints the first and third columns.
4.  **Output Formatting:**

    ```bash
    awk '{printf "Name: %s, Age: %d\\\\n", $1, $2}' file.txt

    ```

    This command formats the output with specific text and formatting.
5.  **Multi-file Processing:**

    ```bash
    awk 'FNR == 1 {print FILENAME} {print $0}' file1.txt file2.txt

    ```

    This command prints the filename before printing the contents of each file.

#### Practical Use Case: Parsing a Log File

Assume you have a log file `access.log` with lines in the format:

```
192.168.1.1 - - [01/Jan/2024:10:00:00] "GET /index.html HTTP/1.1" 200 1024

```

To extract and print the IP address, date, and status code:

```bash
awk '{print $1, $4, $9}' access.log

```

Output:

```
192.168.1.1 [01/Jan/2024:10:00:00] 200

```

#### Combining `awk` with Other Commands

`awk` can be combined with other commands using pipes. For example, to find the total number of unique IP addresses in the log file:

```bash
awk '{print $1}' access.log | sort | uniq | wc -l

```

#### If

```bash
 cat file | awk '{if ($i~/pass/) print}'
```

```bash
➜  OSCP cat /var/log/apache2/access.log | cut -f 1 -d " "  | sort | uniq -c |sort
Output
2 ::1
8 127.0.0.1
```

#### **`uniq`**

### comm

The `comm` command in Unix/Linux is used to compare two sorted files line by line and produces three-column output: lines only in the first file, lines only in the second file, and lines common to both files. It is a useful tool for identifying differences and similarities between two datasets.

#### Basic Syntax

```bash
comm [OPTION]... FILE1 FILE2

```

* **FILE1**: The first sorted file.
* **FILE2**: The second sorted file.

#### Common Options

* `1`: Suppress the first column (lines unique to FILE1).
* `2`: Suppress the second column (lines unique to FILE2).
* `3`: Suppress the third column (lines common to both files).
* `-check-order`: Check that the input files are sorted.
* `-nocheck-order`: Do not check that the input files are sorted. This is the default.

#### Examples

1.  **Basic Comparison:**

    ```bash
    comm file1.txt file2.txt

    ```

    This compares `file1.txt` and `file2.txt` and outputs three columns:

    * Lines only in `file1.txt`.
    * Lines only in `file2.txt`.
    * Lines common to both files.
2.  **Suppress First Column:**

    ```bash
    comm -1 file1.txt file2.txt

    ```

    This suppresses the first column and only shows lines unique to `file2.txt` and lines common to both files.
3.  **Suppress Second Column:**

    ```bash
    comm -2 file1.txt file2.txt

    ```

    This suppresses the second column and only shows lines unique to `file1.txt` and lines common to both files.
4.  **Suppress Third Column:**

    ```bash
    comm -3 file1.txt file2.txt

    ```

    This suppresses the third column and only shows lines unique to `file1.txt` and lines unique to `file2.txt`.
5.  **Find Common Lines Only:**

    ```bash
    comm -12 file1.txt file2.txt

    ```

    This suppresses the first and second columns, showing only lines common to both files.
6.  **Find Unique Lines in Both Files:**

    ```bash
    comm -3 file1.txt file2.txt

    ```

    This suppresses the third column, showing only lines unique to `file1.txt` and lines unique to `file2.txt`.

#### Practical Use Case: Comparing Lists

Assume you have two files, `list1.txt` and `list2.txt`, each containing a list of items.

**list1.txt:**

```
apple
banana
cherry
date

```

**list2.txt:**

```
banana
cherry
fig
grape

```

To find items that are in both lists:

```bash
comm -12 list1.txt list2.txt

```

Output:

```
banana
cherry

```

To find items that are only in `list1.txt`:

```bash
comm -23 list1.txt list2.txt

```

Output:

```
apple
date

```

To find items that are only in `list2.txt`:

```bash
comm -13 list1.txt list2.txt

```

Output:

```
fig
grape

```

#### Sorting Before Comparison

The `comm` command requires the input files to be sorted. If the files are not sorted, you can sort them before using `comm`:

```bash
sort file1.txt -o file1_sorted.txt
sort file2.txt -o file2_sorted.txt
comm file1_sorted.txt file2_sorted.txt

```

Alternatively, you can use pipes to sort and compare in one command:

```bash
comm <(sort file1.txt) <(sort file2.txt)

```

#### Conclusion

The `comm` command is a simple yet powerful tool for comparing two sorted files line by line. By using various options, you can customize the output to focus on unique or common lines, making it easier to analyze differences and similarities between datasets.

### diff

The `diff` command in Unix/Linux is used to compare the contents of two files line by line. It outputs the differences between the files in a format that can be used to create patches or to understand the changes between the two versions of a file. Here's a detailed guide on how to use the `diff` command along with various options:

#### Basic Syntax

```bash
diff [OPTION]... FILES

```

* **FILES**: Two files to be compared.

#### Common Options

* `u` or `-unified`: Produces a unified format diff with a few lines of context. This is the most commonly used format.
* `c` or `-context`: Produces a context format diff with a few lines of context.
* `i` or `-ignore-case`: Ignores case differences in file contents.
* `w` or `-ignore-all-space`: Ignores all white space.
* `B` or `-ignore-blank-lines`: Ignores changes that just insert or delete blank lines.
* `r` or `-recursive`: Recursively compares any subdirectories found.
* `q` or `-brief`: Outputs only whether files differ, not the details of the differences.

#### Examples

1.  **Basic Comparison:**

    ```bash
    diff file1.txt file2.txt

    ```

    This compares `file1.txt` and `file2.txt` and outputs the differences.
2.  **Unified Format:**

    ```bash
    diff -u file1.txt file2.txt

    ```

    This compares the files and outputs the differences in the unified format, which shows a few lines of context around the changes.
3.  **Context Format:**

    ```bash
    diff -c file1.txt file2.txt

    ```

    This compares the files and outputs the differences in the context format.
4.  **Ignore Case Differences:**

    ```bash
    diff -i file1.txt file2.txt

    ```

    This ignores case differences when comparing the files.
5.  **Ignore All White Space:**

    ```bash
    diff -w file1.txt file2.txt

    ```

    This ignores all white space when comparing the files.
6.  **Ignore Blank Lines:**

    ```bash
    diff -B file1.txt file2.txt

    ```

    This ignores changes that only involve blank lines.
7.  **Recursive Comparison:**

    ```bash
    diff -r dir1 dir2

    ```

    This recursively compares directories `dir1` and `dir2`.
8.  **Brief Output:**

    ```bash
    diff -q file1.txt file2.txt

    ```

    This outputs only whether the files differ, not the details of the differences.

#### Interpreting the Output

The default output format of `diff` can be a bit cryptic at first. Here's how to interpret it:

```
1,3c1,3
< line1
< line2
< line3
---
> line1
> line2 changed
> line3

```

* **1,3c1,3**: Indicates that lines 1-3 in the first file are changed in lines 1-3 in the second file.
* Lines prefixed with `<` are from the first file.
* Lines prefixed with `>` are from the second file.
* `--` separates the lines from the two files.

#### Practical Use Case: Creating a Patch

You can use `diff` to create a patch file that contains the differences between two files. This patch file can then be applied to the original file to update it.

**Creating a Patch:**

```bash
diff -u original.txt modified.txt > patch.diff

```

**Applying the Patch:**

```bash
patch original.txt < patch.diff

```

#### Conclusion

The `diff` command is a powerful tool for comparing files and directories. By understanding its options and output, you can effectively identify and manage differences between file versions. This is particularly useful in software development for tracking changes and creating patches.

### vimdiff

`vimdiff` is a powerful tool that uses the Vim text editor to display the differences between two or more files side by side. It highlights the differences and allows you to interactively merge and edit files. Here's a detailed guide on how to use `vimdiff`:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0cf0a799-143b-40ad-bd72-1de5eb986a7f/fa8ca18e-9b96-462a-8a84-90b10dbf7077/Untitled.png)

#### Basic Usage

To compare two files:

```sh
vimdiff file1.txt file2.txt

```

To compare three files:

```bash
vimdiff file1.txt file2.txt file3.txt

```

To compare more files (up to four files), simply list them all in the command.

#### Navigating Differences

When you open files with `vimdiff`, each file is shown in a separate window. The differences between the files are highlighted. Here are some common commands for navigating and managing differences in `vimdiff`:

* `]c`: Jump to the next change.
* `[c`: Jump to the previous change.
* `do` or `:diffget`: Get (copy) the changes from the other file into the current file.
* `dp` or `:diffput`: Put (copy) the changes from the current file into the other file.
* `:diffupdate`: Manually update the differences.
* `:diffoff`: Turn off the diff mode.
* `:diffthis`: Turn on the diff mode for the current window.
* `zo` / `zc`: Open/close folded text.

#### Editing and Merging

In `vimdiff`, you can edit any of the files just like in Vim. The changes will be reflected immediately, and the differences will be updated. Here are some useful commands for editing and merging:

*   **Copy Changes from One File to Another:** Place the cursor on the change you want to copy, then use:

    ```bash
    do

    ```

    to copy the change from the other file into the current file.
*   **Copy Changes from Current File to Another:** Place the cursor on the change you want to copy, then use:

    ```bash
    dp

    ```

    to copy the change from the current file to the other file.

#### Customizing the Diff Display

You can customize how `vimdiff` displays differences by modifying your `.vimrc` configuration file. Here are some options:

*   **Set the Number of Context Lines:**

    ```
    set diffopt+=context:3

    ```

    This sets the number of context lines to 3.
*   **Highlight Differences with Custom Colors:**

    ```
    highlight DiffAdd    ctermfg=NONE ctermbg=LightBlue
    highlight DiffChange ctermfg=NONE ctermbg=LightMagenta
    highlight DiffDelete ctermfg=NONE ctermbg=LightCyan

    ```

    This sets custom colors for added, changed, and deleted lines.

#### Using `vimdiff` with Git

`vimdiff` is particularly useful when used as a merge tool in version control systems like Git. To set `vimdiff` as the default diff tool in Git, you can use the following command:

```bash
git config --global diff.tool vimdiff

```

To set `vimdiff` as the default merge tool in Git, use:

```bash
git config --global merge.tool vimdiff

```

You can then use `git difftool` and `git mergetool` to resolve conflicts with `vimdiff`.

#### Practical Example

Suppose you have two files, `file1.txt` and `file2.txt`, with the following contents:

**file1.txt:**

```
apple
banana
cherry
date

```

**file2.txt:**

```
apple
banana
citrus
date

```

Running `vimdiff file1.txt file2.txt` will open Vim with both files side by side, highlighting "cherry" in `file1.txt` and "citrus" in `file2.txt` as the differing lines. You can then navigate to the differences and use the `do` or `dp` commands to merge the changes as needed.

#### Conclusion

`vimdiff` is a powerful and flexible tool for comparing and merging files. By leveraging Vim's extensive capabilities, you can efficiently navigate, edit, and resolve differences between files. Whether you are comparing simple text files or resolving complex merge conflicts in a version control system, `vimdiff` provides a robust solution for managing differences.
