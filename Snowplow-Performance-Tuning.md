_Many thanks to [Ryan Doherty](https://github.com/smugryan) for contributing the original draft of this page_

### Redshift

1. Vacuum - Turn on this feature with the --include vacuum argument for storage-loader. Any Postrgresql or Redshift DBA will tell you that you *must* do this regularly (ideally after each load). When you load lots of new data into Redshift it does not insert it into the same files that currently existing data resides in, only after a vacuum is it merged into the normal storage location. More info: http://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html

2. Make sure you query on your sort keys, if you don't you effectively are doing a full table scan, which is non-performant. http://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-sort-key.html

3. Learn about workload management http://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html Especially 'wlm_query_slot_count'. When we increased the wlm_query_slot_count for running vacuum queries (manually) we saw > 10x performance increase. Vacuums that took > 12hrs take <1hr. It's also good to setup groups for Snowplow or other automated tools so you can limit the amount of CPU usage.

4. Determine your types of workloads. We have 2 clusters, one is running on high storage, one high compute. 

5. Make sure to monitor your disk usage, *especially* when you want to upgrade snowplow versions because the upgrade scripts do a complete copy of public.events, so your storage will increase 2x temporarily. 

### Elastic MapReduce

1. You can resize your EMR cluster on the fly, super handy if you've fallen behind and need to catch up. 

### Elastic Beanstalk

1. When your EB collectors are pushing their logs to S3 the latency for the collectors will jump considerably. This can have adverse effects on systems that make synchronous calls to the collectors. We doubled the size of our collector AS group to alleviate this problem.

