[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-718a45dd9cf7e7f842a935f5ebbe5719a5e09af4491e668f4dbf3b35d5cca122.svg)](https://classroom.github.com/online_ide?assignment_repo_id=12094574&assignment_repo_type=AssignmentRepo)
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

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

# Analysis
I first broke the function down into steps
1. If $n <= 1$, return - Constant time of operation, 1?
2. Recurse with $(n/3)$ - Time of $T(n/3)$
3. Recurse with $(n/3)$ - Time of $T(n/3)$
4. Repeat Step 5 $n^2$ times
5. Repeat Step 6 $n$ times
6. Repeat Step 7 $n^2$ times
7. Increment count variable - Constant Time
8. Recurse with $(n/3)$ - Time of $T(n/3)$

Together, steps 4-7 have a time of $n^2(n(n^2(1))) = n^5$. Not sure a good way to show that with the above framework. This leads to the piecewise function
        
$T\left(n\right) =
  \begin{cases}
    1 &\text{if } (n) \leq 1\\
    3T \left(\frac{n}{3}\right)+n^5s &\text{else }
  \end{cases}$

First attempt, before I came back and figured out proper piecewise notation <br>
$T(n) = 1$, if $n \leq 1$<br>
$T(n) = 3T(\frac{n}{3})+n^5$, if $n > 1$

Then, I approached this as a formal proof. <br>
$T\left(n\right) = 3T\left(\frac{n}{3}\right)+n^5$<br>
$ = 3[3T\left(\frac{\frac{n}{3}}{3}\right)+\frac{n}{3}^5]+n^5$<br>
$ = 9T\left(\frac{n}{9}\right)+3(\frac{n}{3})^5+n^5$<br>
$ = 9[T\left(\frac{\frac{n}{9}}{3}\right)+\frac{n}{9}^5]+3(\frac{n}{3})^5+n^5$<br>
$ = 9T\left(\frac{n}{27}\right)+9(\frac{n}{9})^5+3(\frac{n}{3})^5+n^5$<br>
$ = 3^iT\left(\frac{n}{3^i}\right)+3^{i-1}(\frac{n}{3^{i-1}})^{5}+3^{i-2}(\frac{n}{3^{i-2}})^{5}+3^{i-3}(\frac{n}{3^{i-3}})^5$<br>

Couldn't figure out how to make that 3^(i-1) in markdown before class, then fixed it!

This is where it got a little complicated, and I needed help from Ali in the lab. He recommended I use Sigma Notation to represent the $n^5$ terms. I then spent a bit to figure out how to write that in markdown.

$ = 3^iT\left(\frac{n}{3^i}\right)+\sum_{x=1}^{i} 3^{x-1}(\frac{n}{3^{x-1}})^5$


Then, solved $\frac{n}{3^i}=1$ for $i$, so that I can plug in $i$ and get $T(1)$ and simplify the equation. This resulted in: <br>
$ i = log_3(n)$ <br>
Plug that into the equation and it simplifies out to: <br>
$ T\left(n\right) = n + \sum_{x=1}^{log_3(n)} 3^{x-1}(\frac{n}{3^{x-1}})^5$

At this point, I had gotten stuck until we went over it in class together, only able to see that $n^5$ should be a part of the final result for the asymptotic analysis. But, it could be rearranged like this, since $n^5$ is a factor of every term in the sum.

$ T\left(n\right) = n + n^5\sum_{x=1}^{log_3(n)} 3^{x-1}(\frac{1}{3^{x-1}})^5$

Like that, it is much easier to see that there are $log_3(n)$ terms of $n^5$ order in the sum. From there, we find that: <br>
$T\left(n\right) \in \Theta(n^5log_3(n))$

# Questions
1. I wrote the way I had written the sum notation on my own before we went over it in class. I think this is equivalent to what we did in class. Was there a specific reason you did the sum from $j=0$ to $log_3(n)-1$?

2. In class you pulled out the constant in a step between the last two steps I did. Was it alright to skip that step the way I did? I kinda hand-waved it because I thought it was clear enough and wasn't sure how to represent it.

3. Were there any issues with the clarity of the mathematical notation? After class, I spent about an hour in total experimenting with it to learn it a little better. It was fun and fairly simple now that I've got the hang of it and found a few resources with good information, which I've listed below.

# References
1. Ali helped me with pointing me towards sum notation for adding up the $n^5$ terms. I then found a detailed explanation on it here I used as a refresher. https://www.varsitytutors.com/hotmath/hotmath_help/topics/sigma-notation-of-a-series#:~:text=A%20series%20can%20be%20represented,goes%20from%201%20to%206%20.

2. I had been trying to figure out the syntax for LaTex using these resources, but copied too much from the sites and couldn't figure out what was broken until class, where you gave us that example which I could extrapolate out from.
https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions
https://en.wikibooks.org/wiki/LaTeX/Mathematics
https://vincenttam.github.io/blog/2014/09/08/another-way-of-writing-piecewise-functions/
https://github.com/mattcone/markdown-guide/blob/master/cheat-sheet.md
