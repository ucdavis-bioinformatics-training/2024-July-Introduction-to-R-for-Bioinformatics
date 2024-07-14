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









R and RStudio
========================================================

### What is R?
[R](http://r-project.org/) is a language and environment for statistical computing and graphics developed in 1993. It provides a wide variety of statistical and graphical techniques (linear and nonlinear modeling, statistical tests, time series analysis, classification, clustering, ...), and is highly extensible, meaning that the user community can write new R tools. It is a GNU project (Free and Open Source).

The R language has its roots in the S language and environment which was developed at Bell Laboratories (formerly AT&T, now Lucent Technologies) by John Chambers and colleagues. R was created by Ross Ihaka and Robert Gentleman at the University of Auckland, New Zealand, and now, R is developed by the R Development Core Team, of which Chambers is a member. R is named partly after the first names of the first two R authors (Robert Gentleman and Ross Ihaka), and partly as a play on the name of S. R can be considered as a different implementation of S. There are some important differences, but much code written for S runs unaltered under R.

Some of R's strengths:

* The ease with which well-designed publication-quality plots can be produced, including mathematical symbols and formulae where needed. Great care has been taken over the defaults for the minor design choices in graphics, but the user retains full control. Such as examples like the following (extracted from http://web.stanford.edu/class/bios221/book/Chap-Graphics.html):

<img src="chap3-rgraphics-heatmap-1.png" width="32%" /><img src="chap3-rgraphics-darned1-1.png" width="32%" /><img src="chap3-rgraphics-twodsp4-1.png" width="32%" />

* It compiles and runs on a wide variety of UNIX platforms and similar systems (including FreeBSD and Linux), Windows and MacOS.
* R can be extended (easily) via packages.
* R has its own LaTeX-like documentation format, which is used to supply comprehensive documentation, both on-line in a number of formats and in hardcopy.
* It has a vast community both in academia and in business.
* It's FREE!   

### The R environment
R is an integrated suite of software facilities for data manipulation, calculation and graphical display. It includes

* an effective data handling and storage facility,
* a suite of operators for calculations on arrays, in particular matrices,
* a large, coherent, integrated collection of intermediate tools for data analysis,
* graphical facilities for data analysis and display either on-screen or on hardcopy, and
* a well-developed, and effective programming language which includes conditionals, loops, user-defined recursive functions and input and output facilities.

The term "environment" is intended to characterize it as a fully planned and coherent system, rather than an incremental accretion of very specific and inflexible tools, as is frequently the case with other data analysis software.

R, like S, is designed around a true computer language, and it allows users to add additional functionality by defining new functions. Much of the system is itself written in the R dialect of S, which makes it easy for users to follow the algorithmic choices made. For computationally-intensive tasks, C, C++ and Fortran code can be linked and called at run time. Advanced users can write C code to manipulate R objects directly.

Many users think of R as a statistics system. The R group prefers to think of it of an environment within which statistical techniques are implemented.   

### The R Homepage
The R homepage has a wealth of information on it,

[R-project.org](http://r-project.org/)

On the homepage you can:

* Learn more about R
* Download R
* Get Documentation (official and user supplied)
* Get access to CRAN 'Comprehensive R archival network'

### Interface for R

There are many ways one can interface with R language. Here are a few popular ones:

* [RStudio](https://www.rstudio.com/)
* [Jupyter](https://jupyter.org/) and R notebooks
* text editors, such as vi/vim, Emacs...


### RStudio

[RStudio](https://www.rstudio.com/) started in 2010, to offer R a more full featured integrated development environment (IDE) and modeled after matlab's IDE.

RStudio has many features:

* syntax highlighting
* code completion
* smart indentation
* "Projects"
* workspace browser and data viewer
* embedded plots
* Markdown notebooks, Sweave authoring and knitr with one click pdf or html
* runs on all platforms and over the web
* etc. etc. etc.

RStudio and its team have contributed to many R packages.[13] These include:

* Tidyverse – R packages for data science, including ggplot2, dplyr, tidyr, and purrr
* Shiny – An interactive web technology
* RMarkdown – Insert R code into markdown documents
* knitr – Dynamic reports combining R, TeX, Markdown & HTML
* packrat – Package dependency tool
* devtools – Package development tool

RStudio Cheat Sheets: [rstudio-ide.pdf](https://github.com/rstudio/cheatsheets/raw/master/rstudio-ide.pdf)

---

Programming fundamentals
====================================================


There are three concepts that are essential in any programming language:

* <font color='purple'>VARIABLES</font>

A variable is a named storage. Creating a variable is to reserve some space in memory. In R, the name of a variable can have letters, numbers, dot and underscore. However, a valid variable name cannot start with a underscore or a number, or start with a dot that is followed by a number.


* <font color='purple'>FUNCTIONS</font>

A function is a block of organized, reusable code that is used to perform a set of predefined operations. A function may take zero or more parameters and return a result.

<img src="./func.png" width="15%" style="display: block; margin: auto;" />


The way to use a function in R is:

**function.name(parameter1=value1, ...)**

In R, to get help information on a funciton, one may use the command: 

**?function.name**


* <font color='purple'>OPERATIONS</font>

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Assignment Operators in R</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Operator </th>
   <th style="text-align:center;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> &lt;-, = </td>
   <td style="text-align:center;"> Assignment </td>
  </tr>
</tbody>
</table>

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Arithmetic Operators in R</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Operator </th>
   <th style="text-align:center;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> \+ </td>
   <td style="text-align:center;"> Addition </td>
  </tr>
  <tr>
   <td style="text-align:center;"> \- </td>
   <td style="text-align:center;"> Subtraction </td>
  </tr>
  <tr>
   <td style="text-align:center;"> \* </td>
   <td style="text-align:center;"> Multiplication </td>
  </tr>
  <tr>
   <td style="text-align:center;"> / </td>
   <td style="text-align:center;"> Division </td>
  </tr>
  <tr>
   <td style="text-align:center;"> ^ </td>
   <td style="text-align:center;"> Exponent </td>
  </tr>
  <tr>
   <td style="text-align:center;"> %% </td>
   <td style="text-align:center;"> Modulus </td>
  </tr>
  <tr>
   <td style="text-align:center;"> %/% </td>
   <td style="text-align:center;"> Integer Division </td>
  </tr>
</tbody>
</table>

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Relational Operators in R</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Operator </th>
   <th style="text-align:center;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> &lt; </td>
   <td style="text-align:center;"> Less than </td>
  </tr>
  <tr>
   <td style="text-align:center;"> &gt; </td>
   <td style="text-align:center;"> Greater than </td>
  </tr>
  <tr>
   <td style="text-align:center;"> &lt;= </td>
   <td style="text-align:center;"> Less than or equal to </td>
  </tr>
  <tr>
   <td style="text-align:center;"> &gt;= </td>
   <td style="text-align:center;"> Greater than or equal to </td>
  </tr>
  <tr>
   <td style="text-align:center;"> == </td>
   <td style="text-align:center;"> Equal to </td>
  </tr>
  <tr>
   <td style="text-align:center;"> != </td>
   <td style="text-align:center;"> Not equal to </td>
  </tr>
</tbody>
</table>

<table class="table table-striped" style="width: auto !important; margin-left: auto; margin-right: auto;">
<caption>Logical Operators in R</caption>
 <thead>
  <tr>
   <th style="text-align:center;"> Operator </th>
   <th style="text-align:center;"> Description </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:center;"> ! </td>
   <td style="text-align:center;"> Logical NOT </td>
  </tr>
  <tr>
   <td style="text-align:center;"> &amp; </td>
   <td style="text-align:center;"> Element-wise logical AND </td>
  </tr>
  <tr>
   <td style="text-align:center;"> &amp;&amp; </td>
   <td style="text-align:center;"> Logical AND </td>
  </tr>
  <tr>
   <td style="text-align:center;"> \| </td>
   <td style="text-align:center;"> Element-wise logical OR </td>
  </tr>
  <tr>
   <td style="text-align:center;"> \|\| </td>
   <td style="text-align:center;"> Logical OR </td>
  </tr>
</tbody>
</table>

---


Start an R session
====================
**BEFORE YOU BEGIN, YOU NEED TO START AN R SESSION**

You can run this tutorial in an IDE (like Rstudio) on your laptop, or you can run R on the command-line on tadpole by logging into tadpole in a terminal and running the following commands:

> module load R
>
> R

**NOTE: Below, the text in the yellow boxes is code to input (by typing it or copy/pasting) into your R session, the text in the white boxes is the expected output.**

---

Topics covered in this introduction to R
====================================================
1. Basic data types in R
2. Basic data structures in R
3. Import and export data in R
4. Functions in R
5. Basic statistics in R
6. Simple data visulization in R
7. Install packages in R
8. Save data in R session
9. R markdown and R notebooks

---

Topic 1. Basic data types in R
====================================================

#### There are 5 basic atomic classes: numeric (integer, complex), character, logical


##### Examples of numeric values.


```{.r .colsel}
# assign number 150 to variable a.
a <- 150
a
```

```
## [1] 150
```

```{.r .colsel}
# assign a number in scientific format to variable b.
b <- 3e-2
b
```

```
## [1] 0.03
```
<br>

##### Examples of character values.


```{.r .colsel}
# assign a string "BRCA1" to variable gene
gene <- "BRCA1"
gene
```

```
## [1] "BRCA1"
```

```{.r .colsel}
# assign a string "Hello World" to variable hello
hello <- "Hello World"
hello
```

```
## [1] "Hello World"
```
<br>

##### Examples of logical values.


```{.r .colsel}
# assign logical value "TRUE" to variable brca1_expressed
brca1_expressed <- TRUE
brca1_expressed
```

```
## [1] TRUE
```

```{.r .colsel}
# assign logical value "FALSE" to variable her2_expressed
her2_expressed <- FALSE
her2_expressed
```

```
## [1] FALSE
```

```{.r .colsel}
# assign logical value to a variable by logical operation
her2_expression_level <- 0
her2_expressed <- her2_expression_level > 0
her2_expressed
```

```
## [1] FALSE
```
<br>

##### To find out the type of variable.


```{.r .colsel}
class(her2_expressed)
```

```
## [1] "logical"
```

```{.r .colsel}
# To check whether the variable is a specific type
is.numeric(gene)
```

```
## [1] FALSE
```

```{.r .colsel}
is.numeric(a)
```

```
## [1] TRUE
```

```{.r .colsel}
is.character(gene)
```

```
## [1] TRUE
```
<br>

##### In the case that one compares two different classes of data, the coersion rule in R is <font color='red'>logical -> integer -> numeric -> complex -> character</font> . The following is an example of converting a numeric variable to character.



```{.r .colsel}
b
```

```
## [1] 0.03
```

```{.r .colsel}
as.character(b)
```

```
## [1] "0.03"
```
<br>

What happens when one converts a logical variable to numeric?


```{.r .colsel}
# recall her2_expressed
her2_expressed
```

```
## [1] FALSE
```

```{.r .colsel}
# conversion
as.numeric(her2_expressed)
```

```
## [1] 0
```

```{.r .colsel}
her2_expressed + 1
```

```
## [1] 1
```
<br>

##### A logical *TRUE* is converted to integer 1 and a logical *FALSE* is converted to integer 0.

<br>

## Quiz 1

<div id="quiz1" class="quiz"></div>
<button id="submit1">Submit Quiz</button>
<div id="results1" class="output"></div>
<script>
quizContainer1 = document.getElementById('quiz1');
resultsContainer1 = document.getElementById('results1');
submitButton1 = document.getElementById('submit1');

myQuestions1 = [
  {
    question: "Create a variable a and set it to 3, and a variable b set to 'gene'. What is a + b?",
    answers: {
      a: "a",
      b: "3",
      c: "Gives an error",
      d: "4"
    },
    correctAnswer: "c"
  },
  {
    question: "Create another variable c set to FALSE. What is a + c?",
    answers: {
      a: "Gives an error",
      b: "3",
      c: "a",
      d: "4"
    },
    correctAnswer: "b"
  },
  {
    question: "What is 1 + TRUE?",
    answers: {
      a: "2",
      b: "1",
      c: "TRUE",
      d: "FALSE"
    },
    correctAnswer: "a"
  }
];

buildQuiz(myQuestions1, quizContainer1);
submitButton1.addEventListener('click', function() {showResults(myQuestions1, quizContainer1, resultsContainer1);});
</script>

---


Topic 2. Basic data structures in R
====================================================

<table class="table table-striped" style="font-size: 18px; width: auto !important; margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> Homogeneous </th>
   <th style="text-align:center;"> Heterogeneous </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 1d </td>
   <td style="text-align:center;"> Atomic vector </td>
   <td style="text-align:center;"> List </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2d </td>
   <td style="text-align:center;"> Matrix </td>
   <td style="text-align:center;"> Data frame </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Nd </td>
   <td style="text-align:center;"> Array </td>
   <td style="text-align:center;">  </td>
  </tr>
</tbody>
</table>
<br>


#### Atomic vectors: an atomic vector is a combination of multiple values(numeric, character or logical) in the same object. An atomic vector is created using the function c().


```{.r .colsel}
gene_names <- c("ESR1", "p53", "PI3K", "BRCA1", "EGFR")
gene_names
```

```
## [1] "ESR1"  "p53"   "PI3K"  "BRCA1" "EGFR"
```

```{.r .colsel}
gene_expression <- c(0, 100, 50, 200, 80)
gene_expression
```

```
## [1]   0 100  50 200  80
```
<br>

##### One can give names to the elements of an atomic vector.

```{.r .colsel}
# assign names to a vector by specifying them
names(gene_expression) <- c("ESR1", "p53", "PI3K", "BRCA1", "EGFR")
gene_expression
```

```
##  ESR1   p53  PI3K BRCA1  EGFR 
##     0   100    50   200    80
```

```{.r .colsel}
# assign names to a vector using another vector
names(gene_expression) <- gene_names
gene_expression
```

```
##  ESR1   p53  PI3K BRCA1  EGFR 
##     0   100    50   200    80
```
<br>

##### Or One may create a vector with named elements from scratch.

```{.r .colsel}
gene_expression <- c(ESR1=0, p53=100, PI3K=50, BRCA1=200, EGFR=80)
gene_expression
```

```
##  ESR1   p53  PI3K BRCA1  EGFR 
##     0   100    50   200    80
```
<br>

##### To find out the length of a vector:

```{.r .colsel}
length(gene_expression)
```

```
## [1] 5
```

   
##### <font color='red'>NOTE: a vector can only hold elements of the same type. If there are a mixture of data types, they will be coerced according to the coersion rule mentioned earlier in this documentation.</font>  

<br>

#### Factors: a factor is a special vector. It stores categorical data, which are important in statistical modeling and can only take on a limited number of pre-defined values. The function factor() can be used to create a factor.


```{.r .colsel}
disease_stage <- factor(c("Stage1", "Stage2", "Stage2", "Stage3", "Stage1", "Stage4"))
disease_stage
```

```
## [1] Stage1 Stage2 Stage2 Stage3 Stage1 Stage4
## Levels: Stage1 Stage2 Stage3 Stage4
```
<br>

##### In R, categories of the data are stored as factor levels. The function levels() can be used to access the factor levels.


```{.r .colsel}
levels(disease_stage)
```

```
## [1] "Stage1" "Stage2" "Stage3" "Stage4"
```

##### A function to compactly display the internal structure of an R object is str(). Please use str() to display the internal structure of the object we just created *disease_stage*. It shows that _disease_stage_ is a factor with four levels: "Stage1", "Stage2", "Stage3", etc... The integer numbers after the colon shows that these levels are encoded under the hood by integer values: the first level is 1, the second level is 2, and so on. Basically, when _factor_ function is called, R first scan through the vector to determine how many different categories there are, then it converts the character vector to a vector of integer values, with each integer value labeled with a category.



```{.r .colsel}
str(disease_stage)
```

```
##  Factor w/ 4 levels "Stage1","Stage2",..: 1 2 2 3 1 4
```


##### By default, R infers the factor levels by ordering the unique elements in a factor alphanumerically. One may specifically define the factor levels at the creation of the factor.



```{.r .colsel}
disease_stage <- factor(c("Stage1", "Stage2", "Stage2", "Stage3", "Stage1", "Stage4"), levels=c("Stage2", "Stage1", "Stage3", "Stage4"))
# The encoding for levels are different from above.
str(disease_stage)
```

```
##  Factor w/ 4 levels "Stage2","Stage1",..: 2 1 1 3 2 4
```

If you want to know the number of individuals at each levels, there are two functions: _summary_ and _table_.


```{.r .colsel}
summary(disease_stage)
```

```
## Stage2 Stage1 Stage3 Stage4 
##      2      2      1      1
```


```{.r .colsel}
table(disease_stage)
```

```
## disease_stage
## Stage2 Stage1 Stage3 Stage4 
##      2      2      1      1
```


## Quiz 2

<div id="quiz2" class="quiz"></div>
<button id="submit2">Submit Quiz</button>
<div id="results2" class="output"></div>
<script>
quizContainer2 = document.getElementById('quiz2');
resultsContainer2 = document.getElementById('results2');
submitButton2 = document.getElementById('submit2');

myQuestions2 = [
  {
    question: "Create a new factor with levels specified. What happens when the factor contains elements that are not included in the levels?",
    answers: {
      a: "A new level will be added to the factor",
      b: "A new element will be added to the factor that is an NA",
      c: "Nothing happens",
      d: "Gives a warning"
    },
    correctAnswer: "b"
  },
  {
    question: "You can type a '?' and then a function name to get help for that function. What does the 'relevel' function do?",
    answers: {
      a: "Sorts the factors",
      b: "Overwrites the factor levels",
      c: "Adds a new level to the factors",
      d: "Reorders the levels"
    },
    correctAnswer: "d"
  },
  {
    question: "What would the levels be for the following vector as a factor:<br>c('a','C','d','b',1,'!')",
    answers: {
      a: "a b C d 1 !",
      b: "! 1 a b d C",
      c: "1 a b C d !",
      d: "! 1 a b C d"
    },
    correctAnswer: "d"
  }
];

buildQuiz(myQuestions2, quizContainer2);
submitButton2.addEventListener('click', function() {showResults(myQuestions2, quizContainer2, resultsContainer2);});
</script>


<br>

---

#### Matrices: A matrix is like an Excel sheet containing multiple rows and columns. It is used to combine vectors of the same type.


```{.r .colsel}
col1 <- c(1,3,8,9)
col2 <- c(2,18,27,10)
col3 <- c(8,37,267,19)

my_matrix <- cbind(col1, col2, col3)
my_matrix
```

```
##      col1 col2 col3
## [1,]    1    2    8
## [2,]    3   18   37
## [3,]    8   27  267
## [4,]    9   10   19
```

##### One other way to create a matrix is to use *matrix()* function.


```{.r .colsel}
nums <- c(col1, col2, col3)
nums
```

```
##  [1]   1   3   8   9   2  18  27  10   8  37 267  19
```

```{.r .colsel}
matrix(nums, ncol=2)
```

```
##      [,1] [,2]
## [1,]    1   27
## [2,]    3   10
## [3,]    8    8
## [4,]    9   37
## [5,]    2  267
## [6,]   18   19
```


```{.r .colsel}
rownames(my_matrix) <- c("row1", "row2", "row3", "row4")
my_matrix
```

```
##      col1 col2 col3
## row1    1    2    8
## row2    3   18   37
## row3    8   27  267
## row4    9   10   19
```

```{.r .colsel}
t(my_matrix) # transposing the matrix
```

```
##      row1 row2 row3 row4
## col1    1    3    8    9
## col2    2   18   27   10
## col3    8   37  267   19
```

##### To find out the dimension of a matrix:


```{.r .colsel}
ncol(my_matrix)
```

```
## [1] 3
```

```{.r .colsel}
nrow(my_matrix)
```

```
## [1] 4
```

```{.r .colsel}
dim(my_matrix)
```

```
## [1] 4 3
```


##### Calculations with numeric matrices.


```{.r .colsel}
my_matrix * 3
```

```
##      col1 col2 col3
## row1    3    6   24
## row2    9   54  111
## row3   24   81  801
## row4   27   30   57
```

```{.r .colsel}
log10(my_matrix)
```

```
##           col1     col2     col3
## row1 0.0000000 0.301030 0.903090
## row2 0.4771213 1.255273 1.568202
## row3 0.9030900 1.431364 2.426511
## row4 0.9542425 1.000000 1.278754
```

Total of each row.

```{.r .colsel}
rowSums(my_matrix)
```

```
## row1 row2 row3 row4 
##   11   58  302   38
```

Total of each column.

```{.r .colsel}
colSums(my_matrix)
```

```
## col1 col2 col3 
##   21   57  331
```

##### There is a data structure _Array_ in R, that holds multi-dimensional (d > 2) data and is a generalized version of a matrix. *Matrix* is used much more commonly than *Array*, therefore we are not going to talk about *Array* here.


#### Data frames: a data frame is like a matrix but can have columns with different types (numeric, character, logical).

##### A data frame can be created using the function data.frame().


```{.r .colsel}
# creating a data frame using pre-defined vectors
patients_name=c("Patient1", "Patient2", "Patient3", "Patient4", "Patient5", "Patient6")
Family_history=c("Y", "N", "Y", "N", "Y", "Y")
patients_age=c(31, 40, 39, 50, 45, 65)
meta.data <- data.frame(patients_name=patients_name, disease_stage=disease_stage, Family_history=Family_history, patients_age=patients_age)
meta.data
```

```
##   patients_name disease_stage Family_history patients_age
## 1      Patient1        Stage1              Y           31
## 2      Patient2        Stage2              N           40
## 3      Patient3        Stage2              Y           39
## 4      Patient4        Stage3              N           50
## 5      Patient5        Stage1              Y           45
## 6      Patient6        Stage4              Y           65
```

###### To check whether a data is a data frame, use the function is.data.frame().


```{.r .colsel}
is.data.frame(meta.data)
```

```
## [1] TRUE
```


```{.r .colsel}
is.data.frame(my_matrix)
```

```
## [1] FALSE
```

###### One can convert a matrix object to a data frame using the function as.data.frame().


```{.r .colsel}
class(my_matrix)
```

```
## [1] "matrix" "array"
```


```{.r .colsel}
my_data <- as.data.frame(my_matrix)
class(my_data)
```

```
## [1] "data.frame"
```

##### A data frame can be transposed in the similar way as a matrix. However, the result of transposing a data frame might not be a data frame anymore.


```{.r .colsel}
my_data
```

```
##      col1 col2 col3
## row1    1    2    8
## row2    3   18   37
## row3    8   27  267
## row4    9   10   19
```


```{.r .colsel}
t(my_data)
```

```
##      row1 row2 row3 row4
## col1    1    3    8    9
## col2    2   18   27   10
## col3    8   37  267   19
```

##### A data frame can be extended.


```{.r .colsel}
# add a column that has the information on harmful mutations in BRCA1/BRCA2 genes for each patient.
meta.data
```

```
##   patients_name disease_stage Family_history patients_age
## 1      Patient1        Stage1              Y           31
## 2      Patient2        Stage2              N           40
## 3      Patient3        Stage2              Y           39
## 4      Patient4        Stage3              N           50
## 5      Patient5        Stage1              Y           45
## 6      Patient6        Stage4              Y           65
```

```{.r .colsel}
meta.data$BRCA <- c("YES", "NO", "YES", "YES", "YES", "NO")
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

##### A data frame can also be extended using the functions cbind() and rbind(), for adding columns and rows respectively. When using cbind(), the number of values in the new column must match the number of rows in the data frame. When using rbind(), the two data frames must have the same variables/columns.


```{.r .colsel}
# add a column that has the information on the racial information for each patient.
cbind(meta.data, Race=c("AJ", "AS", "AA", "NE", "NE", "AS"))
```

```
##   patients_name disease_stage Family_history patients_age BRCA Race
## 1      Patient1        Stage1              Y           31  YES   AJ
## 2      Patient2        Stage2              N           40   NO   AS
## 3      Patient3        Stage2              Y           39  YES   AA
## 4      Patient4        Stage3              N           50  YES   NE
## 5      Patient5        Stage1              Y           45  YES   NE
## 6      Patient6        Stage4              Y           65   NO   AS
```

```{.r .colsel}
# rbind can be used to add more rows to a data frame.
rbind(meta.data, data.frame(patients_name="Patient7", disease_stage="Stage4", Family_history="Y", patients_age=48, BRCA="YES"))
```

```
##   patients_name disease_stage Family_history patients_age BRCA
## 1      Patient1        Stage1              Y           31  YES
## 2      Patient2        Stage2              N           40   NO
## 3      Patient3        Stage2              Y           39  YES
## 4      Patient4        Stage3              N           50  YES
## 5      Patient5        Stage1              Y           45  YES
## 6      Patient6        Stage4              Y           65   NO
## 7      Patient7        Stage4              Y           48  YES
```

##### One may use the function *merge* to merge two data frames horizontally, based on one or more common key variables.


```{.r .colsel}
expression.data <- data.frame(patients_name=c("Patient3", "Patient4", "Patient5", "Patient1", "Patient2", "Patient6"), EGFR=c(10, 472, 103784, 1782, 187, 18289), TP53=c(16493, 72, 8193, 1849, 173894, 1482))
expression.data
```

```
##   patients_name   EGFR   TP53
## 1      Patient3     10  16493
## 2      Patient4    472     72
## 3      Patient5 103784   8193
## 4      Patient1   1782   1849
## 5      Patient2    187 173894
## 6      Patient6  18289   1482
```

```{.r .colsel}
md2 <- merge(meta.data, expression.data, by="patients_name")
md2
```

```
##   patients_name disease_stage Family_history patients_age BRCA   EGFR   TP53
## 1      Patient1        Stage1              Y           31  YES   1782   1849
## 2      Patient2        Stage2              N           40   NO    187 173894
## 3      Patient3        Stage2              Y           39  YES     10  16493
## 4      Patient4        Stage3              N           50  YES    472     72
## 5      Patient5        Stage1              Y           45  YES 103784   8193
## 6      Patient6        Stage4              Y           65   NO  18289   1482
```


Save your workspace to a file so we can load it for day 2:

```{.r .colsel}
save.image("day1.RData")
```

<br>

## Quiz 3

<div id="quiz3" class="quiz"></div>
<button id="submit3">Submit Quiz</button>
<div id="results3" class="output"></div>
<script>
quizContainer3 = document.getElementById('quiz3');
resultsContainer3 = document.getElementById('results3');
submitButton3 = document.getElementById('submit3');

myQuestions3 = [
  {
    question: "Find a function to add up the EGFR column in md2. What is the total?",
    answers: {
      a: "124524",
      b: "124526",
      c: "124528",
      d: "124530"
    },
    correctAnswer: "a"
  },
  {
    question: "Multiply my_matrix by itself, sum each column, and then use the 'mean' function to find the mean:",
    answers: {
      a: "24799.33",
      b: "24797.33",
      c: "24798.33",
      d: "24796.33"
    },
    correctAnswer: "c"
  }
];

buildQuiz(myQuestions3, quizContainer3);
submitButton3.addEventListener('click', function() {showResults(myQuestions3, quizContainer3, resultsContainer3);});
</script>

## HOMEWORK
Using the **mtcars** built-in dataset (Type "mtcars" to see it), add a row that has the averages of each column and name it "Averages". Now, add a column to mtcars called "hp.gt.100" that is TRUE or FALSE depending on whether the horsepower (hp) for that car is greater than 100 or not.
