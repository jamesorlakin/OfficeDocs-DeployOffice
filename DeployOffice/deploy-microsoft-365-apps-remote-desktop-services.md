---
title: "Deploy Microsoft 365 Apps by using Remote Desktop Services"
ms.author: danbrown
author: DHB-MSFT
manager: laurawi
audience: ITPro
ms.topic: get-started-article
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Ent_Office_ProPlus
description: "If you use Remote Desktop Services (RDS) to provide shared computers to users in your organization, you can install Microsoft 365 Apps on those computers. But, you have to use the Office Deployment Tool and enable shared computer activation to do the installation."
---

# Deploy Microsoft 365 Apps by using Remote Desktop Services

> [!IMPORTANT]
> Office 365 ProPlus is being renamed to **Microsoft 365 Apps for enterprise**, starting with Version 2004. To learn more, [read this article](name-change.md). In our documentation, we'll usually just refer to it as Microsoft 365 Apps.

If you use Remote Desktop Services (RDS) to provide shared computers to users in your organization, you can install Microsoft 365 Apps for enterprise on those computers. But, you have to use the Office Deployment Tool and enable [shared computer activation](overview-shared-computer-activation.md) to do the installation.

The following are two common RDS scenarios:

- Install Microsoft 365 Apps for enterprise on an RDS server.

- Install Microsoft 365 Apps for enterprise on a shared virtual machine.

## What you need to get started
<a name="Started"> </a>

The following is a list of prerequisites that you need to deploy Microsoft 365 Apps for enterprise with RDS:

- An Office 365 (or Microsoft 365) plan that includes Microsoft 365 Apps for enterprise. Also, make sure that you [assign each user a license](https://support.office.com/article/997596b5-4173-4627-b915-36abac6786dc) for Microsoft 365 Apps for enterprise.

    > [!NOTE]
    > You also can use RDS to deploy the subscription versions of the Project and Visio desktop apps, if you have a subscription plan that includes those products. 

- The Office Deployment Tool, which is available on the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?LinkID=626065). You can download the Microsoft 365 Apps for enterprise software to your local network by using the [Office Deployment Tool](overview-office-deployment-tool.md).

- Any supported version of Microsoft 365 Apps for enterprise.

- A supported version of RDS or Windows with Microsoft 365 Apps for enterprise, which includes any of the following:

  - Windows 8.1
  - Windows Server 2016
  - Currently supported Windows 10 SAC release

- Reliable connectivity between the shared computer and the internet.

    This is required because the shared computer must be able to contact the Office Licensing Service on the internet to obtain a license for each user who uses Microsoft 365 Apps for enterprise on the computer and then activate Microsoft 365 Apps for enterprise. Internet connectivity is also needed to renew the license, which occurs every few days.

- A separate user account for each user who logs on to the shared computer.

- A server that supports [Hyper-V](https://go.microsoft.com/fwlink/p/?LinkId=510585) if you're deploying Microsoft 365 Apps for enterprise on a shared virtual machine.

## Install Microsoft 365 Apps on an RDS server
<a name="Server"> </a>

In this scenario, you install Microsoft 365 Apps for enterprise on a computer configured as a Remote Desktop Session Host server. This enables multiple users to connect remotely to this computer. The users can each run Office programs, such as Word or Excel, at the same time.

Here are the basic steps of how to install Microsoft 365 Apps for enterprise on an RDS server:

1. Install and configure Windows Server.

2. Install and configure the Remote Desktop Session Host role service.

    For example, [follow these steps to install RD Session Host](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure) on Windows Server.

    For users to be able to connect remotely to the server to use Microsoft 365 Apps for enterprise, their accounts must be members of the Remote Desktop Users group on the RD Session Host server.

3. [Create a configuration.xml file](office-deployment-tool-configuration-options.md) that includes the following lines:

   ```xml
   <Display Level="None" AcceptEULA="True" /> 
   <Property Name="SharedComputerLicensing" Value="1" />
   ```

    You set the display level to "None" to do a silent installation of Microsoft 365 Apps for enterprise. This prevents Microsoft 365 Apps for enterprise from trying to activate during the installation. This also means that you won't see any user interface elements during the installation, such as the progress of the installation or error messages.

    You use the SharedComputerLicensing setting to enable [shared computer activation](overview-shared-computer-activation.md), which is required to use Microsoft 365 Apps for enterprise on a shared computer.

4. Use the [Office Deployment Tool](overview-office-deployment-tool.md) and the configuration.xml file to install Microsoft 365 Apps for enterprise on the RD Session Host server.

At this point, users can connect to the RD Session Host server and use Microsoft 365 Apps for enterprise. Users can connect to the server by using Remote Desktop Connection, which is available in Windows, or by using other [Remote Desktop clients](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

## Install Microsoft 365 Apps on a shared virtual machine
<a name="VM"> </a>

In this scenario, you install Microsoft 365 Apps for enterprise as part of a client operating system image, such as one running Windows 8.1 or Windows 10. Then, you use RDS and Hyper-V to create a group of virtual machines based on that image. These virtual machines can be shared by multiple users. In RDS, this is known as either a virtual desktop pool or a pooled virtual desktop collection, depending on which version of RDS that you're using.

> [!NOTE]
> You can also use RDS to assign a virtual machine to a specific user. RDS calls that a personal virtual desktop. In that scenario, you don't use shared computer activation, because the virtual machine isn't shared among multiple users. 

Here are the basic steps of how to configure RDS to deploy Microsoft 365 Apps for enterprise on a shared virtual machine:

1. Create the operating system image:

   - Follow the instructions to [Deploy Microsoft 365 Apps as part of an operating system image](deploy-microsoft-365-apps-operating-system-image.md). In Step 2 of the instructions, make sure that your configuration.xml file also includes the following line to enable shared computer activation:

   ```xml
   <Property Name="SharedComputerLicensing" Value="1" />
   ```

   - You also need to [make some RDS-specific changes on the virtual machine](https://go.microsoft.com/fwlink/p/?LinkId=510584), such as enabling Remote Desktop.

2. Install and configure Windows Server.

3. Install and configure RDS.

    For example, [follow these steps to deploy a virtual desktop collection](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-create-collection) on Windows Server.

After you've completed all the RDS configuration steps, users can connect to any of the virtual machines and run Microsoft 365 Apps for enterprise.

## Related topics
<a name="VM"> </a>

[Overview of shared computer activation for Microsoft 365 Apps](overview-shared-computer-activation.md)

[Troubleshoot issues with shared computer activation for Microsoft 365 Apps](troubleshoot-shared-computer-activation.md)

[Remote Desktop Services](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/welcome-to-rds)
