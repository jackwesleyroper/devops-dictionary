layout: page
title: "Terraform"
permalink: /Iac/Terraform

# Terraform
Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp. It allows you to define and manage cloud and on-premises resources using human-readable configuration files. Think of Terraform as a blueprint for your infrastructure, where you can specify what resources you need, and Terraform will create and manage them for you.

## Terraform Example: Deploying to Azure
Imagine you are an architect designing a building. You create detailed blueprints that specify every aspect of the building, from the foundation to the roof. Similarly, with Terraform, you write configuration files that describe your desired cloud infrastructure.

Here's a simplified example of deploying a virtual machine (VM) to Azure using Terraform:

Write the Configuration: You create a Terraform configuration file (main.tf) that specifies the resources you need. For example, to deploy a VM in Azure, you might write:

```
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_virtual_network" "example" {
  name                = "example-network"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_subnet" "example" {
  name                 = "example-subnet"
  resource_group_name  = azurerm_resource_group.example.name
  virtual_network_name = azurerm_virtual_network.example.name
  address_prefixes     = ["10.0.2.0/24"]
}

resource "azurerm_network_interface" "example" {
  name                = "example-nic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  ip_configuration {
    name                          = "internal"
    subnet_id                     = azurerm_subnet.example.id
    private_ip_address_allocation = "Dynamic"
  }
}

resource "azurerm_virtual_machine" "example" {
  name                  = "example-machine"
  location              = azurerm_resource_group.example.location
  resource_group_name   = azurerm_resource_group.example.name
  network_interface_ids = [azurerm_network_interface.example.id]
  vm_size               = "Standard_DS1_v2"

  storage_os_disk {
    name              = "example-os-disk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "18.04-LTS"
    version   = "latest"
  }

  os_profile {
    computer_name  = "hostname"
    admin_username = "adminuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = false
  }
}
```

Plan and Apply: You run terraform plan to see what changes Terraform will make, and then terraform apply to create the resources.

## Pros and Cons of Terraform
Pros:

* Multi-Cloud Support: Terraform supports multiple cloud providers, including Azure, AWS, and Google Cloud, allowing you to manage your infrastructure across different platforms with a single tool1.
* Declarative Language: You describe the desired state of your infrastructure, and Terraform handles the rest, ensuring consistency and reducing manual errors2.
* Version Control: Configuration files can be versioned and stored in a repository, making it easy to track changes and collaborate with others2.
* Reusable Modules: You can create reusable modules for common infrastructure patterns, saving time and effort2.

Cons:

* Learning Curve: Terraform has a steep learning curve, especially for those new to IaC concepts3.
* State Management: Terraform uses a state file to keep track of your infrastructure. Managing this state file can be challenging, especially in a team environment3.
* Limited Azure Support: While Terraform supports many Azure resources, some newer services might not be fully supported or require additional configuration2.
* Complexity: For very large and complex infrastructures, Terraform configurations can become difficult to manage and maintain3.