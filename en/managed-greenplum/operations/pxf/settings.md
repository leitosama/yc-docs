# Changing PXF settings

The [PXF](../external-tables.md) settings you can configure using the {{ yandex-cloud }} tools match those in the {{ GP }} [pxf-application.properties]({{ gp.docs.vmware }}-Platform-Extension-Framework/6.9/greenplum-platform-extension-framework/config_files.html#pxfapplicationproperties-1) configuration file. It describes the features of the PXF protocol. In {{ GP }} clusters, PXF settings are at defaults. To optimize operations with [external tables](../../concepts/external-tables.md), you can update the PXF settings using the {{ yandex-cloud }} interfaces instead of editing the file.

{% list tabs group=instructions %}

- Management console {#console}

   To change the PXF settings:

   1. Go to the [folder page]({{ link-console-main }}) and select **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-greenplum }}**.
   1. Click the cluster name and select ![image](../../../_assets/console-icons/arrow-right-arrow-left.svg) **{{ ui-key.yacloud.greenplum.label_pxf }}** in the left-hand panel.
   1. Click **{{ ui-key.yacloud.greenplum.cluster.pxf.action_edit-settings }}** at the top of the page.
   1. Change the settings:

      * **Connection Timeout**: Timeout for connection to the Apache Tomcat® server when making read requests. The values may range from `5` to `600` seconds. You can specify the values in various time units.
      * **Upload Timeout**: Timeout for connection to the Apache Tomcat® server when making write requests. The values may range from `5` to `600` seconds. You can specify the values in various time units.
      * **Max Threads**: Maximum number of the Apache Tomcat® threads. The values may range from `1` to `1024`.

         To prevent situations when requests get stuck or fail due to running out of memory or malfunctioning of the Java garbage collector, specify the number of the Apache Tomcat® threads. Learn more about adjusting the number of threads in the [VMware {{ GP }} Platform Extension Framework](https://docs.vmware.com/en/VMware-Greenplum-Platform-Extension-Framework/6.9/greenplum-platform-extension-framework/cfg_mem.html) documentation.

      * **Pool Allow Core Thread Timeout**: Determines whether a timeout for core streaming threads is allowed.
      * **Pool Core Size**: Number of core streaming threads per pool. The values may range from `1` to `1024`.
      * **Pool Queue Capacity**: Maximum number of requests you can add to a pool queue for core streaming threads. The values may range from zero upward. If `0`, no pool queue is generated.
      * **Pool Max Size**: Maximum allowed number of core streaming threads. The values may range from `1` to `1024`.
      * **Xmx**: Initial size of the JVM heap for the PXF daemon. The values may range from `64` to `16384` megabytes. You can specify the values in various units.
      * **Xms**: Maximum size of the JVM heap for the PXF daemon. The values may range from `64` to `16384` megabytes. You can specify the values in various units.

   1. Click **{{ ui-key.yacloud.common.save }}**.

- CLI {#cli}

   {% include [cli-install](../../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

   To change the PXF settings:

   1. View a description of the update cluster configuration CLI command:

      ```bash
      {{ yc-mdb-gp }} cluster update --help
      ```

   1. Specify PXF settings:

      ```bash
      {{ yc-mdb-gp }} cluster update <cluster_name_or_ID> \
         --pxf-connection-timeout=<read_request_timeout> \
         --pxf-upload-timeout=<write_request_timeout> \
         --pxf-max-threads=<maximum_number_of_Apache_Tomcat®_threads> \
         --pxf-pool-allow-core-thread-timeout=<whether_timeout_for_streaming_threads_is_permitted> \
         --pxf-poll-core-size=<number_of_streaming_threads> \
         --pxf-pool-queue-capacity=<capacity_of_pool_queue_for_streaming_threads> \
         --pxf-pool-max-size=<maximum_number_of_streaming_threads> \
         --pxf-xmx=<initial_size_of_JVM_heap> \
         --pxf-xms=<maximum_size_of_JVM_heap>
      ```

      Where:

      * `pxf-connection-timeout`: Timeout for connection to the Apache Tomcat® server when making read requests, in seconds. The values may range from `5` to `600`.
      * `pxf-upload-timeout`: Timeout for connection to the Apache Tomcat® server when making write requests, in seconds. The values may range from `5` to `600`.
      * `pxf-max-threads`: Maximum number of the Apache Tomcat® threads. The values may range from `1` to `1024`.

         To prevent situations when requests get stuck or fail due to running out of memory or malfunctioning of the Java garbage collector, specify the number of the Apache Tomcat® threads. Learn more about adjusting the number of threads in the [VMware {{ GP }} Platform Extension Framework](https://docs.vmware.com/en/VMware-Greenplum-Platform-Extension-Framework/6.9/greenplum-platform-extension-framework/cfg_mem.html) documentation.

      * `pxf-pool-allow-core-thread-timeout`: Determines whether a timeout for core streaming threads is allowed. The default value is `false`.
      * `pxf-poll-core-size`: Number of core streaming threads per pool. The values may range from `1` to `1024`.
      * `pxf-pool-queue-capacity`: Maximum number of requests you can add to a pool queue for core streaming threads. The values may range from zero upward. If `0`, no pool queue is generated.
      * `pxf-pool-max-size`: Maximum allowed number of core streaming threads. The values may range from `1` to `1024`.
      * `pxf-xmx`: Initial size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.
      * `pxf-xms`: Maximum size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.

      You can [retrieve the cluster name with a list of clusters in the folder](../cluster-list.md#list-clusters).

- {{ TF }} {#tf}

   To change the PXF settings:

   1. Open the current {{ TF }} configuration file with an infrastructure plan.

      For more information about creating this file, see [{#T}](../cluster-create.md).

      For a complete list of available {{ mgp-name }} cluster configuration fields, see the [{{ TF }} provider documentation]({{ tf-provider-mgp }}).

   1. In the cluster description, under `pxf_config`, configure the PXF settings:

      ```hcl
      resource "yandex_mdb_greenplum_cluster" "<cluster_name>" {
        ...
        pxf_config {
          connection_timeout             = <read_request_timeout>
          upload_timeout                 = <write_request_timeout>
          max_threads                    = <maximum_number_of_Apache_Tomcat®_threads>
          pool_allow_core_thread_timeout = <whether_timeout_for_streaming_threads_is_permitted>
          pool_core_size                 = <number_of_streaming_threads>
          pool_queue_capacity            = <capacity_of_pool_queue_for_streaming_threads>
          pool_max_size                  = <maximum_number_of_streaming_threads>
          xmx                            = <initial_size_of_JVM_heap>
          xms                            = <maximum_size_of_JVM_heap>
        }
      }
      ```

      Where:

      * `connection_timeout`: Timeout for connection to the Apache Tomcat® server when making read requests, in seconds. The values may range from `5` to `600`.
      * `upload_timeout`: Timeout for connection to the Apache Tomcat® server when making write requests, in seconds. The values may range from `5` to `600`.
      * `max_threads`: Maximum number of the Apache Tomcat® threads. The values may range from `1` to `1024`.

         To prevent situations when requests get stuck or fail due to running out of memory or malfunctioning of the Java garbage collector, specify the number of the Apache Tomcat® threads. Learn more about adjusting the number of threads in the [VMware {{ GP }} Platform Extension Framework](https://docs.vmware.com/en/VMware-Greenplum-Platform-Extension-Framework/6.9/greenplum-platform-extension-framework/cfg_mem.html) documentation.

      * `pool_allow_core_thread_timeout`: Determines whether a timeout for core streaming threads is allowed. The default value is `false`.
      * `pool_core_size`: Number of core streaming threads per pool. The values may range from `1` to `1024`.
      * `pool_queue_capacity`: Maximum number of requests you can add to a pool queue for core streaming threads. The values may range from zero upward. If `0`, no pool queue is generated.
      * `pool_max_size`: Maximum allowed number of core streaming threads. The values may range from `1` to `1024`.
      * `xmx`: Initial size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.
      * `xms`: Maximum size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.

   1. Make sure the settings are correct.

      {% include [terraform-validate](../../../_includes/mdb/terraform/validate.md) %}

   1. Confirm updating the resources.

      {% include [terraform-apply](../../../_includes/mdb/terraform/apply.md) %}

- API {#api}

   To change PXF settings, use the [update](../../api-ref/Cluster/update.md) REST API method for the [Cluster](../../api-ref/Cluster/index.md) resource or the [ClusterService/Update](../../api-ref/grpc/cluster_service.md#Update) gRPC API call and provide the following in the request:

   * Cluster ID in the `clusterId` parameter. To find out the cluster ID, [get a list of clusters in the folder](../cluster-list.md#list-clusters).
   * List of PXF settings to change in the `configSpec.pxfConfig` parameter:

      * `connectionTimeout`: Timeout for connection to the Apache Tomcat® server when making read requests, in seconds. The values may range from `5` to `600`.
      * `uploadTimeout`: Timeout for connection to the Apache Tomcat® server when making write requests, in seconds. The values may range from `5` to `600`.
      * `maxThreads`: Maximum number of the Apache Tomcat® threads. The values may range from `1` to `1024`.

         To prevent situations when requests get stuck or fail due to running out of memory or malfunctioning of the Java garbage collector, specify the number of the Apache Tomcat® threads. Learn more about adjusting the number of threads in the [VMware {{ GP }} Platform Extension Framework](https://docs.vmware.com/en/VMware-Greenplum-Platform-Extension-Framework/6.9/greenplum-platform-extension-framework/cfg_mem.html) documentation.

      * `poolAllowCoreThreadTimeout`: Determines whether a timeout for core streaming threads is allowed. The default value is `false`.
      * `poolCoreSize`: Number of core streaming threads per pool. The values may range from `1` to `1024`.
      * `poolQueueCapacity`: Maximum number of requests you can add to a pool queue for core streaming threads. The values may range from zero upward. If `0`, no pool queue is generated.
      * `poolMaxSize`: Maximum allowed number of core streaming threads. The values may range from `1` to `1024`.
      * `xmx`: Initial size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.
      * `xms`: Maximum size of the JVM heap for the PXF daemon, in megabytes. The values may range from `64` to `16384`.

   * List of cluster configuration fields to update in the `UpdateMask` parameter.

   {% include [note-api-updatemask](../../../_includes/note-api-updatemask.md) %}

{% endlist %}

{% include [greenplum-trademark](../../../_includes/mdb/mgp/trademark.md) %}
