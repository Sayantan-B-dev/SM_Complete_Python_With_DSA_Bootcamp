## Table of Contents

1. [Definition and Concept Overview](#1-definition-and-concept-overview)  
   1.1 [What are Classes and Objects?](#11-what-are-classes-and-objects)  
   1.2 [Need for Classes and Objects](#12-need-for-classes-and-objects)  
   1.3 [Object-Oriented Programming in Python](#13-object-oriented-programming-in-python)  
   1.4 [When to Use Classes, When Not To](#14-when-to-use-classes-when-not-to)  

2. [Core Principles and Internal Mechanics](#2-core-principles-and-internal-mechanics)  
   2.1 [Class as Blueprint, Object as Instance](#21-class-as-blueprint-object-as-instance)  
   2.2 [Class Definition and Execution](#22-class-definition-and-execution)  
   2.3 [Attribute Resolution: Instance vs Class](#23-attribute-resolution-instance-vs-class)  
   2.4 [Method Binding and the `self` Parameter](#24-method-binding-and-the-self-parameter)  
   2.5 [Special Methods (`__init__`, `__str__`, etc.)](#25-special-methods-__init__-__str__-etc)  
   2.6 [Instance Variables and Instance Methods](#26-instance-variables-and-instance-methods)  
   2.7 [Class Variables and Class Methods](#27-class-variables-and-class-methods)  
   2.8 [Static Methods](#28-static-methods)  

3. [Step-by-Step Logical Breakdown](#3-step-by-step-logical-breakdown)  
   3.1 [Defining a Class](#31-defining-a-class)  
   3.2 [Creating an Instance (Instantiation)](#32-creating-an-instance-instantiation)  
   3.3 [Accessing Attributes and Methods](#33-accessing-attributes-and-methods)  
   3.4 [Initialization with `__init__`](#34-initialization-with-__init__)  
   3.5 [Method Invocation and `self`](#35-method-invocation-and-self)  
   3.6 [Modifying and Deleting Attributes](#36-modifying-and-deleting-attributes)  

4. [Implementation: Banking System Example](#4-implementation-banking-system-example)  
   4.1 [Custom Exceptions](#41-custom-exceptions)  
   4.2 [Base Class: `BankAccount`](#42-base-class-bankaccount)  
   4.3 [Derived Class: `SavingsAccount`](#43-derived-class-savingsaccount)  
   4.4 [Derived Class: `CheckingAccount`](#44-derived-class-checkingaccount)  
   4.5 [Demonstration of Usage](#45-demonstration-of-usage)  

5. [Time and Space Complexity Analysis](#5-time-and-space-complexity-analysis)  
   5.1 [Time Complexity of Core Operations](#51-time-complexity-of-core-operations)  
   5.2 [Space Complexity](#52-space-complexity)  

6. [Edge Cases and Failure Scenarios](#6-edge-cases-and-failure-scenarios)  
   6.1 [Invalid Amounts](#61-invalid-amounts)  
   6.2 [Insufficient Funds](#62-insufficient-funds)  
   6.3 [Account Closure](#63-account-closure)  
   6.4 [Type Errors and Non‑numeric Input](#64-type-errors-and-nonnumeric-input)  
   6.5 [Overdraft Limit Exceeded](#65-overdraft-limit-exceeded)  
   6.6 [Concurrent Access (Thread Safety)](#66-concurrent-access-thread-safety)  

7. [Practical Use Cases](#7-practical-use-cases)  

8. [Limitations and Trade-offs](#8-limitations-and-trade-offs)  

---

## 1. Definition and Concept Overview

### 1.1 What are Classes and Objects?

A **class** is a user-defined blueprint or prototype from which objects are created. It encapsulates data (attributes) and functions (methods) that operate on that data. An **object** is an instance of a class—a concrete entity that exists in memory and holds specific values for the attributes defined by the class.

In Python, classes are themselves objects (instances of `type`), reinforcing the language’s consistent object model.

### 1.2 Need for Classes and Objects

Classes address several software engineering concerns:

- **Encapsulation**: Bundling data and the operations that manipulate that data into a single unit, hiding internal representation.
- **Abstraction**: Exposing only essential interfaces while concealing complex implementation details.
- **Reusability**: Classes can be instantiated multiple times, and through inheritance, existing classes can be extended.
- **Modularity**: Code is organized into discrete, self‑contained units, improving maintainability.
- **Modeling Real‑World Entities**: Classes naturally map to real‑world concepts (e.g., BankAccount, Customer, Transaction), making system design more intuitive.

### 1.3 Object-Oriented Programming in Python

Python is a multi‑paradigm language but fully supports object‑oriented programming (OOP). Key OOP features in Python include:

- Classes and objects.
- Inheritance (single and multiple).
- Polymorphism (method overriding, duck typing).
- Encapsulation via name mangling (`__private` attributes) and property decorators.

Every value in Python is an object, including integers, strings, functions, and classes themselves.

### 1.4 When to Use Classes, When Not To

**Use classes when:**
- The system involves entities that have both state and behavior.
- Multiple instances of the same concept are needed.
- Code reuse through inheritance or composition is beneficial.
- The problem naturally fits an object‑oriented model (e.g., simulations, GUI applications, business domain modeling).

**Avoid classes when:**
- The task is trivial and a simple function suffices.
- The overhead of defining a class is not justified (e.g., one‑off scripts).
- The data is purely passive and a `namedtuple` or `dataclass` provides a lighter alternative.
- Performance is critical and the overhead of attribute lookup matters (though this is rarely the deciding factor).

## 2. Core Principles and Internal Mechanics

### 2.1 Class as Blueprint, Object as Instance

A class defines the structure and behavior; an object is a concrete allocation of that structure with specific attribute values. Multiple objects can be created from the same class, each with its own independent attribute storage.

### 2.2 Class Definition and Execution

When the `class` keyword is encountered, Python executes the class body in a new namespace. The resulting namespace (dictionary) is then used to create a class object (an instance of `type`). This class object is bound to the class name in the surrounding scope.

### 2.3 Attribute Resolution: Instance vs Class

Attribute access (e.g., `obj.attr`) follows a hierarchy:
1. Instance `__dict__`.
2. Class `__dict__`.
3. Base classes (in method resolution order).

If the attribute is not found, `AttributeError` is raised. Methods are attributes that happen to be functions; they are bound when accessed through an instance.

### 2.4 Method Binding and the `self` Parameter

When a function defined in a class is accessed via an instance, Python automatically binds the method: the instance is passed as the first argument (`self` by convention). For example, `obj.method(args)` is equivalent to `Class.method(obj, args)`. The name `self` is not a keyword; it is a conventional name for the instance reference.

### 2.5 Special Methods (`__init__`, `__str__`, etc.)

Special methods (also called “dunder” methods) are surrounded by double underscores and allow classes to define behavior for built‑in operations. Examples:

- `__init__(self, ...)`: Initializes a newly created instance.
- `__str__(self)`: Returns a user‑friendly string representation.
- `__repr__(self)`: Returns an unambiguous string representation.
- `__add__(self, other)`: Defines behavior for the `+` operator.

### 2.6 Instance Variables and Instance Methods

**Instance variables** are attributes unique to each instance. They are typically set in `__init__` using `self.attr = value`. **Instance methods** are functions defined inside the class that take `self` as the first parameter; they operate on a specific instance’s data.

### 2.7 Class Variables and Class Methods

**Class variables** are shared across all instances of a class. They are defined directly in the class body, outside any method. Modifying a class variable through an instance actually creates an instance variable that shadows the class variable.

**Class methods** are declared with the `@classmethod` decorator and take the class as the first argument (`cls`). They can access or modify class‑level state and are often used as factory methods.

### 2.8 Static Methods

Static methods (decorated with `@staticmethod`) do not receive an implicit first argument (neither `self` nor `cls`). They behave like plain functions but belong to the class’s namespace. They are used when the method is related to the class conceptually but does not depend on instance or class state.

## 3. Step-by-Step Logical Breakdown

### 3.1 Defining a Class

```python
class ClassName:
    """Optional docstring."""
    class_variable = value

    def __init__(self, parameters):
        self.instance_variable = parameters

    def instance_method(self):
        # operates on self
```

### 3.2 Creating an Instance (Instantiation)

`obj = ClassName(arguments)` triggers:
1. Creation of a new instance object.
2. Call to `__new__` (rarely overridden) to allocate memory.
3. Call to `__init__` to initialize the instance with the provided arguments.

### 3.3 Accessing Attributes and Methods

- Get: `obj.attribute`, `obj.method()`
- Set: `obj.attribute = new_value`
- Delete: `del obj.attribute`

### 3.4 Initialization with `__init__`

`__init__` is called after the instance is created. It sets up initial state. It should not return a value (implicitly returns `None`).

### 3.5 Method Invocation and `self`

Inside a method, `self` refers to the instance on which the method was called. All accesses to other instance attributes or methods must use `self.attribute`.

### 3.6 Modifying and Deleting Attributes

Attributes can be added, modified, or deleted dynamically (Python is dynamic). Deleting an instance variable is possible with `del self.attr`, but this is rarely needed.

## 4. Implementation: Banking System Example

The following implementation demonstrates a professional banking system with multiple account types, validation, and error handling. It includes custom exceptions, encapsulation, class methods, and static methods.

### 4.1 Custom Exceptions

```python
class BankingError(Exception):
    """Base exception for banking operations."""
    pass

class InsufficientFundsError(BankingError):
    """Raised when withdrawal amount exceeds available balance."""
    pass

class InvalidAmountError(BankingError):
    """Raised when transaction amount is negative or zero."""
    pass

class AccountClosedError(BankingError):
    """Raised when attempting to operate on a closed account."""
    pass
```

### 4.2 Base Class: `BankAccount`

```python
from datetime import datetime
from typing import List, Tuple, Optional

class BankAccount:
    """
    Base class representing a generic bank account.
    """

    # Class variable: interest rate (as decimal) shared by all accounts
    base_interest_rate = 0.01  # 1%

    # Class variable to generate unique account numbers
    _last_account_number = 1000

    def __init__(self, account_holder: str, initial_balance: float = 0.0):
        """
        Initialize a new bank account.

        :param account_holder: Name of the account holder.
        :param initial_balance: Opening balance (must be non-negative).
        :raises InvalidAmountError: If initial_balance is negative.
        """
        if initial_balance < 0:
            raise InvalidAmountError("Initial balance cannot be negative.")

        # Instance variables (encapsulated with leading underscore)
        self._account_holder = account_holder
        self._balance = initial_balance
        self._is_closed = False
        self._transaction_history: List[Tuple[str, float, datetime]] = []
        self._account_number = self._generate_account_number()

        # Record initial deposit if any
        if initial_balance > 0:
            self._record_transaction("DEPOSIT", initial_balance)

    @classmethod
    def _generate_account_number(cls) -> int:
        """Generate a unique account number."""
        cls._last_account_number += 1
        return cls._last_account_number

    def _record_transaction(self, transaction_type: str, amount: float) -> None:
        """Internal method to record a transaction with timestamp."""
        self._transaction_history.append((transaction_type, amount, datetime.now()))

    def _check_account_active(self) -> None:
        """Raise AccountClosedError if account is closed."""
        if self._is_closed:
            raise AccountClosedError("Account is closed.")

    def deposit(self, amount: float) -> float:
        """
        Deposit funds into the account.

        :param amount: Positive amount to deposit.
        :return: New balance after deposit.
        :raises InvalidAmountError: If amount <= 0.
        :raises AccountClosedError: If account is closed.
        """
        self._check_account_active()
        if amount <= 0:
            raise InvalidAmountError("Deposit amount must be positive.")

        self._balance += amount
        self._record_transaction("DEPOSIT", amount)
        return self._balance

    def withdraw(self, amount: float) -> float:
        """
        Withdraw funds from the account.

        :param amount: Positive amount to withdraw.
        :return: New balance after withdrawal.
        :raises InvalidAmountError: If amount <= 0.
        :raises InsufficientFundsError: If balance < amount.
        :raises AccountClosedError: If account is closed.
        """
        self._check_account_active()
        if amount <= 0:
            raise InvalidAmountError("Withdrawal amount must be positive.")
        if self._balance < amount:
            raise InsufficientFundsError(
                f"Insufficient funds. Available: {self._balance}, Requested: {amount}"
            )

        self._balance -= amount
        self._record_transaction("WITHDRAWAL", amount)
        return self._balance

    def get_balance(self) -> float:
        """Return current balance."""
        self._check_account_active()
        return self._balance

    def get_statement(self) -> List[Tuple[str, float, str]]:
        """
        Return transaction history with formatted timestamps.

        :return: List of (type, amount, timestamp_string)
        """
        self._check_account_active()
        return [
            (t_type, amt, ts.strftime("%Y-%m-%d %H:%M:%S"))
            for t_type, amt, ts in self._transaction_history
        ]

    def transfer_to(self, target_account: 'BankAccount', amount: float) -> None:
        """
        Transfer funds to another bank account.

        :param target_account: Another BankAccount instance.
        :param amount: Positive amount to transfer.
        :raises InvalidAmountError: If amount <= 0.
        :raises InsufficientFundsError: If insufficient balance.
        :raises AccountClosedError: If either account is closed.
        """
        self._check_account_active()
        target_account._check_account_active()
        if amount <= 0:
            raise InvalidAmountError("Transfer amount must be positive.")
        if self._balance < amount:
            raise InsufficientFundsError(
                f"Insufficient funds for transfer. Available: {self._balance}"
            )

        # Perform transfer atomically (in a real system, this would be a transaction)
        self._balance -= amount
        target_account._balance += amount

        self._record_transaction(f"TRANSFER OUT to {target_account._account_number}", amount)
        target_account._record_transaction(f"TRANSFER IN from {self._account_number}", amount)

    def close_account(self) -> float:
        """
        Close the account after withdrawing all funds.

        :return: Final balance paid out.
        :raises AccountClosedError: If already closed.
        """
        self._check_account_active()
        payout = self._balance
        self._balance = 0.0
        self._is_closed = True
        self._record_transaction("ACCOUNT CLOSED", payout)
        return payout

    @property
    def account_holder(self) -> str:
        """Return account holder's name."""
        return self._account_holder

    @property
    def account_number(self) -> int:
        """Return account number."""
        return self._account_number

    @property
    def is_closed(self) -> bool:
        """Return True if account is closed."""
        return self._is_closed

    @classmethod
    def set_base_interest_rate(cls, new_rate: float) -> None:
        """
        Class method to change the base interest rate for all accounts.

        :param new_rate: New interest rate as decimal (e.g., 0.02 for 2%).
        :raises ValueError: If rate is negative.
        """
        if new_rate < 0:
            raise ValueError("Interest rate cannot be negative.")
        cls.base_interest_rate = new_rate

    @staticmethod
    def validate_amount(amount: float) -> bool:
        """Static method to validate a monetary amount."""
        return isinstance(amount, (int, float)) and amount > 0

    def __str__(self) -> str:
        """User-friendly string representation."""
        status = "OPEN" if not self._is_closed else "CLOSED"
        return f"Account {self._account_number} [{self._account_holder}] Balance: ${self._balance:.2f} ({status})"
```

### 4.3 Derived Class: `SavingsAccount`

Inherits from `BankAccount` and adds interest calculation.

```python
class SavingsAccount(BankAccount):
    """Savings account that earns interest."""

    def __init__(self, account_holder: str, initial_balance: float = 0.0, interest_rate: Optional[float] = None):
        """
        :param interest_rate: If None, uses base_interest_rate.
        """
        super().__init__(account_holder, initial_balance)
        self._interest_rate = interest_rate if interest_rate is not None else BankAccount.base_interest_rate

    def apply_interest(self) -> None:
        """Calculate and deposit interest based on current balance."""
        self._check_account_active()
        interest = self._balance * self._interest_rate
        if interest > 0:
            self._balance += interest
            self._record_transaction("INTEREST", interest)

    @property
    def interest_rate(self) -> float:
        return self._interest_rate

    @interest_rate.setter
    def interest_rate(self, new_rate: float) -> None:
        if new_rate < 0:
            raise ValueError("Interest rate cannot be negative.")
        self._interest_rate = new_rate
```

### 4.4 Derived Class: `CheckingAccount`

Adds overdraft protection.

```python
class CheckingAccount(BankAccount):
    """Checking account with an overdraft limit."""

    def __init__(self, account_holder: str, initial_balance: float = 0.0, overdraft_limit: float = 100.0):
        super().__init__(account_holder, initial_balance)
        self._overdraft_limit = max(overdraft_limit, 0.0)  # non‑negative

    def withdraw(self, amount: float) -> float:
        """
        Override withdraw to allow overdraft up to the limit.
        """
        self._check_account_active()
        if amount <= 0:
            raise InvalidAmountError("Withdrawal amount must be positive.")
        # Available funds = balance + overdraft limit
        if self._balance + self._overdraft_limit < amount:
            raise InsufficientFundsError(
                f"Cannot withdraw {amount}. Available including overdraft: {self._balance + self._overdraft_limit}"
            )
        self._balance -= amount
        self._record_transaction("WITHDRAWAL", amount)
        return self._balance

    @property
    def overdraft_limit(self) -> float:
        return self._overdraft_limit

    @overdraft_limit.setter
    def overdraft_limit(self, new_limit: float) -> None:
        if new_limit < 0:
            raise ValueError("Overdraft limit cannot be negative.")
        self._overdraft_limit = new_limit
```

### 4.5 Demonstration of Usage

```python
# Example usage (commented to keep documentation focused)
# try:
#     acc1 = SavingsAccount("Alice", 1000.0, interest_rate=0.02)
#     acc2 = CheckingAccount("Bob", 500.0, overdraft_limit=200.0)
#
#     print(acc1)  # Account 1001 [Alice] Balance: $1000.00 (OPEN)
#     acc1.deposit(200)
#     acc1.withdraw(50)
#     acc1.apply_interest()
#     print(f"Alice's balance after interest: {acc1.get_balance()}")
#
#     acc2.withdraw(600)  # Uses overdraft: balance becomes -100
#     print(f"Bob's balance: {acc2.get_balance()}")
#
#     # Transfer
#     acc1.transfer_to(acc2, 300)
#     print(f"After transfer: Alice={acc1.get_balance()}, Bob={acc2.get_balance()}")
#
#     # Statement
#     for t in acc1.get_statement():
#         print(t)
#
# except BankingError as e:
#     print(f"Banking error: {e}")
```

## 5. Time and Space Complexity Analysis

### 5.1 Time Complexity of Core Operations

- **deposit**, **withdraw**, **get_balance**, **apply_interest**: O(1) – constant time operations, assuming no iteration over transaction history.
- **transfer_to**: O(1) – updates balances and records two transactions; constant time.
- **get_statement**: O(n) where n is the number of transactions, because it constructs a list by iterating over the history.
- **close_account**: O(1) – marks closed and records one transaction.

The transaction history storage does not affect the time complexity of core operations, but generating a statement requires scanning all records.

### 5.2 Space Complexity

- Each account instance stores its attributes: account holder (string), balance (float), status (bool), transaction history (list). The history list grows linearly with the number of transactions: O(m) space per account, where m is the number of transactions.
- Class variables (`base_interest_rate`, `_last_account_number`) occupy a single memory location shared by all instances.

The design trades memory for auditability; if transaction history were omitted, space would be O(1) per account, but accountability would be lost.

## 6. Edge Cases and Failure Scenarios

### 6.1 Invalid Amounts

- Depositing or withdrawing zero or negative amounts raises `InvalidAmountError`.
- Creating an account with negative initial balance raises the same.

### 6.2 Insufficient Funds

- Withdrawal from a standard account when balance < amount raises `InsufficientFundsError`.
- Transfer with insufficient balance raises the same.
- Checking accounts allow overdraft; if amount exceeds balance + limit, `InsufficientFundsError` is raised.

### 6.3 Account Closure

- Any operation (deposit, withdraw, transfer, get_balance) on a closed account raises `AccountClosedError`.
- Closing an already closed account raises the same.
- Closing an account pays out the remaining balance and sets balance to zero.

### 6.4 Type Errors and Non‑numeric Input

If non‑numeric values are passed (e.g., string), Python raises `TypeError` before the method body executes. The methods do not explicitly catch this; it is expected that callers provide correct types. For robustness, one could add type checking, but Python’s duck typing philosophy typically leaves such checks to the caller.

### 6.5 Overdraft Limit Exceeded

For `CheckingAccount`, if the withdrawal amount exceeds balance + overdraft limit, `InsufficientFundsError` is raised, consistent with the base class but with a different threshold.

### 6.6 Concurrent Access (Thread Safety)

The implementation is not thread‑safe. In a multi‑threaded environment, simultaneous deposits/withdrawals could lead to race conditions. A production system would require locks or database transactions.

## 7. Practical Use Cases

- **Banking Software**: Core domain model for accounts, transactions, and customers.
- **E‑commerce Wallets**: Managing user balances, payments, and refunds.
- **Subscription Billing**: Tracking customer credits and debits.
- **Accounting Systems**: Representing ledgers, invoices, and payments.
- **Educational Examples**: Teaching OOP concepts, inheritance, and encapsulation.

## 8. Limitations and Trade-offs

- **Memory Overhead**: Storing full transaction history for each account consumes memory. For long‑lived accounts with many transactions, this can become significant. Alternatives include archiving old transactions to a database.
- **Performance**: The linear scan for `get_statement` may become slow for thousands of transactions. Indexing or pagination would be needed in a real system.
- **Thread Safety**: Not addressed; real applications require synchronization or use of database transactions.
- **Persistence**: The classes do not provide any persistence mechanism. In production, account data would be stored in a database; these classes would serve as an in‑memory representation (e.g., ORM models).
- **Inheritance Depth**: The example uses single inheritance; deep hierarchies can become brittle. Composition is often preferred over inheritance in large systems.
- **Encapsulation**: Python’s name mangling (`__attr`) provides limited privacy; the leading underscore is a convention, not enforcement. Trust is required that client code respects the interface.
- **Error Handling Granularity**: Custom exceptions provide clear semantics but add complexity. For smaller projects, using built‑in exceptions (e.g., `ValueError`) might suffice.