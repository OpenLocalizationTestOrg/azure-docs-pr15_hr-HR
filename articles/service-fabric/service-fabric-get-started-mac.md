<properties
   pageTitle="Postavljanje okruženja za razvoj operacijskom sustavu Mac OS X | Microsoft Azure"
   description="Instalirajte izvođenja, SDK i alate i stvaranje lokalne razvoj klaster. Nakon dovršetka ovaj će instalacijski program, bit će spremna za izgradnju aplikacija u sustavu Mac OS X."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Postavljanje okruženja za razvoj operacijskom sustavu Mac OS X

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Možete izrađivati aplikacije servisa tkanina pokrenuti Linux klastere pomoću Mac OS X. U ovom se članku opisuje kako postaviti Mac za razvoj.

## <a name="prerequisites"></a>Preduvjeti

Servis tkanina ne pokreće se nativno OS X. Da biste pokrenuli lokalnog servisa tkanina klaster, dajemo je unaprijed konfigurirana Ubuntu virtualnog računala pomoću Vagrant i VirtualBox. Prije nego što počnete, potrebno je:

- [Vagrant (v1.8.4 ili noviji)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Stvaranje lokalne VM

Da biste stvorili lokalne VM koja sadrži klaster 5 čvor tkanina servisa, učinite sljedeće:

1. Kloniraj Vagrantfile repo

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Idite na lokalni Kloniraj od na repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Neobavezno) Izmjena zadanih postavki VM

    Prema zadanim postavkama lokalni VM je konfiguriran na sljedeći način:

    - 3 GB memorije
    - Glavno računalo za privatne mreže konfiguraciji IP 192.168.50.50 Omogućivanje passthrough prometa na glavnom računalu za Mac

    Možete promijeniti neke od ovih postavki ili dodati druge konfiguracije VM u na Vagrantfile. Potražite u [dokumentaciji Vagrant](http://www.vagrantup.com/docs) cijeli popis mogućnosti konfiguracije.

4. Stvaranje na VM

    ```bash
    vagrant up
    ```

    Ovaj korak preuzima konfiguriranog VM slika, pokretanje it lokalno, a zatim postavite lokalnog servisa tkanina skupine u njoj. Možete očekivati da će potrajati nekoliko minuta. Ako instalacijski program uspješno završi, prikazat će se poruka u kojoj stoji da se pokreće klaster izlaz.

    ![Skupine postavljanje pokretanja praćenja VM dodjele resursa][cluster-setup-script]

5. Testiranje klaster je postavljen ispravno tako da odete servisa tkanina Explorer pri http://192.168.50.50:19080/Explorer (pod pretpostavkom zadržane IP privatne mreže zadani).

    ![Servis tkanina Explorer pregledavati s računala Mac][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Instalirajte servis tkanina za Eclipse Neon (nije obavezno)

Servis tkanina nudi dodatak za IDE Neon Eclipse koji može pojednostavniti postupak stvaranja i Implementacija servisa Java.

1. Eclipse, provjerite je li u imate Buildship verziju 1.0.17 ili noviju verziju. Verzija instalirane komponente možete provjeriti tako da odaberete **Pomoć > Detalji o instalaciji**. Možete ažurirati Buildship prema uputama [u nastavku][buildship-update].

2. Da biste instalirali dodatak tkanina servisa, odaberite **Pomoć > instalacija novi softver...**

3. U tekstni okvir "Rad s" unesite: http://dl.windowsazure.com/eclipse/servicefabric.

4. Kliknite Dodaj.

    ![Dodatak za Neon eclipse za servis tkanina][sf-eclipse-plugin-install]

5. Odaberite dodatak za servis tkanina, a zatim kliknite Dalje.

6. Nastavite postupak instalacije i prihvatite licencni ugovor za krajnjeg korisnika.

## <a name="next-steps"></a>Daljnji koraci

- [Stvaranje prve tkanina servisa aplikacije za Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Stvaranje klaster tkanina servisa na portalu za Azure](service-fabric-cluster-creation-via-portal.md)
- [Stvaranje servisa tkanina klaster pomoću upravitelja resursa za Azure](service-fabric-cluster-creation-via-arm.md)
- [Objašnjenje modela servisa tkanina aplikacije](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
