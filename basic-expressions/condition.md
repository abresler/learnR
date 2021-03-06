

# Condition

It is common that the logic of a program is not perfectly sequential but contains several branches dependent on certain condition. In R, `if-else` expressions can be used to branch the logic flow.

## if-else

The `if` expression works with single-element logical vectors. For example we can write a single `if` expression:


```r
num <- 1
num > 0
```

```
[1] TRUE
```

```r
if(num > 0) {
  num + 1
}
```

```
[1] 2
```

or we can write a `if-else` expression.


```r
if(num > 0) {
  num + 1
} else {
  num - 1
}
```

```
[1] 2
```

or we can add `else if` to create more logical branches.


```r
num <- -1
if(num > 0) {
  cat("The number is greater than 0")
} else if(num < 0) {
  cat("The number is less than 0")
} else {
  cat("The number is 0")
}
```

```
The number is less than 0
```

Its workflow is very simple: 

- Evaluate the expression after the first `if`. 
- If it is `TRUE`, then evaluate its corresponding expression, otherwise evaluate the expression in the next `else if` branch. 
- Repeat
- If the logical conditions in all `if` and `else if` branches are violated, then evaluate the expression in `else` branch, if any.

Since `if` is in essence a primitive function in R, `if` expression can be used as an inline expression too.

Here we can remove the braces if only a single expression is to be evaluated.


```r
num <- 1
p <- if(num > 1) num + 1 else num -1
p
```

```
[1] 0
```

Its syntax allows the expression to be written in one line.


```r
num <- 1
p <- if(num > 1) 1 else if(num >= -2 || num <= 1) 0 else num -1
p
```

```
[1] 0
```

Since `if` only works with single-entry logical vectors, it will ignore other values if a multi-entry logical vector is supplied.


```r
num <- c(1,2,3)
if(num > 2) {
  cat("num > 2!")
}
```

```
Warning: the condition has length > 1 and only the first element will be
used
```

The expression throws a warning saying that only the first element (`1 > 2`) will be used. That is why the expression does not return anything.

Some logical functions are useful to avoid such ambiguity. If what we really mean is to print the message if *any* single value is greater than 2, we should call `any()`.


```r
any(c(TRUE,FALSE,FALSE))
```

```
[1] TRUE
```

The function returns `TRUE` if *any* of the value in the vector is `TRUE`, otherwise returns `FALSE`.


```r
if(any(num > 2)) {
  cat("num > 2!")
}
```

```
num > 2!
```

If we mean to print the first message if *all* values are greater than 2, we should instead call `all()`.


```r
all(c(TRUE,FALSE,FALSE))
```

```
[1] FALSE
```

The function returns `TRUE` if *all* the values in the vector is `TRUE`, otherwise returns `FALSE`.


```r
if(all(num > 2)) {
  cat("num > 2!")
} else {
  cat("Not all values are greater than 2!")
}
```

```
Not all values are greater than 2!
```

In conclusion, every time we use `if` expression to branch the workflow, we should ensure that the condition is a single-value logical vector, or otherwise something unexpected may happen.

## ifelse

An alternative method to branch the workflow is the function `ifelse`. It is simple a function that accepts a single-value logical vector as the test condition. If it is `TRUE` then the second argument will be returned otherwise the third.


```r
num1 <- 2
num2 <- 3
num3 <- ifelse(num1 > 0 & num2 < 0, 1, 0)
num3
```

```
[1] 0
```

However, this function is not recommend because it only supports single-value vector in all arguments.


```r
ifelse(TRUE, c(1,2), c(2,3))
```

```
[1] 1
```

We can always rewrite the function to its equivalent `if` expression which is able to return any object.


```r
if(TRUE) c(1,2) else c(2,3)
```

```
[1] 1 2
```
