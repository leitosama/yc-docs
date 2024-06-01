1. [Prepare your cloud](#before-begin).
1. [Create an infrastructure](#deploy).
1. [Check that the hosting is running properly](#test).

We will use the `my-site.com` domain name as an example.

If you no longer need the resources you created, [delete them](#clear-out).

## Prepare your cloud {#before-begin}

{% include [before-you-begin](../_tutorials_includes/before-you-begin.md) %}

### Required paid resources {#paid-resources}

{% include [tls-termination-paid-resources](../_tutorials_includes/tls-termination-paid-resources.md) %}

## Create an infrastructure {#deploy}

{% include [terraform-definition](../_tutorials_includes/terraform-definition.md) %}

To create an infrastructure using {{ TF }}:
1. [Install {{ TF }}](../../tutorials/infrastructure-management/terraform-quickstart.md#install-terraform), [get the authentication credentials](../../tutorials/infrastructure-management/terraform-quickstart.md#get-credentials), and specify the source for installing the {{ yandex-cloud }} provider (see [{#T}](../../tutorials/infrastructure-management/terraform-quickstart.md#configure-provider), step 1).
1. Prepare files with the infrastructure description:

   {% list tabs group=infrastructure_description %}

   - Ready-made configuration {#ready}

      1. Clone the repository with configuration files.

         ```bash
         git clone https://github.com/yandex-cloud-examples/yc-alb-tls-termination.git
         ```

      1. Go to the directory with the repository. Make sure it contains the following files:
         * `tls-termination-config.tf`: Configuration of the infrastructure you create.
         * `tls-terminationg.auto.tfvars`: User data file.

   - Manually {#manual}

      1. Create a directory for configuration files.
      1. In the directory, create:
         1. `tls-termination-config.tf` configuration file:

            {% cut "tls-termination-config.tf" %}

            {% include [tls-termination-config](../../_includes/application-load-balancer/tls-termination-config.md) %}

            {% endcut %}

         1. `tls-termination.auto.tfvars` user data file:

            {% cut "tls-termination.auto.tfvars" %}

            ```hcl
            folder_id    = "<folder_ID>"
            vm_user      = "<VM_user_name>"
            ssh_key_path = "<path_to_public_SSH_key>"
            domain       = "<domain>"
            certificate  = "<path_to_certificate_file>"
            private_key  = "<path_to_file_with_private_key"
            ```

            {% endcut %}

   {% endlist %}

   For more information about the parameters of resources used in {{ TF }}, see the provider documentation:
   * [Network](../../vpc/concepts/network.md#network): [yandex_vpc_network]({{ tf-provider-resources-link }}/vpc_network)
   * [Subnets](../../vpc/concepts/network.md#subnet): [yandex_vpc_subnet]({{ tf-provider-resources-link }}/vpc_subnet)
   * [Static public IP address](../../vpc/concepts/address.md#public-addresses): [yandex_vpc_address]({{ tf-provider-resources-link }}/vpc_address)
   * [Security groups](../../vpc/concepts/security-groups.md): [yandex_vpc_security_group]({{ tf-provider-resources-link }}/vpc_security_group)
   * [TLS certificate](../../certificate-manager/concepts/imported-certificate.md): [yandex_cm_certificate]({{ tf-provider-resources-link }}/cm_certificate)
   * [VM image](../../compute/concepts/image.md): [yandex_compute_image]({{ tf-provider-resources-link }}/compute_image)
   * [Service account](../../iam/concepts/users/service-accounts.md): [yandex_iam_service_account]({{ tf-provider-resources-link }}/iam_service_account)
   * [Role](../../iam/concepts/access-control/roles.md): [yandex_resourcemanager_folder_iam_member]({{ tf-provider-resources-link }}/resourcemanager_folder_iam_member)
   * [Instance group](../../compute/concepts/instance-groups/index.md): [yandex_compute_instance_group]({{ tf-provider-resources-link }}/compute_instance_group)
   * [Backend group](../../application-load-balancer/concepts/backend-group.md): [yandex_alb_backend_group]({{ tf-provider-resources-link }}/alb_backend_group)
   * [HTTP router](../../application-load-balancer/concepts/http-router.md): [yandex_alb_http_router]({{ tf-provider-resources-link }}/alb_http_router)
   * [Virtual host](../../application-load-balancer/concepts/http-router.md#virtual-host): [yandex_alb_virtual_host]({{ tf-provider-resources-link }}/alb_virtual_host)
   * [L7 load balancer](../../application-load-balancer/concepts/application-load-balancer.md): [yandex_alb_load_balancer]({{ tf-provider-resources-link }}/alb_load_balancer)
   * [DNS zone](../../dns/concepts/dns-zone.md): [yandex_dns_zone]({{ tf-provider-resources-link }}/dns_zone)
   * [DNS resource record](../../dns/concepts/resource-record.md): [yandex_dns_recordset]({{ tf-provider-resources-link }}/dns_recordset)

1. In the `tls-termination.auto.tfvars` file, set the user-defined parameters:
   * `folder_id`: [Folder ID](../../resource-manager/operations/folder/get-id.md).
   * `vm_user`: VM user name.
   * `ssh_key_path`: Path to the file with the public SSH key. For more information, see [{#T}](../../compute/operations/vm-connect/ssh.md#creating-ssh-keys).
   * `domain`: Domain to host the site.
   * `certificate`: Path to the file with the [user certificate](../../certificate-manager/operations/import/cert-create.md#create-file).
   * `private_key`: Path to the file with the user certificate's private key.
1. Create resources:

   {% include [terraform-validate-plan-apply](../_tutorials_includes/terraform-validate-plan-apply.md) %}

1. [Get public IP addresses](../../compute/operations/instance-groups/get-info.md): you will need them to [check that the hosting is running properly](#test).

{% endlist %}

After creating the infrastructure, [check that the hosting is running properly](#test).

## Check that the hosting is running properly {#test}

{% include [tls-termination-test](../_tutorials_includes/tls-termination-test.md) %}

## Delete the resources you created {#clear-out}

To shut down the hosting and stop paying for the created resources:

1. Open the `tls-termination-config.tf` configuration file and delete the description of the infrastructure being created from it.
1. Apply the changes:

   {% include [terraform-validate-plan-apply](../_tutorials_includes/terraform-validate-plan-apply.md) %}

