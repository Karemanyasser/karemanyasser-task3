#const usage 


The const keyword allows you to specify whether or not a variable is modifiable.
You can use const to prevent modifications to variables and const pointers and const references prevent changing the data pointed to (or referenced),
if you didn't use const references, you'd have no easy way to ensure that your data wasn't modified.

##Documentation and Safety

The primary purpose of const-ness is to provide documentation and prevent programming mistakes. It's particularly useful to declare reference parameters to functions as const references:
```
bool verifyObjectCorrectness (const myObj& obj);
```
 myObj object is passed by reference into verifyObjectCorrectness. For safety's sake, const is used to ensure that verifyObjectCorrectness cannot change the object--after all, it's just supposed to make sure that the object is in a valid state. This can prevent silly programming mistakes that might otherwise result in damaging the object

###Syntax Note

It is possible to put const either before or after the type:
```
int const x = 5;
or
const int x = 4;
```

##Const Pointers

there are two ways of declaring a const pointer: one that prevents you from changing what is pointed to, and one that prevents you from changing the data pointed to.
The syntax for declaring a pointer to constant data is natural enough:
```
const int *p_int;
```
The key here is that the const appears before the *.
On the other hand, if you just want the address stored in the pointer itself to be const, then you have to put const after the *:
```
int x;
int * const p_int = &x;
```
Generally, the first type of pointer, where the data is immutable, is what I'll refer to as a "const pointer"

##Const Functions

The way to declare that a function is safe for const objects is simply to mark it as const; the syntax for const functions is a little bit peculiar because there's only one place where you can really put the const: at the end of the function
```
int Loan::calcInterest() const
{
        return loan_value * interest_rate; 
}
```
Note that just because a function is declared const that doesn't prohibit non-const functions from using it; the rule is this:

*Const functions can always be called   
*Non-const functions can only be called by non-const objects

##Const Overloading

Because const functions cannot return non-const references to an objects' data, there are many times where it might seem appropriate to have both const and non-const versions of a function. For instance, if you are returning a reference to some member data
then you may want to have a non-const version of the function that returns a non-const reference, C++ allows you to overload based on the const-ness of a method. So you can have both const and non-const methods, and the correct version will be chosen.
```
int& myClass::getData()
{
        return data;
}
```
```
// called for const objects only since a non-const version also exists
const int& myData::getData() const
{
        return data;
}
```

##Const iterators

we've already seen, in order to enforce const-ness, C++ requires that const functions return only const pointers and references. Since iterators can also be used to modify the underlying collection, when an STL collection is declared const, then any iterators used over the collection must be const iterators
Since iterators are a generalization of the idea of pointers
```
std::vector<int> vec;
vec.push_back( 3 );
vec.push_back( 4 );
vec.push_back( 8 );
 
for ( std::vector<int>::const_iterator itr = vec.begin(), end = vec.end(); 
      itr != end;
      ++itr )
{
        // just print out the values...
        std::cout<< *itr <<std::endl;
}
```
##Const cast

Sometimes, you have a const variable and you really want to pass it into a function that you are certain won't modify it. But that function doesn't declare its argument as const
if you know that you are safe in passing a const variable into a function that doesn't explicitly indicate that it will not change the data, then you can use a const_cast in order to temporarily strip away the const-ness of the object.
```
int bad_strlen (char *x)
{
        strlen( x );
}
 


const char *x = "abc";
 
// cast away const-ness for our strlen function 
bad_strlen( const_cast<char *>(x) );
```
Note that you can also use const_cast to add const-ness

##summary

Don't look at const as a means of gaining efficiency so much as a way to document your code and ensure that some things cannot change. Remember that const-ness propagates throughout your program, so you must use const functions, const references, and const iterators to ensure that it would never be possible to modify data that was declared const.

# & operator usage 

The & symbol is used as an operator in C++. It is used in 2 different places, one as a bitwise and operator and one as a pointer address of operator.

##Bitwise AND

It compares each bit of the first operand to that bit of the second operand. If both bits are 1, the bit is set to 1. Otherwise, the bit is set to 0. Both operands to the bitwise AND operator must be of integral types.
```
unsigned short a = 0x5555;      // pattern 0101 ...  
   unsigned short b = 0xAAAA;      // pattern 1010 ...  

   cout << hex << ( a & b ) << endl
```

##Address Of operator

C++ provides two-pointer operators, which are Address of Operator (&) and Indirection Operator (*).
The address of Operator (&), and it is the complement of *. It is a unary operator that returns the address of the variable(r-value) specified by its operand.
```
int  var;
   int  *ptr;
   int  val;

   var = 3000;

   // take the address of var
   ptr = &var;

   // take the value available at ptr
   val = *ptr;
```
