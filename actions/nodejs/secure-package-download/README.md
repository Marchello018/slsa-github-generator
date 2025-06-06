# secure-package-download

the `actions/nodejs/secure-package-download` action provides a way to
download the Node.js package tarball generated by the [Node.js
builder](../../../internal/builders/nodejs/README.md). The package can then
be used to publish the package or upload to a secondary storage.

## Example

```yaml
jobs:
  build:
    permissions:
      id-token: write
      contents: read
      actions: read
    if: startsWith(github.ref, 'refs/tags/')
    uses: slsa-framework/slsa-github-generator/.github/workflows/builder_nodejs_slsa3.yml@v2.1.0
    with:
      run-scripts: "ci, build"

  download:
    needs: [build]
    runs-on: ubuntu-latest
    steps:
      - name: Download tarball
        uses: slsa-framework/slsa-github-generator/actions/nodejs/secure-package-download@v2.1.0
        with:
          name: ${{ needs.build.outputs.package-download-name }}
          path: ${{ needs.build.outputs.package-name }}
          sha256: ${{ needs.build.outputs.package-download-sha256 }}
```

This will download the package tarball to `<GITHUB_WORKSPACE>/<tarball file name>`.

See [Custom Publishing](../../../internal/builders/nodejs/README.md#custom-publishing) for
a full example of publishing using a custom tool.

## Inputs

| Name     | Required | Default | Description                                                                                                          |
| -------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| `name`   | yes      |         | The GitHub Actions workflow run artifact name. Note that this is a name given to an upload, not the path or filename |
| `path`   | no       | "."     | The path to download the tarball into. Must be under the `GITHUB_WORKSPACE`                                          |
| `sha256` | yes      |         | The SHA256 of the artifact for verification                                                                          |

## Outputs

There are no outputs.
