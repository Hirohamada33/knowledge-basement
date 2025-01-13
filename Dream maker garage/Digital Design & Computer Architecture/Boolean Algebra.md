

##### De Morgan's Theorem
$${\displaystyle {\begin{aligned}{\overline {A\cup B}}&={\overline {A}}\cap {\overline {B}}\\{\overline {A\cap B}}&={\overline {A}}\cup {\overline{B}}\end{aligned}}}$$

This applies to all POS (product of sum) and SOP (sum of product) conversion. 

Below are some common results of boolean algebra calculation.
![[截圖 2024-10-24 上午11.35.21.png]]


##### Canonical form
In an expression of a canonical form, every variable appears in every term. It is not an efficient expression (e.g., `1 = AB + A(~B) + (~A)B + (~A)(~B)`), however is unique henceforth useful in checking if two functions are identical in truth table. 

The product term in a canonical SOP expression is called a "minterm" (c.f. "maxterm" for POS). 
(If any of the minterm is a 1, then the output will be one. Likewise, if any of the maxterm is 0, it will force the function to be 0.)
![[截圖 2024-10-24 上午11.38.26.png]]

