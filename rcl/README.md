# RCL (Recall Language)

This directory will contain the RCL BNF grammar — the query language for the recall algebra.

RCL must express every operation in the algebra (remember, recall, rehearse, reconsolidate, associate, forget, consolidate, inhibit, reinforce, project_tier, introspect, simulate, dream, commit_simulation) and must not require the caller to compose results from multiple foreign query languages.

A future specification should include an `EXPLAIN RECALL` statement that returns the per-signal score breakdown for every trace in the result set, analogous to `EXPLAIN` in SQL.

Syntax is deferred to a companion specification.
