---
share: true
---
We will make blue print using box diagram with key components like 
- Clients(web/mobile)
- APIs
- Web Server 
- Database, Cache
- CDN
- message Queue
Now from interviews question we get that their is 2 flows 
1. Feed Publish : publish post -> DB/cache -> Friends news feed 
   ![[Pasted image 20260710164224.png|Pasted image 20260710164224.png]]
2. News feed Update : Aggregate friends posts in Reverse chronological order 
   ![[Pasted image 20260710164251.png|Pasted image 20260710164251.png]]