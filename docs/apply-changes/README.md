&larr; [back to Commands](../README.md)

# `om apply-changes`

The `apply-changes` command will kick-off an installation on the Ops Manager VM.
It will then track the installation progress, printing logs as they become available.

## Command Usage
```
ॐ  apply-changes
This authenticated command kicks off an install of any staged changes on the Ops Manager.

Usage: om [options] apply-changes [<args>]
  --client-id, -c, OM_CLIENT_ID                          string  Client ID for the Ops Manager VM (not required for unauthenticated commands)
  --client-secret, -s, OM_CLIENT_SECRET                  string  Client Secret for the Ops Manager VM (not required for unauthenticated commands)
  --connect-timeout, -o, OM_CONNECT_TIMEOUT              int     timeout in seconds to make TCP connections (default: 10)
  --decryption-passphrase, -d, OM_DECRYPTION_PASSPHRASE  string  Passphrase to decrypt the installation if the Ops Manager VM has been rebooted (optional for most commands)
  --env, -e                                              string  env file with login credentials
  --help, -h                                             bool    prints this usage information (default: false)
  --password, -p, OM_PASSWORD                            string  admin password for the Ops Manager VM (not required for unauthenticated commands)
  --request-timeout, -r, OM_REQUEST_TIMEOUT              int     timeout in seconds for HTTP requests to Ops Manager (default: 1800)
  --skip-ssl-validation, -k, OM_SKIP_SSL_VALIDATION      bool    skip ssl certificate validation during http requests (default: false)
  --target, -t, OM_TARGET                                string  location of the Ops Manager VM
  --trace, -tr, OM_TRACE                                 bool    prints HTTP requests and response payloads
  --username, -u, OM_USERNAME                            string  admin username for the Ops Manager VM (not required for unauthenticated commands)
  --version, -v                                          bool    prints the om release version (default: false)

Command Arguments:
  --config, -c                     string             path to yml file containing errand configuration (see docs/apply-changes/README.md for format)
  --ignore-warnings, -i            bool               ignore issues reported by Ops Manager when applying changes
  --product-name, -n               string (variadic)  name of the product(s) to deploy, cannot be used in conjunction with --skip-deploy-products (OM 2.2+)
  --skip-deploy-products, -sdp     bool               skip deploying products when applying changes - just update the director
  --skip-unchanged-products, -sup  bool               skip deploying unchanged products - just run changed or new products --skip-unchanged-products (OM 2.2+)
```

### Configuring via YAML config file

The preferred approach is to include all configuration in a single YAML
configuration file.

#### Example YAML

You can turn on or off errands for products you are deploying (or set it to `default`):

```yaml
errands:
  sample-product:
    run_post_deploy:
      smoke_tests: default
      push-usage-service: false
    run_pre_delete:
      smoke_tests: true
  test-product:
    run_post_deploy:
      smoke_tests: default
```

To retrieve the default configuration of your product's errands you can use the `om
staged-config` command (although the returned shape is different).