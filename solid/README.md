# SOLID Principles Questions

<!-- start question 1 -->
### 1. In the following block of code, which SOLID principle is used?

```csharp
public interface IService
{
  void DoSomething();
}

public class ServiceA
{
  private readonly IService _service;

  public ServiceA(IService service)
  {
    this._service = service;
  }

  public void Method()
  {
    this._service.DoSomething();
  }
}
```

Select all the right Answers:

- A: Open/Closed Principles (OCP)
- B: Liskov Substitution (LSP)
- C: Single Resposibility (SRP)
- D: Dependency Inversion (DIP)

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The Dependency Inversion Principle states that:

High-level modules should not depend on low-level modules. Both should depend on abstractions.

`ServiceA` (high-level) depends entirely on the `IService` (abstraction) and receives its dependency through the constructor (a form of Dependency Injection).

</p>
</details>
<!-- end question 1 -->
