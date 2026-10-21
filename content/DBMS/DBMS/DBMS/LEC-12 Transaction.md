---
share: true
---
LEC-12: Transaction

1. Transaction
	1. A unit of work done against the DB in a logical sequence.
	2. Sequence is very important in transaction.
	3. It is a logical unit of work that contains one or more SQL statements. The result of all these statements in a transaction either gets completed successfully (all the changes made to the database are permanent) or if at any point any failure happens it gets rollbacked (all the changes being done are undone.)
2. ==🟠ACID Properties==
	1. ==🟠To ensure integrity of the data, we require that the DB system maintain the following properties of the transaction.==
	2. ==🟠Atomicity==
		1. ==🟠Either all operations of transaction are reflected properly in the DB, or none are.==
	3. ==🟠Consistency==
		1. ==🟠Integrity constraints must be maintained before and after transaction.==
		2. ==🟠DB must be consistent after transaction happens.==
	4. ==🟠Isolation==
		1. ==🟠Even though multiple transactions may execute concurrently, the system guarantees that, for every pair of transactions Ti and Tj, it appears to Ti that either Tj finished execution before Ti started, or Tj started execution after Ti finished. Thus, each transaction is unaware of other transactions executing concurrently in the system.==
		2. ==🟠Multiple transactions can happen in the system in isolation, without interfering each other.==
	5. ==🟠Durability==
		1. ==🟠After transaction completes successfully, the changes it has made to the database persist, even if there are system failures.==
3. Transaction states
	1. ![[Pasted image 20260521103936.png|Pasted image 20260521103936.png]]
	2. Active state
		1. The very first state of the life cycle of the transaction, all the read and write operations are being performed. If they execute without any error the T comes to Partially committed state. Although if any error occurs then it leads to a Failed state.
	3. Partially committed state
		1. After transaction is executed the changes are saved in the buffer in the main memory. If the changes made are permanent on the DB then the state will transfer to the committed state and if there is any failure, the T will go to Failed state.
	4. Committed state
		1. When updates are made permanent on the DB. Then the T is said to be in the committed state. Rollback can’t be done from the committed states. New consistent state is achieved at this stage.
	5. Failed state
		1. When T is being executed and some failure occurs. Due to this it is impossible to continue the execution of the T.
	6. Aborted state
		1. When T reaches the failed state, all the changes made in the buffer are reversed. After that the T rollback completely. T reaches abort state after rollback. DB’s state prior to the T is achieved.
	7. Terminated state
		1. A transaction is said to have terminated if has either committed or aborted.


