# Massign: Simple Matrix Construction in R
Erik-Jan van Kesteren  



###[Back to index](../index.html)



## The problem
I'm currently (finally) learning more about linear algebra, statistical optimisation, and other matrix-related topics. While learning about such abstract topics, for me it really helps to convert the material into small `R` programs. While doing this, I stumbled upon a problem: __Matrix construction in `R` kind of sucks__. Why? Look at this:


```r
M <- matrix(c(1, 0.2, -0.3, 0.4,
              0.2, 1, 0.6, -0.4,
              -0.3, 0.6, 1, 0.4,
              0.4, -0.4, 0.4, 1),
            nrow = 4,
            ncol = 4,
            byrow = TRUE)
```

If I want to create a matrix, I need to (a) create a full vector of values to put in the matrix, (b) decide into how many rows/columns I want to put these values, and (c) decide whether to fill these values in a columnwise (default) or rowwise manner. This last step in particular is a nuisance, because the default makes sure we cannot have any form of "what you see is what you get" (WYSIWYG) in our script.

## The solution
To solve this issue for people who want to rapidly create legible matrices, I created the package `Massign`. This package has only one operator, `%<-%`, and it works as follows to create the same matrix as above:


```r
M %<-% "   1
         0.2,    1
        -0.3,  0.6,    1
         0.4, -0.4,  0.4,    1"
```

Neat right? There are a few features to it:

## WYSIWYG Matrix Construction

In it's most basic form, `Massign` makes for legible code because of the "what you see is what you get" nature of the matrix construction function. 


```r
M %<-% "   1,  0.2, -0.3,  0.4
         0.2,    1,  0.6, -0.4
        -0.3,  0.6,    1,  0.4
         0.4, -0.4,  0.4,    1"
M
```

```
##      [,1] [,2] [,3] [,4]
## [1,]  1.0  0.2 -0.3  0.4
## [2,]  0.2  1.0  0.6 -0.4
## [3,] -0.3  0.6  1.0  0.4
## [4,]  0.4 -0.4  0.4  1.0
```

## Automatic Symmetric Construction

As shown before, when you enter a lower triangular matrix, `Massign` automatically creates a symmetric matrix. This is a major feature, because properly creating the symmetric elements is not simple using the default `matrix()` function.


## Variables Allowed

Because `Massign` constructs a `matrix()` call under the hood and evaluates it in the environment in which the function is called, it is allowed to enter variables inside the text string:


```r
phi <- 1.5
M %<-% "1,     1,     1
        1,   phi, phi^2
        1, phi^2, phi^4"
M
```

```
##      [,1] [,2]   [,3]
## [1,]    1 1.00 1.0000
## [2,]    1 1.50 2.2500
## [3,]    1 2.25 5.0625
```

For the same reason, complex numbers work too. It does only work with numbers, though, so for character matrices you're stuck with the `matrix()` function for now.

# Conclusion
The `%<-%` operator in `Massign` makes life a little easier for `R` programmers who want to quickly construct a relatively small matrix for prototyping or learning. Take this code piece for generating an 8-dimensional multivariate normal distribution with positive correlations:


```r
library(Massign)
library(MASS)

S %<-% "  1,
         .5,   1
         .5,  .5,   1
         .5,  .5,  .5,   1
         .5,  .5,  .5,  .5,   1
         .6,  .6,  .6,  .6,  .6,   1
         .5,  .5,  .5,  .5,  .5,  .5,   1
         .4,  .4,  .4,  .4,  .4,  .4,  .4,  1"

X <- mvrnorm(10, mu = rep(0,8), Sigma = S)
```

The package is on [CRAN](https://cran.r-project.org/package=Massign), so you can install it as follows:


```r
install.packages("Massign")
```

If you have any complaints, tips, issues, or suggestions, you can [submit an issue](https://github.com/vankesteren/Massign/issues) on the project's [GitHub](https://github.com/vankesteren/Massign) page. Here you can also view the source code of the package!


###[Back to index](../index.html)
