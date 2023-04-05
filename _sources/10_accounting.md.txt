# Usage Accounting
Cluster resources (CPU/GPU hours) consumed by the jobs are recorded by [MOAB Accounting Manager](http://www.adaptivecomputing.com/products/capabilities/accounting/).  Only the job execution time is taken into account. The time that a user spends preparing the job or the queue waiting time is not taken into account. The cost of each job is automatically calculated according to the agreement about the use of HPC services. You can view the account balance with a command:
 ```
 mam-balance
 ```
Get a detailed report of cluster resources consumed by each job, for example, for January 2023:
```
mam-list-usagerecords -s 2023-01-01 -e 2023-02-01
```
