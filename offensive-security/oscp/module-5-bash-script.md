---
description: Black Hat Bash book
---

# Module 5 (Bash Script)



<figure><img src="../../.gitbook/assets/image (258).png" alt=""><figcaption></figcaption></figure>

### Intro to Bash Scripting&#x20;

```bash
#!/bin/bash

VAR="world"
echo "Hello $VAR!" # => Hello world!

```

> ```markup
> #!/bin/bash => To Know this script run in env /bin/bash
> ```

**Shell Scripting – Shell Variables**

* **Valid Variable Names**

```
ABC
_AV_3
AV232
```

* **Invalid variable names**&#x20;

```
2_AN
!ABD
$ABC
&QAID
```

<figure><img src="../../.gitbook/assets/image (257).png" alt=""><figcaption></figcaption></figure>

### Debugging in Bash

Errors will inevitably occur when you’re developing bash scripts. Luckily, debugging the script is quite intuitive. An easy way to check for mistakes early is by running the script using the `-n` parameter. This parameter will read the commands in the script but won’t execute them**, so if there are any syntax errors,** they will be shown on the screen. you can think of it as a dry-run method to test validity of syntax:

> bash -n script.sh

ipt.sh You can also use the `-x` parameter to turn on verbose mode

> bash -x script.sh

&#x20;If you want to start debugging at a given point in the script, you can do this by including the set command in the script itself.

> \#!/bin/bash&#x20;
>
> <mark style="color:red;">set -x</mark>&#x20;
>
> \--snip--
>
> &#x20;<mark style="color:red;">set +x</mark>

### Variables

The following rules govern the naming of bash variables:&#x20;

&#x20;• They can include alphanumeric characters.

&#x20;• They cannot start with a numerical character.

&#x20;• They can contain an underscore (\_).&#x20;

&#x20;• They cannot contain whitespace.

```bash
  book="black hat bash"
  echo "This book's name is ${book}"
```

**Unassigning Variables**

```
$book="Black hat bash"
unset $book
echo "$(book)"
```

**Scoping Variables**

<mark style="color:blue;">**Global**</mark> variables are those available to the entire program. But variables in bash can also be <mark style="color:blue;">**scoped**</mark> so that they are only accessible from within a certain block of code. These variables are called local variables and are declared using the <mark style="color:blue;">**local**</mark> keyword. The following script shows how <mark style="color:blue;">**local**</mark> and global variables work:

```bash
#!/bin/bash
PUBLISHER="No Starch Press" #Global variable
print_name(){
local name #local can't print this variable out of this function 
name="Black Hat Bash"
echo "${name} by ${PUBLISHER}"
}
print_name
echo "The variable name =  ${name} will not be printed because it is a local variable."
```

> remembering to set the executable permission using chmod, and run it using the following command: <mark style="color:red;">./local\_scope\_variable.sh</mark>
>
> <mark style="color:blue;">chmod</mark> +x script.sh => -x to be execution&#x20;

> <mark style="color:red;">**Note**</mark>&#x20;
>
> Use <mark style="color:red;">**`/dev/nul`**</mark>l with caution. You may miss out on important errors if you choose to redirect output to it. When in doubt, redirect standard streams such as standard output and standard error to a dedicated log file instead.



| Operator | Description |
| -------- | ----------- |

| +  | Addition                   |
| -- | -------------------------- |
| -  | Subtraction                |
| \* | Multiplication             |
| /  | Division                   |
| %  | Modulo                     |
| += | Incrementing by a constant |
| -= | Decrementing by a constant |

**Arrays**

```bash
#!/bin/bash
#set array
IP_ADDR=(192.168.1.1. 192.168.1.2 192.168.1.3 192.168.2.1 192.168.2.3)
#Print all element in array
echo "${IP_ADDR[*]}"
#print only the first element 
echo "${IP_ADDR[0]}"
```

### Control Operators

<table data-header-hidden><thead><tr><th width="121"></th><th></th></tr></thead><tbody><tr><td>Operator</td><td>Description</td></tr><tr><td>&#x26;</td><td>Sends a command to the background.</td></tr><tr><td>&#x26;&#x26;</td><td>Used as a logical AND. The second command in the expression will beevaluated only if the first command evaluated to true.</td></tr><tr><td>( and )</td><td>Used for command grouping</td></tr><tr><td>;</td><td>Used as a list terminator. A command following the terminator will run after the preceding command has finished, regardless of whether it evaluates to true or not.</td></tr><tr><td>;;</td><td>Ends a case statement.</td></tr><tr><td>|</td><td>Redirects the output of a command as input to another command.</td></tr><tr><td>||</td><td>Used as a logical OR. The second command will run if the first one evaluates to false.</td></tr></tbody></table>

**Redirection Operators**

<table data-header-hidden><thead><tr><th width="121"></th><th></th></tr></thead><tbody><tr><td>Operator</td><td>Description</td></tr><tr><td>></td><td>Redirects stdout to a file</td></tr><tr><td>>></td><td>Redirects stdout to a file by appending it to the existing content</td></tr><tr><td>&#x26;> or >&#x26;</td><td>Redirects stdout and stderr to a file</td></tr><tr><td>&#x26;>></td><td>Redirects stdout and stderr to a file by appending it to the existing content</td></tr><tr><td>&#x3C;</td><td>Redirects input to a command</td></tr><tr><td>&#x3C;&#x3C;</td><td>Called a here document or heredoc, redirects multiple input lines to a command</td></tr><tr><td>|</td><td>Redirects output of a command as input to another command</td></tr></tbody></table>

\


### **If / Else/ Elif Statements**&#x20;

* Syntax&#x20;

```shell
if[<some test | condation>] 
then 
<cmmands>
.
.
.
.
fi
```

Example add Tow Numbers and sum:

```bash
#!/bin/bash

read -p "Enter number 1: " num1 #read to input from user
read -p "Enter number 2: " num2
if 
sum=$((num1 + num2))
echo "The sum of $num1 and $num2 is: $sum" # echo to output | print

#Using condition

#!/bin/bash
read -p "Enter number 1: " num1 # input form user
read -p "Enter number 2: " num2
if [ $num1 -gt $num2 ]; then
  sum=$((num1 + num2))
  echo "The sum of $num1 and $num2 is: $sum"
elif [ $num1 -eq 0 ] && [ $num2 == 0 ]; then
  echo "num1 = 0 num2 = 0 cant calculated"
else
  echo "$num1 is not greater than $num2, so no sum is calculated."
fi
 
```

### **Function**

**this is function showuptime**&#x20;

```bash
#!/bin/bash

showuptime(){
        up=$(uptime -p | cut -c4-)
        since=$(uptime -s)
        cat << EOF
----
this machine has been up for ${up}
it has been runnin ${since}
----
EOF
}
showuptime

Outout:-
this machine has been up for 3 hours, 39 minutes 
it has been runnin 2024-06-25 21:16:47
```

Using **`local`** varibale&#x20;

```bash
#!/bin/bash
up="before"
since="after"
showuptime(){
        local up=$(uptime -p | cut -c4-) #Local vat
        since=$(uptime -s) #Global var
        cat << EOF
----
this machine has been up for ${up}
it has been runnin ${since}
----
EOF
}
showuptime
echo ${up}
echo ${since}


output:-
his machine has been up for 3 hours, 39 minutes 
it has been runnin 2024-06-25 21:16:47
----
before
it has been runnin 2024-06-25 21:16:47
```

### AWK In Bash Scripting

<pre class="language-bash"><code class="lang-bash"><strong>Ex1:-
</strong><strong>echo "Just enter your name: abdo " | awk -F: '{print $2}'
</strong>
output:- 
abdo

Ex2:-
echo "Just enter your name, abdo ,ali" | awk -F, '{print $2,$3}'

output:- 
abdo ali

</code></pre>

> ## [Bash Scripting Tutorial for Beginners](https://www.youtube.com/watch?v=tK9Oc6AEnR4)

### EX1: Sub\_finder.sh

```bash
#!/bin/bash
for sub in $(cat subdomian.txt)
do 
if [[ $(ping -c 1 $sub 2> /dev/null) ]]
then
echo $sub >> valid_sub.txt
echo "$sub ++++++ Pong"
else
echo "$sub ------ Error"
fi
done

for ip in $(cat valid_sub.txt)
do
host $ip
echo host $ip | cut -d " " -f 4 | head -n 1 >> ips.txt
done

for sc in $(cat ips.txt)
do 
nmap $sc 
done
```
