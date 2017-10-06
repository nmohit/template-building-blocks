#Tutorial 3: Deploy a single virtual machine

Previously in [tutorial 1](/wiki/Tutorial-1-deploy-a-simple-VNet) and [tutorial 2](/wiki/Tutorial-2-connect-two-VNets-using-peering), you created two VNets (`msft-hub-vnet` and `msft-spoke1-vnet`) connected using VNet peering. In this tutorial we will create an additional management subnet in the hub VNet, along with a virtual machine (VM) with a public IP (PIP) address to act as a jumpbox. It is a recommended security practice to use a single jumpbox (or set of jumpboxes) as your point of entry for accessing VMs in Azure. This allows you to minimize the number of VM resources with public-facing addresses.

# Before You Start

After completing the previous tutorials, review the Azure Building Blocks [virtual machine][azbb-vm] settings file schema to become familiar with the properties we'll walk through.

# Architecture

Our architecture builds on the network resources established in the first two tutorials.

![Single VM with public IP address][single-vm]

We have added the following:
- Another subnet in the `msft-hub-vnet` named `mgmt`.
- A virtual machine in the `mgmt` subnet named `jb-vm1`.
- A public IP address for the VM.

Let's review the JSON settings for deploying these additional resources.

# Settings file walkthrough

The JSON settings we'll look at are provided in the [Azure Building Blocks Github repository][azbb]. Navigate to the `\scenarios\vm\` folder and open the `vm-simple.json` file in an editor such as [Visual Studio Code][vs-code].

```json
{
    "type": "VirtualMachine",
    "settings": {
        "vmCount": 1,
        "osType": "windows",
        "namePrefix": "jb",
        "adminPassword": "testPassw0rd!23",
        "nics": [
            {
                "subnetName": "mgmt"
            }
        ],
        "virtualNetwork": {
            "name": "msft-hub-vnet"
        }
    }
}
```

To deploy a VM, we specify a `VirtualMachine` building block type. Let's look at the properties of a basic VM deployment. You can review the default values for properties not specified in the virtual machine][azbb-vm] settings file schema.

* **"vmcount"**  
  The Azure Building Blocks can create multiple VMs with a common configuration. The `vmcount` property lets you specify how many VMs with this configuration you want to deploy. In this tutorial, we are deploying a single VM jumpbox, so we specify a count of `1`.

* **"osType"**  
  You can specify either `linux` or `windows` as the `osType`. This tutorial specifies `windows`, which will use the latest image for Windows Server 2016 Datacenter edition.

* **"namePrefix"**  
  Azure Building Blocks will deploy multiple VMs using a standard naming format. We've specified a `namePrefix` of `jb` for our jumpbox VM. Since we're only deploying one VM, the name calculated for the VM is `jb-vm1`.  

* **"adminPassword"**  
  Indicates the Administrator password for the VM's operating system.

* **"nics"**  
  In this tutorial, we create a single network interface card (NIC) for our VM and connect it to the `mgmt` subnet. By default, this NIC is assigned a PIP. 

* **"virtualNetwork"**  
  The `virtualNetwork` property is set to `msft-hub-vnet`, since the `mgmt` subnet specified earlier resides there.

# Add the JSON object to your settings file

Currently, your settings file looks like this:

[TODO: Add code example #1]
[TODO: Specify different settings file?]

Copy and paste the JSON from the `vm-simple.json` settings file as follows:

[TODO: Add code example #2]

Save your settings file. Now let's deploy your VM to Azure.

# Deploy the virtual machine using Azure Building Blocks

As before, we'll use the `azbb` command to deploy your updated settings.

```
azbb -g <new or existing resource group> -s <subscription ID> -l <region> -p <path to your settings file> --deploy
```

[TODO: Revise paragraph]
To verify that your VM was deployed, visit the [Azure Portal][azure-portal], click on `Resource Groups` in the left-hand pane to open the **Resource Groups** blade, then click on the name of the resource group you specified above. In that blade, you should see the `jb-vm1` virtual machine, along with its associated resources:
  * `jb-vm1-nic1` - The NIC for the VM.
  * `jb-vm1-nic1-pip` - The public IP address for the NIC.
  * `jb-vm1-os` - The disk used for the VM's operating system.

To verify that the `mgmt` subnet was deployed, click on the `msft-hub-vnet` resource. You should see the `jb-vm1-nic1` network interface resource, assigned to the `mgmt` subnet. If you click on `Subnets` in the virtual network blade, you will see   


<!-- links -->
[azbb]: https://github.com/mspnp/template-building-blocks
[azbb-command-line]: https://github.com/mspnp/template-building-blocks/wiki/Use-Azure-Building-Blocks-from-the-command-line
[azbb-wiki]: https://github.com/mspnp/template-building-blocks/wiki
[azbb-overview]: https://github.com/mspnp/template-building-blocks/wiki/overview
[azbb-install]: https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks
[azbb-parameter-file]: https://github.com/mspnp/template-building-blocks/wiki/create-a-template-building-blocks-parameter-file
[azbb-vnet]: https://github.com/mspnp/template-building-blocks/wiki/virtual-network
[azbb-vm]: https://github.com/mspnp/template-building-blocks/wiki/virtual-machines
[azure-limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits
[azure-portal]: http://portal.azure.com
[azure-vnet-overview]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview
[azure-vnet-peering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[cidr-notation]: https://wikipedia.org/wiki/Classless_Inter-Domain_Routing
[create-a-vnet]: http:/azure/virtual-network/virtual-networks-create-vnet-arm-pportal
[github-how-to-clone]: https://help.github.com/articles/cloning-a-repository/
[plan-and-design-vnet]: https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm
[tutorial-1]: https://github.com/mspnp/template-building-blocks/wiki/Tutorial-1-deploy-a-simple-VNet
[single-vm]: https://raw.githubusercontent.com/wiki/mspnp/template-building-blocks/images/single-vm.png
[vs-code]: https://code.visualstudio.com/