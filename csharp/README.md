# C# Questions

<!-- start question 1 -->

### 1. What's name result for the following example?

```csharp
var e1 = new Employee
{
  Id = 1,
  Name = "X",
};

var e2 = new Employee
{
  Id = 2,
  Name = "Y",
};

var array = new Employee[] { e1, e2 };
var employee = array[0];
employee.Name = "Z";
array[1].Name = "U";
struct Employee
{
  public int Id { get; set; }
  public string Name { get; set; }
}
```

- A: `e1.Name = "X"` and `e2.Name = "Y"`
- B: `e1.Name = "Z"` and `e2.Name = "Y"`
- C: `e1.Name = "Z"` and `e2.Name = "U"`
- D: `e1.Name = "X"` and `e2.Name = "U"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The core reason for the answer lies in how the `struct` keyword works in C#: it defines a **Value Type**.

##### Key Principle: Value Types are **COPIED**

When you use a struct, every time you assign it to a new variable or place it in an array, the entire value (the data itself) is **copied**.

###### Step-by-Step Breakdown:

1. Array Creation

   ```csharp
   var array = new Employee[] { e1, e2 };
   var employee = array[0];
   ```

   The `array` takes a copy of `e1` and a copy of `e2`.

   The `employee` takes a copy of `array[0]` which is copy of `e1`.

2. First Modification:

   ```csharp
   employee.Name = "Z";
   ```

   You are only changing the copy stored in the `employee` variable.

   - **Result**: `e1` is still **"X"**.

3. Second Modification:

   ```csharp
   array[1].Name = "U";
   ```

   You are changing the **copy** stored inside the array at index 1.

   - **Result**: `e2` (the original variable) is still **"Y"**.

##### Conclusion

Because `e1` and `e2` are structs (Value Types), their original values are never modified by operations on the array or the temporary `employee` variable. These variables only operate on **copies**.

</p>
</details>
<!-- end question 1 -->

<!-- start question 2 -->

### 2. What is the advantage of using the `using` statement with an `IDisposable` object?

Select all the right Answers:

- A: It ensures the object is finalized
- B: It forces garbage collection
- C: It ensures the object is disposed of immediately after use
- D: It prevents memory leaks

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C,D

**C:** correct because of the **primary function** of the `using` statement. It calls the `Dispose()` method as soon as execution leaves the `using` block, guaranteeing immediate resource release.

**D:** Yes, indirectly. While `using` doesn't prevent _managed_ memory leaks (unreachable objects staying in memory), it prevents **resource leaks**. By ensuring unmanaged resources (like file handles or database connections) are reliably closed/freed via `Dispose()`, it prevents the application from exhausting the operating system's supply of those crucial resources.

</p>
</details>
<!-- end question 2 -->

<!-- start question 3 -->

### 3. what is the result of this code block?

```csharp
GenericBase<int> x = new GenericBase<int>();
x.Display();
Console.Write(",");
GenericBase<int> x1 = new Child();
x1.Display();

class GenericBase<T>
{
  public virtual void Display()
  {
    Console.Write(typeof(T).ToString());
  }
}
class Child : GenericBase<int>
{
  public override void Display()
  {
    Console.Write("from child");
  }
}
```

- A: System.Int32,from child
- B: System.Int32,System.Int32
- C: runtime error
- D: compilation error

<details><summary><b>Answer</b></summary>

<p>

#### Answer: A

```csharp
GenericBase<int> x = new GenericBase<int>(); // x's actual type is GenericBase<int>
x.Display();
```

- `x` is an instance of the base class itself.
- The base class's `Display()` method is called.
- Inside `GenericBase<T>.Display()`, it prints the string representation of the type argument `T`. Since `x` was created as `GenericBase<int>`, `T` is `int`.

`x.Display(); // Print: System.Int32`

`Console.Write(","); // Print: ,`

```csharp
GenericBase<int> x1 = new Child(); // x1's declared type is GenericBase<int>, but its actual type is Child
x1.Display();
```

- This demonstrates **Polymorphism**. The variable `x1` is declared as the base type (`GenericBase<int>`), but it holds an object whose runtime type is `Child`.
- Because `Display()` is `virtual` and `Child` provides an `override`, the **runtime type ** dictates which method is executed.
- The `Child.Display()` method is called.

`x1.Display(); // Print: from child`

</p>
</details>
<!-- end question 3 -->

<!-- start question 4 -->

### 4. what is the result of this code block?

```csharp
class Base
{
  public void Display()
  {
    Console.WriteLine("from Base");
  }
}

class Derived
{
  public override void Display()
  {
    Console.WriteLine("from Derived");
  }
}

Base x = new Derived();
x.Display();
```

- A: from Base
- B: from Derived
- C: runtime error
- D: compilation error

<details><summary><b>Answer</b></summary>

<p>

#### Answer: D

- Missing inheritance:
  Derived must inherit from Base to override its method.\
   ✅ Fix: `class Derived : Base`
- `Display()` in Base is not `virtual`:
  You cannot override a method unless it's marked `virtual`, `abstract`, or `override` in the base class.\
   ✅ Fix: `public virtual void Display()`

</p>
</details>
<!-- end question 4 -->

<!-- start question 5 -->

### 5. what is the result of this code block?

```csharp
class Foo
{
  public int Method()
  {
    //implementation
  }
  public string Method()
  {
    //implementation
  }
}
```

Select all the right Answers:

- A: overloading
- B: overriding
- C: Polymorphism
- D: compilation error

<details><summary><b>Answer</b></summary>

<p>

#### Answer: D

Since both `Method()` definitions have the same name and the exact same empty parameter list, the compiler sees them as the **same method**, even though their return types are different. The compiler cannot determine which method to call based on the return type alone. This results in a fatal error during the build process.

</p>
</details>
<!-- end question 5 -->

<!-- start question 6 -->

### 6. What is the purpose of using ConfigureAwait(false) in an async method in C#?

```csharp
async Task LoadDataAsync()
{
  await GetDataAsync().ConfigureAwait(false);
}
```

Select all the right Answers:

- A: It forces the method to continue on the original context, such as the UI thread.
- B: It allows the continuation to run on a thread pool thread rather than resuming on the original context.
- C: It makes the async method run synchronously.
- D: It optimizes the performance by reducing the overhead of context capturing.

<details><summary><b>Answer</b></summary>

<p>

#### Answer: B,D

Firstly, Capturing and restoring the Synchronization Context involves overhead and can lead to deadlocks if not handled carefully. Setting it to `false` avoids this work, resulting in a small performance gain and preventing potential deadlocks in library code.

Secondly, `ConfigureAwait(false)` tells the runtime not to capture or resume on the original context. It allows the continuation to run on **any available thread pool thread**.

</p>
</details>
<!-- end question 6 -->

<!-- start question 7 -->

### 7. What is the result of this code?

```csharp

await MethodA();

static async Task MethodA()
{
  var t = MethodB();
  Console.Write("MethodA");
  await t;
}
static Task MethodB()
{
  Thread.Sleep(1000);
  Console.WriteLine("MethodB");
  return Task.CompletedTask;
}
```

Select all the right Answers:

- A: "MethodAMethodB"
- B: "MethodB" \n "MethodA"
- C: runtime error
- D: compilation error

<details><summary><b>Answer</b></summary>

<p>

#### Answer: B

The final output is the result of the synchronous execution order, where `MethodB` fully completes (including its **blocking sleep**) before MethodA's output statement is reached.

```csharp
// MethodA() starts: Execution enters MethodA().
var t = MethodB(); // Call to MethodB() starts synchronously.

// MethodB() starts and blocks: Execution jumps to MethodB().
Thread.Sleep(1000); // The current thread blocks for 1 second.
Console.WriteLine("MethodB"); // Execution resumes after the sleep.
return Task.CompletedTask; // MethodB returns.

// MethodA() continues (Synchronous Part): Execution returns to MethodA().
Console.Write("MethodA"); // This is executed immediately after MethodB completes.
await t; // The task 't' is already completed, so the 'await' does nothing but proceed immediately.
```

</p>
</detai>
<!-- end question 7 -->
