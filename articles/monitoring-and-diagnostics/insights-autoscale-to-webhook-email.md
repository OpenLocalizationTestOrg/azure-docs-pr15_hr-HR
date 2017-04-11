<properties
    pageTitle="Automatsko skaliranje akcije koristite za slanje e-pošte i webhook upozorenja. | Microsoft Azure"
    description="Saznajte kako pomoću akcije automatsko skaliranje poziva URL-ovi web ili slanje obavijesti e-poštom u Azure Monitor. "
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="ashwink"/>

# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Korištenje automatsko skaliranje akcija za slanje e-pošte i webhook upozorenja Azure monitora

U ovom se članku prikazuje kako postavite okidača tako da možete nazvati URL-ovi s određenim web ili slati e-poštu koji se temelji na automatsko skaliranje akcije u Azure.  

## <a name="webhooks"></a>Webhooks
Webhooks omogućuju da biste usmjerili Azure upozorenja na drugi sustavi za naknadno obradi ili prilagođeni obavijesti. Ako, na primjer, usmjeravanje upozorenje za servise koje možete učiniti dolazni web zahtjev za slanje SMS, pogreškama zapisnika obavijestiti tim pomoću razgovora ili poruke services, itd. Webhook URI mora biti valjane HTTP ili HTTPS krajnje.

## <a name="email"></a>E-pošte
Bilo koje adrese e-pošte valjani može poslati e-pošte. Administratori i dodatnih administratora pretplate na kojem se pokreće pravilo također ćete primati obavijesti.


## <a name="cloud-services-and-web-apps"></a>Servisi u oblaku i web-aplikacije
Koje možete prijava s portala za Azure za servise u Oblaku i farme poslužitelja (Web Apps).

- Odaberite metriku **Skala po** .

![Promjena veličine po](./media/insights-autoscale-to-webhook-email/insights-autoscale-scale-by.png)

## <a name="virtual-machine-scale-sets"></a>Skupovi skaliranje virtualnog računala
Za novije virtualnim strojevima stvorene pomoću upravitelja resursa (skupovima skaliranje virtualnog računala), možete konfigurirati to pomoću REST API-JA, predlošci Voditelj resursa, PowerShell i EŽA. Portala sučelja još nije dostupna.
Kada pomoću predloška za REST API-JA ili upravitelj resursa, uvrstite element obavijesti pomoću sljedećih mogućnosti.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
|Polje                              |Obavezna? |Opis|
|---                                |---        |---|
|postupak                          |Da        |vrijednost mora biti "Skaliranje"|
|sendToSubscriptionAdministrator    |Da        |vrijednost mora biti "true" ili "false"|
|sendToSubscriptionCoAdministrators |Da        |vrijednost mora biti "true" ili "false"|
|customEmails                       |Da        |vrijednost može biti null [] ili polja niz poruka e-pošte|
|webhooks                           |Da        |vrijednost može biti vrijednost null ili je valjan URI-ja|
|serviceUri                         |Da        |valjani https URI-ja|
|Svojstva                         |Da        |vrijednost mora biti prazna {} ili može sadržavati parovima ključnih vrijednosti|


## <a name="authentication-in-webhooks"></a>Provjera autentičnosti u webhooks
Postoje dva oblika URI provjere autentičnosti:

1. Token Osnovna provjera autentičnosti koje spremate webhook URI tokena ID kao parametra upita. Na primjer, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Osnovna provjera autentičnosti, koju koristite, korisnički ID i lozinku. Na primjer,https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## <a name="autoscale-notification-webhook-payload-schema"></a>Automatsko skaliranje obavijesti webhook tereta sheme
Kada je generiran automatsko skaliranje obavijesti, sljedeće metapodataka je sve obuhvaćeno opseg webhook:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


|Polje  |Obavezna?|    Opis|
|---|---|---|
|status |Da    |Status koji upućuje na to je generirana akciju automatsko skaliranje|
|postupak| Da |Povećava instanci, ona će biti "Skaliranje Out" i za smanjenje u slučajevima, bit će "Skaliranje u"|
|Kontekst|   Da |Automatsko skaliranje kontekstu akcija|
|Vremenska oznaka| Da |Vremenska oznaka kada je pokrenut akciju automatsko skaliranje|
|ID-a |Da|   Voditelj resursa ID postavku automatsko skaliranje|
|ime   |Da|   Naziv postavku automatsko skaliranje|
|pojedinosti|   Da |Objašnjenje akciju koju traje servis za automatsko skaliranje i promjenu broj instanci|
|subscriptionId|    Da |ID pretplate cilj resursa koja je neproporcionalno|
|resourceGroupName| Da|    Grupa resursa ime ciljnog resursa koja je neproporcionalno|
|resourceName   |Da|   Ime ciljnog resursa koja je neproporcionalno|
|resourceType   |Da|   Tri podržane vrijednosti: "microsoft.classiccompute/domainnames/slots/roles" - uloge u Oblaku, "microsoft.compute/virtualmachinescalesets" - skupova skaliranje virtualnog računala i "Microsoft.Web/serverfarms" - Web App|
|resourceId |Da|Voditelj resursa ID ciljne resursa koja je neproporcionalno|
|portalLink |Da    |Azure portala vezu na stranicu sažetak cilj resursa|
|oldCapacity|   Da |Trenutni (stari) instance broj kada automatsko skaliranje snimljene skaliranje akcija|
|newCapacity|   Da |Novi broj instanci koje automatsko skaliranje skalirana resurs|
|Svojstva|    ne| Neobavezno. Skup < ključ, vrijednost > parove (npr. rječnika < niza, niz >). Svojstva polja nije obavezno. Korisnička sučelja ili logike aplikacije koje se temelje tijeka, možete unijeti ključeva i vrijednosti koje se mogu proslijediti pomoću opseg. Drugi način za prosljeđivanje prilagođena svojstva natrag odlazni webhook poziv je za korištenje webhook URI-JA sam (kao što je parametara upita)|
