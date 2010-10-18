!SLIDE 
# Scala saves you LOC
## Without sacrificing maintainability or introducing risk

!SLIDE 
# Problem
## Given a customer, find his active utility account for a given type of energy as of a given date

!SLIDE small
# Java
    @@@ Java
    private Account findAccount(
            Customer customer, Energy energy, Date date) {

      Set<Account> accounts = findAccounts(customer,date);
      return findAccountByType(accounts,energy);
    }

!SLIDE smaller
# <code>findAccounts</code>

    @@@ Java
    private Set<Account> findAccounts(Customer customer, Date date) {
      Set<Account> accounts = newHashSet();
      for (Account account : customer.getAccounts()) {
        if (isDateInWindow(date, 
                    account.getMoveInDate(), 
                    account.getMoveOutDate())) {
          accounts.add(account);
        }
      }
      return accounts;
    }

!SLIDE smaller
# <code>findAccountByType</code>

    @@@ Java
    private Account findAccountByType(
            Set<Account> accounts, Energy energy) {
      for (Account account : accounts) {
        if (account.getServicePoint().getType() == energy) {
          return account;
        }
      }
      return null;
    }

!SLIDE bullets incremental
# What's the problem?
* Not clear why we are iterating these collections
* In one case we are filtering a list, and the other we are searching it

!SLIDE
# More Importantly:
## The business rules are buried admist collection searches

!SLIDE smaller

<pre>
private Set&lt;Account&gt; findAccounts(Customer customer, Date date) {
  Set&lt;Account&gt; accounts = newHashSet();
  for (Account account : customer.getAccounts()) {
    <b>if (isDateInWindow(date,
                account.getMoveInDate(), 
                account.getMoveOutDate())) {</b>
      accounts.add(account);
    }
  }
  return accounts;
}

private Account findAccountByType(
        Set&lt;Account&gt; accounts, Energy energy) {
  for (Account account : accounts) {
    <b>if (account.getServicePoint().getType() == energy) {</b>
      return account;
    }
  }
  return null;
}
</pre>

!SLIDE smaller
    @@@ Java
    private Set<Account> findAccounts(Customer customer, Date date) {
      Set<Account> accounts = newHashSet();
      for (Account account : customer.getAccounts()) {
        if (isDateInWindow(date,
                    account.getMoveInDate(), 
                    account.getMoveOutDate())) {
          accounts.add(account);
        }
      }
      return accounts;
    }

    private Account findAccountByType(
            Set<Account> accounts, Energy energy) {
      for (Account account : accounts) {
        if (account.getServicePoint().getType() == energy) {
          return account;
        }
      }
      return null;
    }

!SLIDE bullets incremental
# 24 LOC; only 4 are related to business logic
* How quickly can you find them 6 months from now?

!SLIDE smaller
# Scala 

    @@@ Scala
    def findAccount(customer:Customer, energy:Energy, date:Date) = {
      val accounts = customer.getAccounts filter { (account) => 
        isDateInWindow(date,
                account.getMoveInDate,
                account.getMoveInDate)
      }
      accounts find { (account) => 
        account.getServicePoint.getType == energy
      } getOrElse null
    }

!SLIDE smaller
# 10 LOC; same 4 are relevant
<pre>
def findAccount(customer:Customer, energy:Energy, date:Date) = {
  val accounts = customer.getAccounts filter { (account) => 
    <b>isDateInWindow(date,
            account.getMoveInDate,
            account.getMoveInDate)</b>
  }
  accounts find { (account) => 
    <b>account.getServicePoint.getType == energy</b>
  } getOrElse null
}
</pre>

!SLIDE bullets incremental
# This is 100% statically typed
* <code>accounts</code> is still a collection of <code>Account</code>
* <code>account</code> is still an <code>Account</code>


!SLIDE bullets incremental
# Maintainability?
* Easier to write
* Easier to understand
* Easier to change
* Therefore more maintainable


