# new TransactionsScope() harmful when working against SQL server

TransactionScope’s default constructor defaults the isolation level to Serializable and the timeout to 1 minute. The timeout in longer than the default timeout of a SqlCommand, so the transaction will timeout unexpected after 30 sec anyways. serializable issolation level is very deadlock prone and not realy usefull in many cases.

Use this extension instead of the default constructor when creating a transaction scope and using SQL server.

```csharp
public class TransactionUtils 
{
  public static TransactionScope CreateTransactionScope()
  {
    var transactionOptions = new TransactionOptions();
    transactionOptions.IsolationLevel = IsolationLevel.ReadCommitted;
    transactionOptions.Timeout = TransactionManager.MaximumTimeout;
    return new TransactionScope(TransactionScopeOption.Required, transactionOptions);
  }
}

```

