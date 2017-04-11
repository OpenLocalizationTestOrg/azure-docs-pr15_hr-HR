<properties 
pageTitle="Oblak ulogu Services config XPath oglasi lista | Microsoft Azure" 
description="Različite postavke XPath možete koristiti u config za uloge servisa oblaka da bi se prikazale postavke kao varijabla okruženja." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Ponudili uloga konfiguracijske postavke kao varijabla okruženja s XPath

U oblak servisa tempiranja ili datoteka za definiciju web uloge servisa, možete izložiti runtime konfiguracijskih vrijednosti kao varijable okruženja. Sljedeće vrijednosti XPath podržani su (koji odgovaraju vrijednosti API-JA).

Ove XPath vrijednosti su dostupni putem [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) biblioteke. 

## <a name="app-running-in-emulator"></a>Aplikacije koje se izvodi u emulator

Upućuje na to da aplikacije nije ostalo na emulator.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Kod  | VAR x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>ID implementacije

Dohvaća ID za uvođenje za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Kod  | VAR deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>ID uloga 

Dohvaća trenutni ID uloga za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kod  | VAR id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Ažuriranje domene

Dohvaća ažuriranje domene instance.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kod  | VAR ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Kvara domene

Dohvaća domene kvara instance.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kod  | VAR fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Naziv uloge

Dohvaća uloga naziv instance.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kod  | VAR rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Konfiguracija postavki

Dohvaća vrijednost navedena konfiguracijska postavke.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Kod  | Postavka VAR = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Lokalno spremište put

Dohvaća put lokalno spremište za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kod  | VAR localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Lokalno spremište veličina

Dohvaća veličina lokalno spremište za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kod  | VAR localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Protokol za krajnje točke 

Dohvaća protokol krajnja točka za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kod  | VAR prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokol; |

## <a name="endpoint-ip"></a>Krajnja točka IP

Dohvaća navedene krajnje IP adresa.

| Vrsta | Primjer |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kod  | VAR adresa = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Priključak krajnje točke 

Dohvaća priključak krajnja točka za instancu.

| Vrsta  | Primjer |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kod  | VAR priključak = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Primjer

Evo primjera ulogu suradnika koji stvara zadatak pokretanja s varijabla okruženja pod nazivom `TestIsEmulated` postavite na [ @emulated xpath vrijednost](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o datoteci [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) .

Stvaranje paketa [ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) .

Omogućivanje [udaljene radne površine](cloud-services-role-enable-remote-desktop.md) za uloge.
