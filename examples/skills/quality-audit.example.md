# Quality Audit Skill (Sanitized Example)

# Universal audit pattern for any data source: CRM records, knowledge bases,
# database tables, intelligence files, or archives.

## When to Trigger
- Checking data quality or completeness
- Finding gaps in any structured data
- Validating records before reporting
- Auditing any data source on demand

## Workflow

1. **Define scope**: What data source? What quality criteria?
2. **Pull raw counts**: Verify actual numbers vs claimed numbers
3. **Quality tier analysis**:
   - Tier 1: Complete records (all required fields populated)
   - Tier 2: Partial records (key fields present, gaps in secondary)
   - Tier 3: Minimal records (basic info only)
   - Tier 4: Stale/invalid (outdated, duplicate, or broken)
4. **Gap identification**: What's missing that should be there?
5. **Score**: 0-100 based on completeness, freshness, accuracy
6. **Recommendations**: Specific actions to close gaps, prioritized by impact

## Output Format

| Metric | Value |
|--------|-------|
| Total records | {n} |
| Tier 1 (complete) | {n} ({%}) |
| Tier 2 (partial) | {n} ({%}) |
| Tier 3 (minimal) | {n} ({%}) |
| Tier 4 (stale) | {n} ({%}) |
| Quality score | {0-100} |

## Top 3 Gaps
1. {Most impactful gap + fix}
2. {Second gap + fix}
3. {Third gap + fix}
