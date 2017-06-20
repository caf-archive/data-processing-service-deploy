#### Version Number
${version-number}

#### New Features

- [CAF-2192](https://jira.autonomy.com/browse/CAF-2192): Extend documentation for error handling in document workers
- [CAF-2762](https://jira.autonomy.com/browse/CAF-2762): New Policy Handler for renaming fields
- [CAF-2795](https://jira.autonomy.com/browse/CAF-2795): Data Processing: Queue specification cleanup / improvement
- [CAF-2906](https://jira.autonomy.com/browse/CAF-2906): Markup Worker: Log pollution from Natty
- [CAF-2928](https://jira.autonomy.com/browse/CAF-2928): Improvements required to the task receiver
- [CAF-2946](https://jira.autonomy.com/browse/CAF-2946): Improve hashing performance
- [CAF-2951](https://jira.autonomy.com/browse/CAF-2951): Policy Worker performance issues
- [CAF-2952](https://jira.autonomy.com/browse/CAF-2952): Data Processing Performance is slow and can hold up subfile
- [CAF-3064](https://jira.autonomy.com/browse/CAF-3064): Create a new CAF Docker Images projects

#### Bug Fixes
- [CAF-3104](https://jira.autonomy.com/browse/CAF-3104): Document worker messages could end up on a random queue
- [CAF-2889](https://jira.autonomy.com/browse/CAF-2889): Elastic index worker does not output errors
- [CAF-2189](https://jira.autonomy.com/browse/CAF-2189): Markup worker does not recognize headers which are surrounded with asterisks
- [CAF-2725](https://jira.autonomy.com/browse/CAF-2725): Markup worker marks up first field if expected field is not present
- [CAF-2770](https://jira.autonomy.com/browse/CAF-2770): Syntax error when starting up OCR through docker-compose
- [CAF-2853](https://jira.autonomy.com/browse/CAF-2853): Data Processing: Refine settings to increase throughput
- [CAF-2942](https://jira.autonomy.com/browse/CAF-2942): CAF_LOG_LEVEL environment variable is not used by workers
- [CAF-3010](https://jira.autonomy.com/browse/CAF-3010): Indexing Worker is removing the BINARY_HASH_SHA1 field from documents
- [CAF-3071](https://jira.autonomy.com/browse/CAF-3071): Comparison Hashes are different for sent and received MSG