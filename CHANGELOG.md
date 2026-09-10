# Changelog

## v0.8.0

- **Breaking for existing installs.** The API group is renamed from
  `cert-manager-webhook-inwx.git.cluster.tf` to
  `cert-manager-webhook-inwx.corelyr.com`. The old name referred to a Gitea host
  that no longer exists; nothing ever resolved it — an API group is an
  identifier, not an address — but it read like a live dependency.
  Upgrading deletes and recreates the `APIService`, and **every ClusterIssuer
  naming this webhook must change `groupName` in the same rollout**. Between the
  two, a DNS-01 challenge resolves to a group nothing answers for.
- The group name is no longer compiled into the binary. `main.go` reads
  `GROUP_NAME` from the environment, which the chart sets from `.Values.groupName`
  — so the APIService, the RBAC rule and the running webhook cannot disagree.
  The container panics on startup if it is unset, rather than defaulting to
  something plausible and quietly answering for the wrong group.

## v0.7.0

- Moved to GitHub (`corelyr-oss/cert-manager-webhook-inwx`) and published from
  GitHub Actions to **public** GitHub Packages — image
  `ghcr.io/corelyr-oss/cert-manager-webhook-inwx`, chart
  `oci://ghcr.io/corelyr-oss/charts`. The Gitea pipeline and the
  `git.cluster.tf` registry behind it are gone, and the `repository.tf`
  migration they were briefly repointed at never published anything.
- Chart version is declared in `Chart.yaml` instead of being substituted from a
  CI variable; `appVersion` is set at package time to the image built from the
  same commit, so `image.tag` no longer needs to be pinned by consumers.
- `imagePullSecrets` defaults to empty: the packages are public, so no
  consuming namespace needs a registry credential.
- `replicaCount` is declared (was read by the Deployment template but never set).
- Test suite compiles again against cert-manager v1.15.2: `logf.NewContext` takes
  a real logger and context, and `dns.SetBinariesPath` — removed upstream — gives
  way to `KUBEBUILDER_ASSETS`. The suite still needs a live INWX account and is
  not run by CI.

## v0.5.0

- Support for multiple credentialsSecretRefs [#7](https://git.cluster.tf/los/cert-manager-webhook-inwx/-/issues/7)

## v0.4.1

- Add CA certificates to Docker image

## v0.4.0

- Add multi arch container images
- Support INWX accounts protected by multi factor authentication
