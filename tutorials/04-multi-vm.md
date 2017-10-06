#Tutorial 4: Deploy multiple virtual machines

Previously in [tutorial 3](/wiki/Tutorial-3-deploy-a-single-vm), you created a single virtual machine to act as a jumpbox.. In this tutorial we will create an additional subnet with two virtual machines. These VMs will be configured as Active Directory domain controllers in a later tutorial.

##Architecture

Our architecture builds upon the resources established in the previous tutorials.

[TODO: Add architecture diagram]

We have added the following:
- Another subnet in the `msft-hub-vnet` named `ad`.
- Two virtual machines in the `ad` subnet named `dc-vm1` and `dc-vm2`.

Let's review the JSON settings for deploying these additional resources.

## Settings file walkthrough


