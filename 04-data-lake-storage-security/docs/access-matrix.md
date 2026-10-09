# Access Matrix

## Planned and Validated Access

| Path | Claims Analysts | HR Analysts |
|---|---|---|
| `/` | Read + Execute | Read + Execute |
| `/claims` | Read + Execute | No access |
| `/human-resources` | No access | Read + Execute |
| `/shared` | Read + Execute | Read + Execute |

## File Access

| File | Claims Analysts | HR Analysts |
|---|---|---|
| `/claims/claims_2026.csv` | Read | No access |
| `/claims/claims_summary.csv` | Read | No access |
| `/human-resources/compensation.csv` | No access | Read |
| `/human-resources/employee_directory.csv` | No access | Read |
| `/shared/department_codes.csv` | Read | Read |

## Notes

The storage-account-level `Reader` role was used only so the lab users could locate and browse to the Azure resource in the portal. It did not provide Blob/Data Lake data access.

The actual data permissions were controlled with Data Lake ACLs.
