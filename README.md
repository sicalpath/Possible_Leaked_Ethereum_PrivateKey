# Find leaked privatekey via Bigquery
Since Arbitrum airdrop is approaching, [hackers](https://twitter.com/ArkhamIntel/status/1637759733737177091) are geared up.
Inspired by [LiveOverflow](https://www.youtube.com/watch?v=Xml4Gx3huag), I extracted some shit(maybe not pk, just bunch of hexes) from github repos. Try other DBs please.

## Tool
[Google Bigquery](https://console.cloud.google.com/bigquery)


## Query
``` sql
SELECT
f.repo_name,
f.path,
DISTINCT(c.pkey)
FROM
`bigquery-public-data.github_repos.files` f
JOIN (
SELECT
id,
REGEXP_EXTRACT(content, r'(?:^|[^a-zA-Z0-9])(0x[a-fA-F0-9]{64})(?:$|[^a-zA-Z0-9])') AS pkey
FROM
`bigquery-public-data.github_repos.contents`
WHERE
REGEXP_CONTAINS(content, r'(?:^|[^a-zA-Z0-9])(0x[a-fA-F0-9]{64})(?:$|[^a-zA-Z0-9])') ) c
ON
f.id = c.id
```

## For your convenience
[Result](bquxjob.csv)