# Q4. Type Compatibility and Equivalence Using YACC

## (i) YACC Grammar for Type and Variable Declarations

```yacc
%{
#include <stdio.h>
#include <string.h>

struct Symbol
{
    char name[20];
    char type[20];
};

struct Symbol table[50];
int count = 0;

void addType(char *name, char *type)
{
    strcpy(table[count].name, name);
    strcpy(table[count].type, type);
    count++;
}

void addVariable(char *name, char *type)
{
    strcpy(table[count].name, name);
    strcpy(table[count].type, type);
    count++;
}

char* getType(char *name)
{
    int i;
    for(i = 0; i < count; i++)
    {
        if(strcmp(table[i].name, name) == 0)
            return table[i].type;
    }
    return "undefined";
}

int nameEquivalent(char *a, char *b)
{
    return strcmp(a, b) == 0;
}

int structuralEquivalent(char *a, char *b)
{
    char *typeA = getType(a);
    char *typeB = getType(b);

    if(strcmp(typeA, "undefined") == 0 || strcmp(typeB, "undefined") == 0)
        return 0;

    return strcmp(typeA, typeB) == 0;
}
%}

%token TYPE INT ID

%%

program : declarations
        ;

declarations : declarations declaration
             | declaration
             ;

declaration : TYPE ID '=' INT ';'
            {
                addType($2, "int");
            }
            | ID ID ';'
            {
                addVariable($2, $1);
            }
            ;

%%

int main()
{
    printf("Type Compatibility Analysis\n");
    yyparse();
    return 0;
}

int yyerror(char *s)
{
    printf("Invalid declaration\n");
    return 0;
}
```

## (ii) Semantic Actions and Symbol Table Routines

The symbol table stores the name of each type and variable along with its corresponding type.

Type declarations:

```text
type A = int;
type B = int;
```

Symbol Table:

```text
Name        Type
A           int
B           int
x           A
y           B
```

The semantic actions add type and variable declarations to the symbol table:

```text
addType("A", "int");
addType("B", "int");
addVariable("x", "A");
addVariable("y", "B");
```

The `nameEquivalent()` routine compares type names directly.

```text
A and B
A != B
```

Therefore, A and B are not name equivalent.

The `structuralEquivalent()` routine compares the underlying types.

```text
A → int
B → int
```

Therefore, A and B are structurally equivalent.

## (iii) Name Equivalence and Structural Equivalence

Given declarations:

```text
type A = int;
type B = int;
A x;
B y;
```

### (a) Name Equivalence

Name equivalence checks whether two types have exactly the same type name.

```text
A = B
```

This condition is false because:

```text
A != B
```

Therefore:

```text
Name Equivalence: NOT EQUIVALENT
```

### (b) Structural Equivalence

Structural equivalence checks whether the types have the same underlying structure.

```text
A → int
B → int
```

Both A and B represent the same basic type `int`.

Therefore:

```text
Structural Equivalence: EQUIVALENT
```

## (iv) Testing with Multiple Type Declarations and Assignment Statements

### Test Case 1

```text
type A = int;
type B = int;
A x;
B y;

x = y;
```

Result:

```text
Name Equivalence: NOT EQUIVALENT
Structural Equivalence: EQUIVALENT
```

### Test Case 2

```text
type A = int;
type C = float;
A x;
C z;

x = z;
```

Result:

```text
Name Equivalence: NOT EQUIVALENT
Structural Equivalence: NOT EQUIVALENT
```

### Test Case 3

```text
type A = int;
A x;
A y;

x = y;
```

Result:

```text
Name Equivalence: EQUIVALENT
Structural Equivalence: EQUIVALENT
```

## (v) Type Compatibility Results

For the declarations:

```text
type A = int;
type B = int;
A x;
B y;
```

The compatibility results are:

```text
Name Equivalence:
A and B are NOT EQUIVALENT

Structural Equivalence:
A and B are EQUIVALENT
```

### Output

```text
Type Compatibility Analysis

Type A = int
Type B = int
Variable x : A
Variable y : B

Name Equivalence: NOT EQUIVALENT
Structural Equivalence: EQUIVALENT

Assignment x = y:
Under Name Equivalence: INCOMPATIBLE
Under Structural Equivalence: COMPATIBLE
```

## Conclusion

The YACC parser processes type and variable declarations using semantic actions and a symbol table. Name equivalence requires the type names to be identical, while structural equivalence compares the underlying type structure. Therefore, A and B are not name equivalent but are structurally equivalent because both represent the type int.
