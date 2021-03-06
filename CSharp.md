# Doc Stack
https://docs.microsoft.com/en-us/dotnet/articles/welcome
  https://docs.microsoft.com/en-us/dotnet/articles/csharp/
    Fin: Get Started
    https://docs.microsoft.com/en-us/dotnet/articles/csharp/tutorials/index
      Fin: console teleprompter, inheritance
    https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/
      Fin: Program Structure, Types and variables, Expressions, Statements, Classes and objects, Structs, Arrays, Interfaces, Enums, Delegates, 
      https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/arrays

https://www.microsoft.com/net/tutorials/csharp/getting-started/

## IDE
[Visual Studio](https://docs.microsoft.com/en-us/dotnet/articles/csharp/getting-started/with-visual-studio)

`Debug > Windows > Immediate` open a cmd which can interact with the app

F5 debug, F11 stepping

Condition break point

Debug vs Release: Release version incorporates compiler optimizations

`Build > Publish {projectname}`

Run it:
```
dotnet HelloWorld.dll
```

## Console
[src](https://docs.microsoft.com/en-us/dotnet/articles/csharp/getting-started/getting-started-with-csharp)

```
var name = Console.ReadLine();
var date = DateTime.Now;
Console.WriteLine("\n {0}, {1:d}, {1:t}", name, date);
```

Create a class library as an extension method
- a third-party component
- include it as a bundled component with one or more applications.

Create a unit test
- Add a dependency
- Use `Assert` class

If the library will be used by a single solution, include it as a project in your solution
- create a project and `Set as StartUp Project`
- `Add Reference`

If the library will be generally accessible, you can distribute it as a NuGet package
- Under the project directory with `*.csproj` file, run `dotnet pack --no-build`. The `*.nupkg` is the package

`where dotnet.exe`

Create package from CLI: [src](https://docs.microsoft.com/en-us/dotnet/articles/csharp/tutorials/console-teleprompter)
1. `dotnet new console`
2. `dotnet restore`: run NuGet package management
3. `dotnet build`
4. `dotnet run`

Compile C# program: 
- `csc hello.cs`. It return an exe, which compiles for the full framework, and may not be available on all platforms.
- `csc /t:library acme.cs` return a dll
- `csc /r:acme.dll example.cs`
- When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other
- `dotnet` also manage dependencies and then invoke `csc`

## Concepts
`String.Empty` vs `null`

A static method can be called as a method of an instance and as a static method

Enumerator method: [example](https://github.com/dotnet/docs/tree/master/samples/csharp/getting-started/console-teleprompter)
- Enumerator methods return sequences that are evaluated lazily.
- contain one or more `yield` return statements. 
- `foreach (var x in IEnumerable)`

I​Disposable Interface
- Provides a mechanism for releasing unmanaged resources.
- `Dispose`
- `using`

implicitly typed local variable
- `var`

Async Tasks
- `private static async Task Foo()`
- `await` return a `Task`. When reach `await`, the `Task` returns but will resume
- `Task.Wait()`
- `Main` cannot use `await` operator
- `Task.Run(lambda)`
- `Task.WhenAny(Task[])` when anyone first finish

Lock
- Create a private object as a mutex: `private object lockHandle = new object();`
- `lock (lockHandle)`

Lambda
- `Action work = () => {};`
- can also used to define class memeber `public override string ToString() => Title;`

Access
- `internal class MyClass`
- `public int DelayInMilliseconds { get; private set; } = 200;`

Using
- `using static System.Math;`

Class property
- define `internal class Config`
- `public bool Done => done;` make a property `Done` that can access private `done`

type categories
- class: inherit from `Object`
- structs: `Object` and `ValueType`
- delegates: `MulticastDelegate`, `Delegate`, `Object`
- enums: `Enum`, `System.ValueType`, `Object`

`Object`
- All types implicitly inherit from Object, include primitive types such as int and double
- members: 
  - `ToString`
  - `Equals(Object)` and static `Equals(Object, Object)` and static `ReferenceEquals(Object, Object)`. Unless it is overridden, the `Equals(Object)` method tests for reference equality.
  - `GetHashCode`: When you override the `Equals(Object)` method, you must also override the `GetHashCode()` method
  - `GetType`
  - protected `Finalize` is desctructor
  - `MemberwiseClone` return a shallow clone
  - `.ctor` default constructor

`System.Reflection`
- inspect a type's metadata to get information about that type
- `Type t = typeof(SimpleClass);`

inheritance
- base class vs derived class
- support single inheritance only, but transitive
- Static constructors, Instance constructors, Finalizers are not inherited
- accessibility (See below Accessibility)
  - Private members are visible only in derived classes that are nested in their base class.
  - Protected members are visible only in derived classes.
  - Internal members are visible only in derived classes that are located in the same assembly as the base class.
  - Public members are visible in derived classes and are part of the derived class' public interface.
- In order to be able to override a member, the member in the base class must be marked with the `virtual`, `abstract`, or `override`.
- `sealed` to make a sure a class cannot be use as a base class
- `this`, `base`


"is a" relation represent by inherit from a class, "can do" represents by inherit from interfaces  

Nested class `A.B`:
```
public class A 
{
   private int value = 10;

   public class B : A
   {
       public int GetValue()
       {
           return this.value;
       }
   }
}
```

Ctor
- If constructor is not present in the base class' source code, the C# compiler automatically provides a default (parameterless) constructor.
- If you don't make an explicit call to a base class constructor in your source code, the C# compiler automatically supplies a call to the base class' default or parameterless constructor.

component-oriented programming
- components present a programming model with properties, methods, and events

Program Structure
- programs
- namespaces
- types
- members
- assemblies

Assemblies
- typically have the file extension .exe or .dll, depending on whether they implement applications or libraries
- contain executable code in the form of Intermediate Language (IL) instructions
- contains symbolic information in the form of metadata
- Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime

Types [src](https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/types-and-variables)
- value
  - simple
  - enum: Every enum type has an underlying type
  - struct: do not typically require heap allocation.
  - nullable value: Extensions of all other value types with a null value. `T?` is the extension of `T`
- Reference, i.e. object
  - class
  - interface
  - array: multi-dimensional `int[,]`
  - delegate: `delegate int D(...)`, references to methods with a particular parameter list and return type

Boxing: `int i = 1; object o = i;`; Unboxing: `int j = (int)o;`

variables : represent storage locations
- fields
- array elements
- local variables
- parameters

Expression: [src](https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/expressions)
- precedence of the operators
  - Object creation with initializer: `new T(...){...}`. It returns a reference
  - Anonymous object initializer: `new {...}`
  - Array creation: `new T[..]`
  - Obtain Type object: `typeof(T)`
  - Evaluate expression in checked/unchecked context: `checked(x)`/`unchecked(x)`
  - Obtain default value: `default(T)`
  - Anonymous function: `delegate {...}`
  - Bitwise negation: `~x`
  - Asynchronously wait: `await x`
  - Return true if x is a T: `x is T`
  - Return x typed as T, or null if x is not a T: `x as T`. `var method = member as MethodBase;` convert type. If not convertable, return null
  - Null coalescing: `x ?? y` Evaluates to y if x is null, to x otherwise
  - Anonymous function (lambda expression): `(T x) => y`
- associativity of the operators 
  - assignment operator `=` and conditional operator `?:` is right-associative
- overloaded operators 

Statements
- block: a list of statements written between the delimiters `{` and `}`
- Declaration statement
- Expression statement
- Selection statement: `switch (var) { }`
- Iteration statement: `foreach (var in Collection)`
- Jump statement: `goto`, `throw`, `yield`
- `try`, `catch`, `finally`
- `checked` and `unchecked` statements are used to control the overflow-checking context for integral-type arithmetic operations and conversions
- `lock` statement
- `using` statement

`const`

The end of foreach
```
static IEnumerable<int> Range(int from, int to) 
{
    for (int i = from; i < to; i++) 
    {
        yield return i;
    }
    yield break;
}
static void YieldStatement(string[] args)
{
    foreach (int i in Range(-10,10)) 
    {
        Console.WriteLine(i);
    }
}
```

throw and catch in the same block
```
    try 
    {
        if (args.Length != 2) 
        {
            throw new InvalidOperationException("Two numbers required");
        }
    }
    catch (InvalidOperationException e) 
    {
        Console.WriteLine(e.Message);
    }
```

Check if overflow
```
    int x = int.MaxValue;
    unchecked 
    {
        Console.WriteLine(x + 1);  // Overflow
    }
    checked 
    {
        Console.WriteLine(x + 1);  // Exception
    }
```

class members [src](https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/classes-and-objects#other-function-members)
- constants
- fields: variables. `public static readonly Color Black = new Color(0, 0, 0);` or `private byte r, g, b;`
- methods: 
  - Use reference parameters: `static void Swap(ref int x, ref int y);`; 
  - Use output parameters: `static void Divide(int x, int y, out int result, out int remainder);`; 
  - Use parameter array: Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type
- properties: Actions associated with reading and writing named properties of the class.  do not denote storage locations. 
  - `public int Count => count;` where `count` is a field
  - `public int Capacity { get { return items.Length; } set {} }` where `items` is an array and a field
- indexers: Actions associated with indexing instances of the class like an array. Actually it is `operator[]`
- events: Notifications that can be generated by the class. the type must be a delegate type. `public event EventHandler Changed;` **?**
- operators: All operators must be declared as public and static.
- constructors
- finalizers: desctructor. classes should implement finalizers only when no other solutions are feasible.
- types: Nested types declared by the class

Accessibility
- public
- protected
- internal: Access limited to the current assembly (.exe, .dll, etc.)
- protected internal: Access limited to the containing class or classes derived from the containing class
- private

Type parameters: used to create a constructed type
```
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
```

Struct
- do not require heap allocation
- all struct types implicitly inherit from type `System.ValueType`

Array
- elements 
- Array types are reference types, means an array variable is a pointer
- `int[] a = new int[10];`
- a rank 3 array: `int[,,] a3 = new int[10, 5, 2];`
- jagged array: An array with elements of an array type `int[][] a = new int[3][];`
- default is 0 for int or null
- array initializer: `int[] a = new int[] {1, 2, 3};`


Interface [src](https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/interfaces)
- explicit interface member implementations: `void IControl.Paint() { }`. But can only access throw the interface type.

Enum
- a distinct value type with a set of named constants
- use one of the integral value types as their underlying storage
- define enum use underlying type
```
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```

delegate type
- can reference any method, even static method, that has the same argument list and return type
- anonymous functions: `(double x) => x * 2.0`
```
delegate double Function(double x);
class Multiplier
{
    double factor;
    public Multiplier(double factor) 
    {
        this.factor = factor;
    }
    public double Multiply(double x) 
    {
        return x * factor;
    }
}

Multiplier m = new Multiplier(2.0);
Function f = m.Multiply;
double res = f(3.0);
```

## APIs
[doc](https://docs.microsoft.com/en-us/dotnet/api/index?view=netframework-4.7)

[sample](https://github.com/dotnet/docs/tree/master/samples)

namespace
- System

class
- System.Console

`System.Threading.Tasks.Task(seconds).Wait()`

## Others
resx
- https://docs.microsoft.com/en-us/dotnet/framework/resources/working-with-resx-files-programmatically

## Question
`public int DelayInMilliseconds { get; private set; } = 200;` // what is get and private set?
```
   public int Pages
   { get { return totalPages; }
     set 
     {
         if (value <= 0)
            throw new ArgumentOutOfRangeException("The number of pages cannot be zero or negative.");
         totalPages = value;   
     }
   }
```

`BindingFlags flags = BindingFlags.Instance | BindingFlags.Static | BindingFlags.Public | BindingFlags.NonPublic | BindingFlags.FlattenHierarchy;`

instantiate the class by using reflection

attribute and reflection https://docs.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/attributes

mechanisms

^((.|[\r\n])+\:(.|[\r\n])+)$



# C# Regex
[Rule](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)

E.g.:
A pattern:
'^(([^sS](.|[\r\n])*)|([sS][^mM](.|[\r\n])*)|([sS][mM][^tT](.|[\r\n])*)|([sS][mM][tT][^pP](.|[\r\n])*)|([sS][mM][tT][pP][^\:](.|[\r\n])*)|([sS][mM][tT][pP]\:((([\u0021-\u007E-[<>\(\)\[\]\\\.,;:@"]]|(\\[\u0000-\u007F]))+(\.([\u0021-\u007E-[<>\(\)\[\]\\\.,;:@"]]|(\\[\u0000-\u007F]))+)*)|("((\\[\u0000-\u007F])|[\u0000-\u007F-[\r\n"\\]])+"))@[^@]*))$'

^ // line start with
( // group a subexpression
 ([^sS] // not in this character group, here means not s or S
  (
    . // any single character except \n
    | // or operator
    [\r\n] // in this character group, here means enter
  )
  * // match previous element 0 or more times
 )
 |
 ([sS][^mM](.|[\r\n])*)
 |
 ([sS][mM][^tT](.|[\r\n])*)
 |
 ([sS][mM][tT][^pP](.|[\r\n])*)
 |
 ([sS][mM][tT][pP][^\:](.|[\r\n])*)
 |
 ([sS][mM][tT][pP]
 \: // escape :
  (
   (
    ([
      \u0021 // !
      -
      \u007E // ~
      -
      [<>\(\)\[\]\\\.,;:@"] // one of those symbols
     ] // [A-Z-[BCD]] means A to Z exclude BCD
     |
     (\\[\u0000-\u007F])
    )
    + // match previous element 0 or more times
    (\.
     ([\u0021-\u007E-[<>\(\)\[\]\\\.,;:@"]]
      |
      (\\[\u0000-\u007F])
     )+
    )*
   )|
   ("
    (
     (\\[\u0000-\u007F])|[\u0000-\u007F-[\r\n"\\]]
    )+"
   )
  )
  @[^@]* // Actually it is an @ with repeat 0 or more no @
 )
)
$ // line end with



# LINQ

## query
https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/query-keywords

## Examples
[src](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)

group: return an IGroup

_NOT DONE_

## Function / Action / Predicate
[src](https://stackoverflow.com/questions/4317479/func-vs-action-vs-predicate)

_NOT DONE_

## Delegate
[src](https://stackoverflow.com/questions/8694921/delegates-vs-interfaces-in-c-sharp)

[use delegate as a callback](https://stackoverflow.com/questions/667742/callbacks-in-c-sharp)



# Unit Test

## Visual studio UT framework
[src](https://docs.microsoft.com/en-us/visualstudio/test/unit-test-basics)

Add a test project to the solution and add reference.

A test case:
```
[TestMethod]
[ExpectedException(typeof(ArgumentException))]
[Timeout(2000)]  // Milliseconds
public void TestFoo() {
    ...
    Assert.AreEqual(expected, actual);
}
```

## Rhino mocks
[src](https://hibernatingrhinos.com/oss/rhino-mocks)

### Create a mock object
[src](https://stackoverflow.com/questions/7831404/can-you-explain-difference-between-strictmock-and-partialmock)

```
var myClassObj = MockRepository.GenerateMock<MyClass>();
```

### Mock a method
```
class MyClass {
    public MyReturn Foo(MyArg1 arg1, MyArg2 arg2) {
        try {
            return MyReturn r;
        } catch(Exception e) {
             throw(new MyException());
        }
    }
}

MyClass mc = MockRepository.GenerateMock<MyClass>();
MyReturn mr = new MyReturn();
MyException me = new MyException();

Expect.Call(mc.Foo(Arg<MyArg1>.Is.Anything, Arg<MyArg2>.Is.Anything)).Return(mr);
Expect.Call(mc.Foo(Arg<MyArg1>.Is.Anything, Arg<MyArg2>.Is.Anything)).Return(mr).Repeat.Times(5);
Expect.Call(mc.Foo(Arg<MyArg1>.Is.Anything, Arg<MyArg2>.Is.Anything)).Throw(me);
```

or

```
MockRepository mocks = new MockRepository();
MyClass mc = mocks.PartialMock<MyClass>();
MyReturn mr = new MyReturn();
MyException me = new MyException();

Expect.Call(mc.Foo(Arg<MyArg1>.Is.Anything, Arg<MyArg2>.Matches(() => true))).Return(mr);
mocks.ReplayAll(); // to call those mock methods

// Test some real code.

mc.VerifyAllExpectations();
mocks.VerifyAll();
```

[src](https://stackoverflow.com/questions/466520/what-is-rhino-mocks-repeat)
If not a repeat, it only call once.

[src](https://stackoverflow.com/questions/2764929/rhino-mocks-partial-mock)
Partial mock needs work on virtual methods.

### Arg Matches
[src](https://stackoverflow.com/questions/3520911/rhino-mocks-using-arg-matches)



# Collection

## Init a Directory
```
Dictionary<string, List<string>> myD = new Dictionary<string, List<string>>()
{
  {"tab1", MyList }
};
```

## Dictionary
TryGetValue can test if key exist or not.

## Enumerable.Intersect
https://msdn.microsoft.com/en-us/library/bb460136(v=vs.110).aspx



# NuGet
https://stackoverflow.com/questions/7015149/multiperson-team-using-nuget-and-source-control

https://stackoverflow.com/questions/7018913/where-does-nuget-put-the-dll

## CxCache
It is folder to hold dependency packages. Maybe is related to https://www.nuget.org/packages/xCache/ ?



# Multi-thread

## Dispose
If an object is disposed, you cannot access it's field any more.
[example in DB](https://stackoverflow.com/questions/5350109/cannot-access-a-disposed-object)



# MySQL
[MySQL connect](https://stackoverflow.com/questions/21618015/how-to-connect-to-mysql-database)



# SQLite
https://stackoverflow.com/questions/19665370/missing-sqlite-data-provider-in-vs-2013
- My solution: find `SQLite.Interop.dll` and put under the run path. For example:
- Build setting: Debug x64

Folder
```
Project
  bin
    x64
      Debug
        put x64\SQLite.Interop.dll here
```



# UI
See VisualStudio.md

Choose Windows Application.

FlowLayoutPanel: arranges its contents in a horizontal or vertical flow direction. You can wrap the control's contents from one row to the next, or from one column to the next.



# Miscellaneous

## Compare two lists
https://msdn.microsoft.com/en-us/library/bb348567(v=vs.110).aspx

## Write to a file
https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/file-system/how-to-write-to-a-text-file

## Action
https://msdn.microsoft.com/en-us/library/018hxwa8(v=vs.110).aspx

## sleep
Thread.sleep

## GUID never conflict
https://www.guidgen.com/

## [Traverse an Enum](https://stackoverflow.com/questions/972307/can-you-loop-through-all-enum-values)
var values = Enum.GetValues(typeof(Foos));

## [foreach skip one](https://stackoverflow.com/questions/7942389/how-to-skip-a-specific-position-within-a-for-each-loop-in-c-sharp)
Can also use if == continue.

## [ReadOnlyDictionary](http://www.dotnetcurry.com/dotnet/973/read-only-dictionary-dotnet-45)
Need create by a dictionary and convert to readonly.

## [obj folder struct](https://social.msdn.microsoft.com/Forums/en-US/456ebb0e-6fa3-4a77-a723-6984c5208562/what-is-with-all-of-the-files-for-a-simple-program?forum=csharpide)

## [Tuple as key](https://stackoverflow.com/questions/955982/tuples-or-arrays-as-dictionary-keys-in-c-sharp)

## [print object id](https://stackoverflow.com/questions/5703993/how-to-print-object-id)

## [using for unmanaged resources](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/using-statement)

## Cannot use out in linq
Cannot:
```
void foo(out int i) {
    ()=>(i);
}
```

Can:
```
void foo(out int i) {
    int j;
    ()=>(j);
    i = j;
}
```

## Check if a base object is a derive class
```
MyBase myDerive = new MyDerive();
MyDerive res = myDerive as MyDerive;

if (res == null)
{
    Console.WriteLine("It is not derive");
}
```

## Public attribute
Don't forget to add `[DataMember]`

## sealed class
Cannot inherit from it.

## string with @
https://stackoverflow.com/questions/6134547/what-does-the-prefix-do-on-string-literals-in-c

Means no need to escape.