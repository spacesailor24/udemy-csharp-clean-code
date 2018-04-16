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
Switch Statements | [Lecture 13](#section-2-lecture-13)
Duplicated Code | [Lecture 14](#section-2-lecture-14)
Comments | [Lecture 15](#section-2-lecture-15)

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
```

The above should be...

```csharp
Customer customer

List<Customer> listOfApprovedCustomers;
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

To go a step further, the above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
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
```

The above should be...

```csharp
if(!a)
    return;

if(!b)
    return;

// ...
```

Additonally, the above could utilize Combine...

```csharp
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
```

The above could be...

```csharp
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
```

Additonally, the above could utilize Combine...

```csharp
if(b)
{
    if(a || c)
    {
        isValid = true;
    }
}
```

Additonally, the above could utilize Combine...

```csharp
if(b && (a || c))
{
    isValid = true;
}
```

And the above could be simplified to...

```csharp
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
```

The above should be...

```csharp
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

### Section 2 Lecture 13

#### Switch Statements

- Replace Switch Statements with polymorphic dispatch
- Use push member down refactoring

```csharp
public class MonthlyStatement
{
    public float CallCost { get; set; }
    public float SmsCost { get; set; }
    public float TotalCost { get; set; }

    public void Generate(MonthlyUsage usage)
    {
        switch (usage.Customer.Type)
        {
            case CustomerType.PayAsYouGo:
                CallCost = 0.12f * usage.CallMinutes;
                SmsCost = 0.12f * usage.SmsCount;
                TotalCost = CallCost + SmsCost;
                break;

            case CustomerType.Unlimited:
                TotalCost = 54.90f;
                break;

            default:
                throw new NotSupportedException("The current customer type is not supported");
        }
    }
}

public class MonthlyUsage
{
    public Customer Customer { get; set; }
    public int CallMinutes { get; set; }
    public int SmsCount { get; set; }
}

public class Customer
{
    public CustomerType Type { get; set; }
}

public enum CustomerType
{
    PayAsYouGo = 1,
    Unlimited
}
```

The above should be...

```csharp
public class MonthlyStatement
{
    public float CallCost { get; set; }
    public float SmsCost { get; set; }
    public float TotalCost { get; set; }
}

public class MonthlyUsage
{
    public Customer Customer { get; set; }
    public int CallMinutes { get; set; }
    public int SmsCount { get; set; }
}

public class Customer
{
    public abstract MonthlyStatement GenerateStatement(MonthlyUsage monthlyUsage);
}

public class PayAsYouGoCustomer : Customer
{
    public MonthlyStatement GenerateStatement(MonthlyUsage monthlyUsage)
    {
        var statement = new MonthlyStatement();

        statement.CallCost = 0.12f * monthlyUsage.CallMinutes;
        statement.SmsCost = 0.12f * monthlyUsage.SmsCount;
        statement.TotalCost = statement.CallCost + statement.SmsCost;

        return statement;
    }
}

public class UnlimitedCustomer : Customer
{
    public MonthlyStatement GenerateStatement(MonthlyUsage monthlyUsage)
    {
        var statement = new MonthlyStatement();

        statement.TotalCost = 54.90f;

        return statement;
    }
}
```

- And don't forget to run tests!

```csharp
[TestClass]
public class MonthlyStatementTests
{
    [TestMethod]
    public void PayAsYouGoCustomer_IsChargedBasedOnTheSumOfCostOfCallAndSms()
    {
        var customer = new PayAsYouGoCustomer();
        var usage = new MonthlyUsage { CallMinutes = 100, SmsCount = 100, Customer = customer };
        var statement = statement.Generate(usage);

        Assert.AreEqual(12.0f, statement.CallCost);
        Assert.AreEqual(12.0f, statement.SmsCost);
        Assert.AreEqual(24.0f, statement.TotalCost);
    }

    [TestMethod]
    public void UnlimitedCustomer_IsChargedAFlatRatePerMonth()
    {
        var customer = new UnlimitedCustomer();
        var usage = new MonthlyUsage { CallMinutes = 100, SmsCount = 100, Customer = customer };
        var statement = statement.Generate(usage);

        Assert.AreEqual(0, statement.CallCost);
        Assert.AreEqual(0, statement.SmsCost);
        Assert.AreEqual(54.90f, statement.TotalCost);
    }
}
```

### Section 2 Lecture 14

#### Duplicated Code

- **DRY**: Don't Repeat Yourself

```csharp
class DuplicatedCode
{
    public void AdmitGuest(string name, string admissionDateTime)
    {
        // Some logic
        // ...

        int time;
        int hours = 0;
        int minutes = 0;
        if (!string.IsNullOrWhiteSpace(admissionDateTime))
        {
            if (int.TryParse(admissionDateTime.Replace(":", ""), out time))
            {
                hours = time / 100;
                minutes = time % 100;
            }
            else
            {
                throw new ArgumentException("admissionDateTime");
            }

        }
        else
            throw new ArgumentNullException("admissionDateTime");

        // Some more logic
        // ...
        if (hours < 10)
        {

        }
    }

    public void UpdateAdmission(int admissionId, string name, string admissionDateTime)
    {
        // Some logic
        // ...

        int time;
        int hours = 0;
        int minutes = 0;
        if (!string.IsNullOrWhiteSpace(admissionDateTime))
        {
            if (int.TryParse(admissionDateTime.Replace(":", ""), out time))
            {
                hours = time / 100;
                minutes = time % 100;
            }
            else
            {
                throw new ArgumentException("admissionDateTime");
            }
        }
        else
            throw new ArgumentNullException("admissionDateTime");

        // Some more logic
        // ...
        if (hours < 10)
        {

        }
    }
}
```

The above should be...

```csharp
public class Time
{
    public int Hours { get; set; }
    public int Minutes { get; set; }

    public Time(int hours, int minutes)
    {
        Hours = hours;
        Minutes = minutes;
    }

    public static Time Parse(string str)
    {
        if (
            !string.IsNullOrWhiteSpace(str) &&
            int.TryParse(str.Replace(":", ""))
           )
        {
            var time = int.TryParse(str.Replace(":", "");
            var hours = time / 100;
            var minutes = time % 100;
        }
        else
            throw new ArgumentNullException("str");

        return new Time(hours, minutes);
    }
}

class DuplicatedCode
{
    public void AdmitGuest(string name, string admissionDateTime)
    {
        // Some logic
        // ...

        var time = Time.Parse(out hours, admissionDateTime, out minutes);

        // Some more logic
        // ...
        if (time.Hours < 10)
        {

        }
    }

    public void UpdateAdmission(int admissionId, string name, string admissionDateTime)
    {
        // Some logic
        // ...

        var time = Time.Parse(out hours, admissionDateTime, out minutes);

        // Some more logic
        // ...s
        if (time.Hours < 10)
        {

        }
    }
}
```

### Section 2 Lecture 15

#### Comments

- The ultimate comment for the code is **the code itsself**
- Don't write comments, re-write your code!
- Don't explain "whats" (the obvious)
- Explain "whys" and "hows"

Code Smells:

- Stating the Obvious

```csharp
// Returns the list of customers
public List<Customer> GetCustomers()
{
    // ...
}
```

OR

```csharp
// Create a new connection to the database
var connection = new SqlConnection();
```

OR

```csharp
// If the number of overdue days is less than 5, notify
// the customer. Otherwise, issue a fine.
if(overueDays < 5)
    NotifyCustomer();
else
    IssueFine();
```

- Version History

```csharp
// Prior to v1.3
if(isActive) {}
```

OR

```csharp
// 1 Jan 2000 - John Smith: ...
// 4 Jun 2003 - John Smith: ...
// 21 Dec 2005 - Andy McDonald: ...
public class WorkScheduler
```

- Clarify the Code

```csharp
var pf = 10; // Pay Frequency
```

- Dead Code

```csharp
// public class WorkScheduler
// {
// }
```

```csharp
public class Comments
{
    private int _pf;  // pay frequency
    private DbContext _dbContext;

    public Comments()
    {
        _dbContext = new DbContext();
    }

    // Returns list of customers in a country.
    public List<Customer> GetCustomers(int countryCode)
    {
        //TODO: We need to get rid of abcd once we revisit this method. Currently, it's 
        // creating a coupling betwen x and y and because of that we're not able to do 
        // xyz. 

        throw new NotImplementedException();
    }

    public void SubmitOrder(Order order)
    {
        // Save order to the database
        _dbContext.Orders.Add(order);
        _dbContext.SaveChanges();

        // Send an email to the customer
        var client = new SmtpClient();
        var message = new MailMessage("noreply@site.com", order.Customer.Email, "Your order was successfully placed.", ".");
        client.Send(message);

    }
}

public class DbContext
{
    public DbSet<Order> Orders { get; set; }

    public void SaveChanges()
    {


    }
}

public class DbSet<T>
{
    public void Add(Order order)
    {


    }
}
public class Order
{
    public Customer Customer { get; set; }
}

public class Customer
{
    public string Email { get; set; }
}
```

The above should be...

```csharp
public class Comments
{
    private int _payFrequency;
    private DbContext _dbContext;

    public Comments()
    {
        _dbContext = new DbContext();
    }

    public List<Customer> GetCustomers(int countryCode)
    {
        //TODO: We need to get rid of abcd once we revisit this method. Currently, it's
        // creating a coupling betwen x and y and because of that we're not able to do
        // xyz.

        throw new NotImplementedException();
    }

    public void SubmitOrder(Order order)
    {
        SaveOrder(order);

        NotifyCustomer(order);

    }

    public static void SaveOrder(Order order)
    {
        _dbContext.Orders.Add(order);
        _dbContext.SaveChanges();
    }

    public static void NotifyCustomer(Order order)
    {
        var client = new SmtpClient();
        var message = new MailMessage("noreply@site.com", order.Customer.Email, "Your order was successfully placed.", ".");
        client.Send(message);
    }
}

public class DbContext
{
    public DbSet<Order> Orders { get; set; }

    public void SaveChanges()
    {


    }
}

public class DbSet<T>
{
    public void Add(Order order)
    {


    }
}
public class Order
{
    public Customer Customer { get; set; }
}

public class Customer
{
    public string Email { get; set; }
}
```