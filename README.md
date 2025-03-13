# C# Class, Struct, and Record

In C#, **class**, **struct**, and **record** are different types used for defining custom data structures. Each has its own characteristics and ideal use cases. Below is a detailed comparison and explanation.

---

## 1. Class

A **class** is a **reference type**, meaning when you assign one class variable to another, both point to the same object in memory.

### Characteristics:
- **Reference type** (stored on the heap).
- Supports **inheritance**.
- Supports **polymorphism**.
- Supports **destructors**.
- Can have **null** value.
- Suitable for complex objects with identity and behavior.

### Example:
```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Hi, I'm {Name} and I'm {Age} years old.");
    }
}
```

### Use Cases:
- Represent entities with identity.
- Complex objects with methods and behavior.
- Objects that require inheritance or polymorphism.

## 2. Struct

A **struct** is a **value type**, meaning when you assign one struct variable to another, a **copy** is made.

### Characteristics:
- **Value type** (stored on the stack, when possible).
- **No inheritance** (except from `System.ValueType`).
- Lightweight and efficient for small data structures.
- Cannot have a parameterless constructor (before C# 10).
- Cannot be null (non-nullable by default).
- Better for small, immutable data.

### Example:
```csharp
public struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public double Distance() => Math.Sqrt(X * X + Y * Y);
}
```

### Use Cases:
- Small, lightweight objects.
- Objects that represent a single value or small set of related values.
- High-performance scenarios avoiding heap allocations.
- Immutable data containers (recommended).

## 3. Record

A **record** is a **reference type** (by default) introduced in **C# 9.0**, designed to hold **immutable data** and provide **built-in value-based equality**.

### Characteristics:
- **Reference type** (default) or **value type** (`record struct`).
- **Value-based equality** (compares contents, not reference).
- **Immutable** by default (with `init` accessor).
- Supports **inheritance** (record class).
- Concise syntax for data models.
- Supports **with-expressions** for non-destructive mutation.

### Example:
```csharp
public record Person(string Name, int Age);
```

### With-expression example:
```csharp
var person1 = new Person("Alice", 30);
var person2 = person1 with { Age = 31 };
```

### Use Cases:
- Immutable data models.
- DTOs (Data Transfer Objects).
- Data that should be compared by value, not reference.
- Functional programming scenarios.

### Shortcut (Positional Record)

This is the most concise way to declare a `record`, where properties are defined in the constructor-like parameter list.

```csharp
public record Person(string FirstName, string LastName);
```

#### Features of Positional Record:
* Automatically creates `init`-only properties.
* Provides value-based equality and deconstruction.
* Generates `ToString`, `Equals`, `GetHashCode`, and `Deconstruct` automatically.

#### Usage Example:
```csharp
var person = new Person("John", "Doe");
Console.WriteLine(person.FirstName); // John
Console.WriteLine(person);           // Person { FirstName = John, LastName = Doe }
```

### Standard (Full Declaration)

If you want to define properties manually or add more logic, you can write a `standard record`.

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
}
```

#### Usage Example:
```csharp
var person = new Person { FirstName = "John", LastName = "Doe" };
Console.WriteLine(person.FirstName); // John
Console.WriteLine(person);           // Person { FirstName = John, LastName = Doe }
```

### 🚀 Quick Summary Table

| Feature                           | Shortcut (Positional)                     | Standard (Full)                          |
|-----------------------------------|------------------------------------------|------------------------------------------|
| Syntax                            | `public record Person(string Name);`    | `public record Person { string Name { get; init; } }` |
| Auto-generated `init` properties   | ✅                                        | ❌ (You write them)                      |
| Easy deconstruction                | ✅                                        | ✅                                        |
| Custom logic in properties/methods | ❌ (unless you extend it)                 | ✅                                        |
| Use case                          | Simple data models                       | Complex models needing logic             |

## 4. Summary Table

| Feature                 | Class           | Struct            | Record                       |
|------------------------|-----------------|------------------|-----------------------------|
| **Type**               | Reference Type  | Value Type        | Reference Type (default)    |
| **Inheritance**        | Yes             | No                | Yes (record class)          |
| **Value Equality**     | No (Reference)  | Yes (Field-wise)  | Yes (Value-based)           |
| **Nullability**        | Nullable        | Non-nullable      | Nullable (record class)     |
| **Default Mutability** | Mutable         | Mutable           | Immutable (by default)      |
| **Memory Allocation**  | Heap            | Stack (if possible)| Heap                        |
| **Suitable For**       | Complex objects | Small data        | Immutable data models       |
| **Performance**        | Slower (heap)   | Faster (stack)    | Moderate (depends on usage) |

---

## 5. When to Use What?

| Scenario                                               | Recommended Type |
|--------------------------------------------------------|------------------|
| Large, complex objects with behavior                   | Class            |
| Small, lightweight objects                             | Struct           |
| Immutable data that should use value equality          | Record           |
| Data Transfer Objects (DTOs)                           | Record           |
| High-performance small data in tight loops             | Struct           |
| Objects requiring polymorphism or inheritance          | Class or Record  |

## 6. Record Struct (C# 10+)

From **C# 10** onwards, `record struct` allows creating **value-type records** — immutable structs with **value-based equality**.

### Example:
```csharp
public readonly record struct Point(int X, int Y);
```

## References

* [C# Classes and Structs](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/)
* [C# Records](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record)
* [Choosing Between Class and Struct](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/choosing-between-class-and-struct)

