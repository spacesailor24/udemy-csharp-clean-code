# C# Clean Code

[**Udemy Course Link**](https://www.udemy.com/clean-code/learn/v4/content) - https://www.udemy.com/clean-code/learn/v4/content

## Table of Contents

Lecture Topic | Link
--- | ---
General Notes | [General Notes](#general-notes)
**SECTION 2** | [**Section 2**](#section-2)
Poor Names | [Lecture 5](#section-2-lecture-5)
Poor Naming Conventions | [Lecture 6](#section-2-lecture-6)
Poor Method Signatures | [Lecture 7](#section-2-lecture-7)
Long Parameter List | [Lecture 8](#section-2-lecture-8)
Output Parameters | [Lecture 9](#section-2-lecture-9)
Variable Declaration at the Top | [Lecture 10](#section-2-lecture-10)
Magic Numbers | [Lecture 11](#section-2-lecture-11)
Nested Conditionals | [Lecture 12](#section-2-lecture-12)

## General Notes

<!-- ################################################################################################################ -->
<!--                                                     SECTION 2                                                    -->
<!-- ################################################################################################################ -->

## SECTION 2

### Section 2 Lecture 5

#### Poor Names

Names **SHOULD**

- Be not too short, not too long
- Meaningful
- Reveal intention
- Chosen from problem domain (should utilize lanuage specific to the problem the code is solving. This help the next developer quickly understand what the code is for in reference to the project)

- Naming `variables`, `functions`, etc. with poor, undescriptive, names cause the person reading the code to look elsewhere to understand why that piece of code exists e.g.:

```csharp
SqlDataReader dr1;

int od;

void Button1_Click();

class Page1 {}
```

- To make reading of the code faster, names of  `variables`, `functions`, etc. should be descriptive in what they're there for (due names a reasonable length though!) e.g.:

```csharp
SqlDataReader dataReader;

int overdueDays;

void CheckAvailability_Click();

class ViewCustomerPage {}
```

- Don't use **meaningless names** such as: `void BeginCheckFunctionality_StoreClientSideCheckBoxIDsArray();` (also keep in mind the length of names)

- Don't use **names with encodings**:`int iMaxRequests;` should be `int maxRequsts` e.g.:

```csharp
var m_objCollection = new StringCollection(); // Bad, uses encoding

var countryNames = new StringCollection(); // Good, is descriptive

bool MultiSelect() {} // Bad, isn't descriptive enough

int? incidentNameId; // Bad, isn't descriptive enough
```

- Avoid **noisy names** such as:

```csharp
Customer theCustomer;

// The above should be...
Customer customer

List<Customer> listOfApprovedCustomers;

// The above should be...
List<Customer> approvedCustomers
```

### Section 2 Lecture 6

#### Poor Naming Conventions

- Inconsistent naming conventions e.g.:

```csharp
Customer GetCustomer(int _id); // The first letters in GetCustomer are capatilized - (Upper) CamelCase or PascalCase. The parameter gets a prefix of _

Customer saveCustomer(Customer Customer); // The first letter in each word AFTER the first are capatilized - (Lower) camelCase, The parameter gets the first letter capatilized

private int customerId; // Variable starts with lower case letter
```

- .NET Naming Conventions:

**PascalCase** the first letter in every word is capatilized

**(Lower) camelCase** the first letter in each word AFTER the first are capatilized

```csharp
public class Customer
{
    private int _id; // privite variables get prefix of _

    public string Name { get; set; }

    public void Charge(int amount) // paramters are (Lower) camelCase
    {
        var tax = 0; // variables are (Lower) camelCase
    }
}
```

### Section 2 Lecture 7

#### Poor Method Signatures

When Creating Method Signatures **YOU SHOULD**

- Check the return type
- Check the method name
- Check the parameters

```csharp
void Parse(int command); // Typically a parser would take a string and return a value

// The above should be...
int Parse(string command);
```

- Method signatures **SHOULD NOT** contain `boolean` as this typically means that the `method` will be doing two or more things, when each `method` should only be doing **one** thing e.g.:

```csharp
public User GetUser(string username, string password, bool login)
{
    if (login)
    {
        // Check if there is a user with the given username and password in db
        // If yes, set the last login date 
        // and then return the user. 
        var user = _dbContext.Users.SingleOrDefault(u => u.Username == username && u.Password == password);
        if (user != null)
            user.LastLogin = DateTime.Now;
        return user;
    }
    else
    {
        // Check if there is a user with the given username
        // If yes, return it, otherwise return null
        var user = _dbContext.Users.SingleOrDefault(u => u.Username == username);
        return user;
    }
}
```

### Section 2 Lecture 8

#### Long Parameter List

- Method Signatues **should** have **NO MORE** than 3 parameters e.g.:

```csharp
public IEnumerable<Reservation> GetReservations(
    DateTime dateFrom, DateTime dateTo,
    User user, int locationId,
    LocationType locationType, int? customerId = null)
{
    if (dateFrom >= DateTime.Now)
        throw new ArgumentNullException("dateFrom");
    if (dateTo <= DateTime.Now)
        throw new ArgumentNullException("dateTo");

    throw new NotImplementedException();
}

public IEnumerable<Reservation> GetUpcomingReservations(
    DateTime dateFrom, DateTime dateTo,
    User user, int locationId,
    LocationType locationType)
{
    if (dateFrom >= DateTime.Now)
        throw new ArgumentNullException("dateFrom");
    if (dateTo <= DateTime.Now)
        throw new ArgumentNullException("dateTo");

    throw new NotImplementedException();
}

// The above should be...

public class DateRange
{
    public DateTime dateFrom { get; set; }
    public DateTime dateTo { get; set; }
}

public class ReservationQuery
{
    public User user { get; set; }
    public int locationId { get; set; }
    public LocationType locationType { get; set; }
    public int customerId { get; set; }
}

public IEnumerable<Reservation> GetReservations(DateRange, ReservationQuery)
{
    if (DateRange.dateFrom >= DateTime.Now)
        throw new ArgumentNullException("dateFrom");
    if (DateRange.dateTo <= DateTime.Now)
        throw new ArgumentNullException("dateTo");

    throw new NotImplementedException();
}

public IEnumerable<Reservation> GetUpcomingReservations(DateRange, ReservationQuery)
{
    if (DateRange.dateFrom >= DateTime.Now)
        throw new ArgumentNullException("dateFrom");
    if (DateRange.dateTo <= DateTime.Now)
        throw new ArgumentNullException("dateTo");

    throw new NotImplementedException();
}
```

### Section 2 Lecture 9

#### Output Parameters

- Output Paramets **SHOULD BE AVOIDED**
- Return an object from a method instead

```csharp
public void DisplayCustomers()
{
    int totalCount = 0;
    var customers = GetCustomers(1, out totalCount);

    Console.WriteLine("Total customers: " + totalCount);
    foreach (var c in customers)
        Console.WriteLine(c);
}

public IEnumerable<Customer> GetCustomers(int pageIndex, out int totalCount)
{
    totalCount = 100;
    return new List<Customer>();
}

// The above should be...

public void DisplayCustomers()
{
    int totalCount = 0;
    var tuple = GetCustomers(1);
    totalCount = tuple.Item2;
    var customers = tuple.Item1;

    Console.WriteLine("Total customers: " + totalCount);
    foreach (var c in customers)
        Console.WriteLine(c);
}

public Tuple<IEnumerable<Customer>, int> GetCustomers(int pageIndex)
{
    totalCount = 100;
    return Tuple.Create((IEnumerable<Customer), new List<Customer>(), totalCount);
}

// To go a step further, the above should be...

public class GetCustomersResult
{
    public IEnumerable<Customer> Customers { get; set; }
    public int TotalCount { get; set; }
}

public void DisplayCustomers()
{
    var result = GetCustomers(pageIndex: 1);

    Console.WriteLine("Total customers: " + result.TotalCount);
    foreach (var c in result.Customers)
        Console.WriteLine(c);
}

public GetCustomersResult GetCustomers(int pageIndex)
{
    totalCount = 100;
    return new GetCustomersResult() { Customers = new List<Customer>(), TotalCount = totalCount };
}
```

### Section 2 Lecture 10

#### Variable Declaration at the Top

- Variables should be declared close to their usage
- If variables are used in many places within a `method`, declare them at the top of the `method`

```csharp
private PayFrequency _payFrequency;

public PayCalculator(PayFrequency payFrequency)
{
    _payFrequency = payFrequency;
}

public decimal CalcGross(decimal rate, decimal hours)
{
    decimal overtimeHours = 0;
    decimal regularHours = 0;
    decimal regularPay = 0;
    decimal overtimePay = 0;

    decimal grossPay = 0;

    if (_payFrequency == PayFrequency.Fortnightly)
    {
        if (hours > 80)
        {
            overtimeHours = hours - 80;
            regularHours = 80;
        }
        else
            regularHours = hours;
    }


    else if (_payFrequency == PayFrequency.Weekly)
    {
        if (hours > 40)
        {
            overtimeHours = hours - 40;
            regularHours = 40;
        }
        else
            regularHours = hours;
    }


    if (overtimeHours > 0m)
    {
        overtimePay += (rate * 1.5m) * overtimeHours;
    }

    regularPay = (regularHours * rate);
    grossPay = regularPay + overtimePay;

    return grossPay;
}

public enum PayFrequency
{
    Weekly,
    Fortnightly
}

// The above should be...

private PayFrequency _payFrequency;

public PayCalculator(PayFrequency payFrequency)
{
    _payFrequency = payFrequency;
}

public decimal CalcGross(decimal rate, decimal hours)
{
    // These are used multiple times in the below code, and can be assigned variables
    // at multiple places, so they should be declared at the top.
    decimal overtimeHours = 0;
    decimal regularHours = 0;

    if (_payFrequency == PayFrequency.Fortnightly)
    {
        if (hours > 80)
        {
            overtimeHours = hours - 80;
            regularHours = 80;
        }
        else
            regularHours = hours;
    }


    else if (_payFrequency == PayFrequency.Weekly)
    {
        if (hours > 40)
        {
            overtimeHours = hours - 40;
            regularHours = 40;
        }
        else
            regularHours = hours;
    }

    decimal overtimePay = 0;

    if (overtimeHours > 0m)
    {
        overtimePay += (rate * 1.5m) * overtimeHours;
    }

    var regularPay = (regularHours * rate); // The declaration is moved to the first assignment
    var grossPay = regularPay + overtimePay; // The declaration is moved to the first assignment

    return grossPay;
}

public enum PayFrequency
{
    Weekly,
    Fortnightly
}
```

### Section 2 Lecture 11

#### Magic Numbers

- Use constans or enums in place of Magic Numbers

```csharp
public void ApproveDocument(int status)
{
    if (status == 1) // 1 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
    {
        // ...
    }
    else if (status == 2) // 2 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
    {
        // ...
    }
}

public void RejectDoument(string status)
{
    switch (status)
    {
        case "1": // 1 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
            // ...
            break;
        case "2": // 2 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
            // ...
            break;
    }
}

// The above should be...

public enum DocumentStatus
{
    Draft = 1,
    Lodged = 2
}

public void ApproveDocument(DocumentStatus status)
{
    if (status == DocumentStatus.Draft) // 1 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
    {
        // ...
    }
    else if (status == DocumentStatus.Lodged) // 2 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
    {
        // ...
    }
}

public void RejectDoument(DocumentStatus status)
{
    switch (status)
    {
        case DocumentStatus.Draft: // 1 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
            // ...
            break;
        case DocumentStatus.Lodged: // 2 is a magic number, it has no clear meaning and is hard to reason about what it could stand for
            // ...
            break;
    }
}
```

### Section 2 Lecture 12

#### Nested Conditionals

- Use Ternary Operator e.g.:

```csharp
if (a)
    c = someValue
else
    c = someOtherValue

// The above could be...

c = (a) ? someValue : someOtherValue;
```

- Do **NOT** abuse Ternary Operators e.g.:

```csharp
c = a ? b : d ? e : f;
```

- Simplify true/false e.g.:

```csharp
if(a)
    b = true;
else
    b = false;

// The above should be...

b = a;
```

- Combine logic e.g.:

```csharp
if(a)
{
    if(b)
    {
        // ...
    }
}

// The above could be...

if (a && b)
{
    // ...
}
```

- Early Exit e.g.:

```csharp
if(a)
{
    if(b)
    {
        // ...
    }
}

// The above could be...

if(!a)
    return;

if(!b)
    return;

// ...

// Additonally, the above could utilize Combine...

if(!a || !b)
    return;

// ...
```

- Swap Orders

```csharp
if(a)
{
    if(b)
    {
        isValid = true;
    }
}

if(c)
{
    if(b)
    {
        isValid = true;
    }
}

// The above could be...

if(b)
{
    if(a)
    {
        isValid = true;
    }

    if(c)
    {
        isValid = true;
    }
}

// Additonally, the above could utilize Combine...

if(b)
{
    if(a || c)
    {
        isValid = true;
    }
}

// Additonally, the above could utilize Combine...

if(b && (a || c))
{
    isValid = true;
}

// And the above could be simplified to...

isValid = (b && (a || c));
```

- Everything in Moderation! **Don't** do this:

```csharp
if (a && (b || c) && !d || e && (f && !g || h))
{
    // ...
}
```

```csharp
public class Customer
{
    public int LoyaltyPoints { get; set; }
}

public class Reservation
{
    public Reservation(Customer customer, DateTime dateTime)
    {
        From = dateTime;
        Customer = customer;
    }

    public DateTime From { get; set; }
    public Customer Customer { get; set; }
    public bool IsCanceled { get; set; }

    public void Cancel()
    {
        // Gold customers can cancel up to 24 hours before
        if (Customer.LoyaltyPoints > 100)
        {
            // If reservation already started throw exception
            if (DateTime.Now > From)
            {
                throw new InvalidOperationException("It's too late to cancel.");
            }
            if ((From - DateTime.Now).TotalHours < 24)
            {
                throw new InvalidOperationException("It's too late to cancel.");
            }
            IsCanceled = true;
        }
        else
        {
            // Regular customers can cancel up to 48 hours before

            // If reservation already started throw exception
            if (DateTime.Now > From)
            {
                throw new InvalidOperationException("It's too late to cancel.");
            }
            if ((From - DateTime.Now).TotalHours < 48)
            {
                throw new InvalidOperationException("It's too late to cancel.");
            }
            IsCanceled = true;
        }
    }
}

// The abouve should be...

public class Customer
{
    private int LoyaltyPoints { get; set; }

    public bool isGoldMember()
        return loyaltyPoints > 100;
}

public class Reservation
{
    public Reservation(Customer customer, DateTime dateTime)
    {
        From = dateTime;
        Customer = customer;
    }

    public DateTime From { get; set; }
    public Customer Customer { get; set; }
    public bool IsCanceled { get; set; }

    public void Cancel()
    {
        if (isCancelationPeriodOver())
            throw new InvalidOperationException("It's too late to cancel.");

        IsCanceled = true;
    }

    private bool lessThan(int maxHours)
        return (From - DateTime.Now).TotalHours < maxHours;

    private bool isCancelationPeriodOver()
        return (Customer.isGoldMember && lessThan(maxHours: 24)) || (!Customer.isGoldMember && lessThan(maxHours: 48));
}
```

- And don't forget to write you Unit Tests!

```csharp
[TestClass]
public class CancelReservationTests
{
    [TestMethod]
    public void GoldCustomer_CancellingMoreThan24Hours_ShouldCancel()
    {
        var customer = CreateGoldCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddHours(25));

        reservation.Cancel();

        Assert.IsTrue(reservation.IsCanceled);
    }

    [TestMethod]
    [ExpectedException(typeof(InvalidOperationException))]
    public void GoldCustomer_CancellingLessThan24HoursBefore_ShouldThrowException()
    {
        var customer = CreateGoldCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddHours(23));

        reservation.Cancel();
    }

    [TestMethod]
    [ExpectedException(typeof(InvalidOperationException))]
    public void GoldCustomer_CancellingAfterStartDate_ShouldThrowException()
    {
        var customer = CreateGoldCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddDays(-1));

        reservation.Cancel();
    }

    [TestMethod]
    public void RegularCustomer_CancellingMoreThan48HoursBefore_ShouldCancel()
    {
        var customer = CreateRegularCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddHours(49));

        reservation.Cancel();

        Assert.IsTrue(reservation.IsCanceled);
    }

    [TestMethod]
    [ExpectedException(typeof(InvalidOperationException))]
    public void RegularCustomer_CancellingLessThan48Hours_ShouldThrowException()
    {
        var customer = CreateRegularCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddHours(47));

        reservation.Cancel();
    }

    [TestMethod]
    [ExpectedException(typeof(InvalidOperationException))]
    public void RegularCustomer_CancellingAfterStartDate_ShouldThrowException()
    {
        var customer = CreateRegularCustomer();
        var reservation = new Reservation(customer, DateTime.Now.AddHours(-1));

        reservation.Cancel();
    }

    private static Customer CreateGoldCustomer()
    {
        return new Customer { LoyaltyPoints = 200 };
    }

    private static Customer CreateRegularCustomer()
    {
        return new Customer();
    }
}
```