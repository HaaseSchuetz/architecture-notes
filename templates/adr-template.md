# ADR-000X: [Title]

## Status

- ✅ **Accepted**
- ❌ **Rejected**
- 🔄 **Proposed**
- 🗑️ **Deprecated**

**Date:** YYYY-MM-DD
**Author:** [Your Name]
**Stakeholders:** [List of stakeholders]

---

## Context

[Provide the background and context for this decision. What problem are we trying to solve? What are the constraints?]

Example:
> We need to choose a storage format for our data lake that supports ACID transactions, time travel, and schema enforcement. Our current Parquet-based approach lacks these features, leading to data quality issues and difficulty in managing schema changes.

---

## Decision

[State the decision clearly. What are we choosing to do?]

Example:
> **Use Delta Lake** as our primary storage format for the data lake.

---

## Alternatives Considered

[List the alternatives that were considered. Include a comparison table if helpful.]

| **Option**       | **Pros**                          | **Cons**                          | **Verdict**       |
|------------------|-----------------------------------|-----------------------------------|-------------------|
| Delta Lake       | ACID, time travel, schema enforce | Databricks dependency              | ✅ **Chosen**      |
| Parquet          | Universal, fast reads             | No ACID, no updates               | ❌ Rejected        |
| Iceberg          | Open standard, multi-engine       | Less mature in our stack          | ⚠️ Future option  |
| Hudi             | Incremental processing            | Complex setup                     | ❌ Rejected        |

---

## Consequences

### ✅ Positive Consequences

[What are the benefits of this decision?]

Example:
- **ACID Transactions**: No more dirty data from concurrent writes
- **Time Travel**: Ability to recover from mistakes with `VERSION AS OF`
- **Schema Enforcement**: Prevents breaking changes from upstream systems
- **Performance**: Built-in optimization (Z-ordering, compaction) for faster queries

### ❌ Negative Consequences

[What are the drawbacks or risks?]

Example:
- **Vendor Lock-in**: Tight coupling with Databricks ecosystem
- **Learning Curve**: Team needs to learn Delta-specific features (e.g., `MERGE INTO`)
- **Storage Overhead**: Additional metadata storage requirements

---

## Implementation

[How will this decision be implemented? Include code examples if applicable.]

Example:
```python
# Write to Delta
df.write.format("delta").save("/mnt/data/table")

# Read with time travel
spark.read.format("delta").option("versionAsOf", 0).load("/mnt/data/table")

# Schema enforcement
spark.conf.set("spark.databricks.delta.schema.autoMerge.enabled", "true")
```

---

## Related ADRs

[List any related ADRs that this decision depends on or conflicts with.]

Example:
- ADR-0001
- ADR-0002

---

## References

[List any external references, documentation, or resources.]

Example:
- [Delta Lake Documentation](https://delta.io/)
- [Delta Lake vs. Iceberg vs. Hudi](https://lakefs.io/blog/delta-lake-iceberg-hudi-comparison/)
- [Databricks Delta Lake Guide](https://docs.databricks.com/delta/index.html)

---

## Revision History

| **Date**       | **Author**       | **Change**                          |
|----------------|------------------|-------------------------------------|
| YYYY-MM-DD     | [Your Name]     | Initial version                     |

---

## Notes

[Any additional notes, considerations, or open questions.]
