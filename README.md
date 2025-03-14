“I certify that I have listed all sources used to complete this exercise, including the use
of any Large Language Models. All of the work is my own, except where stated
otherwise. I am aware that plagiarism carries severe penalties and that if plagiarism is
suspected, charges may be filed against me without prior notice.”
-Andrew Thomas
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
And finally accounting for the for-loops we have two loops which go to $n^2$ and one loop which goes to $n$. Additionally we have one operation in the inner most loop which gives us,

$T(n)=n^2\cdot n\cdot n^2\cdot1=n^5$. 

Then altogether have the following recurance relation:

$T(n)=3T(\frac{n}{3})+n^5+c$.

### The Work
$T(n)=3T(\frac{n}{3})+n^5+c$

--------------------
Solving for $3T(\frac{n}{3})$:

$3T(\frac{n}{3})=3T(\frac{n}{3^2})+(\frac{n}{3})^5+c$

---
Plugging this in we have:

$$3\cdot [3T(\frac{n}{3^2})+(\frac{n}{3})^5+c]+n^5+c
  $$

$$= 3^2\cdot T(\frac{n}{3^2})+n^5+\frac{n^5}{3^4}+4c$$

---
Solving for $T(\frac{n}{3^2})$:

$T(\frac{n}{3^2})=3T(\frac{n}{3^3})+\big(\frac{n}{3^2}\big)^5+c$

---

Plugging this in we have:

$3^2\cdot[3T(\frac{n}{3^3})+\big(\frac{n}{3^2}\big)^5+c]+n^5+\frac{n^5}{3^4}+4c$

$=3^3T(\frac{n}{3^3})+n^5+\frac{n^5}{3^4}+\frac{n^5}{3^5}+13c$

Generaling this pattern we have,

$$3^i\cdot T(\frac{n}{3^i})+\sum_{j=1}^{i}n^5\Big(\frac{3^{j-1}}{(3^{j-1})^5}\Big)+c$$

Solving for when our recursion ends we have:

$$\frac{n}{3^i}=1\\
i=log_3(n)$$

Substituting in i:

$$=3^{log_3(n)}\cdot T(\frac{n}{3^{log_3(n)}})+\sum_{j=1}^{log_3(n)}n^5\Big(\frac{3^{log_3(n)-1}}{(3^{log_3(n)-1})^5}\Big) $$

$$= n\cdot 1 +\frac{n^5\Big(1-\frac{3^{log_3(n)-1}}{(3^{log_3(n)-1})^5}\Big)^{log_3(n)}}{1-\frac{log_3(n)-1}{(3^{log_3(n)-1})^5}}$$

$$=n+1+\frac{n^5\Big(1-\frac{n/3}{\frac{n^5}{3^5}}\Big)^{log_3(n)}}{1-\frac{n/3}{\frac{n^5}{3^5}}}$$

$$=
n+1+\frac{n^5(1-\frac{3^4}{n^4})^{log_3(n)}}{1-\frac{3^4}{n^4}}$$

$$=n+1+\frac{n^5(1-\frac{3^{4log_3(n)}}{n^{4log_3(n)}})}{1-\frac{3^4}{n^4}}$$

$$=n+1+\frac{n^5-4n^2}{1-\frac{3^4}{n^4}}$$

$$n+1 +\frac{n^5}{1-\frac{3^4}{n^4}}-\frac{4n^2}{1-\frac{3^4}{n^4}}$$

Dropping the lower order and constant terms we have:

$$=n^5\in \theta(n^5) \implies T(n)\in O(n^5), \hspace{2mm}T(n)\in \Omega(n^5)$$



### Check

Checking our usual mathematical calucaltions by using The Master Theorem, which states:

If $T(n)=aT(\frac{n}{b})+n^d$, then:

$$
T(n) =
\begin{cases} 
\Theta(n^d), & \text{if } \frac{a}{b^d} < 1 \\ 
\Theta(n^d \log(n)), & \text{if } \frac{a}{b^d} = 1 \\ 
\Theta(n^{\log_b(a)}), & \text{if } \frac{a}{b^d} > 1 
\end{cases}
$$

 

We then see that $a=3,b=3,d=5$, which yields $\frac{a}{b^d}=\frac{3}{3^5}<1\implies T(n)\in \theta(n^5)$.

Then because $T(n)\in \theta(n^5)$

$\Longleftrightarrow T(n)\in O(n^5),T(n)\in \Omega(n^5)$.
Thus $T(n)\in O(n^5)$ and forms a tight bound for our algorithm. This verifies the work above.
