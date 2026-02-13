like row 1. ### What is database performance tunning and why it is important?
   It is a process of optimizing a database system for faster queries, less resource use and overall efficiency. It's Crucial because slow database lead to poor application performance and user experiance.

2. ### What are the main types of performance tunning?
   Tunning can focused on query optimization, adjusting database software configuration. Optimizing schema and data structure's and ensuring adequate hardware and OS resources.

3. ### What is Query Optimizer? and how does it work?
   it is a key part of DBMS, query optimizer finds most efficient way to run a SQL query. Analyze query use statastic and indexes, estimates cost and picks the best one.

4. ### Explain the role of database statstic in performance tunning?
   Statistic like row count and value distribution are vital for query opimizer to make accurate cost estimates and choose efficient execution plans.

5. ### How do you identify performance bottleneck in SQL queries?
   Tools like 'explain plan' show query execution details highlighting inefficiencies, monitoring tools track performance metric and analyzing wait events helps to identify resource contention.
  
6. ### What are common performance bottlenecks in database?
   - Slow queries
   - Disk I/O problems
   - High CPU usage
   - insufficient memory
   - lock contention
     
8. ### What are clustered and non-clustered index?
   - Clustered index: It determines physical storage order of data (one per table)
   - Non-Clustered index: It provides pointers to data stored elsewhere (multiple per table)

9. ### What are some best practices for optimizing SQL queries?
   - Use appropirate index
   - Select only needed columns
   - Avoid functions on indexed columns in `WHERE` clause
   - Use bind variables
   - Optimize `join` conditions

10. ### What are difference type of database backups and when would you use each?
 - Full: backups entire database
 - Differential: backups changes since the last full backup
 - Transactional log: backups all modifiction since the last log backup
Best practice is to use a combination of weekly full, daily differential and frequent transactional logs backup.

 
   
