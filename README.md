# Llama

Llama is a language that compiles directly to lambda calculus with almost no layer of abstraction

Variable names follow the regex `[A-Za-z_][A-Za-z0-9_]*`

#### Expression
Expressions are one of anything below, Long expressions (the body of a llama for example) consume multiple expressions until they hit the EOF or some token like `)`

#### LLama
Llamas work the same as lambdas in lambda calculus<br/>
`λx.λy.λz.x` looks like `\x\y\z x` in llama<br/>
syntax: `\name lexpr`

#### Application
Applications work the same as usual, two expressions grouped together is an application<br/>
It is always left associative meaning `a b c d` = `((a b) c) d`<br/>
syntax: `expr expr`

#### Variables
Variables are bound to the closest llama that matches their name, otherwise they are free and applications to it cannot be solved.<br/>
Binding should work exactly like it would in a high level programming language like Dart, where variables are bound based on where they were the application took place, not where it was subsituted into.<br/>
syntax: `name`

#### Parentheses
Parentheses allow you to group expressions together to explicitly control application order<br/>
example: `a (b c)`<br/>
syntax: `(lexpr)`

#### Comments
Comments start with `//` and continue until the end of line<br/>
example:
```
~\flutter 1 // The one
```

#### Squiggly
The squiggly character is like parentheses that dont need to be closed<br/>
example: `\f\x f(f(f(f x)))` = `\f\x f~f~f~f x`<br/>
syntax: `~lexpr`

#### Definitions
This is the first major difference to lambda calculus, you can procedurally define variables to be used in code preceding it<br/>
Only valid inside of a long expression<br/>
example:`~\a 1 ~\b (succ a) ~\b (succ b) b` = `3` (succ not included by default)<br/>
syntax: `~\name expr lexpr`<br/>

The way this works is it puts the lexpr as a body of a llama and applies your expr to it
```
~\a 1
~\b (succ a)
succ b
```
turns into
```
(\a
    (\b
        succ b
    ) (succ a)
) 1
```

#### Vectors
Vectors are dynamically sized right-fold lists and can be created using square brackets<br/>
example: `[1 2 3]` = `\f\x f 1~f 2~f 3 x`<br/>
syntax: `[expr...]`

#### Tuples
Tuples are like vectors but statically sized and it's easier to extract specific elements.<br/>
example: `<1 2 3>` = `\f f 1 2 3`<br/>
syntax: `<expr...>`

#### Numbers
Numbers use basic church encoding so `1` means `\f\x f x` and `3` means `\f\x f~f~f x`<br/>
example:
```
123 // decimal
0x7B // hex
0b1111011 // binary
0o173 // octal
'{' // character
```

#### Signed numbers
Signed numbers are pairs of numbers where the first element is the positive side and second element is the negative.<br/>
example: 
```
+69 = <69 0>
-69 = <0 69>
-0x69 = <0 0x69>
+'A' = <'A' 0>
```

Some standard functions may return signed numbers that aren't normalized, ie when neither of the elements in the pair are 0 and to fix this use `sNormalize`

#### Strings
String literals are equivalent to a vector of char codes<br/>
Both character literals and strings support the common escapes `\a \b \f \n \r \t \v \\ \"` and also number escapes `\123 \x7b \b1111011`<br/>
example: `"foo"` = `['f' 'o' 'o']`<br/>

#### Llama include
Used to include a whole file as a llama, the file must have a main body</br>
syntax: `~"filename"`<br/>
example:
###### foo.lm
```
succ 1
```
###### bar.lm
```
succ \"foo.lm" // result is 3
```

#### Definition include
Unlike llama includes, definition includes import definitions and the file cannot have a main body<br/>
syntax: `~\"filename"`<br/>
example:
###### foo.lm
```
~\concat(\a\b
    \f\x a f ~b f x
)
```
######  bar.lm
```
~\"foo.lm"
concat "foo" "bar" // result is "foobar"
```