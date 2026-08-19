# Q5. Automatic Type Conversion (Coercion) Using YACC

## (i) YACC Grammar for Variable Declarations and Assignment Statements

```yacc
%{
#include <stdio.h>
#include <string.h>

struct Symbol
{
    char name[20];
    char type[10];
};

struct Symbol table[50];
int count = 0;

void addSymbol(char *name, char *type)
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

void checkAssignment(char *left, char *right)
{
    char *leftType = getType(left);
    char *rightType = getType(right);

    printf("Left Variable  : %s (%s)\n", left, leftType);
    printf("Right Variable : %s (%s)\n", right, rightType);

    if(strcmp(leftType, "float") == 0 && strcmp(rightType, "int") == 0)
    {
        printf("Type Conversion: int -> float\n");
        printf("Assignment: Compatible\n");
    }
    else if(strcmp(leftType, rightType) == 0)
    {
        printf("Type Conversion: None\n");
        printf("Assignment: Compatible\n");
    }
    else
    {
        printf("Assignment: Incompatible\n");
    }
}
%}

%token FLOAT INT ID

%%

program : declarations statements
        ;

declarations : declarations declaration
             | declaration
             ;

declaration : FLOAT ID ';'
            {
                addSymbol($2, "float");
            }
            | INT ID ';'
            {
                addSymbol($2, "int");
            }
            ;

statements : statements statement
           | statement
           ;

statement : ID '=' ID ';'
          {
              checkAssignment($1, $3);
          }
          ;

%%

int main()
{
    printf("Type Conversion Analysis\n");
    yyparse();
    return 0;
}

int yyerror(char *s)
{
    printf("Invalid statement\n");
    return 0;
}
```

## (ii) Analysis of the Program Fragment

Input:

```text
float temperature;
int sensor_value;

temperature = sensor_value;
```

The symbol table contains:

```text
Name             Type
temperature      float
sensor_value     int
```

The assignment is:

```text
temperature = sensor_value;
```

The left-hand side variable `temperature` is of type `float`.

The right-hand side variable `sensor_value` is of type `int`.

Therefore, the compiler performs an `int` to `float` conversion.

## (iii) Semantic Actions and Symbol Table Entries

For the declaration:

```text
float temperature;
```

the symbol table entry is:

```text
temperature → float
```

For the declaration:

```text
int sensor_value;
```

the symbol table entry is:

```text
sensor_value → int
```

During assignment, the compiler compares the types:

```text
Left Type  = float
Right Type = int
```

The type promotion rule is:

```text
int → float
```

Therefore, the assignment is type compatible.

## (iv) Implicit Type Conversion

The assignment:

```text
temperature = sensor_value;
```

is internally treated as:

```text
temperature = (float)sensor_value;
```

The compiler automatically converts the `int` value of `sensor_value` into a `float` value before assigning it to `temperature`.

For example:

```text
sensor_value = 25
```

After coercion:

```text
sensor_value = 25.0
```

Then:

```text
temperature = 25.0
```

Thus, explicit type casting is not required from the programmer.

## (v) Resulting Type Information and Final Type

Symbol Table:

```text
Name             Type
temperature      float
sensor_value     int
```

Assignment:

```text
temperature = sensor_value;
```

Type information:

```text
Left Variable  : temperature (float)
Right Variable : sensor_value (int)
Type Conversion: int -> float
Assignment: Compatible
```

### Output

```text
Type Conversion Analysis

Left Variable  : temperature (float)
Right Variable : sensor_value (int)
Type Conversion: int -> float
Assignment: Compatible

Final Type of temperature: float
```

## Conclusion

The YACC-based compiler uses a symbol table to store variable names and their data types. During assignment, the compiler compares the types and automatically performs type coercion when required. In the given example, the `int` value of `sensor_value` is implicitly converted to `float` before being assigned to `temperature`. The final type of `temperature` remains `float`.
