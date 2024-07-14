---
output:
    html_document:
        keep_md: TRUE
---

<script>
function buildQuiz(myq, qc){
  // variable to store the HTML output
  const output = [];

  // for each question...
  myq.forEach(
    (currentQuestion, questionNumber) => {

      // variable to store the list of possible answers
      const answers = [];

      // and for each available answer...
      for(letter in currentQuestion.answers){

        // ...add an HTML radio button
        answers.push(
          `<label>
            <input type="radio" name="question${questionNumber}" value="${letter}">
            ${letter} :
            ${currentQuestion.answers[letter]}
          </label><br/>`
        );
      }

      // add this question and its answers to the output
      output.push(
        `<div class="question"> ${currentQuestion.question} </div>
        <div class="answers"> ${answers.join('')} </div><br/>`
      );
    }
  );

  // finally combine our output list into one string of HTML and put it on the page
  qc.innerHTML = output.join('');
}

function showResults(myq, qc, rc){

  // gather answer containers from our quiz
  const answerContainers = qc.querySelectorAll('.answers');

  // keep track of user's answers
  let numCorrect = 0;

  // for each question...
  myq.forEach( (currentQuestion, questionNumber) => {

    // find selected answer
    const answerContainer = answerContainers[questionNumber];
    const selector = `input[name=question${questionNumber}]:checked`;
    const userAnswer = (answerContainer.querySelector(selector) || {}).value;

    // if answer is correct
    if(userAnswer === currentQuestion.correctAnswer){
      // add to the number of correct answers
      numCorrect++;

      // color the answers green
      answerContainers[questionNumber].style.color = 'lightgreen';
    }
    // if answer is wrong or blank
    else{
      // color the answers red
      answerContainers[questionNumber].style.color = 'red';
    }
  });

  // show number of correct answers out of total
  rc.innerHTML = `${numCorrect} out of ${myq.length}`;
}
</script>


<style type="text/css">
.colsel {
background-color: lightyellow;
}

pre, code {
  white-space:pre !important;
  overflow-x:scroll auto
}
</style>








# Intro to R Day 2

---

Load your day 1 workspace data:

```{.r .colsel}
load("day1.RData")
```

### Lists
#### A list is an ordered collection of objects, which can be any type of R objects (vectors, matrices, data frames, even lists).

##### A list is constructed using the function list().


```{.r .colsel}
my_list <- list(1:5, "a", c(TRUE, FALSE, FALSE), c(3.2, 103.0, 82.3))
my_list
```

```
## [[1]]
## [1] 1 2 3 4 5
## 
## [[2]]
## [1] "a"
## 
## [[3]]
## [1]  TRUE FALSE FALSE
## 
## [[4]]
## [1]   3.2 103.0  82.3
```

```{.r .colsel}
str(my_list)
```

```
## List of 4
##  $ : int [1:5] 1 2 3 4 5
##  $ : chr "a"
##  $ : logi [1:3] TRUE FALSE FALSE
##  $ : num [1:3] 3.2 103 82.3
```

##### One could construct a list by giving names to elements.


```{.r .colsel}
my_list <- list(Ranking=1:5, ID="a", Test=c(TRUE, FALSE, FALSE), Score=c(3.2, 103.0, 82.3))

# display the names of elements in the list using the function *names*, or *str*. Compare the output of *str* with the above results to see the difference.
names(my_list)
```

```
## [1] "Ranking" "ID"      "Test"    "Score"
```

```{.r .colsel}
str(my_list)
```

```
## List of 4
##  $ Ranking: int [1:5] 1 2 3 4 5
##  $ ID     : chr "a"
##  $ Test   : logi [1:3] TRUE FALSE FALSE
##  $ Score  : num [1:3] 3.2 103 82.3
```


```{.r .colsel}
# number of elements in the list
length(my_list)
```

```
## [1] 4
```

### Subsetting data

#### Subsetting allows one to access the piece of data of interest. When combinded with assignment, subsetting can modify selected pieces of data. The operators that can be used to subset data are: [, $, and [[.

##### First, we are going to talk about subsetting data using [, which is the most commonly used operator. We will start by looking at vectors and talk about four ways to subset a vector.

* <font color='purple'>**Positive integers** return elements at the specified positions</font>


```{.r .colsel}
# first to recall what are stored in gene_names
gene_names
```

```
## [1] "ESR1"  "p53"   "PI3K"  "BRCA1" "EGFR"
```

```{.r .colsel}
# obtain the 2nd element
gene_names[2]
```

```
## [1] "p53"
```

```{.r .colsel}
# obtain the first and the third elements
gene_names[c(1,3)]
```

```
## [1] "ESR1" "PI3K"
```

R uses 1 based indexing, meaning the first element is at the position 1, not at position 0.

* <font color='purple'>**Negative integers** omit elements at the specified positions</font>


```{.r .colsel}
gene_names[-c(1,3)]
```

```
## [1] "p53"   "BRCA1" "EGFR"
```

One may not mixed positive and negative integers in one single subset operation.


```{.r .colsel}
# The following command will produce an error.
gene_names[c(-1, 2)]
```

```
## Error in gene_names[c(-1, 2)]: only 0's may be mixed with negative subscripts
```

* <font color='purple'>**Logical vectors** select elements where the corresponding logical value is TRUE</font>, This is very useful because one may write the expression that creates the logical vector.


```{.r .colsel}
gene_names[c(TRUE, FALSE, TRUE, FALSE, FALSE)]
```

```
## [1] "ESR1" "PI3K"
```

Recall that we have created one vector called *gene_expression*. Let's assume that *gene_expression* stores the expression values correspond to the genes in *gene_names*. Then we may subset the genes based on expression values.


```{.r .colsel}
gene_expression
```

```
##  ESR1   p53  PI3K BRCA1  EGFR 
##     0   100    50   200    80
```

```{.r .colsel}
gene_names[gene_expression > 50]
```

```
## [1] "p53"   "BRCA1" "EGFR"
```

If the logical vector is shorter in length than the data vector that we want to subset, then it will be recycled to be the same length as the data vector.


```{.r .colsel}
gene_names[c(TRUE, FALSE)]
```

```
## [1] "ESR1" "PI3K" "EGFR"
```

If the logical vector has "NA" in it, the corresponding value will be "NA" in the output. "NA" in R is a symbol for missing value.


```{.r .colsel}
gene_names[c(TRUE, NA, FALSE, TRUE, NA)]
```

```
## [1] "ESR1"  NA      "BRCA1" NA
```

* <font color='purple'>**Character vectors** return elements with matching names, when the vector is named.</font>


```{.r .colsel}
gene_expression
```

```
##  ESR1   p53  PI3K BRCA1  EGFR 
##     0   100    50   200    80
```

```{.r .colsel}
gene_expression[c("ESR1", "p53")]
```

```
## ESR1  p53 
##    0  100
```

* <font color='purple'>**Nothing** returns the original vector</font>, This is more useful for matrices, data frames than for vectors.


```{.r .colsel}
gene_names[]
```

```
## [1] "ESR1"  "p53"   "PI3K"  "BRCA1" "EGFR"
```

<br>

##### Subsetting a list works in the same way as subsetting an atomic vector. Using [ will always return a list.


```{.r .colsel}
my_list[1]
```

```
## $Ranking
## [1] 1 2 3 4 5
```
<br>

##### Subsetting a matrix can be done by simply generalizing the one dimension subsetting: one may supply a one dimension index for each dimension of the matrix. <font color='red'>Blank/Nothing subsetting is now useful in keeping all rows or all columns.</font>



```{.r .colsel}
# get the element in the 2nd row, 3rd column
my_matrix[2,3]
```

```
## [1] 37
```

```{.r .colsel}
# get the entire 2nd row
my_matrix[2,]
```

```
## col1 col2 col3 
##    3   18   37
```

```{.r .colsel}
# get the entire 3rd column
my_matrix[,3]
```

```
## row1 row2 row3 row4 
##    8   37  267   19
```

```{.r .colsel}
# use a logical vector with recycling
my_matrix[c(TRUE, FALSE), ]
```

```
##      col1 col2 col3
## row1    1    2    8
## row3    8   27  267
```
<br>

##### Subsetting a data frame can be done similarly as subsetting a matrix. In addition, one may supply only one 1-dimensional index to subset a data frame. In this case, R will treat the data frame as a list with each column is an element in the list.


```{.r .colsel}
# recall a data frame created from above: *meta.data*
meta.data
```

```
##   patients_name disease_stage Family_history patients_age BRCA
## 1      Patient1        Stage1              Y           31  YES
## 2      Patient2        Stage2              N           40   NO
## 3      Patient3        Stage2              Y           39  YES
## 4      Patient4        Stage3              N           50  YES
## 5      Patient5        Stage1              Y           45  YES
## 6      Patient6        Stage4              Y           65   NO
```

```{.r .colsel}
# subset the data frame similarly to a matrix
meta.data[c(TRUE, FALSE, FALSE, TRUE),]
```

```
##   patients_name disease_stage Family_history patients_age BRCA
## 1      Patient1        Stage1              Y           31  YES
## 4      Patient4        Stage3              N           50  YES
## 5      Patient5        Stage1              Y           45  YES
```

```{.r .colsel}
# subset the data frame using one vector
meta.data[c("patients_age", "disease_stage")]
```

```
##   patients_age disease_stage
## 1           31        Stage1
## 2           40        Stage2
## 3           39        Stage2
## 4           50        Stage3
## 5           45        Stage1
## 6           65        Stage4
```

<br>

### Subsetting operators: **[[** and **$**

##### **[[** is similar to **[**, except that it returns the content of the element.


```{.r .colsel}
# recall my_list
my_list
```

```
## $Ranking
## [1] 1 2 3 4 5
## 
## $ID
## [1] "a"
## 
## $Test
## [1]  TRUE FALSE FALSE
## 
## $Score
## [1]   3.2 103.0  82.3
```

```{.r .colsel}
# comparing [[ with [ in subsetting a list
my_list[[1]]
```

```
## [1] 1 2 3 4 5
```

```{.r .colsel}
my_list[1]
```

```
## $Ranking
## [1] 1 2 3 4 5
```

<font color='red'>[[ is very useful when working with a list. Because when [ is applied to a list, it always returns a list. While [[ returns the contents of the list. [[ can only extrac/return one element, so it only accept one integer/string as input.</font>

Because data frames are implemented as lists of columns, one may use [[ to extract a column from data frames.


```{.r .colsel}
meta.data[["disease_stage"]]
```

```
## [1] Stage1 Stage2 Stage2 Stage3 Stage1 Stage4
## Levels: Stage2 Stage1 Stage3 Stage4
```


<br>

##### **$** is a shorthand for **[[** combined with character subsetting.


```{.r .colsel}
# subsetting a list using $ 
my_list$Score
```

```
## [1]   3.2 103.0  82.3
```

```{.r .colsel}
# subsetting a data frame using
meta.data$disease_stage
```

```
## [1] Stage1 Stage2 Stage2 Stage3 Stage1 Stage4
## Levels: Stage2 Stage1 Stage3 Stage4
```

<br>

##### Simplifying vs. preserving subsetting

We have seen some examples of simplying vs. preserving subsetting, for example:


```{.r .colsel}
# simplifying subsetting
my_list[[1]]
```

```
## [1] 1 2 3 4 5
```

```{.r .colsel}
# preserving subsetting
my_list[1]
```

```
## $Ranking
## [1] 1 2 3 4 5
```

Basically, simplying subsetting returns the simplest possible data structure that can represent the output. While preserving subsetting keeps the structure of the output as the same as the input. In the above example, [[ simplifies the output to a vector, while [ keeps the output as a list.

Because the syntax of carrying out simplifying and preserving subsetting differs depending on the data structure, the table below provides the information for the most basic data structure.

<table class="table table-striped" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:center;"> Simplifying </th>
   <th style="text-align:center;"> Preserving </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Vector </td>
   <td style="text-align:center;"> x[[1]] </td>
   <td style="text-align:center;"> x[1] </td>
  </tr>
  <tr>
   <td style="text-align:left;"> List </td>
   <td style="text-align:center;"> x[[1]] </td>
   <td style="text-align:center;"> x[1] </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Factor </td>
   <td style="text-align:center;"> x[1:3, drop=T] </td>
   <td style="text-align:center;"> x[1:3] </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Data frame </td>
   <td style="text-align:center;"> x[, 1] or x[[1]] </td>
   <td style="text-align:center;"> x[, 1, drop=F] or x[1] </td>
  </tr>
</tbody>
</table>

## CHALLENGES

Using the built-in dataset **iris**, first subset the dataframe keeping only those rows where the sepal length is greater than 6. Then find the total number for each Species in that subset.

Using **iris**, remove the width columns and then create a new dataframe with the Species and another column that is the sum of the two length columns for each row.


---

Topic 3. Import and export data in R
====================================================

##### R base function read.table() is a general funciton that can be used to read a file in table format. The data will be imported as a data frame.


```{.r .colsel}
# There is a very convenient way to read files from the internet.
data1 <- read.table(file="https://github.com/ucdavis-bioinformatics-training/courses/raw/master/Intro2R/raw_counts.txt", sep="\t", header=T)

# To read a local file. If you have downloaded the raw_counts.txt file to your local machine, you may use the following command to read it in, by providing the full path for the file location. The way to specify the full path is the same as taught in the command line session.
download.file("https://github.com/ucdavis-bioinformatics-training/courses/raw/master/Intro2R/raw_counts.txt", "./raw_counts.txt")
data1 <- read.table(file="./raw_counts.txt", sep="\t", header=T)
```

To check what type of object *data1* is in and take a look at the beginning part of the data.

```{.r .colsel}
is.data.frame(data1)
```

```
## [1] TRUE
```

```{.r .colsel}
head(data1)
```

```
##            C61  C62  C63  C64  C91  C92  C93 C94 I561 I562 I563 I564 I591 I592
## AT1G01010  322  346  256  396  372  506  361 342  638  488  440  479  770  430
## AT1G01020  149   87  162  144  189  169  147 108  163  141  119  147  182  156
## AT1G01030   15   32   35   22   24   33   21  35   18    8   54   35   23    8
## AT1G01040  687  469  568  651  885  978  794 862  799  769  725  715  811  567
## AT1G01046    1    1    5    4    5    3    0   2    4    3    1    0    2    8
## AT1G01050 1447 1032 1083 1204 1413 1484 1138 938 1247 1516  984 1044 1374 1355
##           I593 I594 I861 I862 I863 I864 I891 I892 I893 I894
## AT1G01010  656  467  143  453  429  206  567  458  520  474
## AT1G01020  153  177   43  144  114   50  161  195  157  144
## AT1G01030   16   24   42   17   22   39   26   28   39   30
## AT1G01040  831  694  345  575  605  404  735  651  725  591
## AT1G01046    8    1    0    4    0    3    5    7    0    5
## AT1G01050 1437 1577  412 1338 1051  621 1434 1552 1248 1186
```


##### Depending on the format of the file, several variants of read.table() are available to make reading a file easier.

* read.csv(): for reading "comma separated value" files (.csv).

* read.csv2(): variant used in countries that use a comma "," as decimal point and a semicolon ";" as field separators.

* read.delim(): for reading "tab separated value" files (".tsv"). By default, point(".") is used as decimal point.

* read.delim2(): for reading "tab separated value" files (".tsv"). By default, comma (",") is used as decimal point.

<br>


```{.r .colsel}
# We are going to read a file over the internet by providing the url of the file.
data2 <- read.csv(file="https://github.com/ucdavis-bioinformatics-training/courses/raw/master/Intro2R/raw_counts.csv")

# To look at the file:
head(data2)
```

```
##            C61  C62  C63  C64  C91  C92  C93 C94 I561 I562 I563 I564 I591 I592
## AT1G01010  322  346  256  396  372  506  361 342  638  488  440  479  770  430
## AT1G01020  149   87  162  144  189  169  147 108  163  141  119  147  182  156
## AT1G01030   15   32   35   22   24   33   21  35   18    8   54   35   23    8
## AT1G01040  687  469  568  651  885  978  794 862  799  769  725  715  811  567
## AT1G01046    1    1    5    4    5    3    0   2    4    3    1    0    2    8
## AT1G01050 1447 1032 1083 1204 1413 1484 1138 938 1247 1516  984 1044 1374 1355
##           I593 I594 I861 I862 I863 I864 I891 I892 I893 I894
## AT1G01010  656  467  143  453  429  206  567  458  520  474
## AT1G01020  153  177   43  144  114   50  161  195  157  144
## AT1G01030   16   24   42   17   22   39   26   28   39   30
## AT1G01040  831  694  345  575  605  404  735  651  725  591
## AT1G01046    8    1    0    4    0    3    5    7    0    5
## AT1G01050 1437 1577  412 1338 1051  621 1434 1552 1248 1186
```

<br>

##### R base function write.table() can be used to export data to a file.


```{.r .colsel}
# To write to a file called "output.txt" in your current working directory.
write.table(data2[1:20,], file="output.txt", sep="\t", quote=F, row.names=T, col.names=T)
```

It is also possible to export data to a csv file.

write.csv()

write.csv2()


## Quiz 4

<div id="quiz4" class="quiz"></div>
<button id="submit4">Submit Quiz</button>
<div id="results4" class="output"></div>
<script>
quizContainer4 = document.getElementById('quiz4');
resultsContainer4 = document.getElementById('results4');
submitButton4 = document.getElementById('submit4');

myQuestions4 = [
  {
    question: "Using my_list, multiply the Ranking by the Score and find the mean in one command. What is the output?",
    answers: {
      a: "196.78 without a warning",
      b: "196.78 with a warning",
      c: "210.54 without a warning",
      d: "210.54 with a warning"
    },
    correctAnswer: "b"
  },
  {
    question: "Which of the following code will NOT get a result?",
    answers: {
      a: "gene_expression$ESR1",
      b: "gene_expression['ESR1']",
      c: "gene_expression[c(1,2,3,4)]",
      d: "gene_expression[1:2]"
    },
    correctAnswer: "a"
  },
  {
    question: "Using data1 and the 'max' function, find the maximum value across columns C92, I563, and I861:",
    answers: {
      a: "69853",
      b: "112754",
      c: "88122",
      d: "66890"
    },
    correctAnswer: "a"
  }
];

buildQuiz(myQuestions4, quizContainer4);
submitButton4.addEventListener('click', function() {showResults(myQuestions4, quizContainer4, resultsContainer4);});
</script>

---

Topic 4. R markdown and R notebooks
====================================================

Markdown is a system that allow easy incorporation of annotations/comments together with computing code. Both the raw source of markdown file and the rendered output are easy to read. R markdown allows both interactive mode with R and producing a reproducible document. An R notebook is an R markdown document with code chunks that can be executed independently and interactively, with output visible immediately beneath the input. In RStudio, by default, all R markdown documents are run in R notebook mode. Under the R notebook mode, when executing a chunk, the code is sent to the console to be run one line at a time. This allows execution to stop if a line raises an error.

<br>

In RStudio, creating an R notebook can be done by going to the menu command ** File -> New File -> R Notebook **.

An example of an R notebook looks like:


![](./notebook.png)


The way to run the R code inside the code chunk is to use the green arrow located at the top right corner of each of the code chunk, or use ** Ctrl + Shift + Enter ** on Windows, or ** Cmd + Shift + Enter ** on Mac to run the current code chunk. To run each individual code line, one uses ** Ctrl + Enter ** on Windows, or ** Cmd + Enter ** on Mac.

To render R notebook to html/pdf/word documents can be done using the **Preview** menu.

---

Topic 5. Functions in R
====================================================
#### Invoking a function by its name, followed by the parenthesis and zero or more arguments.


```{.r .colsel}
# to find out the current working directory
getwd()
```

```
## [1] "/home/joshi/Desktop/work/workshops/2024-March-Introduction-to-R-for-Bioinformatics/R"
```

```{.r .colsel}
# to set a different working directory, use setwd
#setwd("/Users/jli/Desktop")

# to list all objects in the environment
ls()
```

```
##  [1] "a"                     "b"                     "brca1_expressed"      
##  [4] "col1"                  "col2"                  "col3"                 
##  [7] "colFmt"                "data1"                 "data2"                
## [10] "disease_stage"         "expression.data"       "Family_history"       
## [13] "gene"                  "gene_expression"       "gene_names"           
## [16] "hello"                 "her2_expressed"        "her2_expression_level"
## [19] "md2"                   "meta.data"             "my_data"              
## [22] "my_list"               "my_matrix"             "nums"                 
## [25] "patients_age"          "patients_name"
```

```{.r .colsel}
# to create a vector from 2 to 3, using increment of 0.1
seq(2, 3, by=0.1)
```

```
##  [1] 2.0 2.1 2.2 2.3 2.4 2.5 2.6 2.7 2.8 2.9 3.0
```

```{.r .colsel}
# to create a vector with repeated elements
rep(1:3, times=3)
```

```
## [1] 1 2 3 1 2 3 1 2 3
```

```{.r .colsel}
rep(1:3, each=3)
```

```
## [1] 1 1 1 2 2 2 3 3 3
```

```{.r .colsel}
# get the unique values in a vector
unique(c(4,4,4,5,5,5,6,6,6))
```

```
## [1] 4 5 6
```

```{.r .colsel}
# to get help information on a function in R: ?function.name
?seq
?sort
?rep
```

##### <font color='red'>One useful function to find out information on an R object: str(). It compactly display the internal structure of an R object.</font>  



```{.r .colsel}
str(data2)
```

```
## 'data.frame':	33602 obs. of  24 variables:
##  $ C61 : int  322 149 15 687 1 1447 2667 297 0 74 ...
##  $ C62 : int  346 87 32 469 1 1032 2472 226 0 79 ...
##  $ C63 : int  256 162 35 568 5 1083 2881 325 0 138 ...
##  $ C64 : int  396 144 22 651 4 1204 2632 341 0 85 ...
##  $ C91 : int  372 189 24 885 5 1413 5120 199 0 68 ...
##  $ C92 : int  506 169 33 978 3 1484 6176 180 0 41 ...
##  $ C93 : int  361 147 21 794 0 1138 7088 195 0 110 ...
##  $ C94 : int  342 108 35 862 2 938 6810 107 0 81 ...
##  $ I561: int  638 163 18 799 4 1247 2258 377 0 72 ...
##  $ I562: int  488 141 8 769 3 1516 1808 534 0 76 ...
##  $ I563: int  440 119 54 725 1 984 2279 300 0 184 ...
##  $ I564: int  479 147 35 715 0 1044 2299 223 0 156 ...
##  $ I591: int  770 182 23 811 2 1374 4755 298 0 96 ...
##  $ I592: int  430 156 8 567 8 1355 3128 318 0 70 ...
##  $ I593: int  656 153 16 831 8 1437 4419 397 0 77 ...
##  $ I594: int  467 177 24 694 1 1577 3726 373 0 77 ...
##  $ I861: int  143 43 42 345 0 412 1452 86 0 174 ...
##  $ I862: int  453 144 17 575 4 1338 1516 266 0 113 ...
##  $ I863: int  429 114 22 605 0 1051 1455 281 0 69 ...
##  $ I864: int  206 50 39 404 3 621 1429 164 0 176 ...
##  $ I891: int  567 161 26 735 5 1434 3867 230 0 69 ...
##  $ I892: int  458 195 28 651 7 1552 4718 270 0 80 ...
##  $ I893: int  520 157 39 725 0 1248 4580 220 0 81 ...
##  $ I894: int  474 144 30 591 5 1186 3575 229 0 62 ...
```

### Statistics functions

<table class="table table-striped table-hover table-responsive" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:center;"> Description </th>
   <th style="text-align:center;"> R_function </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> Mean </td>
   <td style="text-align:center;"> mean() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Standard deviation </td>
   <td style="text-align:center;"> sd() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Variance </td>
   <td style="text-align:center;"> var() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Minimum </td>
   <td style="text-align:center;"> min() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Maximum </td>
   <td style="text-align:center;"> max() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Median </td>
   <td style="text-align:center;"> median() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Range of values: minimum and maximum </td>
   <td style="text-align:center;"> range() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Sample quantiles </td>
   <td style="text-align:center;"> quantile() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Generic function </td>
   <td style="text-align:center;"> summary() </td>
  </tr>
  <tr>
   <td style="text-align:center;"> Interquartile range </td>
   <td style="text-align:center;"> IQR() </td>
  </tr>
</tbody>
</table>

### Save data in R session

#### To save history in R session

savehistory(file="Oct08.history")

loadhistory(file="Oct08.history")

#### To save objects in R session

save(list=c("x", "data"), file="Oct08.RData")

load("Oct08.RData")


Topic 7: Conditional code
================================

Decision making is important in programming. This can be achieved using an **if...else** statement.

The basic structure of an *if...else* statement is 

**if (condition statement){**

**some operation**

**}**


Two examples of *if...else* statement


```{.r .colsel}
Temperature <- 30

if (Temperature < 32) {
  print("Very cold")
}
```

```
## [1] "Very cold"
```


```{.r .colsel}
# recall gene_expression, we are going to design a *if...else* statement to decide treatment plans based on gene expression.

if (gene_expression["ESR1"] > 0) {
  print("Treatment plan 1")
} else if (gene_expression["BRCA1"] > 0) {
  print("Treatment plan 2")
} else if (gene_expression["p53"] > 0) {
  print("Treatment plan 3")
} else {
  print("Treatment plan 4")
}
```

```
## [1] "Treatment plan 2"
```


## Logical values and logical expressions

Within an *if statement* you have a condition statement that is being checked. This condition can have multiple parts to it. For example, let's say we wanted to check if gene expression for a gene was greater than 50 **AND** that the current temperature was greater than 20:


```{.r .colsel}
if (gene_expression["EGFR"] > 50 && Temperature > 20) {
  print("Treatment plan 7")
}
```

```
## [1] "Treatment plan 7"
```

If we want to check if EGFR is greater than 50 **OR** the temp is greater than 40:


```{.r .colsel}
if (gene_expression["EGFR"] > 50 || Temperature > 40) {
  print("Treatment plan 9")
}
```

```
## [1] "Treatment plan 9"
```

We can also use **!** to negate a condition:


```{.r .colsel}
if (!(meta.data[2,"disease_stage"] == "Stage3")) {
  print("not Stage3")
}
```

```
## [1] "not Stage3"
```

```{.r .colsel}
if (meta.data[2,"disease_stage"] != "Stage3") {
  print("not Stage3")
}
```

```
## [1] "not Stage3"
```

Finally, we can also use the logical operations in an element-wise way:


```{.r .colsel}
a = c(TRUE,FALSE,FALSE,TRUE,FALSE)
b = c(TRUE,TRUE,TRUE,FALSE,FALSE)

# element-wise AND
a & b
```

```
## [1]  TRUE FALSE FALSE FALSE FALSE
```

```{.r .colsel}
# element-wise OR
a | b
```

```
## [1]  TRUE  TRUE  TRUE  TRUE FALSE
```

Truth Table
-----------

<table class="table table-striped" style="width: auto !important; ">
 <thead>
  <tr>
   <th style="text-align:center;"> x </th>
   <th style="text-align:center;"> y </th>
   <th style="text-align:center;"> x &amp;&amp; y </th>
   <th style="text-align:center;"> x &amp;#124;&amp;#124; y </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> TRUE </td>
   <td style="text-align:center;"> TRUE </td>
   <td style="text-align:center;"> TRUE </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> FALSE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> TRUE </td>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
  <tr>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> TRUE </td>
   <td style="text-align:center;"> FALSE </td>
   <td style="text-align:center;"> TRUE </td>
  </tr>
</tbody>
</table>


Save your workspace so we can load it for day 3:

```{.r .colsel}
save.image("day2.RData")
```


## HOMEWORK

Using the state.x77 built-in dataset, find the states whose population (in 1000's) is greater than 950 AND whose High School Graduation Rate is less than 40%.

Construct a list with three elements:

1. A vector of numbers 1 through 15 in increments of 0.2
2. A 5x5 matrix using the first 25 letters of the alphabet. (Hint: look at built-in constants)
3. The first 10 rows of the built-in data frame "mtcars".

