# C# Clean Code

[**Udemy Course Link**](https://www.udemy.com/clean-code/learn/v4/content) - https://www.udemy.com/clean-code/learn/v4/content

## Table of Contents

Lecture Topic | Link
--- | ---
General Notes | [General Notes](#general-notes)
**SECTION 2** | [**Section 2**](#section-2)
Poor Names | [Lecture 5](#section-2-lecture-5)
Poor Naming Conventions | [Lecture 6](#section-2-lecture-6)

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
Customer theCustomer; // should be Customer customer

List<Customer> listOfApprovedCustomers; // should be List<Customer> approvedCustomers
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
    private int _id;

    public string Name { get; set; }

    public void Charge(int amount)
    {
        var tax = 0;
    }
}
```