Creating and recovering backups is crucial for safeguarding your data against potential loss, and it's a smart move to have a plan in place. In Solr, there are two main methods for backing up and restoring your data, depending on how you're running Solr.

If you're using SolrCloud, you'll utilize the Collections API for managing backups and recoveries. On the other hand, if you're running Solr in standalone mode, you'll rely on the replication handler.

When it comes to backups (and snapshots), they capture data that has been hard committed. If you're making changes with `softCommit=true`, these changes might be visible in search results but won't necessarily be included in backups. Similarly, if you commit changes with `openSearcher=false`, they'll be committed to disk and included in backups, even if they're not currently visible in search results.
