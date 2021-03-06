---
title: Aan de slag met hybride Relay-verbindingen | Microsoft Docs
description: Een knooppuntconsoletoepassing schrijven voor hybride verbindingen
services: service-bus
documentationcenter: node
author: jtaubensee
manager: timlt
editor: 
ms.assetid: e44e4867-3cf3-46be-8f8a-7671e2013bc4
ms.service: service-bus
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: node
ms.workload: na
ms.date: 10/28/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f59faed2edb75e32ea62fa7f1d7013da5f387d8


---
# <a name="get-started-with-relay-hybrid-connections"></a>Aan de slag met hybride Relay-verbindingen
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Wat wordt bereikt
Aangezien hybride verbindingen zowel een client- als een serveronderdeel vereisen, maken we in deze zelfstudie twee consoletoepassingen. Dit zijn de stappen:

1. Een Relay-naamruimte maken met behulp van Azure Portal.
2. Een hybride verbinding maken met behulp van Azure Portal.
3. Een serverconsoletoepassing schrijven om berichten te ontvangen.
4. Een clientconsoletoepassing schrijven om berichten te verzenden.

## <a name="prerequisites"></a>Vereisten
1. [Node.js](https://nodejs.org/en/) (dit voorbeeld gebruikt Node 7.0).
2. Een Azure-abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Een naamruimte maken met de Azure-portal
Als u al een Relay-naamruimte hebt gemaakt, gaat u naar het gedeelte [Een hybride verbinding maken met behulp van Azure Portal](#2-create-a-hybrid-connection-using-the-azure-portal).

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Een hybride verbinding maken met behulp van Azure Portal
Als u al een hybride verbinding hebt gemaakt, gaat u naar het gedeelte [Een servertoepassing maken](#3-create-a-server-application-listener).

[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Een servertoepassing (listener) maken
We maken een Node.js-consoletoepassing om berichten van de Relay te beluisteren en te ontvangen.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Een clienttoepassing maken (afzender)
We maken een Node.js-consoletoepassing om berichten naar de Relay te verzenden.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. De toepassingen uitvoeren
1. Voer de servertoepassing uit.
2. Voer de clienttoepassing uit en voer wat tekst in.
3. Zorg ervoor dat de servertoepassingsconsole de tekst uitvoert die in de clienttoepassing is ingevoerd.

![actieve-toepassingen](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gefeliciteerd, u hebt een end-to-end toepassing met hybride verbindingen gemaakt.

## <a name="next-steps"></a>Volgende stappen:
* [Veelgestelde vragen over Relay](relay-faq.md)
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)




<!--HONumber=Nov16_HO2-->


