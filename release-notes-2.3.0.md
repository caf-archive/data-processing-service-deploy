#### Version Number
${version-number}

#### New Features
- [CAF-3113](https://jira.autonomy.com/browse/CAF-3113): Reduced the size of the context passed from Workflow Worker to a Composite Document Worker.
  With the Composite Document Handler passing all the fields of the document to the external worker it is unnecessary to duplicate those fields in the context of the sent task message. Now when a message is sent from the Workflow Worker to a Composite Document Worker the context only contains Workflow Worker Task Data not found on the Document sent to the external worker. When the message returns to the Workflow Worker the Policy Worker Task Document is reconstructed by reading from both the context and the Document Worker Document returned.
  
- [CAF-3298](https://jira.autonomy.com/browse/CAF-3298): Apply matched classifications to relevant sub-documents.
  Consuming latest workflow worker with updated classification converter. Classifications that matched due to a sub-document will now have the classification fields added to the relevant sub-document as well as the root document. New fields are added to the document, `CAF_CLASSIFICATION_ID` and `CAF_CLASSIFICATION_NAME`, which will have the same values as the existing `POLICY_MATCHED_POLICYID` and `POLICY_MATCHED_POLICYNAME` fields. The intention is to make the meaning of these field values clearer in the context of data processing. `POLICY_MATCHED_COLLECTION`, `POLICY_MATCHED_POLICYID` and `POLICY_MATCHED_POLICYNAME` are considered deprecated and liable for removal in a future release. The `POLICY_MATCHED_CONDITION_` fields are no longer added to documents.
  
- [CAF-3320](https://jira.autonomy.com/browse/CAF-3320): Add Variant Hash to documents and normalize the case of `BINARY_HASH_SHA1` fields used in `COMPARISON_HASH` field generation.
  All documents processed by the Family Hashing worker will now receive the field `VARIANT_HASH`. The default configuration for this field is to build it from a hash of the `BINARY_HASH_SHA1` and `reference` fields. A new normalization has also been added that can be used to normalize the case of fields that are used in generating output fields. The default `COMPARISON_HASH` and `VARIANT_HASH` hash configurations are using this normalization on the `BINARY_HASH_SHA1` field.

#### Known Issues
