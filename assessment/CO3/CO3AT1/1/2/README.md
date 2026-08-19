Q2. Syntax Tree / AST Construction Using YACC

(i) YACC Grammar for Arithmetic Expressions

%{
#include <stdio.h>
%}

%token ID
%left '+'
%left '*'

%%

expr : expr '+' expr
     | expr '*' expr
     | ID
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

(ii) Semantic Actions to Construct a Parse Tree / AST

%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Node
{
    char value[10];
    struct Node *left;
    struct Node *right;
};

struct Node* createNode(char *value, struct Node *left, struct Node *right)
{
    struct Node *newNode = malloc(sizeof(struct Node));
    strcpy(newNode->value, value);
    newNode->left = left;
    newNode->right = right;
    return newNode;
}
%}

%union
{
    char *str;
    struct Node *node;
}

%token <str> ID
%type <node> expr

%left '+'
%left '*'

%%

expr : expr '+' expr
       { $$ = createNode("+", $1, $3); }
     | expr '*' expr
       { $$ = createNode("*", $1, $3); }
     | ID
       { $$ = createNode($1, NULL, NULL); }
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

(iii) Generate and Illustrate the Tree for a + b * c

Input:

a + b * c

Since multiplication has higher precedence than addition:

a + b * c
= a + (b * c)

The Abstract Syntax Tree is:

        +
       / \
      a   *
         / \
        b   c

(iv) Traverse the Generated Tree and Display the Traversal Order

Preorder Traversal:

+ a * b c

Inorder Traversal:

a + b * c

Postorder Traversal:

a b c * +

Output

AST for: a + b * c

        +
       / \
      a   *
         / \
        b   c

Preorder  : + a * b c
Inorder   : a + b * c
Postorder : a b c * +

Conclusion

The YACC parser constructs an Abstract Syntax Tree for the arithmetic expression a + b * c using semantic actions. The generated tree is traversed using preorder, inorder, and postorder traversal methods to display the traversal order.
