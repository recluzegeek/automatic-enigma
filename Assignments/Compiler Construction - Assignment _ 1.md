---
title: 'Compiler Construction - Assignment # 1'
updated: 2023-06-23 11:26:01Z
created: 2023-06-23 11:02:41Z
latitude: 24.74381520
longitude: 67.57979420
altitude: 0.0000
---

# <p align="right">Assignment # 01<br>Flex Lexical Analysis Tool<br>CS-428A Compiler Construction</p>
### <p>Muhammad Salman  212370003 – Group Leader<br>Muhammad Sami Fayyaz Aujla – 212370034
</p>

## Flex Lexical Code

```c
	/* Declaration Section */
%{
int n = 0;
%}
keywordsPattern while|public|static|void|return|if|else|for|int|double|String|float
letters [A-Za-z_]
digit [0-9]
identifiers ({letters}({letters}{digit})*)+
realNumbers {digit}+(\.{digit}+)?([Ee][-+]?{digit}+)?
operators "+"|"-"|"*"|"/"|"++"|"--"|"+="|"-="
rel_op "<"|">"|"<="|">="|"=="|"!="|"="
brackets "{"|"}"|"["|"]"|"("|")"

%%
{keywordsPattern}+ { n++; printf("\tT_keyword: %s\n", yytext); }

{realNumbers} { n++; printf("\tT_number : %s\n", yytext); }

{identifiers} { n++; printf("\tT_identifier : %s\n", yytext); }

{operators} { n++; printf("\tT_operator : %s\n", yytext); }

{rel_op} { n++; printf("\tT_relop : %s\n", yytext); }

{brackets} {n++; printf("\tT_bracket : %s\n", yytext); }

";" { printf("\tT_delim : %s\n", yytext); }

[ \t\n\r]   { /* Ignore whitespace */ }

"#pt" { printf("\n Total Number of Tokens : %d \n Resetting the counter\n", n); n = 0; }

. {n++; printf("\tT_error : %s\n", yytext);}
%%

int yywrap(void) {}

int main()
{
    yylex();
    return 0;
}
```