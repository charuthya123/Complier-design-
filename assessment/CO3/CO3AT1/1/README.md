Q1. Calculator Compiler Using YACC

(i) YACC Grammar for Arithmetic Operators + and *

%{
  
#include <stdio.h>
  
%}
  

  
%token NUMBER
  
%left '+'
  
%left '*'
  

  
%%
  

  
expr : expr '+' expr
  
     | expr '*' expr
  
     | NUMBER
  
     ;
  

  
%%
  

  
int main()
  
{
  
    yyparse();
  
    return 0;
  
}
  

  
int yyerror(char *s)
  
{
  
    printf("Invalid expression\n");
  
    return 0;
  
}

(ii) Semantic Actions to Compute Expression Values

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
  
    yyparse();
  
    return 0;
  
}
  

  
int yyerror(char *s)
  
{
  
    printf("Invalid expression\n");
  
    return 0;
  
}

(iii) Evaluation of the Input Expression

Input:

3 + 4 * 5

Since multiplication has higher precedence than addition:

3 + 4 * 5
  
= 3 + (4 * 5)
  
= 3 + 20
  
= 23

Result:

23

(iv) Display the Computed Result

%{
  
#include <stdio.h>
  
%}
  

  
%token NUMBER
  
%left '+'
  
%left '*'
  

  
%%
  

  
input : expr { printf("Result = %d\n", $1); }
  
      ;
  

  
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

Output

Enter expression: 3 + 4 * 5
  
Result = 23

Conclusion

The YACC parser evaluates the arithmetic expression 3 + 4 * 5 using semantic actions and operator precedence. The computed result is 23.
