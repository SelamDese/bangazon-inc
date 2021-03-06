# User Defined Types

A class is a blueprint, or a template, for creating an object instance in memory. C# and the .NET Framework define many types, such as `int`, `decimal`,  `bool`, `class`, `string`, and `interface`. In C#, you can also create your own, custom reference types.

You create a new reference type with a class.

> **Further reading:** [Types in C#](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/types)

```cs
public class Writer
{
    public void write (string message)
    {
        Console.WriteLine(message);
    }
}

// The output variable's type is `string` -- a built in type
string output = "Nashville Software School";

// The author variable's type is Writer -- a custom type you defined
Writer author = new Writer();
author.write(output);
```

## Accessibility Levels

In C#, there are five accessibility levels that can be applied to Classes, Methods, Properties and Data Members:

- public
- private
- protected
- internal
- protected internal

## Public

Classes and Methods that have tagged with the `public` keyword are accessibile without restriction. `public` methods are usable from derived classes (i.e. children) and from outside the inheritance heirarchy (i.e. family).

In some `Automobile.cs`:

```cs
// Base class
public class Automobile {

    public string Accelerate() {
        return "zoom!";
    }

}

// Derived class
public class Car : Automobile { }
```

> `Program.cs`:

```cs
Automobile generic_auto = new Automobile();
Console.WriteLine("Automobiles go {0}", generic_auto.Accelerate());

Car stella = new Car();
Console.WriteLine("Cars go {0}", stella.Accelerate());
```

## Private

Classes and Methods that have tagged with the `private` keyword are only accessible from within the class where it's defined. This means that private methods can not be called from anywhere outside the class, including derived classes (children).

Private methods are intended to be internal functionality. Consider the classes below:

```cs
// Base class
public class Automobile {

    public string Accelerate() {
        this.InjectFuel();
        return "zoom";
    }

    private string InjectFuel() {
        return "fueling";
    }
}

// Usage Example in a Program.cs file somewhere

Automobile generic_auto = new Automobile();
Console.WriteLine("Automobiles go {0}", generic_auto.Accelerate());

// However, the following line of code does not compile
generic_auto.InjectFuel();
```

```cs
// Given a Derived class, the follow code does not compile
public class Car : Automobile {

    public string CallAPrivateParentMethod {
        this.InjectFuel(); // Can not call InjectFuel b/c it's listed as private its parent class, Automobile
    }
}

// Usage Example in a Program.cs file somewhere
Car stella = new Car();

// Again, the following line of code does not compile
stella.InjectFuel();
```

## Protected

Classes and Methods that have tagged with the `protected` keyword prevent access from outside the class heirarchy (ie. the family); however, `protected` methods are usable from derived classes (i.e. children).

```cs
// Base class
public class Automobile {

    public string Break() {
        this.SqueezeBreakPads();
        return "skuuuuuur";
    }

    protected string SqueezeBreakPads() {
        return "";
    }
}

// Derived class
public class Car : Automobile {

    public string UseEmergencyBreak() {
        this.SqueezeBreakPads();
        return "skreeech!";
    }
}

// Usage Example in a Program.cs file somewhere
Car stella = new Car();

// UseEmergencyBreak method can use the protected SqueezeBreakPads from the Automobile class.
Console.WriteLine("Applying the break: {0}", stella.UseEmergencyBreak());
```

## Internal

Classes and Methods that have tagged with the `internal` keyword allow access from anywhere within the same compiled DLL (assembly).

## Protected Internal

`protected internal` is the only allowed combination of multiple access modifiers.

## Resources

* https://msdn.microsoft.com/en-us/library/ba0a1yw2.aspx

> **Resource:** [Access Modifiers (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/access-modifiers)

# Properties and Fields

Class properties are the interface you provide to external code to get, and modify, the values of the private fields.

```cs
public class Customer
{
    /*
        Fields should have private accessibility
    */
    private string firstName;
    private string lastName;

    /*
        Properties should have public accessibility
    */

    // Simple property that doesn't allow blank values for first name
    public string FirstName {
        get
        {
            return firstName;
        }
        set
        {
            if (value != "")
            {
                firstName = value;
            }
        }
    }

    // Simple property that doesn't allow blank values for last name
    public string LastName {
        get
        {
            return lastName;
        }
        set
        {
            if (value != "")
            {
                lastName = value;
            }
        }
    }

    // Calculated property that has no mutator (setter)
    public string FullName {
        get
        {
            return string.Format($"{firstName} {lastName}");
        }
    }
}
```

> **Resource:** [Properties (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties)

# Methods

Methods are the new name for functions. They are code blocks on a class that performs a series of logic. Think of them as the behaviors of your custom type. Copy pasta this example code into your `Program.cs` file.

```cs
using System;
using System.Collections.Generic;

namespace Classes
{
    public class Customer
    {
        private string firstName;
        private string lastName;

        public string FirstName
        {
            get
            {
                return firstName;
            }
            set
            {
                if (value != "")
                {
                    firstName = value;
                }
            }
        }

        public string LastName
        {
            get
            {
                return lastName;
            }
            set
            {
                if (value != "")
                {
                    lastName = value;
                }
            }
        }

        public bool IsLocal { get; set; } = false;

        public string FullName
        {
            get
            {
                return string.Format($"{firstName} {lastName}");
            }
        }
    }

    public enum TransitType
    {
        Train,
        Plane,
        Truck
    }

    public class DeliveryService
    {
        /*
          Properties
        */
        public string Name { get; set; }

        public TransitType Transit { get; set; }

        /*
          Methods
        */
        public void Deliver(Product product, Customer customer)
        {
            Console.WriteLine($"Product delivered by {this.Transit} to {customer.FullName}");
        }
    }

    public class Product
    {
        /*
          Properties
        */
        public string Title { get; set; }
        public string Description { get; set; }
        public double Price { get; set; }
        public int Quantity { get; set; }

        /*
          Methods
        */
        public void Ship(Customer customer, DeliveryService service)
        {
            if (!customer.IsLocal)
            {
                service.Deliver(this, customer);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {


            Product tinkerToys = new Product()
            {
                Title = "Tinker Toys",
                Description = "You can build anything you want",
                Price = 32.49,
                Quantity = 25
            };

            tinkerToys.Ship(
                new Customer()
                {
                    FirstName = "Marcus",
                    LastName = "Fulbright",
                    IsLocal = false
                },
                new DeliveryService()
                {
                    Name = "UPS",
                    Transit = TransitType.Train
                }
            );
        }
    }
}

```

> **Resource:** [Properties (C# Programming Guide)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/properties)