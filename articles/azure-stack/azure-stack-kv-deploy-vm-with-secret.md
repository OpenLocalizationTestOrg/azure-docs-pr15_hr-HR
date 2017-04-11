<properties
    pageTitle="Implementacija VM pomoću lozinke pohranjene u Azure stogu ključ sigurnog | Microsoft Azure"
    description="Saznajte kako implementirati VM pomoću lozinke pohranjene u Azure stogu ključ sigurnog"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Implementacija u VM preuzimanjem lozinku spremljenu u sigurnog ključ

Kada se morate proći sigurne vrijednost (kao što je lozinka) kao parametar tijekom implementacije, možete spremiti tu vrijednost kao tajna u sigurnog ključa u stogu Azure i referencirati vrijednosti u drugim predlošcima Voditelj resursa Azure. Samo referenca na tajna uključiti u predložak tako da nikada ne prikazuje na tajna. Ne morate ručno unesite vrijednost za na tajna svaki put implementacija resurse. Odredite koji korisnici ili upravitelji servisa možete pristupiti na tajna.

## <a name="reference-a-secret-with-static-id"></a>Referenca tajna statične ID

Referenca tajna iz unutar parametara datoteke koje prosljeđuje vrijednosti u predložak. Referenca na tajna prosljeđivanjem identifikator resursa ključa sigurnog i naziv u tajna. U ovom primjeru tajna ključa sigurnog mora postojati. Koristite statična vrijednost za njegov ID resursa.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Parametar prihvaća na tajna mora biti *securestring*.

## <a name="next-steps"></a>Daljnji koraci
[Implementacija aplikacije uzorka s sigurnog ključ](azure-stack-kv-sample-app.md)

[Implementacija VM certifikatom sigurnog ključ](azure-stack-kv-push-secret-into-vm.md)

