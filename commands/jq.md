# jq quick guide

`jq` parses, filters, and reshapes JSON in the terminal.

## Pretty-print JSON

```bash
cat file.json | jq .
```

```bash
curl -s https://example.com/api | jq .
```

## Read fields

```bash
jq -r '.name' file.json
```

```bash
jq -r '.items[].name' file.json
```

Use `-r` for raw text output without JSON quotes.

## Filter arrays

```bash
jq '.items[] | select(.status == "failed")' file.json
```

```bash
jq -r '.items[] | select(.state != "DONE") | .name' file.json
```

## Output a table-like TSV

```bash
jq -r '.items[] | [.id, .name, .status] | @tsv' file.json
```

## Handle optional fields

```bash
jq -r '.items[] | [.name, (.owner // "unknown")] | @tsv' file.json
```

## Parse JSON logs

For one JSON object per line:

```bash
kubectl logs deploy/my-service | jq -r '[.timestamp, .level, .message] | @tsv'
```

For mixed plain text and JSON logs:

```bash
kubectl logs deploy/my-service | jq -R 'fromjson? // {message: .}'
```
