<properties
    pageTitle="Primjena pravila na resursima virtualnim računalima sustava Azure | Microsoft Azure"
    description="Upute za primjenu pravila na programa Azure resursima Windows virtualnog računala"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Primjena pravila na resursima virtualnim računalima sustava Azure

Pomoću pravila tvrtki ili ustanovi možete nametnuti različite konvencije i pravila u tvrtki. Provođenje željeni ponašanje može pridonijeti prevladavanje rizika prilikom pojačava uspješnosti tvrtke ili ustanove. U ovom se članku smo će opisuju kako možete koristiti pravila upravljanja resursima Azure za definiranje željeno ponašanje za virtualnih računala u vašoj tvrtki ili ustanovi.

Struktura korake za obavljanje to je kao ispod

1. Azure pravilnik o resursima 101
2. Definiranje pravila za virtualnog računala
3. Stvaranje pravila
4. Primjena pravila

## <a name="azure-resource-manager-policy-101"></a>Azure pravilnik o resursima 101

Za početak rada s pravilima Azure Voditelj resursa, preporučujemo da u članku za čitanje, a zatim nastavite s koracima iz ovog članka. U članku opisuju osnovni definicija i strukturu pravilo, kako se procjenjuje pravila i nudi razne Primjeri pravila definicije.

* [Pomoću pravila za upravljanje resursima i nadzor pristupa](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definiranje pravila za virtualnog računala

Možda nešto uobičajeni scenariji za tvrtku samo korisnicima njihove da biste stvorili virtualnim strojevima iz određene operacijski sustavi testirate nisu kompatibilne s LOB aplikacije. Korištenje pravilo Voditelj resursa Azure ovaj zadatak možete obaviti u nekoliko koraka. U ovom primjeru pravila smo ćete dopustiti samo Windows Server 2012 R2 podatkovnog centra virtualnim strojevima će biti stvoren. Definicija pravila izgleda ispod

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "MicrosoftWindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "WindowsServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "2012-R2-Datacenter"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Iznad pravilnika moguće je izmijeniti jednostavno scenarij u kojem možda želite dopustiti bilo koju sliku podatkovnog centra sustava Windows Server koja će se koristiti za implementaciju virtualnog računala s u nastavku promjena

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*Datacenter"
}
```

#### <a name="virtual-machine-property-fields"></a>Polja svojstava virtualnog računala

U tablici u nastavku opisuju svojstva virtualnog računala koje je moguće koristiti kao polja u definicije pravila. Dodatne informacije o pravilima polja, potražite u članku:

* [Polja i izvora](../resource-manager-policy.md#fields-and-sources)


| Naziv polja     | Opis                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Određuje programa publisher slike               |
| imageOffer     | Određuje ponuda za publisher odabranu sliku |
| imageSku       | Određuje SKU-om za odabrani ponudu             |
| imageVersion   | Određuje verziju slike za odabrali SKU     |

## <a name="create-the-policy"></a>Stvaranje pravila

Pravilo se stvoriti jednostavno pomoću REST API-JA izravno ili cmdleta ljuske PowerShell. Stvaranje pravila, potražite u članku ispod:

* [Stvaranje pravila](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Primjena pravila

Nakon stvaranja pravila morat ćete primijeniti na definirani opseg. Opseg može biti pretplatu, grupu resursa ili čak i resursa. Primjena pravila, potražite u članku ispod:

* [Stvaranje pravila](../resource-manager-policy.md#applying-a-policy)
