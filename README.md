# dns-record-linter

Validate a small DNS inventory for address family and TTL consistency.

## Run

Python 3.8 or later; no third-party packages or cloud credentials are required.

```sh
python3 tool.py examples/input.json
cat examples/input.json | python3 tool.py -
python3 -m unittest -v
```

## Example

Input:

```json
{
  "records": [
    {
      "name": "api.example.test",
      "type": "A",
      "value": "::1",
      "ttl": 60
    }
  ]
}
```

Result:

```json
{
  "issues": [
    {
      "name": "api.example.test",
      "issue": "invalid-address-family"
    }
  ],
  "valid": false
}
```

The command writes JSON to standard output. Exit 0 means evaluation completed;
a finding in the output may still require attention. Invalid input returns exit 2,
with an error on standard error and no result on standard output.

## Operational scope

Does not resolve DNS or validate CNAME coexistence, DNSSEC, TXT data, or authority.

The tool evaluates supplied data locally. It does not change hosts, deploy
resources, or establish production compliance. Example identifiers and observations
are test fixtures. Keep credentials and private inventory out of Git.

## Design and verification

See [NOTES.md](NOTES.md) for the input contract, verification, and integration notes.
Related utilities: [tcp-retry-budget](https://github.com/sal-s-root/tcp-retry-budget), [cidr-overlap-checker](https://github.com/sal-s-root/cidr-overlap-checker).
