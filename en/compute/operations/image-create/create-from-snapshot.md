# Creating an image from a disk snapshot

To create an image from a disk snapshot:

{% list tabs %}

- Management console

   1. In the [management console]({{ link-console-main }}), select the folder where you want to create the image.
   1. Select **{{ compute-name }}**.
   1. On the left-hand panel, select ![image](../../../_assets/compute/snapshots.svg) **Disk snapshots**.
   1. In the row with the desired snapshot, click ![image](../../../_assets/horizontal-ellipsis.svg) and select the **Create image** command from the menu.
   1. Enter the image name.

      * Its length can be from 2 to 63 characters.
      * It may contain lowercase Latin letters, numbers, and hyphens.
      * The first character must be a letter. The last character can't be a hyphen.

   1. If required, provide a free-form text description for the image.
   1. To create an [optimized image](../../concepts/image.md#images-optimized-for-deployment), enable **Optimize for deployment**.
   1. Click **Create**.

- CLI

   {% include [cli-install](../../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

   1. View a description of the CLI command to create an image:

      ```
      yc compute image create --help
      ```

   1. Get a list of snapshots in the default folder:

      {% include [compute-snapshot-list](../../_includes_service/compute-snapshot-list.md) %}

   1. Select the ID (`ID`) or the name (`NAME`) of the desired snapshot.
   1. Create an image in the default folder:

      ```
      yc compute image create \
          --name first-image \
          --source-snapshot-name first-snapshot \
          --description "my first image via yc"
      ```

      This command will create an image named `first-image` and the description `my first image via yc` from the `first-snapshot` snapshot.

      To create an [optimized image](../../concepts/image.md#images-optimized-for-deployment), use the `--pooled` flag:

      ```
      yc compute image create \
          --name first-image \
          --source-snapshot-name first-snapshot \
          --description "my first image via yc" \
          --pooled
      ```

- Terraform

   If you don't have Terraform, [install it and configure the {{ yandex-cloud }} provider](../../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform).

   1. Describe the resource parameters in the `yandex_compute_image` configuration file.

      Example configuration file structure:

      ```
      resource "yandex_compute_image" "image-1" {
        name            = "<image name>"
        source_snapshot = "<snapshot ID>"
      }
      ```

      For more information about resources that you can create with Terraform, please see the [provider documentation]({{ tf-provider-link }}/).

   1. Make sure that the configuration files are correct.

      1. In the command line, go to the directory where you created the configuration file.
      1. Run the check using the command:

         ```bash
         terraform plan
         ```

      If the configuration is described correctly, the terminal displays a list of created resources and their parameters. If there are errors in the configuration, Terraform points them out.

   1. Deploy the cloud resources.

      1. Run the command:

         ```bash
         terraform apply
         ```

      1. Confirm that you want to create the resources.

      Afterwards, all the necessary resources are created in the specified folder. You can check that the resources are there with the correct settings using the [management console]({{ link-console-main }}).

- API

   1. Retrieve a list of snapshots using the [SnapshotService/List](../../api-ref/grpc/snapshot_service.md#List) gRPC API method or the [list](../../api-ref/Snapshot/list.md) method of the REST API `Snapshot` resource.
   1. Create an image using the [ImageService/Create](../../api-ref/grpc/image_service.md#Create) gRPC API method or the [Create](../../api-ref/Image/create.md) method of the REST API `Image` resource. Specify the snapshot ID in your request.

{% endlist %}

After creation, the image will have the `CREATING` status. Wait until the image status changes to `READY` before using it.