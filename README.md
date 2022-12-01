# meta-renovateConfig-sw

Configuration for [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/) to suit the repositories in the `balena-io-hardware` org. Renovate scans for project dependencies and automatically creates PRs if a dependency is updated.

Instead of the standard installation as a GitHub App, we opt to use this repo to create a central configuration, and run it as a GitHub Action on a regular schedule using the Flowzone token. Thus the key files in this repo are `.github/renovate.json` for the Renovate configuration and `.github/workflows/renovate.yml` for the GitHub Action configuration.

This repo is a copy of the original at [balena-io/renovate-config](https://github.com/balena-io/renovate-config), specifically for this org.

## Debugging

### Test runs

Performing a dry run to see what would happen without actually doing it is a little tricky, and the available documentation is not enlightening. Thus, for posterity, here's one way to do it with renovate 34.40.2:

1. Install the cli with `npm install -g renovate`.
2. Copy the `renovate.json` file to a local folder, but *rename* it to `config.js` because otherwise it would be too easy.
3. Run the cli,
    1. specifying log level and dry run as **environment variables** because the cli options don't work;
    1. providing your GitHub personal access token to the `--token` flag (since we don't include one in the config);
    1. and specifying a repository as `org/repo`, because its mandatory and the ones in the config file are ignored.

So something like this:

```
$ npm install -g renovate
$ curl -o config.js https://raw.githubusercontent.com/balena-io-hardware/meta-renovateConfig-sw/master/.github/renovate.json
$ LOG_LEVEL=debug RENOVATE_DRY_RUN=full renovate --token 'ghp_blahblah' balena-io-hardware/etcherPro-fleet-sw
```

Then, assuming you're interested in what dependencies were found that will be actioned, look for the JSON that appears under "DEBUG: packageFiles with updates", and in particular each of the `depName` fields.

### Historical runs

Details on real runs that have occurred recently can be found in the [Actions](https://github.com/balena-io-hardware/meta-renovateConfig-sw/actions) section of GitHub.
