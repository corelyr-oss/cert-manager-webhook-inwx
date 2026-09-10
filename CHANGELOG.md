# Changelog

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
