## Why Confirm Transactions?

IOTA works differently than a traditional ledger. Traditional ledgers are generally managed by a trustworthy third-party, such as a bank or credit card company. With IOTA, there is no third-party, peers reach consensus regarding the trustworthiness of a transaction. When consensus is reached, the transaction is “confirmed”.

#### Traditional Ledger
As shown below, Bob started with $10 in his bank account. He sends three transactions without realizing he has overspent. The first two transactions to be received will be paid. The last transaction will trigger an overdraft. In our example, Alice and Fred receive payment but Sue does not.
It is possible for Bob to, simultaneously, send as many transactions as he wants. But he must deal with overdraft fees and possible legal action if he spends more than he has. Additionally, Bob’s balance is kept in his account at the bank.

![](images/ledger-trad.png?raw=true)

GitHub Logo Account Balances are kept in the account. Transactions may be issued simultaneously

#### IOTA Ledger

In this example, Bob starts with 10i in his seed. He sends a zero value transaction which attaches the address, “FMHZ…” to this seed. Bob wants to send Alice 1i so he contacted her and obtained her deposit address of YCIR…
Bob sends 1i to Alice. This transaction is confirmed. A new address “NAMK…” is generated for Bob containing his remaining 9i. Address “FMHZ…” is archived. It may no longer be used.

![](images/ledger-iota.png?raw=true)

Now, Bob sends another transaction from his new address, “NAMK…” This transaction is also confirmed and a new address, “EHUE…” is generated. However, when Bob attempts to send the final transaction, he receives an insufficient funds message because “EHUE…” has only 1i and Bob attempted to send 3i. By only sending one transaction at a time, Bob cannot over spend.
GitHub Logo Balance kept in deposit address. One transaction at a time.

This bundle shows the details of Bob’s first transaction where he sent 1i to Alice’s address, “YCIR…”. Bob’s original deposit address, “FHMZ…” is archived. It can no longer be used. Bob gets a new deposit address, “NAMK…” with his new balance of 9i. The bundle also shows that “FHMZ…” was attached to the Tangle via a zero balance transaction.
GitHub Logo

This serialization of transactions provides the concurrence and cybersecurity necessary to ensure trustworthy transactions. It also emphasizes the need to manage deposit addresses and monitor transactions.

![](images/ledger-.png?raw=true)

**WARNING**

If you do not wait until a transaction is confirmed, the transaction stays in a “Pending” state. This essentially locks funds because no new transactions can be issued until that transaction is confirmed.
