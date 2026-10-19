---
share: true
---
1. Master-Slave is a general way to optimise IO in a system where number of requests goes way high that a single DB server is not able to handle it efficiently.
2. Its a Pattern 3 in LEC-19 (Database Scaling Pattern). (Command Query Responsibility Segregation)
3. The true or latest data is kept in the Master DB thus write operations are directed there. Reading ops are done only from slaves. This architecture serves the purpose of safeguarding site reliability, availability, reduce latency etc . If a site receives a lot of traffic and the only available database is one master, it will be overloaded with reading and writing requests. Making the entire system slow for everyone on the site.
4. DB replication will take care of distributing data from Master machine to Slaves machines. This can be synchronous or asynchronous depending upon the system’s need.

![[Pasted image 20260709170236.png|Pasted image 20260709170236.png]]