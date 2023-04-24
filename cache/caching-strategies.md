# Caching strategies

- cache-aside
    + Look for entry in cache, resulting in a cache miss
    + Load entry from the database
    + Add entry to cache
    + Return entry
- Write-through
    + Application adds/updates entry in cache
    + Cache synchronously writes entry to data store
    + Return
- Write-behind (write-back)
    + Add/update entry in cache
    + Asynchronously write entry to the data store, improving write performance
- Refresh-ahead
