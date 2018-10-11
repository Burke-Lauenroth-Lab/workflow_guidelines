# C Coding Standard for SOILWAT2

### Entity Naming

*   [Variables shall begin with a lower case letter, shall be lower case and words shall begin with an upper case letter.](#nvcase)
*   [Constants shall begin with an upper case letter and shall be upper case.](#nconstcase)
*   [Functions shall begin with a lower case letter, shall be lower case and words shall be separated by underscore (_).](#nfuncase)
*   [Functions shall be prefixed with "SW_".](#nfunpre)
*   [Types shall begin with an upper case letter.](#ntypecase)
*   [Typedefs shall be upper case.](#ntdefcase)
*   [Typedefs shall be prefixed with "SW_".](#ntdefpre)
*   [Enumerators shall begin with an upper case letter.](#netorcase)
*   [Macros shall begin with a lower case letter and shall be lower case.](#nmacrocase)

### Names

*   [Use sensible, descriptive names.](#NameSensible)
*   [Only use english names.](#NameEnglish)
*   [Variables with a large scope shall have long names, variables with a small scope can have short names.](#NameLength)
*   [Use name prefixes for identifiers declared in different modules](#UseNamePrefix)

### Indentation and Spacing

*   [Braces shall follow "K&R Bracing Style".](#BracesKnR)
*   [Braces shall be indented 2 columns to the right of the starting position of the enclosing statement or declaration.](#IndentLevel)
*   [Loop and conditional statements shall always have brace enclosed sub-statements.](#BracesLoop)
*   [Braces without any contents may be placed on the same line.](#BracesEmpty)
*   [Each statement shall be placed on a line on its own.](#StmtPerLine)
*   [All binary arithmetic, bitwise and assignment operators and the ternary conditional operator (?:) shall be surrounded by spaces; the comma operator shall be followed by a space but not preceded; all other operators shall not be used with spaces.](#OperSpace)
*   [Lines shall not exceed 80 characters.](#LineLength)
*   [Do not use tabs.](#TabsAvoid)

### Comments

*   [Comments shall be written in english](#CommentEnglish)
*   [Use JavaDoc style comments.](#CommentJavaDoc)
*   [All comments shall be placed above the line the comment describes, indented identically.](#CommentPlacement)
*   [Use <tt>#ifdef</tt> instead of /* ... */ to comment out blocks of code.](#CommentIfDef)
*   [Every function shall have a comment that describes its purpose.](#CommentFunction)

### Files

*   [File name shall be treated as case sensitive.](#FileNameCase)
*   [C source files shall have extension "<tt>.c</tt>".](#CFileExt)
*   [C header files shall have extension "<tt>.h</tt>".](#CHeaderExt)
*   [Inline functions shall be declared in header files and defined in inline definition files.](#InlineFunctionDefineSeparate)
*   [Header files must have include guards.](#HeaderIncludeGuard)
*   [The name of the macro used in the include guard shall have the same name as the file (excluding the extension) followed by the suffix "_H".](#IncludeGuardName)
*   [Header files shall be self-contained](#HeaderSelfContained)
*   [System header files shall be included with <> and project headers with "".](#HeaderSystemUser)
*   [Put <tt>#include</tt> directives at the top of files.](#IncludeFirst)
*   [Do not use absolute directory names in <tt>#include</tt> directives.](#IncludeAbsoluteAvoid)
*   [Each file must start with a copyright notice.](#ProvideCopyright)

### Declarations

*   [Provide names of parameters in function declarations.](#FunctionParameterNames)
*   [Always provide the return type explicitly.](#FunctionReturnType)
*   [Use a typedef to define a pointer to a function.](#FunctionPointerTypedef)

### Statements

*   [Never use gotos.](#GotoForbid)
*   [Do not use <tt>continue</tt> in loops.](#ContinueAvoid)
*   [Only have one return in a function](#FunctionOneExit)
*   [All switch statements shall have a default label.](#SwitchDefault)

### Other Typographical Issues

*   [Do not use literal numbers other than 0 and 1.](#MagicNumberAvoid)
*   [Do not rely on implicit conversion to bool in conditions.](#ConditionsExplicitBool)

* * *

## Entity Naming

<a name="nvcase">

### Variables shall begin with a lower case letter, shall be lower case and words shall begin with an upper case letter.

</a><a name="nconstcase">

### Constants shall begin with an upper case letter and shall be upper case.

</a><a name="nfuncase">

### Functions shall begin with a lower case letter, shall be lower case and words shall be separated by underscore (_).

</a><a name="nfunpre">

### Functions shall be prefixed with "SW_".

</a><a name="ntypecase">

### Types shall begin with an upper case letter.

</a><a name="ntdefcase">

### Typedefs shall be upper case.

</a><a name="ntdefpre">

### Typedefs shall be prefixed with "SW_".

</a><a name="netorcase">

### Enumerators shall begin with an upper case letter.

</a><a name="nmacrocase">

### Macros shall begin with a lower case letter and shall be lower case.

## Names

</a><a name="NameSensible">

### Use sensible, descriptive names.

Do not use short cryptic names or names based on internal jokes. It shall be easy to type a name without looking up how it is spelt.</a>

<a name="NameSensible">Exception: Loop variables and variables with a small scope (less than 20 lines) may have short names to save space if the purpose of that variable is obvious.</a><a name="NameEnglish"></a>

### <a name="NameEnglish">Only use english names.</a>

<a name="NameEnglish">It is confusing when mixing languages for names. English is the preferred language because of its spread in the software market and because most libraries used already use english.</a><a name="NameLength">

### Variables with a large scope shall have long names, variables with a small scope can have short names.

Scratch variables used for temporary storage or indices are best kept short. A programmer reading such variables shall be able to assume that its value is not used outside a few lines of code. Common scratch variables for integers are <tt>i</tt>, <tt>j</tt>, <tt>k</tt>, <tt>m</tt>, <tt>n</tt> and for characters <tt>c</tt> and <tt>d</tt>.</a><a name="UseNamePrefix">

### Use name prefixes for identifiers declared in different modules

This avoids name clashes.

## Indentation and Spacing

</a><a name="BracesKnR">

### Braces shall follow "K&R Bracing Style".

The K&R Bracing Style was first introduced in The C Programming Language by Brian Kernighan and Dennis Ritchie.</a>

<a name="BracesKnR">The opening brace is placed at the end of the enclosing statement and the closing brace is on a line on its own lined up with the enclosing statement. Statements and declaration between the braces are indented relative to the enclosing statement Note that the opening brace of a function body is placed on a line on its own lined up with the function declaration.</a><a name="IndentLevel"></a>

### <a name="IndentLevel">Braces shall be indented 2 columns to the right of the starting position of the enclosing statement or declaration.</a>

<a name="IndentLevel">Example:

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>void SW_f(int a)
{
  int i;
  if (a > 0) {
    i = a;
  } else {
    i = a;
  }
}
</pre>

</td>

</tr>

</tbody>

</table>

</a><a name="BracesLoop">

### Loop and conditional statements shall always have brace enclosed sub-statements.

The code looks more consistent if all conditional and loop statements have braces.</a>

<a name="BracesLoop">Even if there is only a single statement after the condition or loop statement today, there might be a need for more code in the future.</a><a name="BracesEmpty"></a>

### <a name="BracesEmpty">Braces without any contents may be placed on the same line.</a>

<a name="BracesEmpty">The only time when two braces can occur on the same line is when they do not contain any code.

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>while (...)
{}
</pre>

</td>

</tr>

</tbody>

</table>

</a><a name="StmtPerLine">

### Each statement shall be placed on a line on its own.

There is no need to make code compact. Putting several statements on the same line only makes the code cryptic to read.</a><a name="OperSpace">

### All binary arithmetic, bitwise and assignment operators and the ternary conditional operator (?:) shall be surrounded by spaces; the comma operator shall be followed by a space but not preceded; all other operators shall not be used with spaces.

</a><a name="LineLength">

### Lines shall not exceed 80 characters.

Even if your editor handles long lines, other people may have set up their editors differently. Long lines in the code may also cause problems for other programs and printers.</a><a name="TabsAvoid">

### Do not use tabs.

Tabs make the source code difficult to read where different programs treat the tabs differently. The same code can look very differently in different views.

Avoid using tabs in your source code to avoid this problem. Use spaces instead.

## Comments

</a><a name="CommentEnglish">

### Comments shall be written in english

</a><a name="CommentJavaDoc">

### Use JavaDoc style comments.

The comment styles <tt>///</tt> and <tt>/** ... */</tt> are used by JavaDoc, Doxygen and some other code documenting tools.</a><a name="CommentPlacement">

### All comments shall be placed above the line the comment describes, indented identically.

Being consistent on placement of comments removes any question on what the comment refers to.</a><a name="CommentIfDef">

### Use <tt>#ifdef</tt> instead of /* ... */ to comment out blocks of code.

The code that is commented out may already contain comments which then terminate the block comment and causes lots of compile errors or other harder to find errors.</a><a name="CommentFunction">

### Every function shall have a comment that describes its purpose.

## Files

</a><a name="FileNameCase">

### File name shall be treated as case sensitive.

</a><a name="CFileExt">

### C source files shall have extension "<tt>.c</tt>".

</a><a name="CHeaderExt">

### C header files shall have extension "<tt>.h</tt>".

</a><a name="InlineFunctionDefineSeparate">

### Inline functions shall be declared in header files and defined in inline definition files.

The keyword <tt>inline</tt> shall be used in both places.</a>

<a name="InlineFunctionDefineSeparate">Using a separate inline file is useful to keep the header files clean and small. The separation is also useful where the inlining is disabled in debug builds. The inline file is then included from the source file instead of the header file to reduce compile time.</a><a name="HeaderIncludeGuard"></a>

### <a name="HeaderIncludeGuard">Header files must have include guards.</a>

<a name="HeaderIncludeGuard">The include guard protects against the header file being included multiple times.

Example:

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>#ifndef FILE_H
#define FILE_H
...
#endif
</pre>

</td>

</tr>

</tbody>

</table>

</a><a name="IncludeGuardName">

### The name of the macro used in the include guard shall have the same name as the file (excluding the extension) followed by the suffix "_H".

</a><a name="HeaderSelfContained">

### Header files shall be self-contained

When a header is included, there shall not be a need to include any other headers first.

A simple way to make sure that a header file does not have any dependencies is to include it first in the corresponding source file. Example:

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>/* foobar.h */

#include "foobar.h"
#include <stdio.h>

...
</pre>

</td>

</tr>

</tbody>

</table>

</a><a name="HeaderSystemUser">

### System header files shall be included with <> and project headers with "".

</a><a name="IncludeFirst">

### Put <tt>#include</tt> directives at the top of files.

Having all <tt>#include</tt> directives in one place makes it easy to find them.</a><a name="IncludeAbsoluteAvoid">

### Do not use absolute directory names in <tt>#include</tt> directives.

The directory structure may be different on other systems.</a><a name="ProvideCopyright">

### Each file must start with a copyright notice.

## Declarations

</a><a name="FunctionParameterNames">

### Provide names of parameters in function declarations.

Parameter names are useful to document what the parameter is used for.</a>

<a name="FunctionParameterNames">The parameter names shall be the same in all declarations and definitions of the function.</a><a name="FunctionReturnType"></a>

### <a name="FunctionReturnType">Always provide the return type explicitly.</a>

<a name="FunctionReturnType"></a><a name="FunctionPointerTypedef">

### Use a typedef to define a pointer to a function.

Pointers to functions have a strange syntax. The code becomes much clearer if you use a typedef for the pointer to function type. This typedef name can then be used to declare variables etc.

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>double SW_sin(double arg);
typedef double (*SW_TRIGFUNC)(double arg);

/* Usage examples */
SW_TRIGFUNC myFunc = SW_sin;
void SW_call_func(SW_TRIGFUNC callback);
SW_TRIGFUNC funcTable[10];
</pre>

</td>

</tr>

</tbody>

</table>

## Statements

</a><a name="GotoForbid">

### Never use gotos.

Gotos break structured coding.</a><a name="ContinueAvoid">

### Do not use <tt>continue</tt> in loops.

A <tt>continue</tt> statement is a goto statement in disguise and makes code less readable.</a><a name="FunctionOneExit">

### Only have one return in a function

It is confusing when there are more than one return statement in a function.

Having only one exit point of a function makes it easy to have a single place for post conditions and invariant check.

When debugging It is useful to have a single exit point of a function where you can put a single breakpoint or trace output.

</a>

<a name="FunctionOneExit">It is sometimes necessary to introduce a result variable to carry the function return value to the end of the function. This is an acceptable compromise for structured code.</a><a name="SwitchDefault"></a>

### <a name="SwitchDefault">All switch statements shall have a default label.</a>

<a name="SwitchDefault">Even if there is no action for the default label, it shall be included to show that the programmer has considered values not covered by case labels. If the case labels cover all possibilities, it is useful to put an assertion there to document the fact that it is impossible to get here. An assertion also protects from a future situation where a new possibility is introduced by mistake.

## Other Typographical Issues

</a><a name="MagicNumberAvoid">

### Do not use literal numbers other than 0 and 1.

Use constants instead of literal numbers to make the code consistent and easy to maintain. The name of the constant is also used to document the purpose of the number.</a><a name="ConditionsExplicitBool">

### Do not rely on implicit conversion to bool in conditions.

<table cellspacing="10" cellpadding="5">

<tbody>

<tr>

<td width="20"></td>

<td bgcolor="eeeeee">

<pre>if (ptr)         // wrong
if (ptr != NULL) // ok
</pre>

</td>

</tr>

</tbody>

</table>

* * *

_Generated 2018-10-11 by_ </a>_[Coding Standard Generator](http://www.rosvall.ie/CSG) version 1.13._
