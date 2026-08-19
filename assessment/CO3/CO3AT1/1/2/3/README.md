Q3. Syntax-Directed Translation Using Synthesized Attributes in YACC
(i) Syntax-Directed Definition (SDD) Using YACC Semantic Values
%{
#include <stdio.h>
%}

%token NUMBER
%left '+'
%left '*'

%%

expr : expr '+' expr   { $$ = $1 + $3; }
     | expr '*' expr   { $$ = $1 * $3; }
     | NUMBER          { $$ = $1; }
     ;

%%

int main()
{
    printf("Enter expression: ");
    yyparse();
    return 0;
}

int yyerror(char *s)
{
    printf("Invalid expression\n");
    return 0;
}
(ii) Evaluate the Expression
Input:
2 * 3 + 4
Since multiplication has higher precedence than addition:
2 * 3 + 4
= (2 * 3) + 4
= 6 + 4
= 10
Result:
10
(iii) Propagation of Attribute Values During Reductions
For the expression 2 * 3 + 4:
2 → expr
$1 = 2
$$ = 2

3 → expr
$1 = 3
$$ = 3

expr * expr
$1 = 2
$3 = 3
$$ = $1 * $3
$$ = 2 * 3
$$ = 6

4 → expr
$1 = 4
$$ = 4

expr + expr
$1 = 6
$3 = 4
$$ = $1 + $3
$$ = 6 + 4
$$ = 10
Final synthesized attribute:
$$ = 10
(iv) Bottom-Up Evaluation Process
The parser evaluates the expression from bottom to top:
Input: 2 * 3 + 4

Step 1: 2 → expr
        Attribute = 2

Step 2: 3 → expr
        Attribute = 3

Step 3: expr * expr
        2 * 3 = 6
        Attribute = 6

Step 4: 4 → expr
        Attribute = 4

Step 5: expr + expr
        6 + 4 = 10
        Attribute = 10
Output
Enter expression: 2 * 3 + 4
Result = 10
Conclusion
The YACC parser evaluates the arithmetic expression using synthesized attributes and bottom-up parsing. The semantic values are propagated using $$, $1, and $3. For the expression 2 * 3 + 4, the final computed result is 10.
