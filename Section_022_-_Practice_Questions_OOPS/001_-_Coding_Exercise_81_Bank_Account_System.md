```python
# Quickcode
class BankAccount:
    def __init__(self, account_holder, initial_balance):
        self.account_holder = account_holder
        self.balance = initial_balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount <= self.balance:
            self.balance -= amount
        else:
            raise ValueError("Insufficient balance")

    def transfer(self, other_account, amount):
        if amount <= self.balance:
            self.balance -= amount
            other_account.balance += amount
        else:
            raise ValueError("Insufficient balance")
```


## Python Implementation — `BankAccount` Class

### Class Design Explanation

A **bank account abstraction** encapsulates account holder identity and monetary balance while exposing controlled operations that manipulate the balance. The object maintains internal state using instance attributes and provides public methods for deposit, withdrawal, and transfer operations.

Key design principles applied include:

* **Encapsulation**: Balance modifications occur only through defined class methods.
* **State integrity**: Withdrawal and transfer operations validate sufficient funds before deduction.
* **Reusability**: Transfer operation internally reuses deposit and withdrawal logic.

### Class Structure

| Component                         | Purpose                                    |
| --------------------------------- | ------------------------------------------ |
| `account_holder`                  | Stores the account owner name              |
| `balance`                         | Maintains current account balance          |
| `deposit(amount)`                 | Adds funds to the account                  |
| `withdraw(amount)`                | Removes funds if sufficient balance exists |
| `transfer(other_account, amount)` | Moves funds between two accounts           |

---

## Python Code Implementation

```python
class BankAccount:
    """
    Class representing a simple bank account system.

    Each account stores:
    - account_holder : name of the account owner
    - balance        : current account balance

    Supported operations:
    - deposit        : add money to the account
    - withdraw       : remove money from the account if funds exist
    - transfer       : send money from one account to another
    """

    def __init__(self, account_holder, initial_balance):
        """
        Constructor method initializes account holder name
        and starting balance for the bank account.

        Parameters
        ----------
        account_holder : str
            Name of the account owner.

        initial_balance : float or int
            Starting balance of the account.
        """

        # Store the account holder's name
        self.account_holder = account_holder

        # Initialize the account balance
        self.balance = initial_balance


    def deposit(self, amount):
        """
        Add money to the account balance.

        Parameters
        ----------
        amount : float or int
            Amount to be deposited.
        """

        # Increase the balance by deposit amount
        self.balance += amount


    def withdraw(self, amount):
        """
        Withdraw money from the account if sufficient balance exists.

        Parameters
        ----------
        amount : float or int
            Amount to withdraw.
        """

        # Check whether the account has enough balance
        if amount <= self.balance:
            self.balance -= amount
        else:
            raise ValueError("Insufficient balance for withdrawal.")


    def transfer(self, other_account, amount):
        """
        Transfer money from the current account to another account.

        Parameters
        ----------
        other_account : BankAccount
            The recipient bank account object.

        amount : float or int
            Amount to transfer.
        """

        # Ensure sufficient funds before transferring
        if amount <= self.balance:

            # Deduct from sender account
            self.balance -= amount

            # Add the same amount to receiver account
            other_account.balance += amount

        else:
            raise ValueError("Insufficient balance for transfer.")
```

---