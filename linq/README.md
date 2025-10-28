# Language-Integrated Query (LINQ) Questions

<!-- start question 1 -->
### 1. How can you filter a list of numbers to only include even numbers using LINQ in C#?

`var numbers = new List<int> { 1, 2, 3, 4, 5};`

Select all the right Answers:

- A: `numbers.Filter(n=> n % 2);`
- B: `numbers.Select(n=> n % 2 == 0);`
- C: `from n in numbers where n % 2 == 0 select n;`
- D: `numbers.Where(n=> n % 2 == 0).ToList();`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C,D

- **A:** Incorrect method name. There is no standard LINQ method called `Filter()`.
- **B:** Incorrect operator. Select is used for projection. This code would return a list of **booleans** (`{false, true, false, true, false}`).
- **C:** Correct. This is the valid **LINQ Query Syntax**. It defines the range (`from n in numbers`), applies the filter (`where n % 2 == 0`), and projects the original number (`select n`).
- **D:** Correct. This is the valid **LINQ Method Syntax**. `Where(n=> n % 2 == 0)` performs the filtering. `.ToList()` is added to immediately execute the query and convert the resulting sequence (an `IEnumerable<int>`) back into a concrete `List<int>`.

</p>
</details>
<!-- end question 1 -->
