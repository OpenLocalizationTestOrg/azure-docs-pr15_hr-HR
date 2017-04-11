<properties 
    pageTitle="Kako instalirati Apache Qpid Proton-C na Linux VM | Microsoft Azure"
    description="Kako stvoriti VM Linux CentOS, pomoću virtualnim računalima sustava Azure i kako stvoriti i instalirati Apache Qpid Proton-C biblioteke."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Kliknite pločicu Apache Qpid Proton-C Azure VM za Linux

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

U ovom se odjeljku prikazuje kako stvoriti VM Linux CentOS, pomoću virtualnim računalima sustava Azure i upute za preuzimanje, stvaranje i instalirati biblioteku Apache Qpid Proton-C uz jezik povezivanja Python i PHP. Nakon tih koraka, moći pokrenuti Python i i uzorke uključene ovaj vodič.

Prvi korak izvodi se [Azure klasični portal][]. Na sljedećoj je snimci zaslona prikazano kako se stvara CentOS VM pod nazivom "luka centos":

![Proton na Azure VM za Linux][0]

Nakon dodjele resursa, na portalu prikazuje sljedeće:

![Proton na Azure VM za Linux][1]

Da bi se prijavite na računalo, morate znati priključak krajnja točka za SSH. Tu vrijednost s [Azure klasični portal][] možete dobiti odabirom novostvorenu VM i klikom na kartici **krajnje točke** . Na sljedećoj je snimci zaslona pokazuje da je javno SSH priključak računala 57146.

![Proton na Azure VM za Linux][2]

Sada se možete povezati s računalom pomoću SSH. U ovom se primjeru koristi alat za PuTTY, kao na sljedećem zaslonu snimka:

![Proton na Azure VM za Linux][3]

Za aplikacije Python i i u ovom se primjeru koristi biblioteka klijentski Proton s Apache. Te biblioteke dostupnih za preuzimanje [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Datoteke Readme u paketu za raspodjelu objašnjava korake potrebne za instalaciju ovisnosti i sastavljanje Proton. Ovo je sažetak koraka:

1.  Uređivanje konfiguracijskoj datoteci yum (/ etc/yum.conf) i komentar izvan izuzetaka ima li ažuriranja za otklanjanje zaglavlja (\# isključi = otklanjanje\*). To je potrebno za instaliranje alata za Kompiliranje gcc.

2.  Instalirajte pripremni paketa:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Preuzimanje biblioteke Proton:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Izdvajanje kod Proton iz arhive raspodjele:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Stvaranje i instalirati na sljedeći način, PDF datoteke Readme kod:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Nakon tih koraka Proton je instaliran na računalu i jeste li spremni za korištenje.

## <a name="next-steps"></a>Daljnji koraci

Jeste li spremni za dodatne informacije? Posjetite sljedeću vezu:

- [Pregled servisa Bus AMQP][]

[Pregled servisa Bus AMQP]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure klasični portal]: http://manage.windowsazure.com


