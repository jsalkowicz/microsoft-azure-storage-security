# Access Test Results

All six access tests produced the expected result.

| Test | Expected | Actual | Status |
|---|---|---|---|
| Claims Lab User opens `/claims` | Allowed | Allowed | Passed |
| Claims Lab User opens `/shared` | Allowed | Allowed | Passed |
| Claims Lab User opens `/human-resources` | Denied | Denied | Passed |
| HR Lab User opens `/human-resources` | Allowed | Allowed | Passed |
| HR Lab User opens `/shared` | Allowed | Allowed | Passed |
| HR Lab User opens `/claims` | Denied | Denied | Passed |

## Validation Notes

The allowed paths displayed the expected synthetic CSV files.

The denied paths returned an Azure authorization error while the user was authenticated with Microsoft Entra ID.

The tests were run with separate lab users rather than the setup/admin account so that the broad permissions used to build the environment would not affect the results.
