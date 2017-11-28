# very-pessimistic-concurrency-control

## Background

Database systems typically need to implement a concurrency protocol to ensure that parallel transactions cannot interfere with each other. Two classes of concurrency protocols have achieved widespread use:

* With *Optimistic concurrency control* (OCC), each transaction applies changes to a local copy of the affected data, and  checks for conflicts with other transactions immediately before committing. If a given transaction finds that it conflicts with any committed transactions, the transaction aborts. This type of protocol is called "optimistic" because each transaction is allowed to proceed until a conflict is detected. OCC yields high performance when conflicts are rare, but can cause a large number of transaction restarts in applications that have many conflicts.
* With *Pessimistic concurrency control* (PCC), each transaction must acquire locks on objects in the database before reading or writing them. This type of protocol is called "pessimistic" because the database system assumes that any given transaction will result in a conflict, so requires locks to be used even when a given transaction would not conflict with any other transactions. In applications where conflicts are common, this often yields better performance than optimistic concurrency control. However, in applications where conflicts are rare, acquiring locks can add significant overhead for each transaction. Additionally, the use of locks can lead to deadlock, which must be detected by the implementation so that a transaction can be aborted.

## Very pessimistic concurrency control

This repository contains a new type of concurrency protocol: *Very pessimistic concurrency control* (VPCC). Under this protocol, the database system assumes that that any given transaction will result in a deadlock. To avoid deadlock, every transaction is immediately aborted as soon as it starts.

This approach has several advantages:

* VPCC has significantly better performance than either OCC or PCC.
* Deadlock cannot occur with VPCC. Transaction retries are also rendered unnecessary.
* VPCC is much more easily parallelizable than either OCC or PCC.
* VPCC is much simpler to implement than OCC and PCC, reducing the liklihood of implementation bugs leading to inconsistent behavior.

A reference implementation of VPCC can be found in [`reference-implementation.py`](./reference-implementation.py).
