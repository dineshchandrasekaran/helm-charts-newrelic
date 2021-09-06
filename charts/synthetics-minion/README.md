# synthetics-minion

## Chart Details

This chart will deploy the New Relic Synthetics Containerized Private Minion as a StatefulSet.

## Configuration

| Parameter                                         | Description                                                                                                                                                                                                                                                                | Default                                                                                                                                                              |
| ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `synthetics.privateLocationKey`                   | *Required* - The [authentication key](https://docs.newrelic.com/docs/synthetics/synthetic-monitoring/private-locations/install-containerized-private-minions-cpms#private-location-key) associated with your Synthetics Private Location                                   |                                                                                                                                                                      |
| `replicaCount`                                    | Number of replicas to maintain with your StatefulSet installation                                                                                                                                                                                                          | `1`                                                                                                                                                                  |
| `imagePullSecrets`                                | The name of a Secret object used to pull an image from a specified container registry                                                                                                                                                                                      |                                                                                                                                                                      |
| `fullnameOverride`                                | Name override used for your StatefulSet installation in place of the default                                                                                                                                                                                               |                                                                                                                                                                      |
| `appVersionOverride`                              | Release version of CPM to use in place of the version specified in [Chart.yml]()                                                                                                                                                                                           |                                                                                                                                                                      |
| `synthetics.minionLogLevel`                       | Modify, to boot the Minion with a specified log level. (`ALL`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `FATAL`, `OFF`, `TRACE`)                                                                                                                                                  | `INFO`                                                                                                                                                               |
| `synthetics.heavyWorkers`                         | The max number of concurrent "heavy" (non-Ping) Synthetics checks to run in standalone Jobs.                                                                                                                                                                               | `2`                                                                                                                                                                  |
| `synthetics.lightweightWorkers`                   | The max number of concurrent "lightweight" (Ping) Synthetics checks to run in threads on the Minion Pod                                                                                                                                                                    | `50`                                                                                                                                                                 |
| `synthetics.minionApiEndpoint`                    | Ensure your CPM can connect to the appropriate endpoint in order to serve your monitor.                                                                                                                                                                                    | For US-based accounts, the endpoint is: `https://synthetics-horde.nr-data.net`. For EU-based accounts, the endpoint is: `https://synthetics-horde.eu01.nr-data.net/` |
| `synthetics.minionDockerRunnerRegistryEndpoint`   | The Docker Registry where the Minion Runner image is hosted. Example: `docker.io`                                                                                                                                                                                          | `quay.io`                                                                                                                                                            |
| `synthetics.minionVsePassphrase`                  | If set, enables verified script execution and uses this value as a passphrase.                                                                                                                                                                                             |                                                                                                                                                                      |
| `synthetics.minionApiProxy`                       | Proxy used to allow the Minion to communicate with New Relic to fetch and reports Synthetics checks. Example format: "host:port"                                                                                                                                           |                                                                                                                                                                      |
| `synthetics.minionApiProxySelfSignedCert`         | If `synthetics.minionApiProxy` is present and uses a self signed cert, enable this value. Acceptable values: true, 1, or yes (any case).                                                                                                                                   |                                                                                                                                                                      |
| `synthetics.minionProxyAuth`                      | If any authentication is needed to communicate with the proxy provide credentials in the format: `"username:password"` - Support HTTP Basic Auth + additional authentication protocols supported by Chrome.                                                                |                                                                                                                                                                      |
| `synthetics.minionCheckTimeout`                   | The maximum amount of seconds that your monitor checks are allowed to run. This value must be an integer between `0 seconds (excluded)` and `900 seconds (included)` (for example, from 1 second to 15 minutes).                                                           | `65` seconds for ping monitors, `180` seconds for the other monitor types.                                                                                           |
| `synthetics.minionUserDefinedEnvVariable`         | A locally hosted set of user defined key value pairs. See [example](https://docs.newrelic.com/docs/synthetics/synthetic-monitoring/private-locations/containerized-private-minion-cpm-configuration#vars-scripted-monitors)                                                |                                                                                                                                                                      |
| `synthetics.minionJVMOpts`                        | Passes command line options to the internal JVM.                                                                                                                                                                                                                           | `-server -XX:-UsePerfData`                                                                                                                                           |
| `synthetics.minionNetworkHealthCheckDisabled`     | The Minion Network Healthcheck disabled state, to manage the CPM check for public internet access. Set as 'true' to bypass this health check.                                                                                                                              | `false`                                                                                                                                                              |
| `image.repository`                                | The container to pull.                                                                                                                                                                                                                                                     | `quay.io/newrelic/synthetics-minion`                                                                                                                                 |
| `image.pullPolicy`                                | The pull policy.                                                                                                                                                                                                                                                           | `IfNotPresent`                                                                                                                                                       |
| `persistence.claimName`                           | The name of the PVC to use. If undefined or not set Statefulset will dynamically create a PVC for each replica                                                                                                                                                             |                                                                                                                                                                      |
| `persistence.storageClass`                        | Override the StorageClass for VolumeClaimTemplates (relevant only if claimName is undefined or empty). For more details see https://kubernetes.io/docs/concepts/storage/persistent-volumes/#class-1                                                                        | See Resources below                                                                                                                                                  |
| `persistence.accessMode`                          | Access mode to be defined for the PVC (relevant only if claimName is undefined or empty).                                                                                                                                                                                  | `ReadWriteOnce`                                                                                                                                                      |
| `persistence.annotations`                         | Annotations to add to the PVC (relevant only if claimName is undefined or empty).                                                                                                                                                                                          |                                                                                                                                                                      |
| `persistence.size`                                | Size to be defined for the PVC (relevant only if claimName is undefined or empty.)                                                                                                                                                                                         | `10Gi`                                                                                                                                                               |
| `persistence.permanentData`                       | Path on the volume to the permanent data storage directory. See Permanent data storage [documentation](https://docs.newrelic.com/docs/synthetics/synthetic-monitoring/private-locations/containerized-private-minion-cpm-configuration#permanent-data-volume) for more.    |                                                                                                                                                                      |
| `persistence.customModules`                       | Path on the volume to the custom modules directory. See Custom Modules [documentation](https://docs.newrelic.com/docs/synthetics/synthetic-monitoring/private-locations/containerized-private-minion-cpm-configuration#custom-modules) for more.                           |                                                                                                                                                                      |
| `persistence.userDefinedVariables`                | Path on the volume to the `user-defined-variable.json` file. See User Defined Variables [documentation](https://docs.newrelic.com/docs/synthetics/synthetic-monitoring/private-locations/containerized-private-minion-cpm-configuration#vars-scripted-monitors) for more.  |                                                                                                                                                                      |
| `serviveAccount.create`                           | If true, a service account would be created and assigned to the deployment                                                                                                                                                                                                 | false                                                                                                                                                                |
| `serviveAccount.name`                             | The service account to assign to the deployment. If `serviveAccount.create` is true then this name will be used when creating the service account                                                                                                                          |                                                                                                                                                                      |
| `serviveAccount.annotations`                      | The annotations to add to the service account if `serviveAccount.create` is set to true.                                                                                                                                                                                   |                                                                                                                                                                      |
| `removeJobsLog`                                   | Default Kubernetes does not include a jobs/log resource. Set this to `true` to remove it from the role if needed                                                                                                                                                           | `false`                                                                                                                                                              |
| `headlessService.serviceName`                     | The name of the headless service to associate to the StatefulSet. If not set a name is generated using the fullname template.                                                                                                                                              |                                                                                                                                                                      |
| `podSecurityContextRunAsUser`                     | Assigning a default uid to the Minion Pod, this can be changed as needed                                                                                                                                                                                                   | `2379`                                                                                                                                                               |
| `appArmorProfileName`                             | Name of an AppArmor profile to load.                                                                                                                                                                                                                                       |                                                                                                                                                                      |

## Example

Make sure you have [added the New Relic chart repository.](../../README.md#installing-charts)

Then, to install this chart, run the following command:

```sh
helm install newrelic/synthetics-minion \
--set synthetics.privateLocationKey=<enter_synthetics_private_location_key> \
```

## Resources

The default set of resources assigned to the pods is shown below:

```yaml
resources:
  requests:
    cpu: 0.5
    memory: 800Mi
  limits:
    cpu: 0.75
    memory: 1.6Gi
```

## Labels

The object used to add custom labels to the Minion Pods is shown below:

```yaml
labels: {}
```

## Annotations

The object used to add custom annotations to the Minion Pods is shown below:

```yaml
annotations: {}
```

## Tolerations

The default set of relations assigned to our StatefulSet is shown below:

```yaml
tolerations: []
```

## NodeSelector

The object used to choose which Node to launch the Minion Pod on

```yaml
nodeSelector: {}
```

## Affinity

The affinity used to select which Nodes Pods are scheduled on

```yaml
affinity: {}
```