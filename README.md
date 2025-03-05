# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}

```

“I certify that I have listed all sources used to complete this exercise, including the use
of any Large Language Models. All of the work is my own, except where stated
otherwise. I am aware that plagiarism carries severe penalties and that if plagiarism is
suspected, charges may be filed against me without prior notice.”-Andrew Thomas

Sources:

No.1-https://www.youtube.com/watch?v=Hn-dG2wWaZ0-Learned about the Master theorem here

# Run Time Analysis

Looking at the first part of the code we have a if statment which yields a constant time complexity of $O(1)$ and we also have a variable declaration for count which is also $O(1)$
```Javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n/3)
        var count=0;

        ...
    }
}


```

Then accounting for all the recursive calls, which there are 3 of we have $T(n)=3T(\frac{n}{3})$

```Javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```
And finally accounting for the for-loops we have two loops which go to $n^2$ and one loop which goes to $n$. Additionally we have one operation in the inner most loop which gives us $T(n)=n^2*n*n^2*1=n^5$. 

Then altogether have the following recurance relation:

$T(n)=3T(\frac{n}{3})+n^5+c$.

Then instead of our usual mathematical calucaltions we can shorten our work load by using The Master Theorem, which states:

If $T(n)=aT(\frac{n}{b})+n^d$, then:

$$
T(n) =
\begin{cases} 
\Theta(n^d) & \text{if } \frac{a}{b^d} < 1 \\ 
\Theta(n^d \log n) & \text{if } \frac{a}{b^d} = 1 \\ 
\Theta(n^{\log_b a}) & \text{if } \frac{a}{b^d} > 1
\end{cases}
$$

We then see that $a=3,b=3,d=5$, which yields $\frac{a}{b^d}=\frac{3}{3^5}<1\implies T(n)\in \Theta(n^5)$.

Then because $T(n)\in \Theta(n^5)$

$\Longleftrightarrow T(n)\in O(n^5),T(n)\in \Omega(n^5)$.
Thus $ T(n)\in O(n^5 ) $ and forms a tight bound for our algorithm.
Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.
