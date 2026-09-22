# Screenshot evidence

These are **edited copies of screenshots from the actual training session**. I cropped browser address bars containing subscription identifiers and covered the Azure header that displayed the signed-in account's name and tenant. I left the commands, result messages, blob names, timestamps, and lab resource names visible. No access-key values, passwords, connection strings, or SAS tokens are shown.

| Image | What I used it to verify |
|---|---|
| [02 — Key 1 / HR](02-key1-humanresources-access.png) | Key 1 could list blobs in Human Resources. |
| [03 — Key 2 / Claims](03-key2-claims-access.png) | The alternate key worked before renewal. |
| [04 — Renew Key 1](04-key1-rotation-command.png) | Renewal command completed without error. |
| [05 — Old Key 1 rejected](05-old-key1-authentication-failure.png) | Old value no longer authenticated. |
| [06 — New Key 1 accepted](06-new-key1-claims-access.png) | New value could list Claims blobs. |
| [07 — AccountKey audit rows](07-log-analytics-accountkey-audit.png) | Account-key Blob listing events appeared in Log Analytics. |
| [08 — Shared Key disabled](08-shared-key-disabled.png) | Setting was disabled. |
| [09 — Valid key rejected](09-valid-key-blocked-shared-key-disabled.png) | Current key received `KeyBasedAuthenticationNotPermitted`. |
| [10 — Claims Reader / Claims](10-claimsreader-claims-entra-success.png) | Scoped Entra identity could read the Claims listing. |
| [11 — Claims Reader / HR](11-claimsreader-humanresources-entra-denied.png) | The same identity was denied HR access. |
| [12 — Shared Key restored](12-shared-key-reenabled.png) | Account returned to Enabled for the SAS exercise. |
| [13 — Variables unset](13-shell-key-variables-unset.png) | Key variables were cleared in the admin shell. |

A separate original image of the initial Key 1 Claims listing was not included in the source evidence archive, so I have not invented one. The portal `Loading…` troubleshooting screenshots were omitted: they exposed a full test-user principal name and object ID but did not add proof beyond the later Cloud Shell tests.
