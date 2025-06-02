# Complete C# Object-Oriented Programming Guide

## Table of Contents
1. [OOP Fundamentals](#oop-fundamentals)
2. [Classes and Objects](#classes-and-objects)
3. [Access Modifiers](#access-modifiers)
4. [Constructors and Destructors](#constructors-and-destructors)
5. [Inheritance](#inheritance)
6. [Polymorphism](#polymorphism)
7. [Encapsulation](#encapsulation)
8. [Abstraction](#abstraction)
9. [Interfaces](#interfaces)
10. [Static Members](#static-members)
11. [Properties](#properties)
12. [Namespaces and Using Statements](#namespaces-and-using-statements)
13. [Class Import/Export Examples](#class-importexport-examples)

## OOP Fundamentals

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects" that contain data (attributes) and code (methods). C# is a fully object-oriented language that supports all four main OOP principles:

- **Encapsulation**: Bundling data and methods together
- **Inheritance**: Creating new classes based on existing ones
- **Polymorphism**: Using one interface for different underlying data types
- **Abstraction**: Hiding complex implementation details

## Classes and Objects

### Basic Class Structure

```csharp
// File: Person.cs
using System;

namespace MyApplication.Models
{
    public class Person
    {
        // Fields (private by default)
        private string name;
        private int age;
        
        // Properties
        public string Name 
        { 
            get { return name; } 
            set { name = value; } 
        }
        
        public int Age 
        { 
            get { return age; } 
            set { age = value >= 0 ? value : 0; } 
        }
        
        // Constructor
        public Person(string name, int age)
        {
            this.name = name;
            this.age = age;
        }
        
        // Default constructor
        public Person() : this("Unknown", 0) { }
        
        // Methods
        public void Introduce()
        {
            Console.WriteLine($"Hi, I'm {name} and I'm {age} years old.");
        }
        
        public virtual void Work()
        {
            Console.WriteLine($"{name} is working.");
        }
    }
}
```

### Creating and Using Objects

```csharp
// File: Program.cs
using System;
using MyApplication.Models;

namespace MyApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Creating objects
            Person person1 = new Person("Alice", 25);
            Person person2 = new Person();
            
            // Using properties
            person2.Name = "Bob";
            person2.Age = 30;
            
            // Calling methods
            person1.Introduce();
            person2.Introduce();
        }
    }
}
```

## Access Modifiers

```csharp
public class AccessModifierExample
{
    public string PublicField;        // Accessible from anywhere
    private string privateField;      // Only within this class
    protected string protectedField;  // Within class and derived classes
    internal string internalField;    // Within same assembly
    protected internal string protectedInternalField; // Protected OR internal
    private protected string privateProtectedField;   // Protected AND internal
    
    public void DemonstrateAccess()
    {
        // All fields accessible within the same class
        PublicField = "Public";
        privateField = "Private";
        protectedField = "Protected";
        internalField = "Internal";
    }
}
```

## Constructors and Destructors

```csharp
public class ConstructorExample
{
    private string name;
    private int id;
    private static int objectCount = 0;
    
    // Default constructor
    public ConstructorExample()
    {
        name = "Default";
        id = ++objectCount;
        Console.WriteLine($"Default constructor called. Object #{id}");
    }
    
    // Parameterized constructor
    public ConstructorExample(string name) : this()
    {
        this.name = name;
        Console.WriteLine($"Parameterized constructor called for {name}");
    }
    
    // Copy constructor
    public ConstructorExample(ConstructorExample other)
    {
        this.name = other.name;
        this.id = ++objectCount;
        Console.WriteLine($"Copy constructor called for {name}");
    }
    
    // Static constructor
    static ConstructorExample()
    {
        Console.WriteLine("Static constructor called once");
    }
    
    // Destructor (Finalizer)
    ~ConstructorExample()
    {
        Console.WriteLine($"Destructor called for {name}");
    }
}
```

## Inheritance

```csharp
// Base class
public class Animal
{
    protected string name;
    protected int age;
    
    public Animal(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
    
    public virtual void MakeSound()
    {
        Console.WriteLine($"{name} makes a sound");
    }
    
    public virtual void Move()
    {
        Console.WriteLine($"{name} is moving");
    }
}

// Derived class
public class Dog : Animal
{
    private string breed;
    
    public Dog(string name, int age, string breed) : base(name, age)
    {
        this.breed = breed;
    }
    
    // Override base method
    public override void MakeSound()
    {
        Console.WriteLine($"{name} barks: Woof! Woof!");
    }
    
    // New method specific to Dog
    public void Fetch()
    {
        Console.WriteLine($"{name} is fetching the ball");
    }
}

// Another derived class
public class Cat : Animal
{
    public Cat(string name, int age) : base(name, age) { }
    
    public override void MakeSound()
    {
        Console.WriteLine($"{name} meows: Meow!");
    }
    
    public void Climb()
    {
        Console.WriteLine($"{name} is climbing");
    }
}
```

## Polymorphism

```csharp
public class PolymorphismExample
{
    public static void DemonstratePolymorphism()
    {
        // Runtime polymorphism
        Animal[] animals = {
            new Dog("Buddy", 3, "Golden Retriever"),
            new Cat("Whiskers", 2),
            new Animal("Generic", 1)
        };
        
        foreach (Animal animal in animals)
        {
            animal.MakeSound(); // Calls appropriate override
            animal.Move();
        }
        
        // Method overloading (compile-time polymorphism)
        Calculator calc = new Calculator();
        Console.WriteLine(calc.Add(5, 3));
        Console.WriteLine(calc.Add(5.5, 3.2));
        Console.WriteLine(calc.Add(1, 2, 3));
    }
}

public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }
    
    public double Add(double a, double b)
    {
        return a + b;
    }
    
    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}
```

## Encapsulation

```csharp
public class BankAccount
{
    private decimal balance;
    private string accountNumber;
    private string ownerName;
    
    public BankAccount(string accountNumber, string ownerName)
    {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0;
    }
    
    // Property with validation
    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; } // Private setter
    }
    
    public string AccountNumber
    {
        get { return accountNumber; }
    }
    
    public string OwnerName
    {
        get { return ownerName; }
        set 
        { 
            if (!string.IsNullOrWhiteSpace(value))
                ownerName = value; 
        }
    }
    
    // Controlled access methods
    public bool Deposit(decimal amount)
    {
        if (amount > 0)
        {
            balance += amount;
            return true;
        }
        return false;
    }
    
    public bool Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= balance)
        {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

## Abstraction

```csharp
// Abstract class
public abstract class Shape
{
    protected string name;
    
    public Shape(string name)
    {
        this.name = name;
    }
    
    // Abstract method (must be implemented by derived classes)
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();
    
    // Concrete method
    public virtual void Display()
    {
        Console.WriteLine($"Shape: {name}");
        Console.WriteLine($"Area: {CalculateArea():F2}");
        Console.WriteLine($"Perimeter: {CalculatePerimeter():F2}");
    }
}

public class Rectangle : Shape
{
    private double width;
    private double height;
    
    public Rectangle(double width, double height) : base("Rectangle")
    {
        this.width = width;
        this.height = height;
    }
    
    public override double CalculateArea()
    {
        return width * height;
    }
    
    public override double CalculatePerimeter()
    {
        return 2 * (width + height);
    }
}

public class Circle : Shape
{
    private double radius;
    
    public Circle(double radius) : base("Circle")
    {
        this.radius = radius;
    }
    
    public override double CalculateArea()
    {
        return Math.PI * radius * radius;
    }
    
    public override double CalculatePerimeter()
    {
        return 2 * Math.PI * radius;
    }
}
```

## Interfaces

```csharp
// Interface definition
public interface IDrawable
{
    void Draw();
    void SetColor(string color);
}

public interface IResizable
{
    void Resize(double factor);
}

// Multiple interface implementation
public class Square : Shape, IDrawable, IResizable
{
    private double side;
    private string color;
    
    public Square(double side) : base("Square")
    {
        this.side = side;
        this.color = "Black";
    }
    
    public override double CalculateArea()
    {
        return side * side;
    }
    
    public override double CalculatePerimeter()
    {
        return 4 * side;
    }
    
    // IDrawable implementation
    public void Draw()
    {
        Console.WriteLine($"Drawing a {color} square with side {side}");
    }
    
    public void SetColor(string color)
    {
        this.color = color;
    }
    
    // IResizable implementation
    public void Resize(double factor)
    {
        side *= factor;
        Console.WriteLine($"Square resized. New side: {side}");
    }
}

// Interface usage example
public class DrawingApp
{
    public static void ProcessDrawables(IDrawable[] drawables)
    {
        foreach (IDrawable drawable in drawables)
        {
            drawable.SetColor("Red");
            drawable.Draw();
        }
    }
}
```

## Static Members

```csharp
public class MathUtility
{
    // Static field
    public static readonly double PI = Math.PI;
    private static int calculationCount = 0;
    
    // Static property
    public static int CalculationCount
    {
        get { return calculationCount; }
    }
    
    // Static method
    public static double CalculateCircleArea(double radius)
    {
        calculationCount++;
        return PI * radius * radius;
    }
    
    public static double Power(double baseValue, int exponent)
    {
        calculationCount++;
        return Math.Pow(baseValue, exponent);
    }
    
    // Static constructor
    static MathUtility()
    {
        Console.WriteLine("MathUtility static constructor called");
    }
}

// Static class (all members must be static)
public static class StringExtensions
{
    public static string Reverse(this string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
    
    public static bool IsPalindrome(this string input)
    {
        string reversed = input.Reverse();
        return input.Equals(reversed, StringComparison.OrdinalIgnoreCase);
    }
}
```

## Properties

```csharp
public class Employee
{
    private string firstName;
    private string lastName;
    private decimal salary;
    private DateTime birthDate;
    
    // Auto-implemented properties
    public int EmployeeId { get; set; }
    public string Department { get; set; }
    
    // Properties with backing fields
    public string FirstName
    {
        get { return firstName; }
        set 
        { 
            if (!string.IsNullOrWhiteSpace(value))
                firstName = value.Trim(); 
        }
    }
    
    public string LastName
    {
        get { return lastName; }
        set 
        { 
            if (!string.IsNullOrWhiteSpace(value))
                lastName = value.Trim(); 
        }
    }
    
    // Read-only property
    public string FullName
    {
        get { return $"{firstName} {lastName}"; }
    }
    
    // Property with validation
    public decimal Salary
    {
        get { return salary; }
        set 
        { 
            if (value >= 0)
                salary = value; 
        }
    }
    
    // Calculated property
    public int Age
    {
        get 
        { 
            return DateTime.Now.Year - birthDate.Year; 
        }
    }
    
    // Property with private setter
    public DateTime HireDate { get; private set; }
    
    public Employee(string firstName, string lastName, DateTime birthDate)
    {
        FirstName = firstName;
        LastName = lastName;
        this.birthDate = birthDate;
        HireDate = DateTime.Now;
    }
}
```

## Namespaces and Using Statements

```csharp
// File: Models/Person.cs
using System;
using System.Collections.Generic;

namespace Company.HRSystem.Models
{
    public class Person
    {
        public string Name { get; set; }
        public DateTime BirthDate { get; set; }
    }
}

// File: Services/PersonService.cs
using System;
using System.Collections.Generic;
using Company.HRSystem.Models; // Import the Models namespace

namespace Company.HRSystem.Services
{
    public class PersonService
    {
        private List<Person> people = new List<Person>();
        
        public void AddPerson(Person person)
        {
            people.Add(person);
        }
        
        public List<Person> GetAllPeople()
        {
            return new List<Person>(people);
        }
    }
}

// File: Program.cs
using System;
using Company.HRSystem.Models;
using Company.HRSystem.Services;
using PersonAlias = Company.HRSystem.Models.Person; // Alias

namespace Company.HRSystem
{
    class Program
    {
        static void Main(string[] args)
        {
            PersonService service = new PersonService();
            PersonAlias person = new PersonAlias 
            { 
                Name = "John Doe", 
                BirthDate = new DateTime(1990, 1, 1) 
            };
            
            service.AddPerson(person);
        }
    }
}
```

## Class Import/Export Examples

### Project Structure Example

```
MyProject/
├── Models/
│   ├── Person.cs
│   ├── Employee.cs
│   └── Department.cs
├── Services/
│   ├── PersonService.cs
│   └── EmployeeService.cs
├── Utilities/
│   └── ValidationHelper.cs
└── Program.cs
```

### Exporting Classes (Making them available)

```csharp
// File: Models/Person.cs
using System;

namespace MyProject.Models
{
    // Public class - can be imported/used by other assemblies
    public class Person
    {
        public string Name { get; set; }
        public int Age { get; set; }
        
        public Person(string name, int age)
        {
            Name = name;
            Age = age;
        }
        
        public virtual void DisplayInfo()
        {
            Console.WriteLine($"Name: {Name}, Age: {Age}");
        }
    }
    
    // Internal class - only available within the same assembly
    internal class InternalHelper
    {
        public static bool ValidateAge(int age)
        {
            return age >= 0 && age <= 150;
        }
    }
}
```

```csharp
// File: Models/Employee.cs
using System;

namespace MyProject.Models
{
    public class Employee : Person
    {
        public string EmployeeId { get; set; }
        public decimal Salary { get; set; }
        public Department Department { get; set; }
        
        public Employee(string name, int age, string employeeId, decimal salary) 
            : base(name, age)
        {
            EmployeeId = employeeId;
            Salary = salary;
        }
        
        public override void DisplayInfo()
        {
            base.DisplayInfo();
            Console.WriteLine($"Employee ID: {EmployeeId}, Salary: {Salary:C}");
            if (Department != null)
                Console.WriteLine($"Department: {Department.Name}");
        }
        
        public void GiveRaise(decimal percentage)
        {
            Salary += Salary * (percentage / 100);
        }
    }
}
```

```csharp
// File: Models/Department.cs
using System;
using System.Collections.Generic;

namespace MyProject.Models
{
    public class Department
    {
        public string Name { get; set; }
        public string Code { get; set; }
        public List<Employee> Employees { get; set; }
        
        public Department(string name, string code)
        {
            Name = name;
            Code = code;
            Employees = new List<Employee>();
        }
        
        public void AddEmployee(Employee employee)
        {
            if (!Employees.Contains(employee))
            {
                Employees.Add(employee);
                employee.Department = this;
            }
        }
        
        public void RemoveEmployee(Employee employee)
        {
            if (Employees.Remove(employee))
            {
                employee.Department = null;
            }
        }
        
        public void DisplayDepartmentInfo()
        {
            Console.WriteLine($"Department: {Name} ({Code})");
            Console.WriteLine($"Employees: {Employees.Count}");
            foreach (var emp in Employees)
            {
                Console.WriteLine($"  - {emp.Name} ({emp.EmployeeId})");
            }
        }
    }
}
```

### Importing and Using Classes

```csharp
// File: Services/PersonService.cs
using System;
using System.Collections.Generic;
using System.Linq;
using MyProject.Models; // Import the Models namespace

namespace MyProject.Services
{
    public class PersonService
    {
        private List<Person> people;
        
        public PersonService()
        {
            people = new List<Person>();
        }
        
        public void AddPerson(Person person)
        {
            if (person != null)
            {
                people.Add(person);
                Console.WriteLine($"Added person: {person.Name}");
            }
        }
        
        public Person FindPersonByName(string name)
        {
            return people.FirstOrDefault(p => 
                p.Name.Equals(name, StringComparison.OrdinalIgnoreCase));
        }
        
        public List<Person> GetAllPeople()
        {
            return new List<Person>(people);
        }
        
        public void DisplayAllPeople()
        {
            Console.WriteLine("All People:");
            foreach (var person in people)
            {
                person.DisplayInfo();
                Console.WriteLine("---");
            }
        }
    }
}
```

```csharp
// File: Services/EmployeeService.cs
using System;
using System.Collections.Generic;
using System.Linq;
using MyProject.Models;

namespace MyProject.Services
{
    public class EmployeeService : PersonService
    {
        private List<Department> departments;
        
        public EmployeeService()
        {
            departments = new List<Department>();
        }
        
        public void CreateDepartment(string name, string code)
        {
            var dept = new Department(name, code);
            departments.Add(dept);
            Console.WriteLine($"Created department: {name}");
        }
        
        public Department FindDepartment(string code)
        {
            return departments.FirstOrDefault(d => 
                d.Code.Equals(code, StringComparison.OrdinalIgnoreCase));
        }
        
        public void AssignEmployeeToDepartment(Employee employee, string departmentCode)
        {
            var department = FindDepartment(departmentCode);
            if (department != null)
            {
                department.AddEmployee(employee);
                Console.WriteLine($"Assigned {employee.Name} to {department.Name}");
            }
            else
            {
                Console.WriteLine($"Department with code {departmentCode} not found");
            }
        }
        
        public void DisplayDepartmentStructure()
        {
            Console.WriteLine("Company Structure:");
            foreach (var dept in departments)
            {
                dept.DisplayDepartmentInfo();
                Console.WriteLine();
            }
        }
        
        public List<Employee> GetEmployeesByDepartment(string departmentCode)
        {
            var department = FindDepartment(departmentCode);
            return department?.Employees ?? new List<Employee>();
        }
    }
}
```

### Utility Classes

```csharp
// File: Utilities/ValidationHelper.cs
using System;
using System.Text.RegularExpressions;

namespace MyProject.Utilities
{
    public static class ValidationHelper
    {
        public static bool IsValidName(string name)
        {
            return !string.IsNullOrWhiteSpace(name) && 
                   name.Length >= 2 && 
                   Regex.IsMatch(name, @"^[a-zA-Z\s]+$");
        }
        
        public static bool IsValidAge(int age)
        {
            return age >= 0 && age <= 150;
        }
        
        public static bool IsValidEmployeeId(string employeeId)
        {
            return !string.IsNullOrWhiteSpace(employeeId) &&
                   Regex.IsMatch(employeeId, @"^EMP\d{4}$");
        }
        
        public static bool IsValidSalary(decimal salary)
        {
            return salary >= 0 && salary <= 1000000;
        }
        
        public static string FormatCurrency(decimal amount)
        {
            return amount.ToString("C");
        }
    }
}
```

### Main Program - Using All Classes

```csharp
// File: Program.cs
using System;
using MyProject.Models;          // Import Models
using MyProject.Services;        // Import Services
using MyProject.Utilities;       // Import Utilities

// Alternative specific imports:
// using Person = MyProject.Models.Person;
// using Employee = MyProject.Models.Employee;

namespace MyProject
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("=== HR Management System ===\n");
            
            // Create service instance
            EmployeeService empService = new EmployeeService();
            
            // Create departments
            empService.CreateDepartment("Information Technology", "IT");
            empService.CreateDepartment("Human Resources", "HR");
            empService.CreateDepartment("Finance", "FIN");
            
            Console.WriteLine();
            
            // Create and validate employees
            if (CreateAndAddEmployee(empService, "John Smith", 30, "EMP0001", 75000, "IT"))
            {
                Console.WriteLine("Employee 1 created successfully");
            }
            
            if (CreateAndAddEmployee(empService, "Jane Doe", 28, "EMP0002", 65000, "HR"))
            {
                Console.WriteLine("Employee 2 created successfully");
            }
            
            if (CreateAndAddEmployee(empService, "Bob Johnson", 35, "EMP0003", 80000, "IT"))
            {
                Console.WriteLine("Employee 3 created successfully");
            }
            
            Console.WriteLine();
            
            // Display all people
            empService.DisplayAllPeople();
            
            Console.WriteLine();
            
            // Display department structure
            empService.DisplayDepartmentStructure();
            
            // Find and give raise to an employee
            var john = empService.FindPersonByName("John Smith") as Employee;
            if (john != null)
            {
                Console.WriteLine($"John's salary before raise: {ValidationHelper.FormatCurrency(john.Salary)}");
                john.GiveRaise(10); // 10% raise
                Console.WriteLine($"John's salary after 10% raise: {ValidationHelper.FormatCurrency(john.Salary)}");
            }
            
            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
        
        private static bool CreateAndAddEmployee(EmployeeService service, string name, int age, 
            string empId, decimal salary, string deptCode)
        {
            // Validate input using utility class
            if (!ValidationHelper.IsValidName(name))
            {
                Console.WriteLine($"Invalid name: {name}");
                return false;
            }
            
            if (!ValidationHelper.IsValidAge(age))
            {
                Console.WriteLine($"Invalid age: {age}");
                return false;
            }
            
            if (!ValidationHelper.IsValidEmployeeId(empId))
            {
                Console.WriteLine($"Invalid employee ID: {empId}");
                return false;
            }
            
            if (!ValidationHelper.IsValidSalary(salary))
            {
                Console.WriteLine($"Invalid salary: {salary}");
                return false;
            }
            
            // Create employee
            var employee = new Employee(name, age, empId, salary);
            
            // Add to service
            service.AddPerson(employee);
            
            // Assign to department
            service.AssignEmployeeToDepartment(employee, deptCode);
            
            return true;
        }
    }
}
```

### Using External Libraries (NuGet Packages)

```csharp
// First install via Package Manager Console:
// Install-Package Newtonsoft.Json

using System;
using System.Collections.Generic;
using Newtonsoft.Json; // External library import
using MyProject.Models;

namespace MyProject.Services
{
    public class DataService
    {
        public string SerializeEmployee(Employee employee)
        {
            return JsonConvert.SerializeObject(employee, Formatting.Indented);
        }
        
        public Employee DeserializeEmployee(string json)
        {
            return JsonConvert.DeserializeObject<Employee>(json);
        }
        
        public void SaveEmployeesToJson(List<Employee> employees, string filePath)
        {
            string json = JsonConvert.SerializeObject(employees, Formatting.Indented);
            System.IO.File.WriteAllText(filePath, json);
        }
        
        public List<Employee> LoadEmployeesFromJson(string filePath)
        {
            if (System.IO.File.Exists(filePath))
            {
                string json = System.IO.File.ReadAllText(filePath);
                return JsonConvert.DeserializeObject<List<Employee>>(json);
            }
            return new List<Employee>();
        }
    }
}
```

## Key Points for Class Import/Export in C#

### Namespace Organization
- Use meaningful namespace hierarchies
- Group related classes in the same namespace
- Use company/project name as root namespace

### Access Modifiers for Export
- `public`: Available to all assemblies
- `internal`: Available only within the same assembly
- `protected`: Available to derived classes
- `private`: Available only within the same class

### Using Statements
- Import specific namespaces with `using`
- Create aliases with `using Alias = Namespace.Class`
- Global usings (C# 10+): `global using System;`

### Best Practices
- One class per file (generally)
- File name should match class name
- Use consistent naming conventions
- Group related functionality in services
- Use dependency injection for loose coupling
- Create utility classes for common operations

This comprehensive guide covers all aspects of C# OOP with practical examples of how to structure, import, and export classes in a real-world application.
