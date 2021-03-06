---
title: Een Azure Container Service-cluster implementeren | Microsoft Docs
description: Een Azure Container Service-cluster implementeren met behulp van Azure Portal, de Azure CLI of PowerShell.
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure
ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c8c06906a5f99890295ff2b2433ff6f7e02dece5


---
# <a name="deploy-an-azure-container-service-cluster"></a>Een Azure Container Service-cluster implementeren
Azure Container Service biedt een snelle implementatie van populaire open-sourceoplossingen voor containerclustering en -orchestration. Met behulp van Azure Container Service kunt u DC/OS- en Docker Swarm-clusters implementeren met Azure Resource Manager-sjablonen of Azure Portal. U implementeert deze clusters met behulp van Azure Virtual Machine-schaalsets, en de clusters profiteren van het Azure-aanbod voor netwerken en opslag. U hebt een Azure-abonnement nodig om toegang te krijgen tot Azure Container Service. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

In dit document vindt u instructies voor het implementeren van een Azure Container Service-cluster met behulp van [Azure Portal](#creating-a-service-using-the-azure-portal), de [Azure-opdrachtregelinterface (CLI)](#creating-a-service-using-the-azure-cli) en de [Azure PowerShell-module](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Een service maken met Azure Portal
Meld u aan bij Azure Portal, selecteer **New** (Nieuw) en zoek in Azure Marketplace naar **Azure Container Service**.

![Implementatie maken - 1](media/acs-portal1.png)  <br />

Selecteer **Azure Container Service** en klik op **Create** (Maken).

![Implementatie maken - 2](media/acs-portal2.png)  <br />

Voer de volgende informatie in:

* **User name** (Gebruikersnaam): dit is de gebruikersnaam die wordt gebruikt voor een account op elk van de virtuele machines en virtuele-machineschaalsets in het Azure Container Service-cluster.
* **Subscription** (Abonnement): selecteer een Azure-abonnement.
* **Resource group** (Resourcegroep): selecteer een bestaande resourcegroep of maak een nieuwe.
* **Locatie**: selecteer een Azure-regio voor de Azure Container Service-implementatie.
* **SSH public key** (Openbare SSH-sleutel): voeg de openbare sleutel toe die wordt gebruikt voor verificatie bij de virtuele machines voor Azure Container Service. Het is heel belangrijk dat deze sleutel geen regeleinden bevat, maar wel de prefix 'ssh-rsa' en de postfix 'username@domain'. Het moet er ongeveer zo uitzien: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. Zie de artikelen voor [Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) en [Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) voor informatie over het maken van SSH-sleutels.

Klik op **OK** wanneer u klaar bent om door te gaan.

![Implementatie maken - 3](media/acs-portal3.png)  <br />

Selecteer een Orchestration-type. De opties zijn:

* **DC/OS**: implementeert een DC/OS-cluster.
* **Swarm**: implementeert een Docker Swarm-cluster.

Klik op **OK** wanneer u klaar bent om door te gaan.

![Implementatie maken - 4](media/acs-portal4.png)  <br />

Voer de volgende informatie in:

* **Master count** (Aantal masters): het aantal masters in het cluster.
* **Agent count** (Aantal agents): voor Docker Swarm is dit het oorspronkelijke aantal agents in de agentschaalset. Voor DC/OS is dit het oorspronkelijke aantal agents in een persoonlijke schaalset. Bovendien wordt een openbare schaalset gemaakt, die een vooraf bepaald aantal agents bevat. Het aantal agents in deze openbare schaalset wordt bepaald door het aantal masters dat in het cluster is gemaakt: één openbare agent voor één master en twee openbare agents voor drie of vijf masters.
* **Agent virtual machine size** (Grootte van virtuele machines van agent): de grootte van de virtuele machines van de agent.
* **DNS prefix** (DNS-voorvoegsel): een wereldwijd unieke naam die wordt gebruikt om sleutelonderdelen toe te voegen voor de volledig gekwalificeerde domeinnamen voor de service.

Klik op **OK** wanneer u klaar bent om door te gaan.

![Implementatie maken - 5](media/acs-portal5.png)  <br />

Klik op **OK** nadat de servicevalidatie is voltooid.

![Implementatie maken - 6](media/acs-portal6.png)  <br />

Klik op **Create** (Maken) om het implementatieproces te starten.

![Implementatie maken - 7](media/acs-portal7.png)  <br />

Als u ervoor gekozen hebt om de implementatie aan Azure Portal vast te maken, kunt u de status van de implementatie zien.

![Implementatie maken - 8](media/acs-portal8.png)  <br />

Wanneer de implementatie is voltooid, is het Azure Container Service-cluster klaar voor gebruik.

## <a name="create-a-service-by-using-the-azure-cli"></a>Een service maken met behulp van de Azure CLI
Wanneer u een Azure Container Service-exemplaar wilt maken met behulp van de opdrachtregel, hebt u een Azure-abonnement nodig. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefabonnement](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Ook moet de Azure CLI [geïnstalleerd](../xplat-cli-install.md) en [geconfigureerd](../xplat-cli-connect.md) zijn.

Selecteer een van de volgende sjablonen van GitHub wanneer u een DC/OS- of Docker Swarm-cluster wilt implementeren. Deze sjablonen zijn hetzelfde, behalve de standaardselectie voor orchestrator.

* [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Controleer vervolgens of de Azure CLI is verbonden met een Azure-abonnement. U gebruikt hiervoor de volgende opdracht:

```bash
azure account show
```
Als er geen Azure-account wordt geretourneerd, gebruikt u de volgende opdracht om de CLI aan te melden bij Azure.

```bash
azure login -u user@domain.com
```

Hierna configureert u de Azure CLI-hulpprogramma's voor het gebruik van Azure Resource Manager.

```bash
azure config mode arm
```

Maak een Azure-resourcegroep- en Container Service-cluster met de volgende opdracht, waarbij:

* **RESOURCE_GROUP** de naam is van de resourcegroep die u wilt gebruiken voor deze service.
* **LOCATION** de Azure-regio is waar de resourcegroep en Azure Container Service-implementatie wordt gemaakt.
* **TEMPLATE_URI** de locatie is van het implementatiebestand. Dit moet het Raw-bestand zijn, niet een verwijzing naar de GitHub-gebruikersinterface. U vindt deze URL door het bestand azuredeploy.json te selecteren in GitHub en op de knop **Raw** te klikken.

> [!NOTE]
> Wanneer u deze opdracht uitvoert, vraagt de shell u parameterwaarden op te geven voor de implementatie.
> 
> 

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sjabloonparameters opgeven
Voor deze versie van de opdracht moet u parameters interactief definiëren. Als u parameters wilt opgeven, zoals een tekenreeks in JSON-indeling, kunt u dit doen met behulp van de `-p`-switch. Bijvoorbeeld:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

U kunt ook parameters in JSON-indeling opgeven met behulp van de `-e`-switch:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

U kunt een voorbeeldparameterbestand met de naam `azuredeploy.parameters.json` bekijken door dit in GitHub op te zoeken met de Azure Container Service-sjablonen.

## <a name="create-a-service-by-using-powershell"></a>Een service maken met PowerShell
U kunt een Azure Container Service-cluster ook implementeren met PowerShell. Dit document is gebaseerd op versie 1.0 van de [Azure PowerShell-module](https://azure.microsoft.com/blog/azps-1-0/).

Selecteer een van de volgende sjablonen wanneer u een DC/OS- of Docker Swarm-cluster wilt implementeren. Deze sjablonen zijn hetzelfde, behalve de standaardselectie voor orchestrator.

* [DC/OS-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Controleer voordat u een cluster maakt in uw Azure-abonnement, of uw PowerShell-sessie is aangemeld bij Azure. U kunt dit doen met de opdracht `Get-AzureRMSubscription`:

```powershell
Get-AzureRmSubscription
```

Als u zich bij Azure wilt aanmelden, gebruikt u de opdracht `Login-AzureRMAccount`:

```powershell
Login-AzureRmAccount
```

Als u naar een nieuwe resourcegroep implementeert, moet u eerst de resourcegroep maken. Voor het maken van een nieuwe resourcegroep gebruikt u de opdracht `New-AzureRmResourceGroup` en geeft u een resourcegroep en bestemmingsregio op:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nadat u een resourcegroep hebt gemaakt, kunt u uw cluster maken met de volgende opdracht. De URI van de gewenste sjabloon wordt opgegeven voor de `-TemplateUri`-parameter. Wanneer u deze opdracht uitvoert, vraagt PowerShell u de parameterwaarden voor de implementatie op te geven.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Sjabloonparameters opgeven
Als u bekend bent met PowerShell, weet u dat u door de beschikbare parameters voor een cmdlet kunt lopen door een minteken (-) te typen en dan op de tabtoets te drukken. Dezelfde functionaliteit werkt ook met parameters die u in de sjabloon definieert. Zodra u de sjabloonnaam gaat typen, haalt de cmdlet de sjabloon op, parseert de parameters en voegt de sjabloonparameters dynamisch aan de opdracht toe. Dit maakt het heel eenvoudig om sjabloonparameterwaarden op te geven. En als u de waarde van een vereiste parameter bent vergeten, vraagt PowerShell u om de waarde.

Hieronder vindt u de volledige opdracht, inclusief parameters. U kunt uw eigen waarden opgeven voor de namen van de resources.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Volgende stappen
Nu u een werkend cluster hebt, kunt u deze documenten lezen voor meer informatie over verbinding en beheer:

* [Verbinding maken met een Azure Container Service-cluster](container-service-connect.md)
* [Werken met de Azure Container Service en DC/OS](container-service-mesos-marathon-rest.md)
* [Werken met de Azure Container Service en Docker Swarm](container-service-docker-swarm.md)




<!--HONumber=Nov16_HO2-->


