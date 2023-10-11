# Информационно-аналитические технологии поиска угроз информационной
безопасности
Kabanova Svetlana

# Лабораторная работа №1

## Цель работы

Изучить базовые принципы работы языка R

## Ход работы

### Задание 1: Basic Building Blocks

R can be used as an interactive calculator, for example, 5+7=

``` r
5+7
```

    [1] 12

To assign the result of 5 + 7 to a new variable called x, you type x \<-
5 + 7. To view the contents of the variable x.

``` r
x<-5+7
x
```

    [1] 12

Store the result of x - 3 in a new variable called y. What\`s the value
of y?

``` r
y<-x-3
y
```

    [1] 9

To create a vector containing the numbers 1.1, 9, and 3.14 and store the
result in a variable called z.

``` r
z<-c(1.1, 9, 3.14)
```

If you want more information on the | c() function, type ?c without the
parentheses that normally follow a function name.

``` r
?c
```

Type z to view its contents.

``` r
z
```

    [1] 1.10 9.00 3.14

Create a new vector that contains z, 555, then z again in that order.

``` r
c(z, 555, z)
```

    [1]   1.10   9.00   3.14 555.00   1.10   9.00   3.14

Numeric vectors can be used in arithmetic expressions.

``` r
z*2+100
```

    [1] 102.20 118.00 106.28

Take the square root of z - 1 and assign it to a new variable called
my_sqrt. And print.

``` r
my_sqrt<-sqrt(z-1)
my_sqrt
```

    [1] 0.3162278 2.8284271 1.4628739

Create a new variable called my_div that gets the value of z divided by
my_sqrt. Print the contents of my_div.

``` r
my_div<-z/my_sqrt
my_div
```

    [1] 3.478505 3.181981 2.146460

To see another example of how this vector ‘recycling’ works, try adding
c(1, 2, 3, | 4) and c(0, 10).

``` r
c(1, 2, 3, 4)+c(0, 10)
```

    [1]  1 12  3 14

Try c(1, 2, 3, 4) + c(0, 10, 100) for an example.

``` r
c(1, 2, 3, 4)+c(0, 10, 100)
```

    Warning in c(1, 2, 3, 4) + c(0, 10, 100): longer object length is not a
    multiple of shorter object length

    [1]   1  12 103   4

Try hitting the up arrow on your keyboard until you get to this command
(z \* 2 + 100), then change 100 to 1000 and hit Enter.

``` r
z*2+1000
```

    [1] 1002.20 1018.00 1006.28

You can type the first two letters of the variable name, then hit the
Tab key. This is called auto-completion.

### Задание 2: Workspace and Files

Determine which directory your R session is using as its current working
directory using getwd().

``` r
getwd()
```

    [1] "/home/irina/threat_hunting_2/lab1"

List all the objects in your local workspace using ls().

``` r
ls()
```

    [1] "has_annotations" "my_div"          "my_sqrt"         "pandoc_dir"     
    [5] "quarto_bin_path" "x"               "y"               "z"              

Assign 9 to x using x \<- 9.

``` r
x<-9
```

Now take a look at objects that are in your workspace using ls().

``` r
ls()
```

    [1] "has_annotations" "my_div"          "my_sqrt"         "pandoc_dir"     
    [5] "quarto_bin_path" "x"               "y"               "z"              

List all the files in your working directory using list.files() or
dir().

``` r
list.files()
```

    [1] "lab1.qmd"       "lab1.rmarkdown" "mytest2.R"      "mytest3.R"     
    [5] "README.md"      "testdir"        "testdir2"      

Check out the help page for list.files with the command ?list.files.

``` r
?list.files
```

Use the args() function to determine the arguments to list.files().

``` r
args(list.files)
```

    function (path = ".", pattern = NULL, all.files = FALSE, full.names = FALSE, 
        recursive = FALSE, ignore.case = FALSE, include.dirs = FALSE, 
        no.. = FALSE) 
    NULL

Assign the value of the current working directory to a variable called
“old.dir”.

``` r
old.dir <- getwd()
```

Use dir.create() to create a directory in the current working directory
called “testdir”.

``` r
dir.create("testdir")
```

    Warning in dir.create("testdir"): 'testdir' already exists

Set your working directory to “testdir” with the setwd() command.

``` r
setwd("testdir")
```

Create a file in your working directory called “mytest.R” using the
file.create() function.

``` r
file.create("mytest.R")
```

    [1] TRUE

This should be the only file in this newly created directory. Let’s
check this by listing all the files in the current directory.

``` r
list.files()
```

    [1] "lab1.qmd"       "lab1.rmarkdown" "mytest.R"       "mytest2.R"     
    [5] "mytest3.R"      "README.md"      "testdir"        "testdir2"      

Check to see if “mytest.R” exists in the working directory using the
file.exists() function.

``` r
file.exists("mytest.R")
```

    [1] TRUE

Access information about the file “mytest.R” by using file.info().

``` r
file.info("mytest.R")
```

             size isdir mode               mtime               ctime
    mytest.R    0 FALSE  664 2023-10-11 08:00:34 2023-10-11 08:00:34
                           atime  uid  gid uname grname
    mytest.R 2023-10-11 08:00:34 1000 1000 irina  irina

Change the name of the file “mytest.R” to “mytest2.R” by using
file.rename().

``` r
file.rename("mytest.R", "mytest2.R")
```

    [1] TRUE

Make a copy of “mytest2.R” called “mytest3.R” using file.copy().

``` r
file.copy("mytest2.R", "mytest3.R")
```

    [1] FALSE

Provide the relative path to the file “mytest3.R” by using file.path().

``` r
file.path("mytest3.R")
```

    [1] "mytest3.R"

You can use file.path to construct file and directory paths that are
independent of the operating system your R code is running on. Pass
‘folder1’ and ‘folder2’ as arguments to file.path to make a
platform-independent pathname.

``` r
file.path("folder1", "folder2")
```

    [1] "folder1/folder2"

Take a look at the documentation for dir.create by entering ?dir.create
.

``` r
?dir.create
```

Create a directory in the current working directory called “testdir2”
and a subdirectory for it called “testdir3”, all in one command by using
dir.create() and file.path().

``` r
dir.create(file.path('testdir2', 'testdir3'), recursive = TRUE)
```

    Warning in dir.create(file.path("testdir2", "testdir3"), recursive = TRUE):
    'testdir2/testdir3' already exists

Go back to your original working directory using setwd().

``` r
setwd(old.dir)
```

### Задание 3: Sequences of Numbers

The simplest way to create a sequence of numbers in R is by using the
`:` operator. Type 1:20 to see how it works.

``` r
1:20
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

That gave us every integer between (and including) 1 and 20. We could
also use it to create a sequence of real numbers. For example, try
pi:10.

``` r
pi:10
```

    [1] 3.141593 4.141593 5.141593 6.141593 7.141593 8.141593 9.141593

What happens if we do 15:1?

``` r
15:1
```

     [1] 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1

Pull up the documentation for `:` now.

``` r
?`:`
```

The most basic use of seq() does exactly the same thing as the `:`
operator. Try seq(1, 20) to see this.

``` r
seq(1,20)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

This gives us the same output as 1:20. However, let’s say that instead
we want a vector of numbers ranging from 0 to 10, incremented by 0.5.
seq(0, 10, by=0.5) does just that. Try it out.

``` r
seq(0,10, by=0.5)
```

     [1]  0.0  0.5  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0  6.5  7.0
    [16]  7.5  8.0  8.5  9.0  9.5 10.0

Or maybe we don’t care what the increment is and we just want a sequence
of 30 numbers between 5 and 10. seq(5, 10, length=30) does the trick.
Give it a shot now and store the result in a new variable called my_seq.

``` r
my_seq<-seq(5, 10, length=30)
```

To confirm that my_seq has length 30, we can use the length() function.

``` r
length(my_seq)
```

    [1] 30

There are several ways we could do this. One possibility is to combine
the `:` operator and the length() function like this: 1:length(my_seq).

``` r
1:length(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

Another option is to use seq(along.with = my_seq).

``` r
seq(along.with = my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

However, as is the case with many common tasks, R has a separate
built-in function for this purpose called seq_along(). Type
seq_along(my_seq) to see it in action.

``` r
seq_along(my_seq)
```

     [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
    [26] 26 27 28 29 30

If we’re interested in creating a vector that contains 40 zeros, we can
use rep(0, times = 40).

``` r
rep(0, times = 40)
```

     [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
    [39] 0 0

If instead we want our vector to contain 10 repetitions of the vector
(0, 1, 2), we can do rep(c(0, 1, 2), times = 10).

``` r
rep(c(0, 1, 2), times = 10)
```

     [1] 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2 0 1 2

Finally, let’s say that rather than repeating the vector (0, 1, 2) over
and over again, we want our vector to contain 10 zeros, then 10 ones,
then 10 twos. We can do this with the `each` argument. Try rep(c(0, 1,
2), each = 10).

``` r
rep(c(0, 1, 2), each = 10)
```

     [1] 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2

### Задание 4: Vectors

First, create a numeric vector num_vect that contains the values 0.5,
55, -10, and 6.

``` r
num_vect<-c(0.5,55,-10,6)
```

Now, create a variable called tf that gets the result of num_vect \< 1.
Print.

``` r
tf<-num_vect < 1
tf
```

    [1]  TRUE FALSE  TRUE FALSE

Type num_vect \>= 6 without assigning the result to a new variable.

``` r
num_vect >= 6
```

    [1] FALSE  TRUE FALSE  TRUE

Create a character vector that contains the following words: “My”,
“name”, “is”. Remember to enclose each word in its own set of double
quotes, so that R knows they are character strings. Store the vector in
a variable called my_char. Print the contents.

``` r
my_char<-c("My", "name", "is")
my_char
```

    [1] "My"   "name" "is"  

Type paste(my_char, collapse = ” “) now. Make sure there’s a space
between the double quotes in the `collapse` argument.

``` r
paste(my_char, collapse = " ")
```

    [1] "My name is"

To add (or ‘concatenate’) your name to the end of my_char, use the c()
function like this: c(my_char, “your_name_here”). Place your name in
double quotes where I’ve put “your_name_here”.

``` r
my_name <- c(my_char, "Sveta")
my_name
```

    [1] "My"    "name"  "is"    "Sveta"

Now, use the paste() function once more to join the words in my_name
together into a single character string.

``` r
paste(my_name, collapse = " ")
```

    [1] "My name is Sveta"

Try paste(“Hello”, “world!”, sep = ” “), where the `sep` argument tells
R that we want to separate the joined elements with a single space.

``` r
paste("Hello", "world!", sep = " ")
```

    [1] "Hello world!"

For a slightly more complicated example, we can join two vectors, each
of length 3. Use paste() to join the integer vector 1:3 with the
character vector c(“X”, “Y”, “Z”).

``` r
paste(1:3, c("X", "Y", "Z"), sep = "")
```

    [1] "1X" "2Y" "3Z"

Try paste(LETTERS, 1:4, sep = “-”), where LETTERS is a predefined
variable in R containing a character vector of all 26 letters in the
English alphabet.

``` r
paste(LETTERS, 1:4, sep = "-")
```

     [1] "A-1" "B-2" "C-3" "D-4" "E-1" "F-2" "G-3" "H-4" "I-1" "J-2" "K-3" "L-4"
    [13] "M-1" "N-2" "O-3" "P-4" "Q-1" "R-2" "S-3" "T-4" "U-1" "V-2" "W-3" "X-4"
    [25] "Y-1" "Z-2"

## Оценка результата

В результате лабораторной работы были разобраны базовые аспекты языка
программирования R.

## Вывод

Мы получили базовые навыки работы с RStudio Desktop и языком R
