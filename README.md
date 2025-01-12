# confluence

![Version: 1.15.0-bb.6](https://img.shields.io/badge/Version-1.15.0--bb.6-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 7.19.12](https://img.shields.io/badge/AppVersion-7.19.12-informational?style=flat-square)

A chart for installing Confluence Data Center on Kubernetes

## Upstream References
* <https://atlassian.github.io/data-center-helm-charts/>

* <https://github.com/atlassian/data-center-helm-charts>
* <https://bitbucket.org/atlassian-docker/docker-atlassian-confluence-server/>

## Learn More
* [Application Overview](docs/overview.md)
* [Other Documentation](docs/)

## Pre-Requisites

* Kubernetes Cluster deployed
* Kubernetes config installed in `~/.kube/config`
* Helm installed

Kubernetes: `>=1.21.x-0`

Install Helm

https://helm.sh/docs/intro/install/

## Deployment

* Clone down the repository
* cd into directory
```bash
helm install confluence chart/
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| replicaCount | int | `1` | The initial number of Confluence pods that should be started at deployment time. Note that Confluence requires manual configuration via the browser post deployment after the first pod is deployed. This configuration must be completed before scaling up additional pods. As such this value should always be kept as 1, but can be altered once manual configuration is complete.  |
| image.repository | string | `"registry1.dso.mil/ironbank/atlassian/confluence-data-center/confluence-node"` |  |
| image.imagePullSecrets | string | `"private-registry"` | Optional image repository pull secret |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.tag | string | `"8.4.0"` | The docker image tag to be used. Defaults to the Chart appVersion. |
| serviceAccount.create | bool | `true` | Set to 'true' if a ServiceAccount should be created, or 'false' if it already exists.  |
| serviceAccount.name | string | `nil` | The name of the ServiceAccount to be used by the pods. If not specified, but the "serviceAccount.create" flag is set to 'true', then the ServiceAccount name will be auto-generated, otherwise the 'default' ServiceAccount will be used. https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#use-the-default-service-account-to-access-the-api-server  |
| serviceAccount.imagePullSecrets | list | `[{"name":"private-registry"}]` | For Docker images hosted in private registries, define the list of image pull secrets that should be utilized by the created ServiceAccount https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod  |
| serviceAccount.annotations | object | `{}` | Annotations to add to the ServiceAccount (if created)  |
| serviceAccount.role.create | bool | `true` | Create a role for Hazelcast client with privileges to get and list pods and endpoints in the namespace. Set to false if you need to create a Role and RoleBinding manually  |
| serviceAccount.clusterRole.create | bool | `false` | Set to 'true' if a ClusterRole should be created, or 'false' if it already exists.  |
| serviceAccount.clusterRole.name | string | `nil` | The name of the ClusterRole to be used. If not specified, but the "serviceAccount.clusterRole.create" flag is set to 'true', then the ClusterRole name will be auto-generated.  |
| serviceAccount.roleBinding | object | `{"create":true}` | Grant permissions defined in Role (list and get pods and endpoints) to a service account.  |
| serviceAccount.clusterRoleBinding.create | bool | `false` | Set to 'true' if a ClusterRoleBinding should be created, or 'false' if it already exists.  |
| serviceAccount.clusterRoleBinding.name | string | `nil` | The name of the ClusterRoleBinding to be created. If not specified, but the "serviceAccount.clusterRoleBinding.create" flag is set to 'true', then the ClusterRoleBinding name will be auto-generated.  |
| serviceAccount.eksIrsa.roleArn | string | `nil` |  |
| database.type | string | `nil` | The database type that should be used. If not specified, then it will need to be provided via the browser during manual configuration post deployment. Valid values include: - 'postgresql' - 'mysql' - 'oracle' - 'mssql' https://atlassian.github.io/data-center-helm-charts/userguide/CONFIGURATION/#databasetype  |
| database.user | string | `nil` |  |
| database.password | string | `"userpassword"` |  |
| database.url | string | `nil` | The jdbc URL of the database. If not specified, then it will need to be provided via the browser during manual configuration post deployment. Example URLs include: - 'jdbc:postgresql://<dbhost>:5432/<dbname>' - 'jdbc:mysql://<dbhost>/<dbname>' - 'jdbc:sqlserver://<dbhost>:1433;databaseName=<dbname>' - 'jdbc:oracle:thin:@<dbhost>:1521:<SID>' https://atlassian.github.io/data-center-helm-charts/userguide/CONFIGURATION/#databaseurl  |
| database.credentials.secretName | string | `nil` | from-literal=password=<password>' https://kubernetes.io/docs/concepts/configuration/secret/#opaque-secrets  |
| database.credentials.usernameSecretKey | string | `"username"` | The key ('username') in the Secret used to store the database login username  |
| database.credentials.passwordSecretKey | string | `"password"` | The key ('password') in the Secret used to store the database login password  |
| ingress.create | bool | `false` | Set to 'true' if an Ingress Resource should be created. This depends on a pre-provisioned Ingress Controller being available.  |
| ingress.className | string | `"nginx"` | The class name used by the ingress controller if it's being used.  Please follow documentation of your ingress controller. If the cluster contains multiple ingress controllers, this setting allows you to control which of them is used for Atlassian application traffic.  |
| ingress.nginx | bool | `true` | Set to 'true' if the Ingress Resource is to use the K8s 'ingress-nginx' controller. https://kubernetes.github.io/ingress-nginx/  This will populate the Ingress Resource with annotations that are specific to the K8s ingress-nginx controller. Set to 'false' if a different controller is to be used, in which case the appropriate annotations for that controller must be specified below under 'ingress.annotations'.  |
| ingress.maxBodySize | string | `"250m"` | The max body size to allow. Requests exceeding this size will result in an HTTP 413 error being returned to the client.  |
| ingress.proxyConnectTimeout | int | `60` | Defines a timeout for establishing a connection with a proxied server. It should be noted that this timeout cannot usually exceed 75 seconds.  |
| ingress.proxyReadTimeout | int | `60` | Defines a timeout for reading a response from the proxied server. The timeout is set only between two successive read operations, not for the transmission of the whole response. If the proxied server does not transmit anything within this time, the connection is closed.  |
| ingress.proxySendTimeout | int | `60` | Sets a timeout for transmitting a request to the proxied server. The timeout is set only between two successive write operations, not for the transmission of the whole request. If the proxied server does not receive anything within this time, the connection is closed.  |
| ingress.host | string | `nil` | The fully-qualified hostname (FQDN) of the Ingress Resource. Traffic coming in on this hostname will be routed by the Ingress Resource to the appropriate backend Service.  |
| ingress.path | string | `nil` | The base path for the Ingress Resource. For example '/confluence'. Based on a 'ingress.host' value of 'company.k8s.com' this would result in a URL of 'company.k8s.com/confluence'. Default value is 'confluence.service.contextPath' |
| ingress.annotations | object | `{}` | The custom annotations that should be applied to the Ingress Resource when NOT using the K8s ingress-nginx controller.  |
| ingress.https | bool | `true` | Set to 'true' if browser communication with the application should be TLS (HTTPS) enforced.  |
| ingress.tlsSecretName | string | `nil` | The name of the K8s Secret that contains the TLS private key and corresponding certificate. When utilised, TLS termination occurs at the ingress point where traffic to the Service, and it's Pods is in plaintext.  Usage is optional and depends on your use case. The Ingress Controller itself can also be configured with a TLS secret for all Ingress Resources. https://kubernetes.io/docs/concepts/configuration/secret/#tls-secrets https://kubernetes.io/docs/concepts/services-networking/ingress/#tls  |
| volumes.localHome.persistentVolumeClaim.create | bool | `false` | If 'true', then a 'PersistentVolume' and 'PersistentVolumeClaim' will be dynamically created for each pod based on the 'StorageClassName' supplied below.  |
| volumes.localHome.persistentVolumeClaim.storageClassName | string | `nil` | Specify the name of the 'StorageClass' that should be used for the local-home volume claim.  |
| volumes.localHome.persistentVolumeClaim.resources | object | `{"requests":{"storage":"1Gi"}}` | Specifies the standard K8s resource requests for the local-home volume claims.  |
| volumes.localHome.customVolume | object | `{}` | Static provisioning of local-home using K8s PVs and PVCs  NOTE: Due to the ephemeral nature of pods this approach to provisioning volumes for pods is not recommended. Dynamic provisioning described above is the prescribed approach.  When 'persistentVolumeClaim.create' is 'false', then this value can be used to define a standard K8s volume that will be used for the local-home volume(s). If not defined, then an 'emptyDir' volume is utilised. Having provisioned a 'PersistentVolume', specify the bound 'persistentVolumeClaim.claimName' for the 'customVolume' object. https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static  |
| volumes.localHome.mountPath | string | `"/var/atlassian/application-data/confluence"` | Specifies the path in the Confluence container to which the local-home volume will be mounted.  |
| volumes.sharedHome.persistentVolumeClaim.create | bool | `false` | If 'true', then a 'PersistentVolumeClaim' and 'PersistentVolume' will be dynamically created for shared-home based on the 'StorageClassName' supplied below.  |
| volumes.sharedHome.persistentVolumeClaim.storageClassName | string | `nil` | Specify the name of the 'StorageClass' that should be used for the 'shared-home' volume claim.  |
| volumes.sharedHome.persistentVolumeClaim.resources | object | `{"requests":{"storage":"1Gi"}}` | Specifies the standard K8s resource requests limits for the shared-home volume claims.  |
| volumes.sharedHome.efs | string | `nil` | If AWS efs is utilized, please make efs true and put id of efs volume to create pv |
| volumes.sharedHome.efsid | string | `nil` |  |
| volumes.sharedHome.driver | string | `"efs.csi.aws.com"` |  |
| volumes.sharedHome.customVolume | object | `{}` | Static provisioning of shared-home using K8s PVs and PVCs  When 'persistentVolumeClaim.create' is 'false', then this value can be used to define a standard K8s volume that will be used for the shared-home volume. If not defined, then an 'emptyDir' volume is utilised. Having provisioned a 'PersistentVolume', specify the bound 'persistentVolumeClaim.claimName' for the 'customVolume' object. https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static https://atlassian.github.io/data-center-helm-charts/examples/storage/aws/SHARED_STORAGE/  |
| volumes.sharedHome.mountPath | string | `"/var/atlassian/confluence-datacenter"` | Specifies the path in the Confluence container to which the shared-home volume will be mounted.  |
| volumes.sharedHome.subPath | string | `nil` | Specifies the sub-directory of the shared-home volume that will be mounted in to the Confluence container.  |
| volumes.sharedHome.nfsPermissionFixer.enabled | bool | `false` | If 'true', this will alter the shared-home volume's root directory so that Confluence can write to it. This is a workaround for a K8s bug affecting NFS volumes: https://github.com/kubernetes/examples/issues/260  |
| volumes.sharedHome.nfsPermissionFixer.mountPath | string | `"/shared-home"` | The path in the K8s initContainer where the shared-home volume will be mounted  |
| volumes.sharedHome.nfsPermissionFixer.imageRepo | string | `"alpine"` | Image repository for the permission fixer init container. Defaults to alpine  |
| volumes.sharedHome.nfsPermissionFixer.imageTag | string | `"latest"` | Image tag for the permission fixer init container. Defaults to latest  |
| volumes.sharedHome.nfsPermissionFixer.command | string | `nil` | By default, the fixer will change the group ownership of the volume's root directory to match the Confluence container's GID (2002), and then ensures the directory is group-writeable. If this is not the desired behaviour, command used can be specified here.  |
| volumes.synchronyHome.persistentVolumeClaim.create | bool | `false` | If 'true', then a 'PersistentVolume' and 'PersistentVolumeClaim' will be dynamically created for each pod based on the 'StorageClassName' supplied below.  |
| volumes.synchronyHome.persistentVolumeClaim.storageClassName | string | `nil` | Specify the name of the 'StorageClass' that should be used for the synchrony-home volume claim.  |
| volumes.synchronyHome.persistentVolumeClaim.resources | object | `{"requests":{"storage":null}}` | Specifies the standard K8s resource requests for the synchrony-home volume claims.  |
| volumes.synchronyHome.customVolume | object | `{}` | Static provisioning of synchrony-home using K8s PVs and PVCs  NOTE: Due to the ephemeral nature of pods this approach to provisioning volumes for pods is not recommended. Dynamic provisioning described above is the prescribed approach.  When 'persistentVolumeClaim.create' is 'false', then this value can be used to define a standard K8s volume that will be used for the synchrony-home volume(s). If not defined, then an 'emptyDir' volume is utilised. Having provisioned a 'PersistentVolume', specify the bound 'persistentVolumeClaim.claimName' for the 'customVolume' object. https://kubernetes.io/docs/concepts/storage/persistent-volumes/#static  |
| volumes.synchronyHome.mountPath | string | `"/var/atlassian/application-data/confluence"` | Specifies the path in the Synchrony container to which the synchrony-home volume will be mounted.  |
| volumes.additional | list | `[{"configMap":{"defaultMode":484,"name":"server-xml-j2"},"name":"server-xml-j2"},{"configMap":{"defaultMode":484,"name":"server-xml"},"name":"server-xml"},{"configMap":{"defaultMode":484,"name":"footer-content-vm"},"name":"footer-content-vm"}]` | Defines additional volumes that should be applied to all Confluence pods. Note that this will not create any corresponding volume mounts; those needs to be defined in confluence.additionalVolumeMounts  |
| volumes.additionalSynchrony | list | `[]` | Defines additional volumes that should be applied to all Synchrony pods. Note that this will not create any corresponding volume mounts; those needs to be defined in synchrony.additionalVolumeMounts  |
| volumes.defaultPermissionsMode | int | `484` | Mode bits used to set permissions on created files by default. Must be an octal value between 0000 and 0777 or a decimal value between 0 and 511 Typically overridden in volumes from Secrets and ConfigMaps to make mounted files executable  |
| confluence.service.port | int | `80` | The port on which the Confluence K8s Service will listen  |
| confluence.service.type | string | `"ClusterIP"` | The type of K8s service to use for Confluence  |
| confluence.service.sessionAffinity | string | `"None"` | Session affinity type. If you want to make sure that connections from a particular client are passed to the same pod each time, set sessionAffinity to ClientIP. See: https://kubernetes.io/docs/reference/networking/virtual-ips/#session-affinity  |
| confluence.service.sessionAffinityConfig | object | `{"clientIP":{"timeoutSeconds":null}}` | Session affinity configuration  |
| confluence.service.sessionAffinityConfig.clientIP.timeoutSeconds | string | `nil` | Specifies the seconds of ClientIP type session sticky time. The value must be > 0 && <= 86400(for 1 day) if ServiceAffinity == "ClientIP". Default value is 10800 (for 3 hours).  |
| confluence.service.loadBalancerIP | string | `nil` | Use specific loadBalancerIP. Only applies to service type LoadBalancer.  |
| confluence.service.contextPath | string | `nil` | The Tomcat context path that Confluence will use. The ATL_TOMCAT_CONTEXTPATH will be set automatically.  |
| confluence.service.annotations | object | `{}` | Additional annotations to apply to the Service  |
| confluence.securityContextEnabled | bool | `true` | Whether to apply security context to pod.  |
| confluence.securityContext.fsGroup | int | `2002` | The GID used by the Confluence docker image GID will default to 2002 if not supplied and securityContextEnabled is set to true. This is intended to ensure that the shared-home volume is group-writeable by the GID used by the Confluence container. However, this doesn't appear to work for NFS volumes due to a K8s bug: https://github.com/kubernetes/examples/issues/260 |
| confluence.securityContext.runAsUser | int | `2002` |  |
| confluence.securityContext.runAsGroup | int | `2002` |  |
| confluence.securityContext.runAsNonRoot | bool | `true` |  |
| confluence.containerSecurityContext | object | `{"runAsGroup":2002,"runAsNonRoot":true,"runAsUser":2002}` | Standard K8s field that holds security configurations that will be applied to a container. https://kubernetes.io/docs/tasks/configure-pod-container/security-context/  |
| confluence.umask | string | `"0022"` | The umask used by the Confluence process when it creates new files. The default is 0022. This gives the new files:  - read/write permissions for the Confluence user  - read permissions for everyone else.  |
| confluence.setPermissions | bool | `true` | Boolean to define whether to set local home directory permissions on startup of Confluence container. Set to 'false' to disable this behaviour.  |
| confluence.ports.http | int | `8090` | The port on which the Confluence container listens for HTTP traffic  |
| confluence.ports.hazelcast | int | `5701` | The port on which the Confluence container listens for Hazelcast traffic  |
| confluence.ports.intconnector | int | `8888` | The port used for Confluence Inernal Connecitons between multiple Confluence nodes |
| confluence.ports.intersvc | int | `8081` | The port used for Confluence internal services |
| confluence.ports.synchrony | int | `8091` | The port on which Synchrony is used for collaborative editing It is easier to manage Synchrony on the container itself rather than deploying a separate stateful set and services |
| confluence.license.secretName | string | `nil` | The name of the K8s Secret that contains the Confluence license key. If specified, then the license will be automatically populated during Confluence setup. Otherwise, it will need to be provided via the browser after initial startup. An Example of creating a K8s secret for the license below: 'kubectl create secret generic <secret-name> --from-literal=license-key=<license> https://kubernetes.io/docs/concepts/configuration/secret/#opaque-secrets  |
| confluence.license.secretKey | string | `"license-key"` | The key in the K8s Secret that contains the Confluence license key  |
| confluence.readinessProbe.enabled | bool | `true` | Whether to apply the readinessProbe check to pod.  |
| confluence.readinessProbe.initialDelaySeconds | int | `10` | The initial delay (in seconds) for the Confluence container readiness probe, after which the probe will start running.  |
| confluence.readinessProbe.periodSeconds | int | `5` | How often (in seconds) the Confluence container readiness probe will run  |
| confluence.readinessProbe.timeoutSeconds | int | `1` | Number of seconds after which the probe times out  |
| confluence.readinessProbe.failureThreshold | int | `6` | The number of consecutive failures of the Confluence container readiness probe before the pod fails readiness checks.  |
| confluence.readinessProbe.customProbe | object | `{}` | Custom readinessProbe to override the default /status httpGet  |
| confluence.startupProbe.periodSeconds | int | `5` | How often (in seconds) the Confluence container startup probe will run  |
| confluence.startupProbe.failureThreshold | int | `120` | The number of consecutive failures of the Confluence container startup probe before the pod fails startup checks.  |
| confluence.livenessProbe.enabled | bool | `false` | Whether to apply the livenessProbe check to pod.  |
| confluence.livenessProbe.initialDelaySeconds | int | `60` | Time to wait before starting the first probe  |
| confluence.livenessProbe.periodSeconds | int | `5` | How often (in seconds) the Confluence container liveness probe will run  |
| confluence.livenessProbe.timeoutSeconds | int | `1` | Number of seconds after which the probe times out  |
| confluence.livenessProbe.failureThreshold | int | `12` | The number of consecutive failures of the Confluence container liveness probe before the pod fails liveness checks.  |
| confluence.accessLog.enabled | bool | `true` | Set to 'true' if access logging should be enabled.  |
| confluence.accessLog.mountPath | string | `"/opt/atlassian/confluence/logs"` | The path within the Confluence container where the local-home volume should be mounted in order to capture access logs.  |
| confluence.accessLog.localHomeSubPath | string | `"logs"` | The subdirectory within the local-home volume where access logs should be stored.  |
| confluence.livenessProbe.enabled | bool | `false` |  |
| confluence.livenessProbe.initialDelaySeconds | int | `300` | The initial delay (in seconds) for the Confluence container liveness probe, |
| confluence.livenessProbe.periodSeconds | int | `5` | How often (in seconds) the Confluence container liveness probe will run |
| confluence.livenessProbe.failureThreshold | int | `12` | The number of consecutive failures of the Confluence container liveness probe |
| confluence.clustering.enabled | bool | `false` | Set to 'true' if Data Center clustering should be enabled This will automatically configure cluster peer discovery between cluster nodes.  |
| confluence.clustering.usePodNameAsClusterNodeName | bool | `true` | Set to 'true' if the K8s pod name should be used as the end-user-visible name of the Data Center cluster node.  |
| confluence.s3AttachmentsStorage.bucketName | string | `nil` |  |
| confluence.s3AttachmentsStorage.bucketRegion | string | `nil` |  |
| confluence.s3AttachmentsStorage.endpointOverride | string | `nil` | EXPERIMENTAL Feature! Override the default AWS API endpoint with a custom one, for example to use Minio as object storage https://min.io/  |
| confluence.resources.jvm.maxHeap | string | `"1g"` | The maximum amount of heap memory that will be used by the Confluence JVM  |
| confluence.resources.jvm.minHeap | string | `"1g"` | The minimum amount of heap memory that will be used by the Confluence JVM  |
| confluence.resources.jvm.reservedCodeCache | string | `"256m"` | The memory reserved for the Confluence JVM code cache  |
| confluence.resources.container.requests.cpu | string | `"2"` | Initial CPU request by Confluence pod.  |
| confluence.resources.container.requests.memory | string | `"2G"` | Initial Memory request by Confluence pod  |
| confluence.shutdown.terminationGracePeriodSeconds | int | `25` | The termination grace period for pods during shutdown. This should be set to the Confluence internal grace period (default 20 seconds), plus a small buffer to allow the JVM to fully terminate.  |
| confluence.shutdown.command | string | `"/shutdown-wait.sh"` | By default pods will be stopped via a [preStop hook](https://kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/), using a script supplied by the Docker image. If any other shutdown behaviour is needed it can be achieved by overriding this value. Note that the shutdown command needs to wait for the application shutdown completely before exiting; see [the default command](https://bitbucket.org/atlassian-docker/docker-atlassian-confluence-server/src/master/shutdown-wait.sh) for details.  |
| confluence.forceConfigUpdate | bool | `false` | The Docker entrypoint.py generates application configuration on first start; not all of these files are regenerated on subsequent starts. By default, confluence.cfg.xml is generated only once. Set `forceConfigUpdate` to true to change this behavior.  |
| confluence.additionalJvmArgs | list | `["-Dcom.redhat.fips=false"]` | Specifies a list of additional arguments that can be passed to the Confluence JVM, e.g. system properties.  |
| confluence.tomcatConfig | object | `{"acceptCount":"10","connectionTimeout":"20000","customServerXml":"","debug":"0","enableLookups":"false","generateByHelm":false,"maxHttpHeaderSize":"8192","maxThreads":"100","mgmtPort":"8000","minSpareThreads":"10","port":"8090","protocol":"org.apache.coyote.http11.Http11NioProtocol","proxyInternalIps":null,"proxyName":null,"proxyPort":null,"redirectPort":"8443","scheme":null,"secure":null,"uriEncoding":"UTF-8"}` | By default Tomcat's server.xml is generated in the container entrypoint from a template shipped with an official Confluence image. However, server.xml generation may fail if container is not run as root, which is a common case if Confluence is deployed to OpenShift.  |
| confluence.tomcatConfig.generateByHelm | bool | `false` | Mount server.xml as a ConfigMap. Override configuration elements if necessary  |
| confluence.tomcatConfig.customServerXml | string | `""` | Custom server.xml to be mounted into /opt/atlassian/confluence/conf  |
| confluence.seraphConfig | object | `{"autoLoginCookieAge":"1209600","generateByHelm":false}` | By default seraph-config.xml is generated in the container entrypoint from a template shipped with an official Confluence image. However, seraph-config.xml generation may fail if container is not run as root, which is a common case if Confluence is deployed to OpenShift.  |
| confluence.seraphConfig.generateByHelm | bool | `false` | Mount seraph-config.xml as a ConfigMap. Override configuration elements if necessary  |
| confluence.additionalLibraries | list | `[]` | Specifies a list of additional Java libraries that should be added to the Confluence container. Each item in the list should specify the name of the volume that contains the library, as well as the name of the library file within that volume's root directory. Optionally, a subDirectory field can be included to specify which directory in the volume contains the library file. Additional details: https://atlassian.github.io/data-center-helm-charts/examples/external_libraries/EXTERNAL_LIBS/  |
| confluence.additionalBundledPlugins | list | `[]` | Specifies a list of additional Confluence plugins that should be added to the Confluence container. Note plugins installed via this method will appear as bundled plugins rather than user plugins. These should be specified in the same manner as the 'additionalLibraries' property. Additional details: https://atlassian.github.io/data-center-helm-charts/examples/external_libraries/EXTERNAL_LIBS/  NOTE: only .jar files can be loaded using this approach. OBR's can be extracted (unzipped) to access the associated .jar  An alternative to this method is to install the plugins via "Manage Apps" in the product system administration UI.  |
| confluence.additionalVolumeMounts | list | `[{"mountPath":"/opt/atlassian/etc/server.xml.j2","name":"server-xml-j2","subPath":"server.xml.j2"},{"mountPath":"/opt/atlassian/confluence/conf/server.xml","name":"server-xml","subPath":"server.xml"},{"mountPath":"/opt/atlassian/confluence/confluence/decorators/includes/footer-content.vm","name":"footer-content-vm","subPath":"footer-content.vm"}]` | Defines any additional volumes mounts for the Confluence container. These can refer to existing volumes, or new volumes can be defined in volumes.additional. |
| confluence.additionalEnvironmentVariables | list | `[]` | Defines any additional environment variables to be passed to the Confluence container. See https://hub.docker.com/r/atlassian/confluence-server for supported variables.  |
| confluence.additionalPorts | list | `[]` | Defines any additional ports for the Confluence container.  |
| confluence.additionalVolumeClaimTemplates | list | `[]` | Defines additional volumeClaimTemplates that should be applied to the Confluence pod. Note that this will not create any corresponding volume mounts; those needs to be defined in confluence.additionalVolumeMounts  |
| confluence.topologySpreadConstraints | list | `[]` | Defines topology spread constraints for Confluence pods. See details: https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/  |
| confluence.jvmDebug.enabled | bool | `false` | Set to 'true' for remote debugging. Confluence JVM will be started with debugging port 5005 open. |
| monitoring.enabled | bool | `false` | ref: https://marketplace.atlassian.com/apps/1222775/prometheus-exporter-for-confluence?hosting=server&tab=overview |
| monitoring.exposeJmxMetrics | bool | `false` | Expose JMX metrics with jmx_exporter https://github.com/prometheus/jmx_exporter  |
| monitoring.fetchJmxExporterJar | bool | `true` | Fetch jmx_exporter jar from the image. If set to false make sure to manually copy the jar to shared home and provide an absolute path in jmxExporterCustomJarLocation  |
| monitoring.jmxExporterImageRepo | string | `"registry1.dso.mil/ironbank/opensource/prometheus/jmx-exporter"` | Image repository with jmx_exporter jar  |
| monitoring.jmxExporterImageTag | string | `"0.18.0"` | Image tag to be used to pull jmxExporterImageRepo  |
| monitoring.jmxExporterPort | int | `9999` | Port number on which metrics will be available  |
| monitoring.jmxExporterPortType | string | `"ClusterIP"` | JMX exporter port type  |
| monitoring.jmxExporterCustomJarLocation | string | `nil` | Location of jmx_exporter jar file if mounted from a secret or manually copied to shared home  |
| monitoring.jmxExporterCustomConfig | object | `{}` | Custom jmx config with the rules. Make sure to keep jmx-config key  |
| monitoring.serviceMonitor.create | bool | `false` | Create ServiceMonitor to start scraping metrics. ServiceMonitor CRD needs to be created in advance.  |
| monitoring.serviceMonitor.prometheusLabelSelector | object | `{}` | ServiceMonitorSelector of the prometheus instance.  |
| monitoring.serviceMonitor.scrapeIntervalSeconds | int | `30` | Scrape interval for the JMX service.  |
| monitoring.grafana.createDashboards | bool | `false` | Create ConfigMaps with Grafana dashboards  |
| monitoring.grafana.dashboardLabels | object | `{"grafana_dashboard":"1"}` | Label selector for Grafana dashboard importer sidecar  |
| monitoring.grafana.dashboardAnnotations | object | `{}` | Annotations added to Grafana dashboards ConfigMaps. See: https://github.com/kiwigrid/k8s-sidecar#usage  |
| synchrony.enabled | bool | `false` | Set to 'true' if Synchrony (i.e. collaborative editing) should be enabled. This will result in a separate StatefulSet and Service to be created for Synchrony. If disabled, then collaborative editing will be disabled in Confluence. |
| synchrony.replicaCount | int | `1` | Number of Synchrony pods  |
| synchrony.podAnnotations | object | `{}` | Custom annotations that will be applied to all Synchrony pods. When undefined, default to '.Values.podAnnotations' which are Confluence pod annotations (if defined) |
| synchrony.service.port | int | `80` | The port on which the Synchrony K8s Service will listen  |
| synchrony.service.type | string | `"ClusterIP"` | The type of K8s service to use for Synchrony  |
| synchrony.service.loadBalancerIP | string | `nil` | Use specific loadBalancerIP. Only applies to service type LoadBalancer.  |
| synchrony.service.annotations | object | `{}` | Annotations to apply to Synchrony Service  |
| synchrony.securityContextEnabled | bool | `true` |  |
| synchrony.securityContext.runAsUser | int | `2002` | The GID used by the Confluence docker image GID will default to 2002 if not supplied and securityContextEnabled is set to true. This is intended to ensure that the shared-home volume is group-writeable by the GID used by the Confluence container. However, this doesn't appear to work for NFS volumes due to a K8s bug: https://github.com/kubernetes/examples/issues/260 |
| synchrony.securityContext.runAsGroup | int | `2002` |  |
| synchrony.securityContext.runAsNonRoot | bool | `true` |  |
| synchrony.securityContext.fsGroup | int | `2002` |  |
| synchrony.containerSecurityContext | object | `{"runAsGroup":2002,"runAsNonRoot":true,"runAsUser":2002}` | Standard K8s field that holds security configurations that will be applied to a container. https://kubernetes.io/docs/tasks/configure-pod-container/security-context/  |
| synchrony.setPermissions | bool | `true` | Boolean to define whether to set synchrony home directory permissions on startup of Synchrony container. Set to 'false' to disable this behaviour.  |
| synchrony.ports.http | int | `8091` | The port on which the Synchrony container listens for HTTP traffic  |
| synchrony.ports.hazelcast | int | `5701` | The port on which the Synchrony container listens for Hazelcast traffic  |
| synchrony.readinessProbe.healthcheckPath | string | `"/synchrony/heartbeat"` | The healthcheck path to check against for the Synchrony container useful when configuring behind a reverse-proxy or loadbalancer https://confluence.atlassian.com/confkb/cannot-enable-collaborative-editing-on-synchrony-cluster-962962742.html  |
| synchrony.readinessProbe.initialDelaySeconds | int | `5` | The initial delay (in seconds) for the Synchrony container readiness probe, after which the probe will start running.  |
| synchrony.readinessProbe.periodSeconds | int | `1` | How often (in seconds) the Synchrony container readiness probe will run  |
| synchrony.readinessProbe.failureThreshold | int | `10` | The number of consecutive failures of the Synchrony container readiness probe before the pod fails readiness checks.  |
| synchrony.resources.jvm.minHeap | string | `"1g"` | The maximum amount of heap memory that will be used by the Synchrony JVM  |
| synchrony.resources.jvm.maxHeap | string | `"2g"` | The minimum amount of heap memory that will be used by the Synchrony JVM  |
| synchrony.resources.jvm.stackSize | string | `"2048k"` | The memory allocated for the Synchrony stack  |
| synchrony.resources.container.requests.cpu | string | `"2"` | Initial CPU request by Synchrony pod  |
| synchrony.resources.container.requests.memory | string | `"2.5G"` | Initial Memory request Synchrony pod  |
| synchrony.additionalJvmArgs | list | `["-Dcom.redhat.fips=false"]` | Specifies a list of additional arguments that can be passed to the Synchrony JVM, e.g. system properties.  |
| synchrony.shutdown.terminationGracePeriodSeconds | int | `25` | The termination grace period for pods during shutdown. This should be set to the Synchrony internal grace period (default 20 seconds), plus a small buffer to allow the JVM to fully terminate. |
| synchrony.additionalLibraries | list | `[]` | Specifies a list of additional Java libraries that should be added to the Synchrony container. Each item in the list should specify the name of the volume that contains the library, as well as the name of the library file within that volume's root directory. Optionally, a subDirectory field can be included to specify which directory in the volume contains the library file. Additional details: https://atlassian.github.io/data-center-helm-charts/examples/external_libraries/EXTERNAL_LIBS/  |
| synchrony.additionalVolumeMounts | list | `[]` | Defines any additional volumes mounts for the Synchrony container. These can refer to existing volumes, or new volumes can be defined via 'volumes.additionalSynchrony'.  |
| synchrony.additionalPorts | list | `[]` | Defines any additional ports for the Synchrony container.  |
| synchrony.topologySpreadConstraints | list | `[]` | Defines topology spread constraints for Synchrony pods. See details: https://kubernetes.io/docs/concepts/workloads/pods/pod-topology-spread-constraints/  |
| fluentd.enabled | bool | `false` | Set to 'true' if the Fluentd sidecar (DaemonSet) should be added to each pod  |
| fluentd.imageRepo | string | `"fluent/fluentd-kubernetes-daemonset"` | The Fluentd sidecar image repository  |
| fluentd.imageTag | string | `"v1.11.5-debian-elasticsearch7-1.2"` | The Fluentd sidecar image tag  |
| fluentd.command | string | `nil` | The command used to start Fluentd. If not supplied the default command will be used: "fluentd -c /fluentd/etc/fluent.conf -v"  Note: The custom command can be free-form, however pay particular attention to the process that should ultimately be left running in the container. This process should be invoked with 'exec' so that signals are appropriately propagated to it, for instance SIGTERM. An example of how such a command may look is: "<command 1> && <command 2> && exec <primary command>" |
| fluentd.customConfigFile | bool | `false` | Set to 'true' if a custom config (see 'configmap-fluentd.yaml' for default) should be used for Fluentd. If enabled this config must be supplied via the 'fluentdCustomConfig' property below.  |
| fluentd.fluentdCustomConfig | object | `{}` | Custom fluent.conf file  |
| fluentd.httpPort | int | `9880` | The port on which the Fluentd sidecar will listen  |
| fluentd.elasticsearch.enabled | bool | `true` | Set to 'true' if Fluentd should send all log events to an Elasticsearch service.  |
| fluentd.elasticsearch.hostname | string | `"elasticsearch"` | The hostname of the Elasticsearch service that Fluentd should send logs to.  |
| fluentd.elasticsearch.indexNamePrefix | string | `"confluence"` | The prefix of the Elasticsearch index name that will be used  |
| fluentd.extraVolumes | list | `[]` | Specify custom volumes to be added to Fluentd container (e.g. more log sources)  |
| podAnnotations | object | `{}` | Custom annotations that will be applied to all Confluence pods  |
| podLabels | object | `{}` | Custom labels that will be applied to all Confluence pods  |
| nodeSelector | object | `{}` | Standard K8s node-selectors that will be applied to all Confluence pods  |
| tolerations | list | `[]` | Standard K8s tolerations that will be applied to all Confluence pods  |
| affinity | object | `{}` | Standard K8s affinities that will be applied to all Confluence pods  |
| schedulerName | string | `nil` | Standard K8s schedulerName that will be applied to all Confluence pods. Check Kubernetes documentation on how to configure multiple schedulers: https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/#specify-schedulers-for-pods  |
| priorityClassName | string | `nil` | Priority class for the application pods. The PriorityClass with this name needs to be available in the cluster. For details see https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#priorityclass  |
| additionalContainers | list | `[]` | Additional container definitions that will be added to all Confluence pods  |
| additionalInitContainers | list | `[]` | Additional initContainer definitions that will be added to all Confluence pods |
| additionalLabels | object | `{}` | Additional labels that should be applied to all resources |
| additionalFiles | list | `[]` | Additional existing ConfigMaps and Secrets not managed by Helm that should be mounted into service container. Configuration details below (camelCase is important!): 'name'      - References existing ConfigMap or secret name. 'type'      - 'configMap' or 'secret' 'key'       - The file name. 'mountPath' - The destination directory in a container. VolumeMount and Volumes are added with this name and index position, for example; custom-config-0, keystore-2  |
| additionalHosts | list | `[]` | Additional host aliases for each pod, equivalent to adding them to the /etc/hosts file. https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/ |
| postgresql.install | bool | `false` |  |
| postgresql.image.registry | string | `"registry1.dso.mil"` |  |
| postgresql.image.debug | bool | `true` |  |
| postgresql.image.repository | string | `"ironbank/opensource/postgres/postgresql"` |  |
| postgresql.image.tag | string | `"14.9"` |  |
| postgresql.image.pullSecrets[0] | string | `"private-registry"` |  |
| postgresql.auth.username | string | `"confuser"` |  |
| postgresql.auth.password | string | `"bogus-satisfy-upgrade"` |  |
| postgresql.auth.postgresPassword | string | `"bogus-satisfy-upgrade"` |  |
| postgresql.auth.database | string | `"confluence"` |  |
| postgresql.auth.existingSecret | string | `nil` |  |
| postgresql.auth.secretKeys.adminPasswordKey | string | `nil` |  |
| postgresql.auth.secretKeys.userPasswordKey | string | `nil` |  |
| postgresql.primary.persistence.mountPath | string | `"/var/lib/postgresql"` |  |
| postgresql.primary.initdb.args | string | `"-A scram-sha-256"` |  |
| postgresql.primary.containerSecurityContext.runAsUser | int | `1001` |  |
| postgresql.primary.containerSecurityContext.runAsGroup | int | `1001` |  |
| postgresql.primary.containerSecurityContext.runAsNonRoot | bool | `true` |  |
| postgresql.primary.extraEnvVars[0].name | string | `"POSTGRES_DB"` |  |
| postgresql.primary.extraEnvVars[0].value | string | `"{{ .Values.auth.database }}"` |  |
| postgresql.postgresqlDataDir | string | `"/var/lib/postgresql/pgdata/data"` |  |
| postgresql.volumePermissions.enabled | bool | `false` |  |
| proxyName | string | `nil` |  |
| hostnamePrefix | string | `"confluence"` |  |
| hostname | string | `"bigbang.dev"` |  |
| istio.enabled | bool | `false` |  |
| istio.gateways[0] | string | `"istio-system/public"` |  |
| bbtests.enabled | bool | `false` |  |
| bbtests.cypress.artifacts | bool | `true` |  |
| bbtests.cypress.envs.cypress_url | string | `"http://{{ include \"common.names.fullname\" . }}:{{ .Values.confluence.service.port }}/setup/setuplicense.action"` |  |
| bbtests.cypress.resources.requests.cpu | string | `"1"` |  |
| bbtests.cypress.resources.requests.memory | string | `"1Gi"` |  |
| bbtests.cypress.resources.limits.cpu | string | `"1"` |  |
| bbtests.cypress.resources.limits.memory | string | `"1Gi"` |  |
| helmTestImage | string | `"registry1.dso.mil/ironbank/big-bang/base:2.0.0"` | Image used for the upstream provided helm tests |
| hpa.enabled | bool | `false` |  |
| hpa.maxReplicas | int | `4` |  |
| hpa.cpu | int | `70` |  |
| hpa.memory | int | `80` |  |
| hpa.behavior.enabled | bool | `false` |  |
| hpa.behavior.time | int | `300` |  |
| podDisruptionBudget | object | `{"annotations":{},"enabled":false,"labels":{},"maxUnavailable":null,"minAvailable":null}` | PodDisruptionBudget: https://kubernetes.io/docs/tasks/run-application/configure-pdb/ You can specify only one of maxUnavailable and minAvailable in a single PodDisruptionBudget. When both minAvailable and maxUnavailable are set, maxUnavailable takes precedence.  |
| networkPolicies.enabled | bool | `false` |  |
| networkPolicies.ingressLabels.app | string | `"public-ingressgateway"` |  |
| networkPolicies.ingressLabels.istio | string | `"ingressgateway"` |  |
| networkPolicies.controlPlaneCidr | string | `"0.0.0.0/0"` |  |
| additionalConfigMaps | list | `[]` | Create additional ConfigMaps with given names, keys and content. Ther Helm release name will be used as a prefix for a ConfigMap name, fileName is used as subPath  |

## Contributing

Please see the [contributing guide](./CONTRIBUTING.md) if you are interested in contributing.
