#Standard Library

###Util
```
null // the identity function \null null
return // is \x x
if b t f // is \b\t\f b t f
for start end iv l // equivalent to fold (vGenerateRange start end) iv l
while st lb l
    // st = l(st) while lb(st) is true
until st lb l
    // st = l(st) until lb(st) is true
match cmp value [<value result>...] default
```

###Tuple
```
tPair a b // makes a pair with a and b
(t) tFirst // gets the first value of a pair
tSetFirst t v // sets the first value of a pair to v
(t) tSecond // gets the second value of a pair
tSetSecond t v // sets the second value of a pair to v
tVararg i f ... // reads a variable number of arguments to a vector and returns f(v)
tTuple i ... // makes a tuple with i length
(t) (tGet i p) // gets the value at p from a tuple with i length
tSet t i n v // sets t[n] of tuple length i to v 
tVector i t // turns a tuple into a vector
```

###Bool
```
true // is \t\f t
false // is \t\f f
bNot x // returns !x
bOr x y // returns x || y
bAnd x y // returns x && y
bXor x y // returns x ^ y
```

###Int
```
uSucc x // returns x + 1
uPred x // returns x - 1
uAdd x y // returns x + y
uSub x y // returns x - y
uMul x y // returns x * y
uExp x y // returns exp(x, y)
uIsZero x // returns x == 0
uIsGT x y // return x > y
uIsLT x y // return x < y
uIsGE x y // return x >= y
uIsLE x y // return x <= y
uIsEQ x y // return x == y
uSigned x // converts x to signed
uString x // converts x to a string
// NYI:
uDiv x y // returns x / y
```

###Signed Int
```
iSucc x // returns x + 1
iPred x // returns x - 1
iNeg x // returns -x
iAdd x y // returns x + y
iSub x y // returns x - y
iMul x y // returns x * y
iIsZero x // returns x == 0
iIsGT x y // return x > y
iIsLT x y // return x < y
iIsGE x y // return x >= y
iIsLE x y // return x <= y
iIsEQ x y // return x == y
iIsNEQ x y // return x != y
iIsNeg x // returns true if x is less than 0
iIsPos x // returns true if x is greater than 0
iNormalize x // normalizes x so that at least one side is zero
iUnsigned x // converts x to unsigned number
// NYI:
iDiv x y // returns x / y
iString x // converts x to a string
```

###Vector

```
vGet v i d f // returns f(v[i]) or d if out of bounds (zero indexed)
vSet v i e // returns v where v[i] = e
vFirst v d f // returns f(v.first) or d if empty
vLast v d f // returns f(v.last) or d if empty
vFirstWhere v l d f // returns first element where l(e) is true else returns d
vLastWhere v l d f // returns last element where l(e) is true else returns d
vLength v // returns v.length
vReverse v // returns the reverse of v
vTake v i // returns the first i values of the vector
vTakeWhile v l // returns the values of v until l(v[i]) is false
vSkip v i // returns the vector but skips the first i elements
vSkipWhile v l // returns the values of v starting when l(v[i]) is true
vMap v l
    // v.map(l) where l is a llama that takes an element and returns the replacement
    // so that out[n] = l(v[n])
vFoldMap v iv lf
    // same as map but keeps a state so that out[n], st = l(st, v[n])
vWhere v l
    // returns v.where(l) where l is a llama that takes an element and returns a boolean
    // the output is an array where l returned true
vReduce v l d f
    // reduces v to a single value x by combining elements with l(last, e) and returns f(x) or d if v is empty
vFold v iv l
    // reduces a vector to a single value where iv is the initial value
    // l is a lambda that takes the previous value (iv if first element) and the current element
    // and returns the next value to pass to the callback or return value if its the last element
vExpand v l
    // expand function like dart's Iterable.expand
vIndexify v // returns an array where out[i] = <i v[i]>
vGenerate j e // generates a Vector with j elements and filling it with e
vGenerateRange i j // generates a Vector of integers starting at i and ending before j
vIsEmpty v // returns true if v is empty, else returns false
vRemoveLast v // removes the last element
vRemoveFirst v // removes the first element
vRemoveAt v i // removes element at i
vRemoveRange v start end // removes range of elements
vRemoveWhere v l // removes elements where l(e) is true
vInsertEnd v e // inserts e at the end of v
vInsertStart v e // inserts e at the start of v
vInsertAt v i e // inserts e into v at i
vConcat va vb // appends vb to va
vSlice v start end // returns a slice of v from start to before end
vAny v l // returns true if any l(e) returns true
vEQ cmp a b // returns true if both vectors have the same contents using cmp
// NYI:
vString v // "casts" a vector of ints to a string for display purposes
```

###String
```
sUnsigned s // parses a string to an unsigned int
sSigned s // parses a string to signed int
```

###Map
```
mFromVec cmp vec k v // gets a map where m[k(e)] = v(e)
mKeys m // gets a vector of all the keys
mValues m // gets a vector of all the values
mGet cmp m k d f // returns f(m[k]) or d if absent where cmp is the comparison function to use on keys, for example uEQ
mSet cmp m k e // sets m[k] to e
mExists cmp m k // returns true if m contains the key k
mContains cmp m v // returns true of m contains the value v
mAddAll cmp ma mb // adds all of the contents of mb to ma
mAddFromVec cmp ma vec k v // same as mAddAll cmp ma~mFromVec cmp vec k v
mRemove cmp m k // removes key if it exists
mPutIfAbsent cmp m k e // sets m[k] to e only if it doesnt exist already
```