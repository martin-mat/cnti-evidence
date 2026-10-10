# CNTi evidence

This repository holds the evidence behind the CNTi badge of network functions whose source is
private. A badge alone is a number; what stands behind it is here: the full results with every
finding and its remediation, the configuration the run used, and a signed statement of which
pipeline produced them.

## acme-cnf

![CNTi essential](https://github.com/martin-mat/cnti-evidence/raw/main/cnti/acme-cnf/latest/cnti-badge.svg)

Published by the private repository `martin-mat/acme-cnf` on every change to its main branch,
with the [CNTi Test Suite GitHub Action](https://github.com/lfn-cnti/testsuite-action).
Each run lands in [`cnti/acme-cnf/<run date>/`](cnti/acme-cnf): the badge, the results file and
its JUnit report, `cnti-evidence.json` (an in-toto Test Result statement), and the configuration
the run used.

## How to verify, without access to the source

The chart is published to `ghcr.io/martin-mat/acme/coredns`, and the signed statement is
attached to it in the registry. Anyone with `cosign` can check that the evidence was produced by
that repository's pipeline, and read the score from the signed statement:

```
cosign verify-attestation --type https://in-toto.io/attestation/test-result/v0.1 \
  --certificate-identity-regexp '^https://github.com/martin-mat/acme-cnf/' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  ghcr.io/martin-mat/acme/coredns:1.48.3 | jq -r .payload | base64 -d | jq .predicate
```

The statement in each run's directory says the same under `predicate.cnti`, and its
`provenance` block names the method and the identity used.
