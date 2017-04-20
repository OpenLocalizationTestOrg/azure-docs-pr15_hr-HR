<properties
   pageTitle="SAP NetWeaver na Linux virtualnim strojevima (VMs) – vodič za implementaciju DBMS | Microsoft Azure"
   description="SAP NetWeaver na Linux virtualnim strojevima (VMs) – vodič za implementaciju DBMS"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver na Azure virtualnim strojevima (VMs) – vodič za implementaciju DBMS

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver na Linux virtualnim strojevima (VMs) – vodič za implementaciju DBMS) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Međuspremanje za VMs i VHDs) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (softver RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure spremište) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (strukturu RDBMS implementacije) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (visoke dostupnosti i oporavak Izrada s Azure VMs) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 i novijim verzijama) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 i prethodnih izdanja) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (pomoću SQL Server slike iz trgovine Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Općenito SQL Server za SAP-a na sažetak Azure) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (specifičnosti za SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (prostora za pohranu konfiguracije) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (sigurnosnog kopiranja i vraćanja) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (performanse zahtjevi za sigurnosno kopiranje i vraćanje) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (ostalo) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver na Linux virtualnim strojevima (VMs) – vodič za implementaciju) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resursi) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (implementacija VM prilagođene slikom) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (scenarij 1: implementacija VM iz trgovine Azure za SAP-a) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (scenarij 2: implementacija VM prilagođene slikom za SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Scenarij 3: Premještanje na VM iz lokalnog VHD Azure za koje nisu GRG generalizirano pomoću SAP-a) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (implementacije scenariji od VMs za SAP-a na Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (implementacija Azure PowerShell cmdleti) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Preuzimanje i uvoz SAP odgovarajući cmdleta ljuske PowerShell) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (uključivanje VM u lokalne domene – samo u sustavu Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [implementacije – vodič za-4.4]: Virtual-machines-Linux-SAP-Deployment-Guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (preuzimanje, instaliranje i Omogući Azure VM Agent) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure EŽA) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfiguriranje Azure Poboljšana nadzor proširenja za SAP-a) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (spremnosti potražiti Azure Poboljšana nadzor za SAP-a) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Provjera stanja za Azure Nadzor Infrastruktura konfiguracije) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (daljnje rješavanje problema Azure nadzor infrastrukture za SAP-a)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver na Linux virtualnim strojevima (VMs) – vodič za implementaciju i planiranje) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resursi) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (visoke dostupnosti (HA) i Izrada oporavak (DR) za SAP NetWeaver koji se izvodi na Azure virtualnim strojevima) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (visoke dostupnosti za poslužitelji aplikacija SAP-a) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (pomoću automatsko pokrenite instanci SAP-a) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (samo za oblak - implementacijama virtualnog računala u Azure bez međuzavisnosti lokalne mreže klijenta) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (unakrsno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalne mreže) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regije) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (kvara domena) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Nadogradnja domena) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure dostupnost skupovima) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (u pojam Microsoft Azure virtualnog računala) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium spremište) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Premještanje na VM s lokalnim Azure s diskom koji nisu GRG generalizirano) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (implementacija VM sa slikom za određene korisnike) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Priprema za premještanje na VM lokalnim Azure s diskom koji nisu GRG generalizirano) [ planning-Guide-5.2.2]:Virtual-machines-Linux-SAP-planning-Guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Priprema za implementaciju VM sa slikom za određene korisnike za SAP-a) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Priprema VMs s SAP-a za Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (razlika između programa Azure na disku i slika Azure) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (prijenos VHD lokalnim za Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopiranje diskova između računa za pohranu za Azure) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD strukturu implementacijama SAP-a) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (postavka automount za priložene diskova) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (jedan VM s NetWeaver SAP pokazni videozapis/obuka scenarij) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (koncepata Cloud-Only implementacija instanci SAP-a) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure nadzor rješenje za SAP-a) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium prostora za pohranu)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog modela.

Ovaj je vodič namijenjen dio u dokumentaciji o implementacijom i implementacija softvera SAP-a na Microsoft Azure. Prije nego što ovaj vodič za čitanje, pročitajte u [planiranje i vodič za implementaciju sustava] [– vodič za planiranje]. Ovaj dokument pokriva implementacije različite relacijske baze podataka upravljanja sustavi (RDBMS) i povezane proizvode u kombinaciji s SAP-a na Microsoft Azure virtualnim strojevima (VMs) pomoću infrastruktura za Azure kao mogućnosti usluge (IaaS).

Dopuna papira dokumentaciju o instalaciji SAP i bilješke SAP-a koji predstavljaju primarni resursi za instalacijama i implementacijama SAP softvera na navedeni platforme

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Općenita razmatranja
U ovom poglavlja pitanja vezana uz izvršenja SAP povezan DBMS sustavi u Azure VMs se kroz njih predstavljaju. Postoji nekoliko reference na određene DBMS sustavi u Ovo poglavlje. Umjesto toga određene sustavi DBMS sinkronizacijom unutar papira, nakon ovog poglavlja.

### <a name="definitions-upfront"></a>Definicija upfront
Cijelom dokumentu koristit ćemo sljedeće uvjete:

* IaaS: Infrastruktura kao usluga.
* PaaS: Platforme kao usluga.
* SaaS: Softver kao usluga.
* SAP komponente: pojedinačne SAP programa kao što su ECC, BW, Upravitelj rješenja ili EP.  Komponente SAP-a mogu se temeljiti na tradicionalni ABAP ili Java tehnologije ili aplikacije koje nisu-NetWeaver temelji kao što su poslovnih objekata.
* SAP okruženja: neke komponente SAP logički grupirane da biste izvršili Poslovna funkcija, kao što su razvoj, QAS, obuka, DR ili radnog.
* Vodoravno SAP: Odnosi se na cijelu resursima SAP-a u klijentu IT vodoravno. Vodoravno SAP uključuje sve proizvodnje i koje nisu radnog okruženja.
* Sustav SAP: Kombinacije DBMS sloja i aplikacijskom sloju – primjerice programa SAP ERP razvoj sustava SAP BW testiranje sustava, SAP CRM operativnom sustavu, itd. U implementacijama Azure nije podržan podjele te dvije slojeve između lokalnog i Azure. To znači upit sustava SAP-a ili je implementiran na lokalni ili ga je uveden u Azure. Možete uvesti i druge sustave programa SAP pejzažno Azure ili na lokalnim poslužiteljima. Na primjer, možete implementirati razvoj SAP CRM i testirali sustavi u Azure, ali u SAP CRM radnog sustava lokalnog.
* Samo oblak implementacije: implementacija gdje Azure pretplate je povezan putem web-mjesto ili ExpressRoute vezu s lokalnim mrežne infrastrukture. Zajednički Azure dokumentaciju ove vrste implementacijama su i što je opisano kao "Samo za oblaka" implementacije. Virtualnim strojevima implementiran ako koristite taj način je moguće pristupiti putem Interneta i javne krajnje točke Internet koje su dodijeljene VMs u Azure. S lokalnim servisom Active Directory (AD) i DNS-a nije proširena za Azure u tim vrstama implementacije. Dakle u VMs nisu dio lokalnog servisa Active Directory. Napomena: Samo oblak implementacije u ovom dokumentu definiraju se kao dovršeno SAP landscapes koje se izvode isključivo u Azure bez nastavka servisa Active Directory ili naziv razlučivost lokalnim u javnom oblaka. Konfiguracija samo oblak nisu podržani za sustavima SAP proizvodnje ili konfiguracije kojima STMS SAP-a ili drugih resursa na lokalno morati koristiti između sustavima SAP hostirane na Azure i resursima residing lokalni.
* Više lokacija: Opisuje scenarij u kojem VMs uvode se Azure pretplatu koja ima web-mjesto, više web-mjesta ili ExpressRoute veze između lokalnog datacenter(s) i Azure. Zajednički Azure dokumentaciju ove vrste implementacijama su i što je opisano kao scenariji više lokacija. Razlog za vezu da biste proširili lokalne domene te lokalnog servisa Active Directory i lokalnih DNS-a u Azure. Vodoravno lokalnog prošireni Azure imovini pretplate. Imate ovaj kućni broj, na VMs može biti dio lokalne domene. Korisnici domene lokalne domene možete pristupiti na poslužitelje, a mogu se izvoditi usluge na te VMs (kao što je DBMS services). Komunikacija i naziv razlučivost između VMs implementiran na lokalni i moguće je VMs u uveden u Azure. Očekivanog biti Najčešći scenarij za implementaciju SAP imovine na Azure. Pogledajte [ovaj] [ vpn-gateway-cross-premises-options] članak i [u ovom] [ vpn-gateway-site-to-site-create] dodatne informacije.

> [AZURE.NOTE] Za sustavima SAP radnog podržane su više lokacija implementacijama sustava SAP gdje Azure virtualnim strojevima sa sustavima SAP su članovi lokalne domene. Konfiguracija više lokacija podržani za implementaciju dijelova ili dovršiti landscapes SAP-a u Azure. Čak i radi dovršeno vodoravno SAP-a u Azure zahtijeva pojavljuju te VMs dio lokalne domene i reklame. U bivšeg verzijama u dokumentaciji smo bila riječ o scenarijima Hibridne IT, gdje je s termina 'Hibridnog' korijenom u činjenica da postoji više lokacija povezivanje između lokalnog i Azure. U ovom slučaju "Hibridnog" i znači da VMs u Azure su dio lokalnog servisa Active Directory.

Neke Microsoft dokumentaciji se opisuje više lokacija scenariji malo drugačije, posebice za DBMS SLIKA konfiguracije. U slučaju sustava SAP povezane dokumente na više lokacija scenarij samo boils prema dolje do imate web-mjesto ili privatnih (ExpressRoute) s povezivanjem i fact vodoravno SAP distribuira između lokalnog i Azure.

### <a name="resources"></a>Resursi
Sljedeći vodiči za temu implementacijama SAP-a na Azure dostupne su:

* [SAP NetWeaver na Azure virtualnim strojevima (VMs) – vodič za implementaciju i planiranje] [– vodič za planiranje]
* [SAP NetWeaver na Azure virtualnim strojevima (VMs) – vodič za implementaciju] [– vodič za implementaciju]
* [SAP NetWeaver na Azure virtualnim strojevima (VMs) – vodič za implementaciju DBMS (ovaj dokument)] [dbms vodič]

Sljedeće SAP bilješke se odnose na temu SAP-a na Azure:

| Broj bilješke   | Naslov
|------------|--------
| [1928533] | Aplikacija za SAP na Azure: vrste podržane proizvodi i Azure VM
| [2015553] | SAP-a na Microsoft Azure: podržava preduvjeti
| [1999351] | Rješavanje problema poboljšanom Azure nadzor za SAP
| [2178632] | Ključ nadzor metriku za SAP-a na Microsoft Azure
| [1409604] | Virtualizacija u sustavu Windows: Poboljšana nadzora
| [2191498] | SAP-a na Linux s Azure: Poboljšana nadzora
| [2039619] | Aplikacija za SAP na Microsoft Azure pomoću baze podataka tvrtke Oracle: podržani proizvodi i verzije
| [2233094] | DB6: Aplikacija za SAP na Azure pomoću IBM DB2 Linux, UNIX i Windows – dodatne informacije
| [2243692] | Linux na Microsoft Azure (IaaS) VM: problema licence SAP-a
| [1984787] | SUSE LINUX Enterprise Server 12: Napomene o instalaciji
| [2002167] | Crvena je vaša Enterprise Linux 7.x: instalacija i nadogradnja

I pročitajte [SKN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) koja sadrži sve bilješke SAP-a za Linux.

Imat ćete raditi znanja o arhitektura za Microsoft Azure i kako uvesti i njihovo virtualnim računalima sustava Microsoft Azure. Dodatne informacije možete pronaći ovdje <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Ne možemo **ne** razgovarate Microsoft Azure platforme kao ponude servisa (PaaS) sustava Microsoft Azure platforme. Ova dokumentacija radi sa sustavom upravljanje bazom podataka (DBMS) u programu Microsoft Azure virtualnim strojevima (IaaS) baš kao i želite pokrenuti na DBMS u lokalnog okruženja. Mogućnosti baze podataka radovi između tih dviju ponuda se vrlo različito i trebali biste neće biti kombinaciju međusobno povezani. Vidi također: <https://azure.microsoft.com/services/sql-database/>

Budući da ne možemo razgovarate IaaS, Općenito Windows, Linux i DBMS instalacije i konfiguracije zapravo isti su kao virtualnog računala ni Gola metalnim računalu želite instalirati lokalni. No postoje neke arhitektura i sustava implementaciju odluke upravljanja koji će biti različiti prilikom korištenja IaaS. Svrha ovaj dokument je objašnjavaju određene arhitektonski i sustava upravljanja razlike koje ste mora se pripremili za prilikom korištenja IaaS.

Općenito govoreći, su cjelokupan područja razlikom što obrađuje ovog članka:

* Planiranje rasporeda VM/VHD proper sustavima SAP da biste bili sigurni da imate odgovarajuće podatke datoteku izgleda web-mjesta i možete postići dovoljno IOPS za svoje radno opterećenje.
* Povezivanje s mrežom čimbenici vezani uz korištenje IaaS.
* Značajke određene baze podataka da biste koristili da biste optimizirali prikaz baze podataka.
* Sigurnosno kopiranje i vraćanje pitanja vezana uz u IaaS.
* Da bi mogli koristiti različite vrste slika radi implementacije.
* Visoke dostupnosti u Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktura RDBMS implementacije
U redoslijedu slijedite ovog poglavlja, to je potrebno da biste shvatili što je unesen u poglavlja [ovaj] [implementacije-vodič-3] [implementacije vodiča] [– vodič za implementaciju]. Saznali više o različite nizove VM i njihove razlike i razlike Azure standardnih ili prostora za pohranu Premium trebali biste razumjeti i poznati prije ovog poglavlja za čitanje.

Do ožujak 2015, Azure VHDs koji sadrže operacijski sustav su ograničeni na 127 GB po veličini. To ograničenje imate lifted u ožujku 2015 (za dodatne informacije o provjeri <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Iz nje na VHDs koja sadrži operacijski sustav može imati iste veličine kao i druge VHD. Ipak, ne možemo i dalje radije strukturu implementacije gdje operacijski sustav, DBMS i usmjerenog SAP binarne datoteke razlikuju od datoteke baze podataka. Stoga očekivanog sustavima SAP izvodi u Azure virtualnim strojevima će imati osnovni VM (ili VHD) instaliran s operacijskim sustavom baze podataka upravljanja sustava izvršne datoteke i SAP izvršne datoteke. DBMS podataka i zapisnika datoteka bit će pohranjene u prostor za pohranu Azure (standardni prikaz ili Premium prostora za pohranu) u zasebne datoteke VHD i kao logička diskova priložiti izvorne slike Azure operacijski sustav VM. 

Zavisne na korištenje Azure standardno ili Premium prostora za pohranu (npr. pomoću DS niz i VMs Oznaka niz) nema su druge kvota u Azure koje se [ovdje][virtual-machines-sizes]. Prilikom planiranja vaše VHDs Azure, morat ćete pronaći najbolje saldo kvote za sljedeće:

* Broj podatkovne datoteke.
* Broj VHDs koje sadrže datoteke.
* Kvota IOPS od jednog VHD.
* Propusnost podataka po VHD.
* Broj dodatne VHDs moguće po veličina VM.
* Cjelokupan propusnost za pohranu na VM unijeti.
 
Azure provodi kvota za IOPS po VHD pogon. Ove kvote razlikuju se za VHDs hostirane na Premium prostora za pohranu i Azure standardne prostor za pohranu. / I latencies će biti vrlo razlike između dvije vrste prostora za pohranu s Premium prostora za pohranu izlaganja čimbenika bolje/i latencies. Svaki od različitih vrsta VM imaju ograničen broj VHDs koje možete priložiti. Drugi ograničenje je samo za određene vrste VM možete koristiti za pohranu Premium Azure. To znači da odluka za određenu vrstu VM možda ne samo se pokreću preduvjeti opterećenje procesora i memorije, ali i tako da na IOPS Latencija i diska preduvjeti propusnost koje obično su skalirana broj VHDs ili vrste diskova Premium prostora za pohranu. Osobito s Premium pohranom veličinu na VHD i možda biti diktirali prema broju IOPS i propusnost koje je potrebno se postiže svakom VHD.

Fact postavljen broj VHDs IOPS stopu cjelokupan i veličine u VM je sve uz zajedno, može uzrokovati Azure Konfiguracija programa sustava SAP se razlikuje od njezinu lokalnu implementaciju. Ograničenja IOPS po LUN su obično konfigurirati u lokalnim implementacijama. Dok je prostora za pohranu Azure ta ograničenja su fiksne ili kao zavisne Premium prostora za pohranu u tipu disk. Stoga s lokalnim implementacijama dobivamo konfiguracije klijenta poslužitelja baze podataka koje koriste mnogo različitih količine za posebne izvršne datoteke kao što su SAP i DBMS ili posebne količine za privremene baze podataka ili tablice razmake. Kada se kao što je sustav lokalnog Premjesti Azure po rasipanje VHD izvršne datoteke ili baze podataka koji će se izvršiti bilo koje ili ne mnogo IOPS može dovesti otpad potencijalne IOPS propusnosti. Stoga u Azure VMs preporučujemo da izvršne datoteke DBMS i SAP, a biti instalirana na disku OS ako je to moguće.

Položaj datoteke baze podataka i datoteke zapisnika i vrstu Azure prostora za pohranu koriste, potrebno je definirati IOPS, Latencija i preduvjeti za propusnost. Da bi dovoljno IOPS za zapisnik transakcija, možda će biti prisilno pod utjecajem više VHDs za zapisnik transakcija ili koristiti veći disk Premium prostora za pohranu. U tom slučaju nešto bi jednostavno stvaranje softverski RAID (npr., Windows prostora za pohranu grupe aplikacija za Windows ili MDADM i LVM (Upravitelj logičkih glasnoću) za Linux) s VHDs koje će sadržavati zapisnik transakcija.

___

> ![Windows][Logo_Windows] Windows
>
> D:\ u programa Azure VM je pogon na koji nisu ista i koje se sigurnosno tako da neke lokalni disk na čvor Azure računalnim. Budući da nije dosljedan, to znači da promjene koje ste napravili sadržaj na pogon D:\ se gube kad je na VM sustava. Po "promjene", ne možemo znači spremljene datoteke Imenici stvoreni, instalirane aplikacije, itd.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VMs automatsko Postavljanje pogona pri /mnt/resource koji je pogon koji nije ista i sigurnosno po lokalni disk na čvor Azure računalnim. Budući da nije dosljedan, to znači da promjene koje ste napravili sadržaja u /mnt/resource se gube kad je na VM sustava. Tako da sve promjene ćemo znači datoteke spremljene Imenici stvoreni, instalirane aplikacije, itd.

___

Ovisno o tome Azure niz VM, lokalni disk na čvor računalnim prikaz različitih performansi koji može se podijeliti kao što su:

* A0 A7: Vrlo ograničeni performansi. Nije moguće koristiti za sve osim datoteka stranice za windows
* A8 A11: Vrlo dobre performanse karakteristike s nekim IOPS deset tisuće i > 1GB/sec propusnost
* D-niz: Vrlo dobre performanse karakteristike s nekim IOPS deset tisuće i > 1GB/sec propusnost
* DS-niz: Vrlo dobre performanse karakteristike s nekim IOPS deset tisuće i > 1GB/sec propusnost
* G nizom: Vrlo dobre performanse karakteristikama s nekim IOPS deset tisuće i > 1GB/sec propusnost
* Serija Oznaka: Vrlo dobre performanse karakteristikama s nekim IOPS deset tisuće i > 1GB/sec propusnost

Naredbe iznad primjene su vrste VM koje posjeduju SAP-a. VM niz s izvrstan IOPS i propusnost zadovoljavate leverage neke značajke DBMS, kao što su tempdb ili privremena prostora.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Predmemoriranje za VMs i VHDs
Kada ćemo stvoriti te diskova/VHDs putem portala sustava ili smo dostupnosti prenesene VHDs za VMs, ne možemo možete odabrati li promet/i između na VM i one VHDs koja se nalazi u odjeljku pohrana Azure predmemoriraju. Azure standardnu i pohranu Premium koristiti dvije različite tehnologije za tu vrstu predmemorije. U vašoj slučajeva predmemoriju sam će biti disk sigurnosno na istom pogonima koristi privremene disk (D:\ u sustavu Windows) ili /mnt/resource na Linux od na VM.
 
Za pohranu u standardni Azure vrste mogućih predmemorije su sljedeće:

* Bez predmemoriranja
* Čitanje predmemoriranja
* Čitanje i pisanje predmemoriranja

Da biste dobili dosljedne i deterministic performanse, trebali biste postaviti predmemoriranje na Azure standardne prostora za pohranu za sve VHDs koja sadrži **DBMS povezane podatkovne datoteke, datoteke zapisnika i tablice prostora za "None" (nema)**. Predmemoriranje na VM može ostati sa zadanim.

Za pohranu Premium Azure postoje predmemoriranja sljedeće mogućnosti:

* Bez predmemoriranja
* Čitanje predmemoriranja

Preporuke za pohranu Premium Azure je odražava **čitati predmemoriju za podatkovne datoteke** baze podataka za SAP, a odabrali **bez predmemoriranja VHD(s) datoteke zapisnika**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Softver RAID
Sljedeći već naveden iznad, morat ćete saldo broj IOPS preko broj VHDs možete konfigurirati su potrebna za datoteke baze podataka, a zatim upišite maksimalno IOPS programa Azure VM dat će po VHD ili prostora za pohranu Premium disk. Radnju za učitavanje IOPS putem VHDs najjednostavnije da biste sastavili softver RAID preko različitih VHDs. Postavite broj podatkovnih datoteka programa SAP DBMS na LUNS carved iz softver RAID. Ovisi o preduvjetima možda ćete morati preporučujemo korištenje prostora za pohranu Premium istodobnog od dva tri različite diskova Premium prostora za pohranu omogućuju veću kvote IOPS od VHDs koji se temelji na standardni prostora za pohranu. Osim na značajnu bolje/i Latencija nudi Azure Premium prostora za pohranu. 

Isto vrijedi za zapisnik transakcija različite DBMS sustava. S mnogo ih jednostavno dodavanje više datoteka Tlog ne pomogne jer sustavi DBMS napišite u jednu od datoteka u trenutku samo. Su po potrebi veći IOPS stope od Jedna standardna prostora za pohranu koji se temelje VHD možete održati, možete stripe preko više standardnih VHDs prostora za pohranu ili možete koristiti veće Premium prostora za pohranu na disku vrstu koja izvan veći IOPS stope također nudi čimbenika donjem Latencija za pisanje I/Os u zapisnik transakcija.
 
Situacija iskustvom u Azure implementacijama koji želite odabrati pomoću softverski RAID su:

* Zapisnik transakcija zapisnika/ponoviti poništeno zahtijevati više IOPS od Azure nudi za jednu VHD. Kao što je rečeno iznad to može se riješiti gradnje na LUN preko više VHDs pomoću softvera RAID.
* Nejednaki/i radno opterećenje distribucija preko različitih vrsta datoteke baze podataka za SAP-a. U tim slučajevima jedan možete doći do primatelja kvote radije često jedan podatkovne datoteke. Dok druge podatkovne datoteke ne čak i stižu blizu kvota za IOPS jedan VHD. U tim slučajevima najjednostavnije je rješenje da biste sastavili jedan LUN preko više VHDs pomoću softvera RAID. 
* Ne znate što točno radno opterećenje/i po podatkovne datoteke je i samo otprilike Saznajte što je cjelokupan radno opterećenje IOPS odnosu na DBMS. Najjednostavniji učiniti je da biste sastavili jedan LUN uz pomoć softvera RAID. Zbroj kvote više VHDs iza ovaj LUN trebali biste zatim ispunjavanje poznati IOPS stopa.

___

> ![Windows][Logo_Windows] Windows
>
> Korištenje sustava Windows Server 2012 ili noviji prostori za pohranu se preporučuje Budući da je učinkovitiji od Windows Podjela starijim verzijama sustava Windows. Imajte na umu da možda ćete morati Stvaranje grupe za pohranu za Windows i prostori za pohranu po naredbe ljuske PowerShell kada se koristi Windows Server 2012 kao operacijski sustav. Naredbe ljuske PowerShell Ovdje možete pronaći <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Da biste sastavili softver RAID na Linux podržani su samo MDADM i LVM (Upravitelj logičkih jedinice). Dodatne informacije potražite u sljedećim člancima:
>
> * [Konfiguriranje softver RAID na Linux] [ virtual-machines-linux-configure-raid] (za MDADM)
> * [Konfiguriranje LVM na Linux VM servisu Azure][virtual-machines-linux-configure-lvm]


___

Zahtjevi za korištenje VM niz koji su možete raditi s Azure Premium pohranom obično su:

* Zahtjeve za latencies/i koji su blizu što izlaganje SAN/NAS uređaji.
* Zahtjev za čimbenika bolje/i latencije od može isporučivati Azure standardne prostora za pohranu.
* Veća IOPS po VM nego što nije moguće postići s više standardnih VHDs prostora za pohranu od određene vrste VM.

Budući da podlozi prostora za pohranu Azure replicira svaki VHD na najmanje tri čvorove prostora za pohranu, jednostavno RAID 0 Podjela može se koristiti. Nema potrebe za implementaciju RAID5 jedinici ili RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Prostor za pohranu Microsoft Azure
Spremište na platformi Microsoft Azure će sadržavati osnovni VM (s operacijskim Sustavom) i VHDs ili blob-ova najmanje 3 čvorove zasebnom prostora za pohranu. Prilikom stvaranja računa za pohranu, postoji mogućnost zaštite kao što je prikazano ovdje:

![Zemlj replikacije omogućeno za račun za Azure prostora za pohranu][dbms-guide-figure-100]

Azure prostora za pohranu lokalne replikacije (lokalno suvišnih) omogućuje razine zaštite od gubitka podataka zbog pogreške infrastrukture koje nekoliko korisnika nije smijete za implementaciju. Kao što je prikazano iznad nje 4 različite mogućnosti peti se varijacija jedne od prva tri. Pogled bliže ih mogu se razlikovati:

* **Premium lokalno suvišnih prostora za pohranu (LRS)**: Azure Premium pohranu nudi visokih performansi, najniža Latencija disk podrška za virtualnim strojevima radi li/O-ćete morati usko radnih opterećenja. Postoje 3 replike podataka unutar iste Azure podatkovnog centra područja za Azure. Kopija bit će različite kvara i nadogradnja domena (za koncepata potražite u članku [Time] [planiranja – vodič za-3,2] poglavlja u na [planiranje Guide][planning-guide]). U slučaju kopije podataka odlazi iz servisa zbog pogreške čvor prostora za pohranu ili diska, nove replike generira se automatski.
* **Lokalno suvišnih prostora za pohranu (LRS)**: U ovom slučaju postoje 3 replike podataka unutar iste Azure podatkovnog centra područja za Azure. Kopija bit će različite kvara i nadogradnja domena (za koncepata potražite u članku [Time] [planiranja – vodič za-3,2] poglavlja u na [planiranje Guide][planning-guide]). U slučaju kopije podataka odlazi iz servisa zbog pogreške čvor prostora za pohranu ili diska, nove replike generira se automatski. 
* **Zemlj suvišnih prostora za pohranu (GRS)**: U ovom slučaju postoji asinkronog replikacije koji umetanja dodatne 3 replike podataka u drugoj regiji Azure koja je u većini slučajeva u istom zemljopisnom području (kao što je Sjeverna Europa i Zapad Europe). Posljedica će 3 dodatne replike, tako da postoje 6 replike zbroj. Varijacije to je dodatak gdje se mogu koristiti podatke u području repliciranu Azure zemlj svrhe čitanja (pristup za čitanje zemlj. – suvišnih).
* **Zone suvišnih prostora za pohranu (ZRS)**: U ovom slučaju 3 replike podaci ostaju na istom području Azure. Kao što je opisano u [] [planiranja – vodič za-3,1] poglavlja [planiranje vodiča] [– vodič za planiranje] Azure regija može biti broj podatkovnim centrima uz. U slučaju LRS na replike želite distribuirati preko različitih podatkovnim centrima koji čine određenu regiju Azure.

Dodatne informacije možete pronaći [ovdje][storage-redundancy].
 
> [AZURE.NOTE] U slučaju implementacije DBMS ne preporučuje korištenje prostora za pohranu na suvišnih zemlj.
>
> Azure prostora za pohranu zemlj. – replikacije je asinkronih. Replikacija pojedinačne VHDs postavljen da jedan VM ne sinkroniziraju se u koraku lock. Zbog toga nije prikladna za replikaciju DBMS datoteke koje su distributed preko različitih VHDs ili implementiran na temelju softver RAID na temelju više VHDs. DBMS softver potreban je da se trajni disk prostora za pohranu točno sinkronizirana preko različitih LUNs i temeljni diskova/VHDs/spindles. Softver DBMS koristi razne mehanizme slijed IO pisanje aktivnostima i na DBMS izvješća prostora za pohranu na disku ciljani tako da na replikacije oštećen ako oni se razlikuju čak ni uz nekoliko milisekundi. Dakle ako nešto zaista želi konfiguraciju baze podataka s bazom podataka Rastegnuto preko više VHDs zemlj replicirati, takve replikacije potrebno izvršiti s znači baze podataka i funkcije. Neki se ne oslanjate na Azure prostora za pohranu zemlj. – replikacije da biste izvršili taj zadatak. 
>
> Problem je najjednostavnije objašnjavaju sa sustavom elektroničke primjer. Recimo da imate sustav SAP prenijeli u Azure koja ima 8 VHDs koji sadrži podatkovne datoteke u DBMS plus jedan VHD datotekom zapisnik transakcija. Svaki od tih 9 VHDs će imati podatke koji su im pisane dosljedan načina prema DBMS, hoće li se zapisuje u datoteke zapisnika podataka ili transakcije.
>
> U narudžbe za pravilno zemlj. – replikaciju podatke i održavanje sliku dosljedan baze podataka sadržaja sve devet VHDs bit će potrebno je replicirati zemlj redoslijedom točno/i operacije su izvršava protiv devet različitih VHDs. Međutim, zemlj replikacije za pohranu Azure ne dopušta deklarirati međuzavisnosti VHDs. To znači da zemlj replikacije spremište na platformi Microsoft Azure ne zna fact međusobno povezani sadržaj te devet različitih VHDs i promjene podataka jesu dosljedan samo kada se replikaciju u redoslijedu operacije/i dogodilo preko 9 VHDs.
>
> Osim vjerojatno se visoke zemlj replicirati slike u scenariju pružaju sliku dosljedan baze podataka, i postoji performanse kazna koja se prikazuje s zemlj suvišnih prostora za pohranu koji uzrokuje može utjecati na performanse. U sažetku nemojte koristiti ovu vrstu zalihosti prostora za pohranu za radnih opterećenja DBMS vrsta.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mapiranje VHDs račune za pohranu servisa Azure virtualnog računala
Račun za pohranu Azure je administratora konstrukta, ali i predmet ograničenja. Dok je na li ćemo objasniti što je Azure standardne prostora za pohranu račun ili račun za pohranu Azure Premium razlikuju se ograničenja. Točne mogućnosti i ograničenja su navedene [ovdje][storage-scalability-targets]
 
Da bi se za pohranu u standardni Azure je važno je napomenuti da ograničen je na IOPS po računu za pohranu (redak koji sadrži "Ukupno zahtjev stopa" u [članku][storage-scalability-targets]). Uz to, postoji početne ograničenje od 100 račune za pohranu po pretplati Azure (na srpanj 2015). Stoga preporučujemo saldo IOPS VMs između više prostora za pohranu računa prilikom korištenja Azure standardne prostora za pohranu. Dok je jedan VM najbolje koristi jednog računa za pohranu ako je to moguće. Pa ako ćemo objasniti što DBMS implementacijama gdje svaki VHD koji se nalazi na standardni prostora za pohranu Azure nije dosegne ograničenje kvote, trebali biste uvesti samo 30 40 VHDs po Azure prostora za pohranu računa koji koristi Azure standardne prostora za pohranu. S druge strane, ako pod utjecajem Azure Premium prostora za pohranu, a želite pohraniti količine velike baze podataka, možda precizno pomoću IOPS. Dok je račun za pohranu Premium Azure način veća ograničenja u opsegu podataka od Azure standardne prostora za pohranu računa. Zbog toga ograničen broj VHDs poslovnog subjekta za pohranu za Azure Premium možete samo implementirati prije odlazak ograničenje jedinice podataka. Na kraj shvatiti račun za pohranu Azure kao "Virtualne SAN" koji sadrži ograničen mogućnosti u IOPS i/ili kapaciteta. Kao rezultat, zadatak će ostati, kao u lokalnim implementacijama da biste odredili izgled VHDs različite sustavima SAP preko različitih 'imaginarne SAN uređaja"ili računi servisa Azure prostora za pohranu.
 
Za pohranu u standardni Azure ne preporučuje se za predstavljanje pohranu prostora za pohranu za različite račune za jednu VM ako je to moguće.

Dok je pomoću DS ili Oznaka-niz Azure VMs moguće je postavljanja VHDs iz Azure standardne račune za pohranu i račune za pohranu Premium. Korištenje slučajeva kao što su pisanje sigurnosnih kopija u standardni prostora za pohranu sigurnosno VHDs dok pojavljuju DBMS podataka i datoteke zapisnika na Premium prostora za pohranu Dođite na umu gdje se može leveraged takve heterogenih prostora za pohranu. 

Na temelju implementacijama klijenta i testiranje oko 30 za 40 VHDs koji sadrži podatkovne datoteke baze podataka i datoteke zapisnika možete dodjeli u jednom Azure standardne prostora za pohranu računu s prihvatljiva performansama. Kao što je rečeno ranije, vjerojatno ga mogu sadržavati kapacitet podataka i ne IOPS je ograničenje prostora za pohranu računa Premium Azure.

Kao SAN uređaji lokalnog sustava, zajedničko korištenje zahtijeva neke nadzor da bi se ipak otkriti grla na račun za Azure prostora za pohranu. Azure nadzor proširenje za SAP i portalu Azure su Alati za koje je moguće koristiti da biste otkrili zauzet Azure račune za pohranu koji izlaganja ili lošije IO performanse.  Ako to je otkriven preporučuje se da biste premjestili zauzet VMs na drugi račun za Azure prostora za pohranu. Pogledajte na [vodič za implementaciju] [– vodič za implementaciju] pojedinosti o aktivaciji voditelju SAP mogućnosti za nadzor.

Drugi članak sa sažetkom najbolje prakse oko Azure standardne prostora za pohranu i računi servisa Azure standardne prostora za pohranu Ovdje možete pronaći <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Premještanje implementiran VMs DBMS iz standardnog spremišta Azure Azure Premium prostora za pohranu
Ne možemo naići prilično nekim scenarijima gdje kao kupac želite premjestiti distribuiranih VM iz Azure standardne prostora za pohranu u Azure Premium prostora za pohranu. To nije moguće bez fizički premještanja podatke. Da biste postigli cilj na nekoliko načina:

* Jednostavno može kopirati sve VHDs, osnovni VHD kao i VHDs podataka na novi račun za pohranu Azure Premium. Često broj VHDs u odaberete Azure standardne prostora za pohranu ne zbog fact zatrebate glasnoće podataka. No potrebno toliko VHDs zbog na IOPS. Sad kad premjestite Azure Premium pohranu možete izabrati način manje VHDs da biste postigli neke IOPS propusnost. Navedeni činjenica da u Azure standardne prostor za pohranu koji morate platiti za korištene podataka i ne veličina nominal diska, broj VHDs jeste li zaista nije bitan pomoću troškove. Međutim, s Azure Premium prostor za pohranu, trebate platiti za veličinu nominal diska. Dakle, većina kupca pokušati čuvajte broj Azure VHDs Premium prostora za pohranu na broj koji je potrebno da biste postigli propusnost IOPS potrebne. Tako, većina kupaca odlučite odnosu na način na koji se jednostavno 1:1 Kopiraj.
* Ako još nije postavljen dostupnosti jedan VHD koje mogu sadržavati sigurnosnu kopiju baze podataka SAP baze podataka. Nakon stvaranja sigurnosne kopije, skidanje sve VHDs uključujući VHD koja sadrži sigurnosne kopije i kopirajte osnovni VHD i u VHD s sigurnosnu kopiju u račun za Azure Premium prostora za pohranu. Koju želite uvesti VM osnovni VHD na temelju pa dostupnosti u VHD s sigurnosnu kopiju. Sada stvorite dodatnih prazan diskova Premium prostora za pohranu za VM koji se koriste za vraćanje baze podataka u. Pretpostavlja se da u DBMS omogućuje promjenu putova datoteka podataka i prijaviti se kao dio postupak vraćanja.
* Moguće je varijacije bivši procesa, gdje samo kopirajte sigurnosnu kopiju VHD u Azure Premium prostora za pohranu i priložite protiv VM koji ste upravo implementiran i instalirali.
* Četvrti mogućnost odaberite kada ste u need da biste promijenili broj podatkovne datoteke baze podataka. U tom slučaju obavljate kopiju homogenous sustava SAP-a pomoću izvoza/uvoza. Stavi one izvezite datoteke VHD koja se kopira u račun za Azure Premium prostora za pohranu i priložite VM koju koristite za pokretanje procesa uvoza. Korisnici koriste tu mogućnost uglavnom kada ih želite smanjiti broj podatkovne datoteke.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Uvođenje VMs za SAP servisu Azure
Microsoft Azure nudi različite načine za implementaciju VMs i pridruženi diskova. Time važno je da biste razumjeli razlike jer pripreme na VMs može se razlikovati od ovisi o način implementacije. Općenito govoreći, ne možemo pogleda scenariji što je opisano u sljedećim poglavlja.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Implementacija VM iz Azure Marketplace
Koje želite snimiti Microsoft ili 3 strana dobili sliku iz trgovine Azure za implementaciju sustava VM. Nakon implementiran na VM u Azure, slijedite isti smjernice i alati da biste instalirali softver SAP unutar vaše VM kao što biste slijedili u okruženju na lokaciji. Za instalaciju softvera SAP unutar Azure VM, SAP i Microsoft preporučujemo da biste prenijeli i pohraniti SAP instalacijskog medija u Azure VHDs ili da biste stvorili programa Azure VM funkcionira u skladu s 'datotečnog poslužitelja"koja sadrži sve potrebne SAP instalacijskog medija.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Implementacija VM određene generalizirano slikom klijenta
Zbog preduvjeti za određene zakrpu u vas pozdravljamo u verziju OS ili DBMS, navedeni slike u trgovine Windows Azure možda odgovara vašim potrebama. Stoga ćete morati stvoriti VM pomoću vlastite 'privatne OS/DBMS VM slike koje možete uvesti naknadno nekoliko puta. Da biste pripremili takve 'privatne sliku za dupliciranje, OS-a mora biti GRG generalizirano na lokalni VM. Pogledajte na [vodič za implementaciju] [– vodič za implementaciju] pojedinosti o generalize na VM.

Ako ste već instalirali SAP sadržaj u vaše lokalne VM (osobito za sustave sloja 2), možete prilagoditi postavke sustava SAP nakon implementacije VM Azure kroz instanci Preimenovanje postupak podržava SAP softver dodjeljivanje Upravitelj (SAP bilješke [1619720]). U suprotnom možete instalirati softver SAP kasnije nakon implementacije Azure VM.
 
Na baze podataka sadržaja koristi aplikacija za SAP možete ili generirati sadržaj freshly po instalaciji SAP-a ili sadržaj možete uvesti Azure pomoću VHD s DBMS sigurnosnu kopiju baze podataka ili korištenje mogućnosti DBMS izravno sigurnosno kopirati u spremište na platformi Microsoft Azure. U ovom slučaju, možete i Priprema VHDs na DBMS podataka i prijaviti se datoteke lokalnog sustava i one kao diskova uvesti Azure. No prijenos podataka DBMS koja se učitava lokalnim za Azure daju putem VHD diskova koje je potrebno da bi se pripremite lokalnog.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Premještanje na VM s lokalnim Azure s diskom koji nisu GRG generalizirano
Planiranje da biste premjestili određene sustava SAP lokalnim Azure (Dizalica i shift). To možete učiniti tako da prenesete VHD koja sadrži OS, binarne datoteke za SAP i usmjerenog binarne datoteke DBMS plus VHDs s podacima i zapisnika datotekama DBMS za Azure. U nasuprot scenarij #2 iznad, držite naziv glavnog računala, SAP SID i SAP korisničkih računa u Azure VM oni su konfigurirano u lokalnog okruženja. Stoga generalizing slike nije potreban. U ovom slučaju primijenit će uglavnom scenarije unakrsno lokalno mjesto dio vodoravno SAP se izvršava lokalnog i dijelove Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Visoke dostupnosti i oporavak Izrada s Azure VMs
Azure nudi sljedeće visoke dostupnosti (HA) i Izrada oporavak (DR) radovi koji se odnose na razne komponente koristit ćemo za SAP i DBMS implementacije

### <a name="vms-deployed-on-azure-nodes"></a>VMs implementiran na čvorove Azure
Platforme Azure nude značajke kao što su Live migracije za distribuiranih VMs. To znači da ako je održavanje potrebno na poslužitelju klaster na koji je implementiran u VM, na VM mora se zaustavio i ponovno pokrenuti. Održavanje u Azure je izvršiti pomoću pa naziva domene za nadogradnju unutar klastere poslužitelja. Samo jednu domenu nadograditi po je koji se održava. Tijekom kao što je ponovno pokretanje pojavit će se do prekida servisa dok u VM isključiti, provodi održavanja i ponovno pokrenuti VM. Većina DBMS dobavljače no funkcionalnosti visoke dostupnosti i Izrada oporavak brzo pokrenite DBMS usluge na drugi čvor ako primarni čvor nije dostupan. Platforme Azure nudi funkcije VMs, za pohranu i drugih servisa za Azure raspodijelite nadograditi domene da biste bili sigurni da planiranog održavanja ili infrastrukture neuspjeha samo bi utjecati mali podskup VMs ili usluge.  Pažljivo planiranje moguće je da biste postigli dostupnost razine usporediti s lokalnim infrastructures.

Microsoft Azure dostupnost skupovi su logički grupiranje VMs ili servise koje osigurava VMs i ostale servise su distribucije za različite kvara i nadogradnja domene unutar klaster tako da bi postojati samo jedan čvor zatvaranja u bilo kojem trenutku jedan vrijeme (pročitajte [ovaj] [ virtual-machines-manage-availability] članka dodatne informacije).

Je potrebno je konfigurirati tako da više nije potrebna kada vodoravnim izvan VMs kao što se vidi u nastavku:

![Definicija skupa dostupnost DBMS SLIKA konfiguracije][dbms-guide-figure-200]

Ako želimo da biste stvorili iznimno dostupna konfiguracije DBMS implementacije (neovisno o pojedinačne DBMS SLIKA funkcionalnost koristi), DBMS VMs potrebni za:

* Dodavanje u VMs s istom Azure virtualne mrežom (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* VMs HA konfiguraciju i mora biti u istoj podmreži. Razlučivanje naziva između različitih podmreže nije moguće u implementacijama samo oblak, funkcionirat će samo IP rješenja. Koristite web-mjesto ili povezivanje ExpressRoute za unakrsno lokalnim implementacijama, mreža s najmanje jedan podmreže će već uspostaviti. Razlučivanje naziva provest će se u skladu s lokalnim AD pravila i mrežne infrastrukture. 
[komentar]: <>  (Test MSSedusch obveze ako je još uvijek true u OBLAK)

#### <a name="ip-addresses"></a>IP adrese
Preporučljivo je za postavljanje VMs HA konfiguracije prebacuju način. Potrebe za oslanjanjem na IP adrese da biste riješili poslovni HA unutar konfiguracije HA nije pouzdano u Azure osim ako se koriste statičke IP adrese. Postoje dva "Isključivanje" pojma u Azure:

* Isključivanje putem portala za Azure ili Azure PowerShell cmdlet Zaustavi AzureRmVM: U ovom slučaju virtualnog računala dobiva zatvaranja i njezini dodijeliti. Za ovaj VM više ne naplaćuje račun za Azure pa samo naknada koje će uzrokovati su za pohranu koriste. Međutim, privatne IP adrese mrežno sučelje nije statične, je objavio IP adresa i se zajamčeno mrežnog sučelja može vidjeti stare IP adresa dodijeljena ponovno nakon ponovnog pokretanja računala na VM. Izvođenje klikom na dolje putem portala za Azure ili tako da nazovete Zaustavi AzureRmVM uzrokovat će automatski de-dodijeljeni. Ako ne želite deallocat računalo pomoću Zaustavi AzureRmVM - StayProvisioned 
* Ako isključite VM iz razinu OS, na VM dobiva isključiti i ne poništite dodijeliti. Međutim, u ovom slučaju račun za Azure i dalje naplatiti za VM, bez obzira na fact ga je isključen. U tom slučaju dodjele IP adresa za prestao VM će ostati ostaje netaknuta. Isključuje VM iz unutar će automatski prisiljava de-dodijeljeni.

Čak i za više lokacija scenarije, po zadanom zatvaranja i de-dodjelom znači de-dodjeljivanjem IP adrese iz VM, čak i ako lokalnog pravila u odjeljku postavke DHCP razlikuju. 

* Je iznimka ako nešto dodjeljuje statičke IP adrese mrežnog sučelja kao što je opisano [u nastavku][virtual-networks-reserved-private-ip].
* U tom slučaju IP adresa ostaje fixed dok god mrežnog sučelja neće se izbrisati.

> [AZURE.IMPORTANT] Da biste zadržali cijeli implementacije jednostavne i moguće upravljati, Očisti preporuke je za postavljanje VMs partnering na DBMS SLIKA ili konfiguracije DR unutar Azure na način koji postoji funkcionira razlučivanje naziva između različitih VMs koji je uključen.
 
## <a name="deployment-of-host-monitoring"></a>Implementacija glavnog računala nadzora
Za produktivni korištenje aplikacija za SAP virtualnim računalima sustava u Azure SAP-a potreban je mogućnost dohvaćanja glavno računalo praćenje podataka iz fizičke hosts izvodi virtualnim računalima sustava Azure. Bit će potrebno određenoj razini zakrpu HostAgent SAP-a koji omogućuje tu mogućnost u SAPOSCOL i HostAgent SAP-a. Razina točno zakrpu navedenih u bilješku SAP [1409604].

Dodatne informacije vezane uz upravljanje životni ciklus navedenih komponenti i implementacija komponente koje izlaganje podataka glavno računalo za SAPOSCOL i SAPHostAgent, pogledajte u [vodič za implementaciju] [– vodič za implementaciju]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Specifičnosti za Microsoft SQL Server

### <a name="sql-server-iaas"></a>SQL Server IaaS
Počevši od sustava Microsoft Azure, možete jednostavno migrirati postojećih aplikacija sustava SQL Server utemeljena na platformi Windows Server na virtualnim strojevima sa sustavom Azure. SQL Server na virtualnim računalu omogućuje vam da biste smanjili ukupni trošak vlasništvo nad implementaciju, upravljanje i održavanje aplikacijama breadth tako da lako migracije te aplikacije za Microsoft Azure. Sa sustavom SQL Server u programa Azure virtualnog računala, administratorima i razvojnim inženjerima i dalje možete koristiti isti razvoja i administracije alatima koji su dostupni na lokalni. 

> [AZURE.IMPORTANT] Uzmite u obzir smo ne razgovarate Microsoft Azure SQL baze podataka koji je kao ponude za servis sustava Microsoft Azure platforme. Rasprave u ovom dokumentu je o pokretanju proizvoda sustava SQL Server, kao što je poznato je za lokalnim implementacijama u virtualnim računalima sustava Azure, korištenje Infrastruktura kao mogućnost servisa Azure. Mogućnosti baze podataka radovi između tih dvaju ponuda razlikuju se i trebali biste neće biti kombinaciju međusobno povezani. Vidi također: <https://azure.microsoft.com/services/sql-database/>
 
Preporučuje da biste pregledali [ovaj] [ virtual-machines-sql-server-infrastructure-services] dokumentacije prije nastavka.

U sljedećim odjeljcima dijelove dijelove dokumentaciju u odjeljku veza iznad bit će pridružuje i spomenuti. Specifičnosti oko SAP navode se kao i, a neki koncepti se podrobnije opisan u. Međutim, preporučujemo da biste u dokumentaciji iznad prvog prije određenog dokumentaciju za SQL Server za čitanje.

Postoji neki SQL Server u trebate znati prije nastavka IaaS određenih informacija:

* **SLA virtualnog računala**: postoji programa SLA za virtualnim strojevima izvodi u Azure koji se nalazi se ovdje: <https://azure.microsoft.com/support/legal/sla/>  
* **Podrška za SQL verzija**: za kupce SAP podržavamo SQL Server 2008 R2 i noviji na Microsoft Azure virtualnog računala. Starije izdanja nisu podržani. Pregledate opće [Izjava podrška](https://support.microsoft.com/kb/956893) više pojedinosti. Napominjemo da Općenito SQL Server 2008 podržava Microsoft kao i. No zbog značajne funkcionalnosti za SAP-a koji je uvedena s SQL Server 2008 R2, SQL Server 2008 R2 je minimalne izdanje za SAP-a. Imajte na umu da je SQL Server 2012 i 2014 imate prošireni s dublju integraciju u scenariju IaaS (kao što je sigurnosno kopiranje izravno u odnosu na prostor za pohranu za Azure). Dakle, ne možemo ograničiti ovog članka SQL Server 2012 i 2014 njegov razinom najnoviju zakrpu za Azure.
* **Podrška za značajku SQL**: najčešće SQL Server značajke podržane na Microsoft Azure virtualnim strojevima uz neke iznimke. **SQL Server klasteriranja pomoću zajednički diskova nije podržano**.  Raspodijeljeno tehnologije kao što su baze podataka, grupe dostupnosti AlwaysOn, replikacije, dostavu zapisnika i broker servisa sustava podržani unutar jedno područje Azure. SQL Server AlwaysOn je podržano i između različitih područja Azure kao što je ovdje navedenih: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Pročitajte članak [Izjava podrška](https://support.microsoft.com/kb/956893) više pojedinosti. Primjer kako uvesti u konfiguraciji AlwaysOn prikazuju se u [ovom] [ virtual-machines-workload-template-sql-alwayson] članka. Osim toga, pogledajte u najbolje prakse dokumentirane [ovdje][virtual-machines-sql-server-infrastructure-services] 
* **SQL performanse**: ne možemo sigurni da se mogu razlikovati Microsoft Azure glavnom računalu virtualnim strojevima vrlo dobro će izvesti usporedbi s drugim javnim oblaka virtualizacije ponude, ali pojedinačne rezultate. Pogledajte [ovaj] [ virtual-machines-sql-server-performance-best-practices] članka.
* **Korištenje slike iz trgovine Azure Marketplace**: najbrži način implementacije novog Microsoft Azure VM je da biste koristili sliku iz trgovine Azure Marketplace. Nema slika u trgovine Windows Azure koji sadrže SQL Server. Slika kojoj SQL Server već instaliran nije moguće odmah koristiti za SAP NetWeaver aplikacije. Razlog je SQL Server razvrstavanje zadani je instaliran unutar tim slikama i ne razvrstavanje potrebnih sustavima SAP NetWeaver. Da biste koristili kao što su slike, provjerite je li koraka navedenih u poglavlja [pomoću SQL Server slike iz trgovine Microsoft Azure Marketplace] [dbms – vodič – 5.6]. 
* Dodatne informacije potražite u članku [Cijene pojedinosti](https://azure.microsoft.com/pricing/) . [SQL Server 2012 licenciranje vodič](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) i [Vodič licenciranje programa SQL Server 2014](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) i su važne resursa.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Smjernice za konfiguraciju sustava SQL Server za SAP povezane instalacija sustava SQL Server u Azure VMs

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Preporuke o strukturi VM/VHD za SAP povezane implementacijama sustava SQL Server
Skladu Opći opis izvršne datoteke sustava SQL Server mora biti nalazi ili instalirali u sistemski pogon na VM osnovni VHD (pogon C:\).  Obično Većina baza podataka sustava SQL Server se ne koristi visoke razine po radno opterećenje NetWeaver SAP-a. Dakle sustav baze podataka sustava SQL Server (matrice, msdb i model) može ostati na disku C:\. Iznimku može biti tempdb koji se slučaju neke ERP SAP i svih radnih opterećenja BW, možda će biti potrebno ili noviji podataka ili/i operacije jedinice koji ne stane u izvornom VM. Za takvi sustavi trebaju izvesti sljedeće korake:

* Premještanje datoteke podataka primarni tempdb isti logički pogon kao primarni podataka datoteke baze podataka za SAP-a.
* Dodajte sve dodatne tempdb podatkovne datoteke za svaki od druge logičke pogone koji sadrži podatkovnu datoteku baze podataka za SAP-a korisnika.
* Dodavanje datoteke zapisnika tempdb logički pogon koja sadrži datoteke zapisnika baze podataka korisnika.
* **Isključivo za VM vrste koja koristi lokalni SSDs** na računalnim čvor tempdb podataka i prijaviti se datoteke mogu nalaziti na disku D:\. Ipak, možda će se preporučuje da biste koristili više tempdb podatkovne datoteke. Imajte na umu diskovnih pogona D:\ razlikuju se ovisno o vrsti VM.
 
Ove konfiguracije omogućiti tempdb trošiti dodatni prostor od osigurati sistemski pogon. Da biste odredili veličinu proper tempdb, jedan možete provjeriti veličine tempdb na postojeću sustavima koji se izvode na lokalni. Osim toga, takve konfiguracije želite omogućiti IOPS brojeva na temelju tempdb koje nije moguće dao sistemski pogon. Ponovno sustavima koji su pokrenuti na lokalni može se koristiti za praćenje radno opterećenje/i protiv tempdb tako da možete proizlaziti brojeva IOPS očekujete da biste vidjeli na vašem tempdb.

VM konfiguracije koji pokreće SQL Server s bazom podataka za SAP i mjesto na kojem nalazi tempdb podataka i datoteci zapisnika tempdb na disku D:\ izgledati:
 
![Konfiguriranje referenca Azure IaaS VM za SAP][dbms-guide-figure-300]

Imajte na umu da pogon D:\ ima različite veličine ovise o vrsti VM. Ovisi o veličini preduvjet tempdb koje možda će biti obavezno par tempdb podataka i datoteka zapisnika s SAP datoteke baze podataka podataka i prijaviti se u slučajevima gdje je premalen D:\ pogon.

#### <a name="formatting-the-vhds"></a>Oblikovanje na VHDs
Za SQL Server NTFS blokirati veličinu za VHDs koji sadrži podatke sustava SQL Server, a zatim datoteke zapisnika mora biti 64K. Nema potrebe da biste oblikovali D:\ pogon. U ovom pogon dolazi unaprijed oblikovanih.

Da biste bili sigurni da vrati ili stvaranja baze podataka se ne pokreće podatkovne datoteke tako da usredotočujući sadržaj datoteke, jedan provjerite je li ima li kontekst korisnika koji se izvodi se usluga za SQL Server u određenim dozvola. Obično korisnici u grupi administratora sustava Windows imaju te dozvole. Ako je u kontekstu korisnika koji nisu iz programa Windows administratori korisnika se pokreće servis sustava SQL Server, morate dodijeliti tom korisniku korisničko pravo "Izvedi glasnoću održavanja Zadaci".  Prikaz detalja u ovom članku Microsoftove baze znanja: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Utjecaj sažimanja baze podataka
U konfiguracijama gdje/i propusnosti može postati ograničavanje faktor svaki mjere koji smanjuje IOPS može pomoći na rastezanje radno opterećenje nešto jer se može pokrenuti u scenariju programa IaaS kao što su Azure. Stoga Ako još niste učinili, Primjena SQL Server PAGE sažimanje preporučuje se SAP i Microsoft prije prijenos postojeće SAP baza podataka za Azure.
 
Preporuke za izvođenje sažimanja baza podataka prije prijenosa Azure dobiva se iz dva razloga:

* Količina podataka koje je moguće prenijeti je niže.
* Sažimanje izvođenja traje kraće uz pretpostavku da se nešto možete koristiti jači hardver s više procesora ili noviji/i propusnosti ili manje/i Latencija lokalnog.
* Manje veličine baze podataka mogu dovesti do manji trošak dodijeljeni na disku

Sažimanje bazu podataka kao i funkcionira u na virtualnim računalima sustava Azure kao lokalnog. Dodatne informacije o tome kako sažeti postojećim SAP SQL Server baze podataka provjerite ovdje: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQL Server 2014. – Spremanje baze podataka datoteka izravno na Azure Blog prostora za pohranu
2014 SQL Server otvorit će se mogućnost pohranjuje datoteke baze podataka izravno na spremište blobova platforme Azure bez 'omot' VHD oko njih. Osobito s pomoću standardne Azure prostora za pohranu ili manje VM vrste omogućuje scenariji kojima ste možete bi se prevladala ograničenja IOPS koju želite provesti ograničen broj VHDs koji se može postaviti neke manje VM vrste. Taj je postupak za korisnika baze podataka, međutim nije za sustav baze podataka sustava SQL Server. Funkcionira i za podatke i datoteka zapisnika sustava SQL Server. Ako želite uvesti bazom podataka sustava SQL Server SAP-a na taj način umjesto "prelamanje" u VHDs, imajte sljedeće na umu:

* Račun za pohranu koristi potrebama budu u istom području Azure kao onu koja se koristi za implementaciju sustava SQL Server VM se izvodi u.
* Pitanja vezana uz spomenuta ranije u vas pozdravljamo za raspodjelu VHDs različite račune za pohranu Azure primijeniti za ovu metodu kao i implementacije. Označava broj/i operacije u odnosu ograničenja prostora za pohranu računa Azure.
[komentar]: <>  (MSSedusch popis obveza, ali to će koristiti bandwith mrežom i ne bandwith prostora za pohranu, ona ne?)

Detalje o ove vrste implementacije su navedene ovdje: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Da biste spremili SQL Server podatkovnih datoteka izravno na Azure Premium prostora za pohranu, morate koristiti minimalne izdanje 2014 SQL Server zakrpu koji ovdje navedenih: <https://support.microsoft.com/kb/3063054>. Spremanje datoteke podataka SQL Server na Azure standardne prostora za pohranu raditi s objavljenu verziju 2014 SQL Server.. Međutim, vrlo isti zakrpa sadrže neki drugi niz popravaka koji su izravno korištenje spremište blobova platforme Azure za SQL Server podatkovne datoteke i sigurnosno kopiranje više pouzdan. Stoga preporučujemo da biste koristili te zakrpa Općenito.

### <a name="sql-server-2014-buffer-pool-extension"></a>Kućni broj skup međuspremnik SQL Server 2014.
2014 SQL Server uvedena je nova značajka koja se naziva proširenje grupe međuspremnik. Ta je funkcija proširuje skupinu međuspremnika sustava SQL Server koja se drži u memoriji s drugom predmemorije razine se sigurnosno lokalni SSDs poslužitelja ili VM. Time se omogućuje da biste zadržali veći radni skup podataka 'u memoriji". U usporedbi s pristupa Azure standardne prostora za pohranu programa access u proširenje skupinu međuspremnika koja je spremljena na lokalni SSDs programa Azure VM je mnogo je čimbenika brže.  Dakle, korištenje na lokalni disk D:\ vrsta VM izvrstan IOPS i propusnost može biti vrlo pametnije način smanjiti učitavanja IOPS protiv Azure prostora za pohranu i znatno poboljšati vrijeme odaziva upita. To se odnosi osobito ako ne koristite za pohranu Premium. U slučaju Premium prostora za pohranu i korištenje predmemorije za čitanje Azure Premium na čvor računalnim, kao što je preporučeno za podatkovne datoteke, bez velike razlike se očekuje. Razlog je oba predmemorije (SQL Server međuspremnik skup proširenje i Premium prostora za pohranu čitanje predmemorije) koristite lokalni disk od čvorove računalnim.
Dodatne informacije o ta je funkcija Provjerite ove dokumentacije: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Sigurnosno kopiranje i vraćanje zahtjevi za SQL Server
Prilikom implementacije sustava SQL Server u Azure vaše sigurnosne kopije methodology morate pregledati. Čak i ako sustav nije produktivni sustava, SAP bazi podataka SQL poslužitelju mora biti redovito sigurnosno kopiraju. Budući da Azure prostora za pohranu zadržava tri slike, sigurnosnu kopiju sada je manje važna u odnosu na Kompenziranje neočekivanog prostora za pohranu. Prioritet razloga za održavanje proper tarifu za sigurnosno kopiranje i vraćanje je više možete vaše pogrešaka logičke/ručno unosom točke u vrijeme mogućnosti za oporavak. Da bi se cilj je ili sigurnosne kopije koristite da biste vratili bazu podataka natrag u određenom trenutku u vremenu ili pomoću sigurnosnih kopija u Azure seed neki drugi sustav kopiranjem postojeće baze podataka. Na primjer, nije moguće prenosite s konfiguraciju SAP 2 sloja 3 sloja sustava postavljanje isti sustava vraćanjem sigurnosnu kopiju.

Postoje tri različite načine sigurnosne kopije sustava SQL Server Azure spremište:
 
1. SQL Server 2012 CU4 i noviji mogu nativno sigurnosnu kopiju baze podataka da bi URL-a. To je detaljan na blogu [nove funkcije u SQL Server 2014. – dio 5 – sigurnosnog kopiranja i vraćanja poboljšanja](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Potražite u članku poglavlja [CU4 programa SQL Server 2012 SP1 ili noviji] [dbms – vodič za-5.5.1].
1. Izdanja sustava SQL Server prije no što SQL 2012 CU4 pomoću funkcija preusmjeravanja možete napraviti sigurnosnu kopiju da biste na VHD i zapravo premještanje tok zapisivanja pri programa Azure mjesta za pohranu koji je konfiguriran. U odjeljku poglavlja [SQL Server 2012 SP1 CU3 i prethodnih izdanja] [dbms – vodič za-5.5.2].
1. Konačni način je sigurnosno kopiranje običan SQL Server na disku naredbu na disku uređaj VHD.  To je jednako uzorak lokalne implementacije i se spominju u ovom dokumentu.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 i novijim verzijama
To vam omogućuje da biste izravno kopija spremište blobova PLATFORME Azure. Bez koristite taj način, mora se sigurnosno druge Azure VHDs koji želite trošiti VHD i IOPS kapacitetu. Ideja je sljedeće:
 
 ![Korištenje sigurnosnih kopija SQL Server 2012 Microsoft Azure spremište blobova PLATFORME][dbms-guide-figure-400]

Prednost je u tom slučaju nešto ne morate provesti VHDs za spremanje sigurnosne kopije sustava SQL Server na. Tako da imate manje VHDs dodijeliti i cijeli propusnost VHD IOPS mogu se koristiti za podatke i datoteka zapisnika. Uzmite u obzir Maksimalna veličina sigurnosnu kopiju je ograničeno na najviše 1 TB kao navedenih u odjeljku "Ograničenja" u ovom članku: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Ako je veličine sigurnosne kopije, bez obzira na pomoću SQL Server sigurnosne sažimanja prekoračuje 1 TB u veličine funkcionalnost opisane u poglavlja [SQL Server 2012 SP1 CU3 i prethodnih izdanja] [dbms – vodič za-5.5.2] u ovom dokumentu treba koristiti.

[Dokumentacija povezane](https://msdn.microsoft.com/library/dn449492.aspx) s opisom Vraćanje baze podataka iz sigurnosne kopije koristeći spremište blobova platforme Azure preporučujemo da ne da biste vratili izravno iz spremišta blobova PLATFORME Azure ako je sigurnosnih kopija > 25 GB. Preporuke u ovom članku jednostavno temelji se na pitanja vezana uz performanse, a ne zbog ograničenja funkcionirati. Dakle, različitim uvjetima može primijeniti na na pojedinačno.

Dokumentacija kako postaviti i leveraged ovu vrstu sigurnosne kopije može se pronaći [ovog](https://msdn.microsoft.com/library/dn466438.aspx) praktičnog vodiča
 
Primjer niza koraka može pročitati [ovdje](https://msdn.microsoft.com/library/dn435916.aspx).

Automatske sigurnosnog kopiranja je najveće važnosti da biste bili sigurni da su drugačije naziva blob-ova za svaku sigurnosnu kopiju. U suprotnom će se prebrisati i lanac Vrati se prekida.
 
Redoslijedom ne želite kombinirati tako što između 3 različite vrste sigurnosne kopije, preporučujemo da biste stvorili drugu spremnika ispod račun za pohranu koji se koristi za sigurnosne kopije. Spremnike može biti po VM samo ili po vrsti VM i sigurnosno kopiranje. Shema može izgledati kao što su:
 
 ![Pomoću SQL Server 2012 kopija Microsoft Azure spremište blobova PLATFORME – različite spremnika u odjeljku zaseban račun za pohranu][dbms-guide-figure-500]

U gornjem primjeru sigurnosnih kopija bi mogla izvršiti iste gdje su implementiran na VMs račun za pohranu. Bi li novi račun za pohranu za sigurnosnih kopija. Unutar računa za pohranu bi postojati različiti spremnici stvorili s matricom vrstu i naziv VM sigurnosne kopije. Takve segmente će olakšati administrirati sigurnosne kopije različitih VMs.

S blob-ova nešto izravno piše sigurnosne kopije, da ne dodajete broj VHDs na VM. Dakle nešto nije Maksimiziranje maksimalno VHDs postavljen od određene SKU VM za podatke i transakcije prijavi datoteku, a i dalje izvršiti sigurnosnu kopiju protiv spremnik za pohranu. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 i prethodnih izdanja
Prvi korak, morate izvršiti da biste postigli sigurnosnu kopiju izravno u odnosu na pohranu Azure bi da biste preuzeli msi koja je povezana s [članka KBA](https://www.microsoft.com/download/details.aspx?id=40740) .
 
Preuzimanje u x64 instalacijsku datoteku i u dokumentaciji. Datoteka će instalirati program pod nazivom: "Microsoft SQL Server sigurnosne kopije Microsoft Azure alat". Temeljito pročitajte dokumentaciju proizvoda.  Alat za zapravo funkcionira na sljedeći način:

* Sa strane SQL Server je definiran na disku mjesto za sigurnosne kopije sustava SQL Server (ne koristiti pogon D:\ za ovu).
* Alat će vam omogućiti da biste definirali pravila koja se može koristiti za usmjeravanje različite vrste sigurnosno kopiranje različite Azure potpisu.
* Kada su pravila na mjestu, alat bit ćete preusmjereni tok zapisivanja kopije na neki od VHDs/diskova za mjesto za pohranu Azure koja je definirana neke starije verzije.
* Alat će ostaviti small djelić datoteke nekoliko KB veličine na VHD/Disk koja je definirana za SQL Server sigurnosne kopije. **Na mjesto za pohranu trebala ostati tu datoteku budući da je potrebno da biste vratili ponovno iz spremišta Azure.**
    * Ako ste izgubili djelić datoteke (primjerice putem gubitak medij za pohranu koji se nalaze u datoteku), a koju ste odabrali mogućnost sigurnosno kopiranje s računom za spremište na platformi Microsoft Azure, djelić datoteke putem spremište na platformi Microsoft Azure može vratiti tako da preuzmete iz spremnik za pohranu u kojem je smjestiti. Zatim morate potvrditi djelić datoteke u mapu na lokalnom računalu koju je konfiguriran alata za otkrivanje i prenesite na istom kontejner s istu lozinku za šifriranje ako šifriranje koristila s izvornog pravila. 

To znači da shemi prethodno opisan za noviju izdanja sustava SQL Server možete staviti na mjestu kao i za izdanja sustava SQL Server koja ne dopuštaju Izravni adresu na mjesto za pohranu Azure.
 
Ovaj postupak ne treba koristiti s novija izdanja sustava SQL Server koja podržava sigurnosnom gore nativno protiv Azure prostora za pohranu. Iznimke su gdje ograničenja nativni sigurnosnu kopiju u Azure onemogućuju nativni sigurnosne kopije izvođenja u Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Mogućnosti za sigurnosne kopije baze podataka SQL Server
Mogućnosti za sigurnosne kopije baze podataka je da biste priložili dodatne VHDs VM koju koristite za spremanje sigurnosne kopije na. U tom slučaju morate da biste bili sigurni da se VHDs sustavom puno. Ako je to slučaj, trebali biste skidanje na VHD i tako da speak ' arhiviranje' ga i zamijeniti ga novi prazan VHD. Ako se prema dolje taj put, koje želite zadržati te VHDs u zasebnom računi servisa Azure prostora za pohranu od one koju VHDs s datoteke baze podataka.

Drugi moguće je koristiti velike VM koji mogu sadržavati mnogo VHDs priloženom. Npr. D14 s 32VHDs. Koristiti prostori za pohranu za izradu fleksibilne okruženje u kojem nije stvaranja zajedničke stavke koje se koriste pa kao sigurnosnu kopiju ciljnih web-mjesta za različite poslužitelje DBMS.
 
Imate nekoliko najboljih postupaka navedenih [u nastavku](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) kao i. 

#### <a name="performance-considerations-for-backupsrestores"></a>Pitanja vezana uz performanse za sigurnosno kopiranje/vraća
Kao implementacijama Gola metal sigurnosnog kopiranja i vraćanja performanse ovisi o količine koliko je moguće čitati paralelno i što bi moglo biti propusnost te količine. Osim toga, potrošnju procesora koristi sigurnosne kopije sažimanja možda isprobavati značajnu uloga na VMs samo do 8 niti procesora. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koristi radi pohrane podataka datoteka, što je manji cjelokupan propusnost u načinu za čitanje.
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja.
* Na manji broj ciljnih web-mjesta (blob-ova ili VHDs) za pisanje sigurnosnog kopiranja, manjeg propusnost.
* Što je manji na VM veličinu, manji kvotu za pohranu propusnost pisanje i čitanju Azure prostora za pohranu. Neovisno o tome je li sigurnosnih kopija izravno pohranjuju se na blobova platforme Azure ili pohranjene u VHDs ponovno pohranjene u Azure blob-ova.

Kada koristite Microsoft Azure spremište blobova PLATFORME kao cilj sigurnosnog kopiranja iz novija izdanja, vi ste ograničeni određivanje samo jedan ciljni URL-a za svaki određene sigurnosnu kopiju.
 
No kada koristite 'Microsoft SQL Server sigurnosnu kopiju alata za Microsoft Azure' u starijim verzijama, možete odrediti cilj više od jedne datoteke. S više od jedne ciljne mogu mijenjati veličinu sigurnosnog kopiranja i veća propusnost sigurnosnu kopiju. Rezultat pa u više datoteka kao i u račun za Azure prostora za pohranu. U našem testirati pomoću više datoteka odredišta nešto postigli sigurno propusnost koje nešto nije moguće postići pomoću sigurnosnog kopiranja proširenja implementirana u iz sustava SQL Server 2012 SP1 CU4 na. Ste i ne blokira ograničenje 1TB kao nativni sigurnosnu kopiju u Azure.

Međutim, imajte na umu, propusnost i ovisi o mjestu račun za Azure prostora za pohranu koji koristite za stvaranje sigurnosne kopije. Da biste pronašli račun za pohranu u nekoj drugoj regiji, nego na VMs pokrenutih u mogu se ideju. Npr. želite pokrenuti VM konfiguraciju u Europi za regije zapada, ali postavite račun za pohranu koji koristite sigurnosne kopije odnosu u Sjevernoj Europe. Certainly koji će imati utjecaj na propusnost sigurnosne kopije i nije vjerojatno da biste generirali propusnost 150MB/sec kao čini se da je moguće u slučajevima gdje radi cilj prostora za pohranu i na VMs u istom regionalnog podatkovnog centra.

#### <a name="managing-backup-blobs"></a>Upravljanje sigurnosne kopije blob-ova
Postoji zahtjev za upravljanje sigurnosno kopiranje vlastite. Budući da u očekivanja je da mnoge blob-ova mogu stvarati izvršavanjem Česti transakcije zapisnika kopija, Administracija te blob-ova jednostavno možete opteretiti Portal za Azure. Stoga je recommendable odražava Explorer za pohranu Azure. Postoji nekoliko dobar onima dostupnima koji omogućuje upravljanje račun za Azure prostora za pohranu

* Microsoft Visual Studio sa Azure SDK instaliran (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure prostora za pohranu Explorer (<https://azure.microsoft.com/downloads/>)
* 3 Alati za zabavu

[komentar]: <>  (Još nisu podržane u OBLAK)
[komentar]: <>  (### Sigurnosne kopije azure VM)
[komentar]: <>  (VMs sustava SAP-a mogu se sigurnosno pomoću funkcije sigurnosne kopije Azure virtualnog računala. Azure virtualnog računala sigurnosne kopije imate uvedene na početku godine 2015, a u međuvremenu je standardni način za sigurnosno kopiranje dovrši VM u Azure. Azure sigurnosne kopije pohranjuje sigurnosnih kopija u Azure i omogućuje vraćanje od na VM ponovno.) 
[komentar]: <> (VMs koji pokretanja baze podataka mogu se sigurnosno dosljedan način kao i ako DBMS sustave podržava VSS Windows (glasnoću sjene Kopiraj servis - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>) kao npr SQL Server. Pomoću sigurnosnog kopiranja Azure VM može biti način da biste pristupili restorable sigurnosnu kopiju baze podataka za SAP-a. Međutim, imajte na umu koji na temelju sigurnosne kopije Azure VM vraća točke u vrijeme baza podataka nije moguće. Zbog toga u preporuka je da biste izvršili sigurnosne kopije baze podataka pomoću funkcije DBMS tipkanje sigurnosne kopije VM Azure). [komentar]: <> (upoznati s sigurnosne kopije virtualnog računala Azure započnite ovdje <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Korištenje sustava SQL Server slike iz trgovine Microsoft Azure Marketplace
Microsoft nudi VMs trgovine Windows Azure koje već sadrže verzijama sustava SQL Server. SAP-a u slučaju korisnika kojima je potreban licence za SQL Server i Windows to može biti priliku zapravo pokrivaju potrebe za licence adrese e gore VMs sa sustavom SQL Server već instaliran. Da biste koristili takve slike SAP, morate izvršiti Imajte na umu sljedeće:

* Nisu procjenu verzijama sustava SQL Server: D5 veće troškove od samo na "Samo za Windows" VM implementiran iz trgovine Azure Marketplace. Potražite u sljedećim člancima da biste usporedili cijene: <https://azure.microsoft.com/pricing/details/virtual-machines/> i <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Možete koristiti samo izdanja sustava SQL Server koja podržava SAP, kao što je SQL Server 2012.
* Razvrstavanje instancu sustava SQL Server koji se instalira u VMs koje se nude Azure Marketplace nije razvrstavanje SAP NetWeaver zahtijeva instancu sustava SQL Server da biste pokrenuli. Iako je s uputama u sljedećem odjeljku možete promijeniti na razvrstavanje.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Promjena SQL Server razvrstavanje sustava Microsoft Windows i SQL Server VM
Budući da se slika sustava SQL Server u trgovine Windows Azure niste postavili za korištenje razvrstavanje koji je potreban SAP NetWeaver aplikacije, je potrebno promijeniti odmah nakon uvođenje. Za SQL Server 2012, to možete učiniti sa sljedećim koracima čim se na VM implementiran i administrator je mogli prijaviti distribuiranih VM:

* Otvorite prozor naredbenog Windows "kao administrator".
* Promijenite imeniku c:\Programske datoteke\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Izvršavanje naredbe: Setup.exe /QUIET /ACTION = REBUILDDATABASE /INSTANCENAME = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> je račun koja je definirana kao administratorski račun pri implementaciji sustava VM prvi put putem galerije.

Postupak traje samo nekoliko minuta. Da biste provjerili hoće li se korak završen prema gore s ispravan rezultat, poduzmite sljedeće korake:

* Otvorite SQL Server Management Studio.
* Otvorite prozor upit.
* Izvršavanje naredbe sp_helpsort u glavnom bazom podataka sustava SQL Server.

Željeni rezultat trebao bi izgledati kao što su:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Ako to ne rezultat, ZAUSTAVITE implementacija SAP i istražiti Zašto naredbu instalacijski program ne funkcioniraju prema očekivanjima. Implementaciju aplikacija za SAP NetWeaver na instancu sustava SQL Server s različitim Rfc2047 SQL Server od onog spomenute iznad **nisu** podržani.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server visoku dostupnost za SAP servisu Azure
Kao što je rečeno ranije u ovom dokumentu nema nema mogućnost da biste stvorili zajednički prostor za pohranu koji je potreban za korištenje najstarije funkcionalnost visoke dostupnosti SQL Server. Ta je funkcija želite instalirati SQL Server instancama dva ili više u programa Windows Server prebacivanje klaster (WSFC) pomoću zajedničkog diska za korisnika baze podataka (i naposljetku tempdb). To je način standardne visoke dostupnosti dugo koju i podržava SAP-a. Budući da Azure ne podržavaju zajedničkog prostora za pohranu, SQL Server visoke dostupnosti konfiguracije konfiguraciji klaster zajedničke disk ne Rač. Međutim, mnoge druge načine visoke dostupnosti i dalje moguće i su opisani u sljedećim odjeljcima.

[komentar]: <>  (Članak je i dalje refering ASM)
[komentar]: <>  (Prije nego što je moguće koristiti za SQL Server u Azure, tehnologije različite određene visoke dostupnosti za čitanje je odlično dokumenta koja daje više pojedinosti i pokazivači [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>Dostave zapisnika sustava SQL Server
Jedan od načina visoke dostupnosti (HA) je dostavu zapisnika sustava SQL Server. Ako VMs sudjelovanje u konfiguraciji HA rad razlučivanje naziva, postoji nema problema, a postavljanje u Azure će razlikovati od bilo kojeg postavljanje koje obavlja lokalnog. Ne preporučuje se oslanjate IP razlučivost samo. Provjerite ove dokumentacije u vas pozdravljamo postavljanje dostavu zapisnika i načela oko dostavu zapisnika:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Da bi doista postigli sve visoke dostupnosti, nešto treba uvesti VMs koje su unutar takvo zapisnika dostavu konfiguraciju biti u istoj skupu za Azure dostupnost.

#### <a name="database-mirroring"></a>Baze podataka
Mirroring bazu podataka kao podržava SAP-a (vidi bilješku SAP [965908]) ovisi definiranje partnera prebacivanje u nizu za povezivanje SAP-a. Za predmete – lokacija smo pretpostavlja da su dva VMs u istu domenu da kontekst korisnika sustava SQL Server se izvode dvije instance u odjeljku se kao i korisnici domene i imati ovlasti u dvije instance sustava SQL Server koji je uključen. Stoga postavkama baze podataka sustava u Azure ne razlikuju uobičajeni lokalne instalacije/konfiguracije.

Nakon implementacije samo oblak Najlakši način je da bi drugi postavljanja domene u Azure da bi se one DBMS VMs (i najbolje namjenski VMs SAP-a) unutar jedne domene.

Ako domena nije moguće, jedan možete koristiti certifikata za bazu podataka zrcaljenje krajnje točke, kao što je opisano u nastavku: <https://technet.microsoft.com/library/ms191477.aspx>

Vodič za postavljanje međuverzije baze podataka sustava u Azure nalazi se ovdje: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Kao što je podržano AlwaysOn za SAP lokalnog (vidi bilješku SAP [1772688]), podržana je koja će se koristiti u kombinaciji s SAP-a u Azure. Fact niste omogućite stvaranje zajedničkog diskova u Azure ne znači da nešto nije moguće stvoriti u konfiguraciji AlwaysOn Windows Server prebacivanje klaster (WSFC) između različitih VMs. Samo znači da nemate mogućnost da biste koristili zajedničke disk kao kvorum u konfiguraciji klaster. Dakle sastavljanje programa AlwaysOn WSFC konfiguraciju u Azure i jednostavno ne odaberite željenu vrstu kvorum koristi zajednički na disku. Okruženje za Azure te VMs su uveden u treba Razriješi VMs po imenu i na VMs mora biti u istoj domeni. To vrijedi samo Azure i implementacijama više lokacija. Postoje neke posebne napomene oko implementaciji sustava SQL Server dostupnost grupe ga Slušatelj (a ne bude zbuniti pomoću postavite Azure dostupnost) jer Azure tom trenutku u trenutku ne dopušta jednostavno stvoriti AD-DNS objekt kao što je moguće lokalnog. Stoga su neke druge instalacije korake potrebne da bi se prevladala određene ponašanje Azure.

Što valja pomoću programa ga Slušatelj grupe dostupnosti su:

* Korištenje programa ga Slušatelj grupe dostupnosti moguće je samo sa sustavom Windows Server 2012 ili Windows Server 2012 R2 kao gost OS na VM. Za Windows Server 2012 potrebno da biste bili sigurni da se primjenjuje tu zakrpu: <https://support.microsoft.com/kb/2854082> 
* Za Windows Server 2008 R2 tu zakrpu ne postoji i AlwaysOn morat ćete koristiti na isti način kao baze podataka navođenjem partnera prebacivanje u nizu veze (izvršiti putem SAP default.pfl parametar dbs/mss/poslužitelju – pogledajte SAP bilješke [965908]).
* Prilikom korištenja programa ga Slušatelj grupe dostupnosti VMs baze podataka morate biti povezani s namjenski opterećenja. Razrješavanje imena u samo oblak implementacijama ili je potrebna sve VMs sustava SAP-a (poslužitelja aplikacije, DBMS poslužitelja i (A) SCS poslužitelja) u istom virtualne mreže ili je potrebna iz programa SAP aplikacijskom sloju održavanja etc\host datoteku da biste dobili imena VM VMs poslužitelja SQL riješiti. Da biste izbjegli Azure je Dodjela nove IP adrese u slučajevima gdje oba VMs incidentally su isključivanje, jedan treba dodijeliti statičke IP adrese sučelje mreže onih VMs u konfiguraciji AlwaysOn (definiranje statičke IP adrese je opisano u [ovom] [ virtual-networks-reserved-private-ip] članak) [komentar]: <> (stari blogovi) [komentar]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Postoje posebne korake potrebne za stvaranje konfiguracije klaster WSFC gdje treba klaster posebno IP adresa dodijeljena jer Azure trenutni funkcionalnošću želite dodijeliti naziv klaster istu IP adresu kao što je stvoreno čvor klaster. To znači da ručno korak moraju izvršiti da biste dodijelili drugu IP adresu klaster.
* Dostupnost grupe ga Slušatelj će stvorena u Azure pomoću krajnje točke TCP/IP koje su dodijeljene u VMs izvodi primarnih i sekundarnih replike grupe dostupnosti.
* Možda postoje potrebe za sigurnu te krajnje točke s ACL-a.

[komentar]: <>  (Popis obveza stare bloga)
[komentar]: <>  (Detaljne upute i necessities instaliranja programa AlwaysOn konfiguracije na Azure su najbolje iskustvom kada prolaska kroz vodiča dostupne [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[komentar]: <>  (Postavljanje unaprijed konfigurirane AlwaysOn putem galerije Azure < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[komentar]: <>  (Stvaranje programa ga Slušatelj grupe dostupnosti je najbolje što je opisano u vodiču za [this][virtual-machines-windows-classic-ps-sql-int-listener])
[komentar]: <>  (Krajnje točke zaštita mreže s ACL-a su objašnjene najbolje ovdje:)
[komentar]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[komentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[komentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[komentar]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Moguće je uvesti grupe dostupnosti AlwaysOn programa SQL Server putem kao i različite regije Azure. Ta je funkcija će pod utjecajem povezivost Azure VNet-na-Vnet ([više detalja][virtual-networks-configure-vnet-to-vnet-connection]).
[komentar]: <> (obveze staru blog) [komentar]: <> (postavljanja sustava SQL Server AlwaysOn dostupnost grupe u takvim scenariju Ovdje se opisuje: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Sažetak na SQL Server visoke dostupnosti servisu Azure
Uz fact Azure prostora za pohranu štiti sadržaja, postoji jedan manje od razloga za insist na sliku tipkovne čekanja. To znači da scenariju visoke dostupnosti treba samo zaštitili u sljedećim slučajevima:

* Skupine nedostupnosti na VM cijela zbog održavanja na poslužitelju u Azure ili drugih razloga
* Problemi sa softverom u instancu sustava SQL Server
* Zaštita od ručnog pogreške koje se briše podataka i potreban je oporavak točke u vrijeme

Pogled na odgovarajuće tehnologije nešto možete argue da se prve dvije slučajevima može prekriveno skupovima baze podataka ili AlwaysOn, dok treći slučaj samo mogu biti prekriveno skupovima zapisnika dostave.

Morat ćete saldo složenije postavljanje AlwaysOn, usporedbi baza podataka zrcaljenja, s prednosti AlwaysOn. Te prednosti može biti navedena kao što su:

*   Čitljive sekundarnog replike.
*   Sigurnosno kopiranje s sekundarnog replike.
*   Bolje skalabilnost.
*   Više od jedne sekundarnog replike.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Općenito SQL Server za SAP-a na Azure sažetka
Nema više preporuke u ovom vodiču za i preporučujemo da čitate je više puta prije planiranja implementaciju sustava Azure. Općenito govoreći, no svakako slijedite deset Općenito DBMS na Azure određene trenutke:

[komentar]: <>  (2.3 veću propusnost nego što? Od jednog VHD?)
1. Koristiti i najnovije izdanje DBMS, kao što je SQL Server 2014., koja ima većinu prednosti u Azure. Za SQL Server, to je SQL Server 2012 SP1 CU4 koji sadrži značajku sigurnosnom s programima Azure prostora za pohranu. Međutim, zajedno s SAP bi preporučujemo da barem SQL Server 2014 SP1 CU1 ili SQL Server 2012 SP2 i najnovije CU.
1. Pažljivo planiranje vaše pejzažno sustava SAP Azure da biste saldo raspored podataka datoteke i Azure ograničenja:
    * Ne sadrže previše VHDs, ali nemate dovoljno da biste bili sigurni da dođete na potrebna IOPS.
    * Imajte na umu da su IOPS i ograničeni po računu Azure prostora za pohranu i da su računi za pohranu ograničeni unutar svake Azure pretplate ([više detalja][azure-subscription-service-limits]). 
    * Samo pruga preko VHDs ako vam je potrebna da biste postigli veću propusnost.
1. Nikad ne instalirati softver ili postavljanje datoteke koje je potrebno postojanost na disku D:\ kao što je – stalno i ništa na pogonu izgubit će se na ponovnog pokretanja sustava Windows.
1. Nemojte koristiti Azure VHD predmemoriju za Azure standardne prostora za pohranu.
1. Nemojte koristiti Azure zemlj replicirati račune za pohranu.  Korištenje lokalno suvišnih radnih opterećenja DBMS.
1. Koristite DBMS dobavljača HA/DR rješenja za replikaciju podacima iz baze podataka.
1. Uvijek koristi naziv razlučivost, nemojte koristiti IP adrese.
1. Koristite najviše moguće sažimanja baze podataka. Za SQL Server to je stranica sažimanja.
1. Budite oprezni korištenju sustava SQL Server slika iz trgovine Azure Marketplace. Ako koristite SQL Server jednu, morate promijeniti razvrstavanje instancu prije instalacije sistemske NetWeaver SAP-a na njega.
1. Instaliranje i konfiguriranje na SAP glavno računalo za nadzor za Azure kao što je opisano u [vodič za implementaciju] [– vodič za implementaciju].

## <a name="specifics-to-sap-ase-on-windows"></a>Specifičnosti za SAP elika i mala slova u sustavu Windows
Počevši od sustava Microsoft Azure, možete jednostavno migrirati postojeće SAP elika i mala slova aplikacije za Azure virtualnih računala. SAP elika i mala slova u virtualnog računala omogućuje vam da jednostavno migracije te aplikacije za Microsoft Azure smanjiti ukupni trošak vlasništvo nad implementaciju, upravljanje i održavanje breadth aplikacijama. S SAP elika i mala slova u programa Azure virtualnog računala, administratorima i razvojnim inženjerima i dalje možete koristiti isti razvoja i administracije alatima koji su dostupni na lokalni.

Postoji SLA za na virtualnim računalima sustava Azure koji se nalazi se ovdje: <https://azure.microsoft.com/support/legal/sla>

Ne možemo sigurni da se mogu razlikovati Microsoft Azure glavnom računalu virtualnim strojevima vrlo dobro će izvesti usporedbi s drugim javnim oblaka virtualizacije ponude, ali pojedinačne rezultate. Promjenu veličine SAPS brojeve SAP različite SAP potvrđen VM SKU-ove objavit ćemo zajedno u zasebnom SAP bilješke [1928533].

Naredbe i preporuke o korištenje Azure prostora za pohranu, VMs za implementaciju SAP-a ili SAP nadzor primjenjuju se na implementacijama sustava SAP elika i mala slova u kombinaciji s aplikacijama SAP-a prema uputama u prve četiri poglavlja ovog dokumenta.

### <a name="sap-ase-version-support"></a>SAP elika i mala slova podrška 
SAP trenutno podržava SAP elika i mala slova verzije 16.0 za korištenje s proizvodima paket servisa Business SAP-a. Sva ažuriranja za poslužitelj za SAP elika i mala slova ili JDBC i ODBC upravljačke programe za uporabu s proizvodima paket servisa Business SAP služe isključivo putem servisa Marketplace SAP pri: <https://support.sap.com/swdc>.

Kao instalacije lokalnim mogao preuzeti ažuriranja za poslužitelj SAP elika i mala slova ili upravljačke programe za JDBC i ODBC izravno iz Sybase web-mjesta. Detaljne informacije o zakrpa koje podržava za korištenje paketa tvrtke SAP proizvodima lokalnog sustava i u Azure virtualnim računalima potražite u članku sljedeće SAP bilješke:

* [1590719]
* [1973241]
 
Opće informacije o pokretanju paket servisa Business SAP-a na elika i mala slova SAP-a nalazi se u [SKN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Smjernice za konfiguraciju elika i mala slova SAP-a za SAP povezane SAP instalacije elika i mala slova u Azure VMs

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura implementacije SAP elika i mala slova
Skladu Opći opis izvršne datoteke SAP elika i mala slova mora biti nalazi ili instalirali u sistemski pogon na VM osnovni VHD (pogon c:\). Obično Većina baza podataka sustava i Alati za SAP elika i mala slova se ne zaista leveraged konačnog po radno opterećenje NetWeaver SAP-a. Dakle sustav i Alati baze podataka (matrice, model, saptools, sybmgmtdb, sybsystemdb) može ostati na na C:\drive. 

Iznimku može biti privremenu bazu podataka koja sadrži sve tablice rad i privremenih tablica stvorio SAP elika i mala slova, koji se u slučaju neke ERP SAP i svih radnih opterećenja BW možda će biti potrebno ili noviji podataka ili/i operacije jedinice koji ne stane u izvornom VM osnovni VHD (pogon c:\).
 
Ovisno o verziji SAPInst/SWPM koristili za instaliranje sustava, baza podataka možda sadrži:

* Jedan tempdb SAP elika i mala slova koji je stvorio prilikom instalacije SAP elika i mala slova
* Programa SAP elika i mala slova tempdb stvorio prilikom instalacije SAP elika i mala slova i dodatnih saptempdb koji se stvorila Rutina instalacija SAP-a
* Programa SAP elika i mala slova tempdb stvorio prilikom instalacije SAP elika i mala slova i dodatnih tempdb stvorene ručno (npr. sljedeći SAP bilješke [1752266]) da biste zadovoljava preduvjete za određene tempdb ERP/BW

U slučaju određene ERP ili svih radnih opterećenja BW smisla, o performansama da biste zadržali uređaji za tempdb dodatno stvoren tempdb (SWPM ili ručno) na pogon koji nije C:\. ako postoji bez dodatnih tempdb se preporučuje se da biste stvorili (SAP bilješke [1752266]).

Za takvi sustavi trebaju izvesti sljedeće korake uz stvoren tempdb:

* Premještanje prvi uređaj tempdb prvi uređaj za SAP bazi podataka
* Dodavanje tempdb uređaja za svaki od VHDs koji sadrži uređaji SAP baze podataka

Tu konfiguraciju omogućuje tempdb ili trošiti dodatni prostor od osigurati sistemski pogon. Kao referenca nešto možete provjeriti uređaj veličine tempdb na postojeću sustavima koji se izvode na lokalni. Ili takve konfiguracije želite omogućiti IOPS brojeva na temelju tempdb koje nije moguće dao sistemski pogon. Ponovno sustavima koji su pokrenuti na lokalni može se koristiti za praćenje radno opterećenje/i protiv tempdb.

Nikad ne postavite uređaje elika i mala slova SAP-a na pogon D:\ na VM. To se odnosi na tempdb, čak i ako su samo privremene objekte koji se sinkroniziraju s tempdb.

#### <a name="impact-of-database-compression"></a>Utjecaj sažimanja baze podataka
U konfiguracijama gdje/i propusnosti može postati ograničavanje faktor svaki mjere koji smanjuje IOPS može pomoći na rastezanje radno opterećenje nešto jer se može pokrenuti u scenariju programa IaaS kao što su Azure. Stoga preporučuje da biste provjerili koristi li se elika i mala slova SAP sažimanje prije prijenosa postojeću bazu podataka SAP-a za Azure.

Preporuke za izvođenje spajanja prije prijenosa Azure ako već nije implementirana dobiva se iz nekoliko razloga:

* Količina podataka koje je moguće prenijeti Azure je manja
* Sažimanje izvođenja traje kraće uz pretpostavku da se nešto možete koristiti jači hardver s više procesora ili noviji/i propusnosti ili manje/i Latencija lokalnog
* Manje veličine baze podataka mogu dovesti do manji trošak dodijeljeni na disku

Podaci i LOB sažimanja raditi u VM hostirane u Azure virtualnim strojevima kao lokalne. Dodatne informacije o tome kako provjeriti jesu li sažimanja već se koristi u postojeću bazu podataka SAP elika i mala slova provjerite je li bilješke SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Korištenje DBACockpit praćenje instanci baze podataka
U DBACockpit je sustavima SAP koji koristite SAP elika i mala slova kao platformu baze podataka, dostupni kao uloženog prozore transakcijom DBACockpit ili Webdynpro. No punu funkcionalnost telefona za nadzor i administratore baze podataka dostupna je u implementaciji Webdynpro DBACockpit samo.

Kao sa sustavima lokalnog nekoliko koraka moraju sve SAP NetWeaver funkcija koristi Webdynpro implementacije u DBACockpit omogućiti. Slijedite SAP bilješke [1245200] da biste omogućili korištenje webdynpros i generiranje potrebna one. Kada se uputama u odjeljku iznad bilješke će konfigurirati i Upravitelja internetskih komunikacija (icm) uz priključke koja će se koristiti za http i https veze. Zadane postavke za http izgleda ovako:

> icm/server_port_0 = PROT = HTTP, PRIKLJUČAK = 8000, PROCTIMEOUT = 600, vremenskog ograničenja = 600
>
> icm/server_port_1 = PROT = HTTPS, PRIKLJUČAK 443$ $, PROCTIMEOUT = = 600, vremenskog ograničenja = 600

i veza koje generira transakcijom DBACockpit izgledat će ovako:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Ovisno o li i kako virtualnog računala Azure hostiranje sustava SAP je povezan putem web-mjesta-na-web-mjesta, više web-mjesta ili ExpressRoute (unakrsno lokalnu implementaciju) morate provjerite je li taj ICM koristi potpuno kvalificiran naziv glavnog računala koja se može riješiti na računalu koju pokušavate otvoriti DBACockpit iz. Pogledajte bilješke SAP [773830] da biste razumjeli kako ICM određuje na potpuno kvalificirani naziv glavnog računala ovisno o parametrima profila i postavljanje icm host_name_full parametar izričito ako je potrebno.

Ako implementiran VM u scenariju samo oblak bez više lokacija veze između lokalnog poslužitelja i Azure, morate definirati javnu IP adresa i u domainlabel. Oblikovanje javno DNS naziva u VM pa će izgledati ovako:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Dodatne informacije vezane uz ime DNS-a nalazi se [ovdje][virtual-machines-azurerm-versus-azuresm].

Postavljanje na SAP profila parametar icm/host_name_full DNS naziva VM Azure vezu može izgledati slično:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

U ovom slučaju morate obavezno:

* Dodavanje ulazna pravila sigurnosne grupe mreže na portalu Azure TCP/IP priključke koji se koristi za komunikaciju s ICM
* Dodavanje ulazna pravila za konfiguriranje vatrozida za Windows za TCP/IP priključke za komunikaciju na ICM

Za programa automatiziranog uvesti ispravaka sve dostupne, preporučuje se povremeno primijeniti ispravak zbirke SAP bilješke odnosi se na vašu verziju SAP:

* [1558958]
* [1619967]
* [1882376]

Dodatne informacije o DBA Cockpit za elika i mala slova SAP-a nalazi se u sljedećim SAP bilješke:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Sigurnosno kopiranje i vraćanje zahtjevi za SAP elika i mala slova
Kada implementacija SAP elika i mala slova u Azure vaše sigurnosne kopije methodology morate pregledati. Čak i ako sustav nije produktivni sustava, hostira tvrtka SAP elika i mala slova SAP bazi podataka mora biti redovito sigurnosno kopiraju. Budući da Azure prostora za pohranu zadržava tri slike, sigurnosnu kopiju sada je manje važna u odnosu na Kompenziranje neočekivanog prostora za pohranu. Primarni razlog za održavanje proper tarifu za sigurnosno kopiranje i vraćanje je više možete vaše pogrešaka logičke/ručno unosom točke u vrijeme mogućnosti za oporavak. Da bi se cilj je ili sigurnosne kopije koristite da biste vratili bazu podataka natrag u određenom trenutku u vremenu ili pomoću sigurnosnih kopija u Azure seed neki drugi sustav kopiranjem postojeće baze podataka. Na primjer, nije moguće prenosite s konfiguraciju SAP 2 sloja 3 sloja sustava postavljanje isti sustava vraćanjem sigurnosnu kopiju.

Sigurnosno kopiranje i vraćanje baze podataka u Azure funkcionira na isti način kao i lokalne. Pročitajte članak SAP bilješke:

* [1588316]
* [1585981]

informacije o stvaranju konfiguracije Ispis i planiranje sigurnosnih kopija. Ovisno o strategije i potrebama možete konfigurirati baze podataka i prijaviti se ako ste na disk na jedan od postojećih VHDs ili dodajte dodatne VHD sigurnosne kopije.  Da biste smanjili opasnost od gubitka podataka u slučaju pogreške preporučuje se korištenje VHD na kojem se nalazi datoteka nema baze podataka.

Osim podataka i LOB sažimanja elika i mala slova SAP-a nudi sigurnosne kopije sažimanja. Proširiti manje prostora s ispisi baze podataka i prijaviti se preporučuje se korištenje sigurnosne kopije sažimanja. Pogledajte bilješke SAP [1588316] dodatne informacije. Sažimanje sigurnosne kopije je presudne da biste smanjili količinu podataka koji će se prenijeti Ako planirate preuzimanje kopije ili VHDs koja sadrži sigurnosne kopije ispisi iz Azure virtualnog računala na lokalni.

Nemojte koristiti pogon D:\ kao baze podataka ili zapisnik odredišta za ispis.

#### <a name="performance-considerations-for-backupsrestores"></a>Pitanja vezana uz performanse za sigurnosno kopiranje/vraća
Kao implementacijama Gola metal sigurnosnog kopiranja i vraćanja performanse ovisi o količine koliko je moguće čitati paralelno i što bi moglo biti propusnost te količine. Osim toga, potrošnju procesora koristi sigurnosne kopije sažimanja možda isprobavati značajnu uloga na VMs samo do 8 niti procesora. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koristi za pohranu uređaja baze podataka, manji cjelokupan propusnost u načinu za čitanje
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja
* Manje ciljevi (pruga direktorija, VHDs) za pisanje sigurnosnog kopiranja, manjeg propusnost

Da biste povećali broj ciljnih web-mjesta za pisanje postoje dvije mogućnosti koje mogu koristiti/kombinirati ovisno o vašim potrebama:

* Striping glasnoće cilj sigurnosnog kopiranja preko više postavljenu VHDs da biste poboljšali propusnost IOPS na toj prugastim jedinici
* Stvaranje ispisa konfiguraciju na razini SAP elika i mala slova koji koristi više od jedne ciljne direktorija za pisanje ispis da biste

Striping jedinica preko više postavljenu VHDs sadrži je spominju ranije u ovom vodiču. Dodatne informacije o korištenju više direktorija u konfiguraciji ispis SAP elika i mala slova potražite u dokumentaciji na sp_config_dump pohranjene Procedure koji se koristi za stvaranje konfiguracije ispis na [Informacijski centar Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Izrada oporavak s Azure VMs

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikacija podataka s poslužiteljem za replikaciju Sybase SAP-a
S elika i SAP Server replikacije SAP Sybase (SRS) u mala slova nudi Toplo čekanja rješenje da biste prenijeli transakcije baze podataka u daleka mjesto asinkrono. 

Instalacija i operacija SRS funkcionira kao i functionally VM smješten u servisa Azure virtualnog računala kao lokalnog.

HADR elika i mala slova putem poslužitelja za replikaciju SAP planu je s buduće izdanje. Bit će s i objavio platforme Microsoft Azure čim postane dostupan.

## <a name="specifics-to-sap-ase-on-linux"></a>Specifičnosti za SAP elika i mala slova na Linux

Počevši od sustava Microsoft Azure, možete jednostavno migrirati postojeće SAP elika i mala slova aplikacije za Azure virtualnih računala. SAP elika i mala slova u virtualnog računala omogućuje vam da jednostavno migracije te aplikacije za Microsoft Azure smanjiti ukupni trošak vlasništvo nad implementaciju, upravljanje i održavanje breadth aplikacijama. S SAP elika i mala slova u programa Azure virtualnog računala, administratorima i razvojnim inženjerima i dalje možete koristiti isti razvoja i administracije alatima koji su dostupni na lokalni.

Za implementaciju Azure VMs važno je znati službeni SLA koji se nalazi se ovdje: <https://azure.microsoft.com/support/legal/sla>

Informacije za promjenu veličine SAP i popis SAP certificirane VM SKU-ove objavit ćemo zajedno u bilješku SAP [1928533]. Dodatni SAP-a dokumenata za Azure virtualnog računala za promjenu veličine Ovdje možete pronaći <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> i ovdje <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Naredbe i preporuke o korištenje Azure prostora za pohranu, VMs za implementaciju SAP-a ili SAP nadzor primjenjuju se na implementacijama sustava SAP elika i mala slova u kombinaciji s aplikacijama SAP-a prema uputama u prve četiri poglavlja ovog dokumenta.

Sljedeća dva SAP bilješke obuhvaćaju opće informacije o elika i mala slova Linux i elika i mala slova u Oblaku:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>SAP elika i mala slova podrška 
SAP trenutno podržava SAP elika i mala slova verzije 16.0 za korištenje s proizvodima paket servisa Business SAP-a. Sva ažuriranja za poslužitelj za SAP elika i mala slova ili JDBC i ODBC upravljačke programe za uporabu s proizvodima paket servisa Business SAP služe isključivo putem servisa Marketplace SAP pri: <https://support.sap.com/swdc>.

Kao instalacije lokalnim mogao preuzeti ažuriranja za poslužitelj SAP elika i mala slova ili upravljačke programe za JDBC i ODBC izravno iz Sybase web-mjesta. Detaljne informacije o zakrpa koje podržava za korištenje paketa tvrtke SAP proizvodima lokalnog sustava i u Azure virtualnim računalima potražite u članku sljedeće SAP bilješke:

* [1590719]
* [1973241]
 
Opće informacije o pokretanju paket servisa Business SAP-a na elika i mala slova SAP-a nalazi se u [SKN](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Smjernice za konfiguraciju elika i mala slova SAP-a za SAP povezane SAP instalacije elika i mala slova u Azure VMs

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura implementacije SAP elika i mala slova
Skladu Opći opis izvršne datoteke SAP elika i mala slova mora biti nalazi ili instalirali u datotečnom sustavu korijenski VM (/sybase). Obično Većina baza podataka sustava i Alati za SAP elika i mala slova se ne zaista leveraged konačnog po radno opterećenje NetWeaver SAP-a. Dakle sustav i Alati baze podataka (matrice, model, saptools, sybmgmtdb, sybsystemdb) može ostati u korijen datotečnom sustavu kao i. 

Iznimku može biti privremenu bazu podataka koja sadrži sve tablice rad i privremenih tablica stvorio SAP elika i mala slova, koji se u slučaju neke ERP SAP i svih radnih opterećenja BW možda će biti potrebno ili noviji podataka ili/i operacije jedinice koji ne stane u izvornom VM OS disk.
 
Ovisno o verziji SAPInst/SWPM koristili za instaliranje sustava, baza podataka možda sadrži:

* Jedan tempdb SAP elika i mala slova koji je stvorio prilikom instalacije SAP elika i mala slova
* Programa SAP elika i mala slova tempdb stvorio prilikom instalacije SAP elika i mala slova i dodatnih saptempdb koji se stvorila Rutina instalacija SAP-a
* Programa SAP elika i mala slova tempdb stvorio prilikom instalacije SAP elika i mala slova i dodatnih tempdb stvorene ručno (npr. sljedeći SAP bilješke [1752266]) da biste zadovoljava preduvjete za određene tempdb ERP/BW

U slučaju određene ERP ili svih radnih opterećenja BW smisla, o performansama da biste zadržali uređaji za tempdb dodatno stvoren tempdb (SWPM ili ručno) u sustavu zasebne datoteke koje se može biti predstavljen jedan podatkovni Azure disk ili Linux RAID koje se protežu na više diskova Azure podataka. Ako postoji bez dodatnih tempdb ga preporučuje se da biste stvorili (SAP bilješke [1752266]).

Za takvi sustavi trebaju izvesti sljedeće korake uz stvoren tempdb:

* Premještanje prvi direktorija tempdb prvi datotečni sustav SAP baze podataka
* Dodavanje tempdb direktorija za svaki od VHDs koji sadrži datotečnom sustavu SAP baze podataka

Tu konfiguraciju omogućuje tempdb ili trošiti dodatni prostor od osigurati sistemski pogon. Kao referenca nešto možete provjeriti veličina direktorija tempdb na postojeću sustavima koji se izvode na lokalni. Ili takve konfiguracije želite omogućiti IOPS brojeva na temelju tempdb koje nije moguće dao sistemski pogon. Ponovno sustavima koji su pokrenuti na lokalni može se koristiti za praćenje radno opterećenje/i protiv tempdb.

Nikad ne postavite sve SAP elika i mala slova direktorija na /mnt ili /mnt/resource na VM. To se odnosi na tempdb, čak i ako su objekti znali što se tempdb samo privremene jer /mnt ili /mnt/resource je zadani Azure VM temp prostor koji nije trajni. Dodatne pojedinosti o prostora temp Azure VM pronaći ćete u [ovom članku][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Utjecaj sažimanja baze podataka
U konfiguracijama gdje/i propusnosti može postati ograničavanje faktor svaki mjere koji smanjuje IOPS može pomoći na rastezanje radno opterećenje nešto jer se može pokrenuti u scenariju programa IaaS kao što su Azure. Stoga preporučuje da biste provjerili koristi li se elika i mala slova SAP sažimanje prije prijenosa postojeću bazu podataka SAP-a za Azure.

Preporuke za izvođenje spajanja prije prijenosa Azure ako već nije implementirana dobiva se iz nekoliko razloga:

* Količina podataka koje je moguće prenijeti Azure je manja
* Sažimanje izvođenja traje kraće uz pretpostavku da se nešto možete koristiti jači hardver s više procesora ili noviji/i propusnosti ili manje/i Latencija lokalnog
* Manje veličine baze podataka mogu dovesti do manji trošak dodijeljeni na disku

Podaci i LOB sažimanja raditi u VM hostirane u Azure virtualnim strojevima kao lokalne. Dodatne informacije o tome kako provjeriti jesu li sažimanja već se koristi u postojeću bazu podataka SAP elika i mala slova provjerite je li bilješke SAP [1750510]. Pogledati i SAP bilješke [2121797] dodatne informacije o sažimanje baze podataka.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Korištenje DBACockpit praćenje instanci baze podataka
U DBACockpit je sustavima SAP koji koristite SAP elika i mala slova kao platformu baze podataka, dostupni kao uloženog prozore transakcijom DBACockpit ili Webdynpro. No punu funkcionalnost telefona za nadzor i administratore baze podataka dostupna je u implementaciji Webdynpro DBACockpit samo.

Kao sa sustavima lokalnog nekoliko koraka moraju sve SAP NetWeaver funkcija koristi Webdynpro implementacije u DBACockpit omogućiti. Slijedite SAP bilješke [1245200] da biste omogućili korištenje webdynpros i generiranje potrebna one. Kada se uputama u odjeljku iznad bilješke će konfigurirati i Upravitelja internetskih komunikacija (icm) uz priključke koja će se koristiti za http i https veze. Zadane postavke za http izgleda ovako:

> icm/server_port_0 = PROT = HTTP, PRIKLJUČAK = 8000, PROCTIMEOUT = 600, vremenskog ograničenja = 600
>
> icm/server_port_1 = PROT = HTTPS, PRIKLJUČAK 443$ $, PROCTIMEOUT = = 600, vremenskog ograničenja = 600

i veza koje generira transakcijom DBACockpit izgledat će ovako:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

Ovisno o li i kako virtualnog računala Azure hostiranje sustava SAP je povezan putem web-mjesta-na-web-mjesta, više web-mjesta ili ExpressRoute (unakrsno lokalnu implementaciju) morate provjerite je li taj ICM koristi potpuno kvalificiran naziv glavnog računala koja se može riješiti na računalu koju pokušavate otvoriti DBACockpit iz. Pogledajte bilješke SAP [773830] da biste razumjeli kako ICM određuje na potpuno kvalificirani naziv glavnog računala ovisno o parametrima profila i postavljanje icm host_name_full parametar izričito ako je potrebno.

Ako implementiran VM u scenariju samo oblak bez više lokacija veze između lokalnog poslužitelja i Azure, morate definirati javnu IP adresa i u domainlabel. Oblikovanje javno DNS naziva u VM pa će izgledati ovako:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Dodatne informacije vezane uz ime DNS-a nalazi se [ovdje][virtual-machines-azurerm-versus-azuresm].

Postavljanje na SAP profila parametar icm/host_name_full DNS naziva VM Azure vezu može izgledati slično:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

U ovom slučaju morate obavezno:

* Dodavanje ulazna pravila sigurnosne grupe mreže na portalu Azure TCP/IP priključke koji se koristi za komunikaciju s ICM
* Dodavanje ulazna pravila za konfiguriranje vatrozida za Windows za TCP/IP priključke za komunikaciju na ICM

Za programa automatiziranog uvesti ispravaka sve dostupne, preporučuje se povremeno primijeniti ispravak zbirke SAP bilješke odnosi se na vašu verziju SAP:

* [1558958]
* [1619967]
* [1882376]

Dodatne informacije o DBA Cockpit za elika i mala slova SAP-a nalazi se u sljedećim SAP bilješke:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Sigurnosno kopiranje i vraćanje zahtjevi za SAP elika i mala slova
Kada implementacija SAP elika i mala slova u Azure vaše sigurnosne kopije methodology morate pregledati. Čak i ako sustav nije produktivni sustava, hostira tvrtka SAP elika i mala slova SAP bazi podataka mora biti redovito sigurnosno kopiraju. Budući da Azure prostora za pohranu zadržava tri slike, sigurnosnu kopiju sada je manje važna u odnosu na Kompenziranje neočekivanog prostora za pohranu. Primarni razloga za održavanje proper tarifu za sigurnosno kopiranje i vraćanje je više možete vaše pogrešaka logičke/ručno unosom točke u vrijeme mogućnosti za oporavak. Da bi se cilj je ili sigurnosne kopije koristite da biste vratili bazu podataka natrag u određenom trenutku u vremenu ili pomoću sigurnosnih kopija u Azure seed neki drugi sustav kopiranjem postojeće baze podataka. Na primjer, nije moguće prenosite s konfiguraciju SAP 2 sloja 3 sloja sustava postavljanje isti sustava vraćanjem sigurnosnu kopiju.

Sigurnosno kopiranje i vraćanje baze podataka u Azure funkcionira na isti način kao i lokalne. Pročitajte članak SAP bilješke:

* [1588316]
* [1585981]

informacije o stvaranju konfiguracije Ispis i planiranje sigurnosnih kopija. Ovisno o strategije i potrebama možete konfigurirati baze podataka i prijaviti se ako ste na disk na jedan od postojećih VHDs ili dodajte dodatne VHD sigurnosne kopije.  Da biste smanjili opasnost od gubitka podataka u slučaju pogreške preporučuje se korištenje VHD na kojem se nalazi bez direktorija/datoteke baze podataka.

Osim podataka i LOB sažimanja elika i mala slova SAP-a nudi sigurnosne kopije sažimanja. Proširiti manje prostora s ispisi baze podataka i prijaviti se preporučuje se korištenje sigurnosne kopije sažimanja. Pogledajte bilješke SAP [1588316] dodatne informacije. Sažimanje sigurnosne kopije je presudne da biste smanjili količinu podataka koji će se prenijeti Ako planirate preuzimanje kopije ili VHDs koja sadrži sigurnosne kopije ispisi iz Azure virtualnog računala na lokalni.

Nemojte koristiti Azure VM temp prostor /mnt ili /mnt/resource kao baze podataka ili zapisnik odredišta za ispis.

#### <a name="performance-considerations-for-backupsrestores"></a>Pitanja vezana uz performanse za sigurnosno kopiranje/vraća
Kao implementacijama Gola metal sigurnosnog kopiranja i vraćanja performanse ovisi o količine koliko je moguće čitati paralelno i što bi moglo biti propusnost te količine. Osim toga, potrošnju procesora koristi sigurnosne kopije sažimanja možda isprobavati značajnu uloga na VMs samo do 8 niti procesora. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koristi za pohranu uređaja baze podataka, manji cjelokupan propusnost u načinu za čitanje
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja
* Manje ciljevi (Linux softver RAID, VHDs) za pisanje sigurnosnog kopiranja, manjeg propusnost

Da biste povećali broj ciljnih web-mjesta za pisanje postoje dvije mogućnosti koje mogu koristiti/kombinirati ovisno o vašim potrebama:

* Striping sigurnosne kopije Ciljna jedinica preko više postavljenu VHDs da biste poboljšali propusnost IOPS na toj prugastim jedinici
* Stvaranje ispisa konfiguraciju na razini SAP elika i mala slova koji koristi više od jedne ciljne direktorija za pisanje ispis da biste

Striping jedinica preko više postavljenu VHDs sadrži je spominju ranije u ovom vodiču. Dodatne informacije o korištenju više direktorija u konfiguraciji ispis SAP elika i mala slova potražite u dokumentaciji na sp_config_dump pohranjene Procedure koji se koristi za stvaranje konfiguracije ispis na [Informacijski centar Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Izrada oporavak s Azure VMs

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikacija podataka s poslužiteljem za replikaciju Sybase SAP-a
S elika i SAP Server replikacije SAP Sybase (SRS) u mala slova nudi Toplo čekanja rješenje da biste prenijeli transakcije baze podataka u daleka mjesto asinkrono. 

Instalacija i operacija SRS funkcionira kao i functionally VM smješten u servisa Azure virtualnog računala kao lokalnog.

HADR elika i mala slova putem SAP Server replikacija nije podržana tom trenutku u trenutku. Možda s i objavio za platforme Microsoft Azure u budućnosti.

## <a name="specifics-to-oracle-database-on-windows"></a>Specifičnosti s bazom podataka tvrtke Oracle u sustavu Windows
Nakon midyear 2013 softver tvrtke Oracle podržava Oracle pokrenuti Microsoft Windows Hyper-V i Azure. Pročitajte ovaj članak da biste dobili dodatne pojedinosti o Općenito podršku te Windows Hyper-V Azure Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Podrška za opće određene scenarij aplikacija SAP korištenje baze podataka tvrtke Oracle je podržane sljedeće kao i. Detalji o imenovane su u taj dio dokumenta.

### <a name="oracle-version-support"></a>Podrška Oracle
Sve pojedinosti o verzijama Oracle i odgovarajuće OS verzije koje podržava radi SAP-a na Oracle virtualnim računalima sustava na Azure pronaći ćete u sljedeće SAP bilješke [2039619]

Opće informacije o pokretanju paket servisa Business SAP-a na Oracle možete pronaći na SKN: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Oracle konfiguracije smjernice za SAP instalacije u Azure VMs

#### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
Samo jednu instancu Oracle pomoću NTFS oblikovani diskova podržana. Sve datoteke baze podataka mora biti pohranjena na temelju VHD diskova datotečnim sustavom NTFS. Ove VHDs su postavljen da Azure VM i temelje se na stranici blobova platforme Azure (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Bilo koju vrstu mrežne pogone ili udaljenom dionice poput servisa Azure datoteke:
 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
**nisu** podržani za datoteke baze podataka tvrtke Oracle!

Korištenje Azure VHDs na temelju blobova platforme Azure stranice, naredbe koje ste napravili u ovom dokumentu poglavlja [Međuspremanje za VMs i VHDs] [dbms – vodič za-2.1] i [Microsoft Azure pohranu] [dbms – vodič – 2.3] primijeniti implementacijama s podataka tvrtke Oracle.

Kao što je opisano u Općenito dio dokumenta, kvota za IOPS propusnost za Azure VHDs postoji. Exact kvote ovisno o vrsti VM koriste. Popis vrsta VM sa svojim kvotama možete pronaći [ovdje][virtual-machines-sizes]

Da biste odredili podržane vrste Azure VM, pogledajte bilješke SAP [1928533]

Sve dok se trenutni kvote IOPS po disk zadovoljava preduvjete, moguće je spremiti sve datoteke baze podataka na jednom VHD Azure postavljenu jedan. 

Ako su potrebne dodatne IOPS, svakako preporučuje se korištenje prozora grupe za pohranu (samo dostupne u sustavu Windows Server 2012 i noviji) ili podjela Windows za Windows 2008 R2 za stvaranje veliki logička uređaja više postavljenu VHD diskova. Vidi također poglavlja [softver RAID] [dbms – vodič – 2.2] ovog dokumenta. Taj se način pojednostavljuje indirektni Administracija da biste upravljali prostora na disku i izbjegava truda za ručno datoteke raspodijelite više postavljenu VHDs.

#### <a name="backup--restore"></a>Sigurnosno kopiranje / vraćanje
Za stvaranje sigurnosne kopije / vratili funkcije, SAP BR * za Oracle podržani su alati na isti način kao i na standardni operacijski sustavi Windows Server i Hyper-V. Upravitelj za oporavak Oracle (RMAN) podržano je i sigurnosnih kopija na disku i vratiti s diska.

#### <a name="high-availability"></a>Visoke dostupnosti
[komentar]: <>  (veza se odnosi na ASM)
Straža podataka tvrtke Oracle nije podržana za visoke dostupnosti i Izrada oporavak svrhe. Detalji o pronaći ćete u [ovom] [ virtual-machines-windows-classic-configure-oracle-data-guard] dokumentacije.

#### <a name="other"></a>Drugi
Sve druge općenite teme kao što su skupovi dostupnost Azure ili SAP nadzor primijeniti kao što je opisano u prva tri poglavlja dokumenta u implementacijama sustava VMs s podataka tvrtke Oracle.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Specifičnosti za bazu podataka MaxDB SAP-a u sustavu Windows

### <a name="sap-maxdb-version-support"></a>SAP MaxDB podrška
SAP trenutno podržava SAP MaxDB verziju 7,9 za korištenje sa sustavom SAP NetWeaver proizvode Azure. Sva ažuriranja za SAP MaxDB server ili JDBC i ODBC upravljačke programe za korištenje sa sustavom SAP NetWeaver proizvodima postoje isključivo putem servisa Marketplace SAP pri <https://support.sap.com/swdc>.
Opće informacije o pokretanju NetWeaver SAP-a na SAP MaxDB pronaći ćete na <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Podržane vrste Microsoft Windows verzije i Azure VM za DBMS MaxDB SAP-a
Da biste pronašli podržanu verziju Microsoft Windows DBMS MaxDB SAP-a na Azure, pogledajte:

* [SAP proizvod dostupnost matrice (PAM)][sap-pam]
* Napomena SAP [1928533]

Preporučujemo da koristite najnoviju verziju operacijskog sustava Microsoft Windows, što je Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Dokumentacija MaxDB dostupna SAP
Ažurirani popis SAP MaxDB dokumentaciju možete pronaći u sljedeće SAP bilješke [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Smjernice za konfiguraciju MaxDB SAP-a za SAP instalacije u Azure VMs

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Konfiguriranje prostora za pohranu
Azure prostora za pohranu najbolje prakse za SAP MaxDB slijedite općenite preporuke spomenute u poglavlja [strukturu implementacija RDBMS] [dbms vodič-2].

> [AZURE.IMPORTANT] Kao što su druge baze podataka MaxDB SAP-a sadrži i podatke i datoteka zapisnika. Međutim, u terminologija SAP MaxDB odgovarajućeg termina jest "jedinica" (nije "datoteka"). Na primjer, postoje SAP MaxDB količine podataka i količine zapisnika. Nemojte zamijeniti njih jedinice diska OS. 

Ukratko morate:

* Postavljanje računa za Azure pohrane koja sadrži SAP MaxDB podataka i zapisnika količine (odnosno datoteke) na **Lokalni suvišnih prostora za pohranu (LRS)** kao navedeno u poglavlja [Microsoft Azure Storage] [dbms – vodič – 2.3].
* Odvojite IO put za SAP MaxDB količine podataka (odnosno datoteke) od IO put za zapisnik količine (odnosno datoteke). To znači da SAP MaxDB količine podataka (odnosno datoteke) mora biti instaliran na jedan logički pogon i SAP MaxDB zapisnika količine (odnosno datoteke) mora biti instaliran na drugi pogon logičke.
* Postavljanje odgovarajućem datotečnom predmemoriju za svaki blobova platforme Azure, ovisno o tome je li ih koristiti za SAP MaxDB podataka ili zapisnik količine (odnosno datoteke) i tome koristite li Azure standardno ili Azure Premium prostor za pohranu, kao što je opisano u poglavlja [Međuspremanje za VMs] [dbms – vodič za-2.1].
* Sve dok se trenutni kvote IOPS po disk zadovoljava preduvjete, moguće je za pohranu sve količine podataka na jednom postavljenu Azure VHD i i pohranjuje sve jedinice zapisnika baze podataka na drugi VHD Azure postavljenu jedan.
* Ako su potrebne dodatne IOPS i/ili razmak, preporučuje da biste koristili Microsoft prozora grupe za pohranu (samo dostupne u sustavu Microsoft Windows Server 2012 i noviji) ili Microsoft Windows Podjela za Microsoft Windows 2008 R2 za stvaranje velikih logička uređaja više postavljenu VHD diskova. Vidi također poglavlja [softver RAID] [dbms – vodič – 2.2] ovog dokumenta. Taj se način pojednostavljuje indirektni Administracija da biste upravljali prostora na disku i izbjegava trud ručnoj distribuciji datoteka preko više postavljenu VHDs.
* Najveće preduvjete IOPS možete koristiti Azure Premium prostor za pohranu, koji je dostupan na DS niza i VMs Oznaka niz.

![Konfiguriranje referenca Azure IaaS VM za DBMS MaxDB SAP-a][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Sigurnosno kopiranje i vraćanje
Kada implementirate MaxDB SAP-a u Azure, morate pregledati vaše sigurnosne kopije methodology. Čak i ako sustav nije produktivni sustava, hostira tvrtka SAP MaxDB SAP bazi podataka mora biti redovito sigurnosno kopiraju. Budući da Azure prostora za pohranu zadržava tri slike, sigurnosnu kopiju sada manje važno je pomoću zaštita sustava izvoditi na pogreške prostora za pohranu i važnijih pogreške pri radu ili administratora. Primarni razlog za održavanje proper sigurnosnog kopiranja i vraćanja plan je tako da možete vaše logičko "ili" ručni pogrešaka unosom mogućnosti za oporavak točke u vrijeme. Da bi se cilj je koristiti sigurnosne kopije da biste vratili bazu podataka u određenom trenutku u vremenu ili pomoću sigurnosnih kopija u Azure seed neki drugi sustav kopirajući postojeću bazu podataka. Na primjer, nije moguće prenosite s konfiguraciju SAP 2 sloja 3 sloja sustava postavljanje isti sustava vraćanjem sigurnosnu kopiju.

Sigurnosno kopiranje i vraćanje baze podataka u Azure funkcionira na isti način kao i za lokalne sustave, da biste mogli koristiti standardni SAP MaxDB sigurnosnog kopiranja i vraćanja Alati za koje su opisane u jedan dokument dokumentaciju SAP MaxDB SAP bilješke [767598]na popisu. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Pitanja vezana uz performanse za sigurnosno kopiranje i vraćanje
Kao implementacijama Gola metal sigurnosnog kopiranja i vraćanja performanse ovisi o količine koliko je moguće čitati u paralelno i propusnost te količine. Uz to, potrošnju procesora koristi sigurnosne kopije sažimanja možete reproducirati značajan uloga na VMs s najviše 8 procesora niti. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koji se koristi za pohranu uređaja baze podataka, u donjem cjelokupan propusnost za čitanje
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja
* Manje ciljevi (pruga direktorija, VHDs) za pisanje sigurnosnog kopiranja, donjoj propusnost

Da biste povećali broj ciljnih web-mjesta za pisanje, postoje dvije mogućnosti koje možete koristiti, eventualno u kombinaciji, ovisno o vašim potrebama:

* Put izračunala zasebne jedinice za stvaranje sigurnosne kopije
* Striping sigurnosne kopije Ciljna jedinica preko više postavljenu VHDs da biste poboljšali propusnost IOPS na toj jedinici prugastim disk
* Ako imate zasebnom namjenski logičkog diska uređaji za sljedeće:
    * SAP MaxDB sigurnosne kopije količine (odnosno datoteke)
    * SAP MaxDB količine podataka (odnosno datoteke)
    * SAP MaxDB zapisnika količine (odnosno datoteke)

Striping jedinica preko više postavljenu VHDs sadrži je spominju ranije u poglavlja [softver RAID] [dbms – vodič – 2.2] ovog dokumenta. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Drugi
Sve druge općenite teme kao što su skupovi dostupnost Azure ili SAP nadzor primijeniti i kako je opisano u prva tri poglavlja dokumenta u implementacijama sustava VMs s bazom podataka MaxDB SAP-a.
Ostale postavke SAP MaxDB specifične za su prozirni za Azure VMs i opisana su u različite dokumente naveden u bilješku SAP [767598] i te SAP bilješke:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Specifičnosti za liveCache SAP-a u sustavu Windows

### <a name="sap-livecache-version-support"></a>SAP liveCache podrška
Minimalna verzija liveCache SAP podržane u Azure virtualnim strojevima je **SAP LC/LCAPPS 10.0 SP 25** uključujući **liveCache 7.9.08.31** i **LCA Sastavi 25**, objavio **EhP** 2 za SAP IO 7.0 i noviji.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Podržane vrste Microsoft Windows verzije i Azure VM za SAP liveCache DBMS
Da biste pronašli podržanu verziju Microsoft Windows liveCache SAP-a na Azure, pogledajte:

* [SAP proizvod dostupnost matrice (PAM)][sap-pam]
* Napomena SAP [1928533]

Preporučujemo da koristite najnoviju verziju operacijskog sustava Microsoft Windows, što je Microsoft Windows 2012 R2. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache konfiguracije smjernice za SAP instalacije u Azure VMs

#### <a name="recommended-azure-vm-types"></a>Preporučuje se vrste Azure VM
Kao SAP liveCache aplikacije koja se izvodi velikog izračune, iznos i brzine RAM-a i procesora sadrži glavne utjecaja na performanse liveCache SAP-a. 

Za Azure VM koje podržava SAP-a (SAP bilješke [1928533]), sve virtualne resursa procesora dodijeliti u VM se sigurnosno namjenski fizičkim resursima procesora za na hypervisor. Nema overprovisioning (i zato nema konkurencije za resurse procesora) odvija.

Isto tako, za sve Azure VM instancu vrste podržava SAP, memorije VM je 100% mapirati fizičke memorije – na primjer, overprovisioning (prekoračenja izvršenja), ne koristi.

Iz ovog perspektive preporučljivo je da biste koristili nove D niz ili DS-niza (u kombinaciji sa spremištem Premium Azure) Azure VM vrste, kao što je imaju 60% procesora brže nego odgovora nizova. Najveći RAM-a i procesora opterećenja, možete koristiti G niza i Oznaka niza (u kombinaciji sa spremištem Premium Azure) VMs s najnovijim Intel® Xeon® procesor E5 v3 obitelji, koji imaju dvaput memorije i četiri puta ispunjeni stanje pogon prostora za pohranu (SSDs) D/DS-niz.

#### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
Kao što je SAP liveCache temelji se na tehnologiju SAP MaxDB, Azure pohranu najbolje vježbe preporuke navedenu za SAP MaxDB u poglavlja [prostora za pohranu konfiguracije] [dbms – vodič za-8.4.1] vrijede i za liveCache SAP-a. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Namjenski Azure VM za liveCache
SAP liveCache intensively pomoću računalne power, za korištenje produktivni preporučljivo je za implementaciju na na namjenski Azure virtualnog računala. 
 
![Namjenski Azure VM za liveCache za slučaj produktivni koristi][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Sigurnosno kopiranje i vraćanje
Sigurnosno kopiranje i vraćanje, uključujući pitanja vezana uz performanse, već je podrobnije opisan u odgovarajuću poglavlja SAP MaxDB [sigurnosno kopiranje i vraćanje] [dbms – vodič za-8.4.2] i [performanse napomene za sigurnosno kopiranje i vraćanje] [dbms – vodič za-8.4.3]. 

#### <a name="other"></a>Drugi
Druge općenite teme opisane su već u odgovarajuću poglavlja [dbms – vodič za-8.4.4] MaxDB SAP [Time]. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Specifičnosti za SAP Server sadržaja u sustavu Windows
SAP Server sadržaj je zasebne, poslužiteljska komponenta za pohranu sadržaja kao što su elektroničke dokumente u različitim oblicima. SAP Server sadržaja omogućuje tvrtka razvojno tehnologije i treba korištenih unakrsno – aplikacije za sve aplikacije SAP-a. Na zasebnom sustav koji je instaliran. Uobičajeni sadržaj je dokumentaciji iz baze znanja skladištu ili tehničke crteže potječu mySAP sustav upravljanja dokumentima PLM i materijala za obuku. 

### <a name="sap-content-server-version-support"></a>Podrška za verziju SAP sadržaja poslužitelja
SAP trenutno podržava:

* **SAP Server sadržaja** s verzije **6.50 (i noviji)**
* **SAP MaxDB verziju 7,9**
* **Microsoft IIS (Internet Information Server) verzije 8.0 (i noviji)**

Preporučujemo korištenje najnovije verzije sustava SAP sadržaja Server, koji se u trenutku pisanja ovog dokumenta je **6.50 SP4**, i najnovija verzija sustava **Microsoft IIS 8,5**. 

Provjera najnoviji podržanim verzijama programa SAP Server sadržaja i Microsoft IIS u [SAP proizvod dostupnost matrice (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Podržane vrste Microsoft Windows i Azure VM za SAP Server sadržaja
Da biste saznali podržanu verziju sustava Windows za SAP Server sadržaja na Azure, pogledajte:

* [SAP proizvod dostupnost matrice (PAM)][sap-pam]
* Napomena SAP [1928533]

Preporučujemo korištenje najnovije verzije sustava Microsoft Windows koja se u trenutku pisanja ovog dokumenta je **Windows Server 2012 R2**.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Smjernice za konfiguraciju poslužitelj sadržaja SAP-a za SAP instalacije u Azure VMs

#### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu 
Ako konfigurirate SAP Server sadržaja za spremanje datoteka u bazi podataka SAP MaxDB, sve Azure prostora za pohranu najbolje prakse preporuke u poglavlja [prostora za pohranu konfiguracije] [dbms – vodič za-8.4.1] navode za SAP MaxDB vrijede i SAP Server sadržaja scenarija. 

Ako konfigurirate SAP Server sadržaja za spremanje datoteka u datotečnom sustavu, preporučuje se da biste koristili namjenski logički pogon. Korištenje prostori za pohranu omogućuje vam i povećati veličinu logičkog diska i propusnost IOPS kao što je opisano u u poglavlja [softver RAID] [dbms – vodič – 2.2]. 

#### <a name="sap-content-server-location"></a>Mjesto SAP sadržaja poslužitelja
SAP Server sadržaja mora biti implementirano u istom Azure području i Azure VNET gdje je implementiran sustava SAP-a. Vi ste slobodni odlučite želite li implementacija komponente SAP Server sadržaja na namjenski VM Azure ili na istom VM na kojem se pokreće sustava SAP. 
 
![Namjenski Azure VM za SAP Server sadržaja][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>SAP Server predmemoriju
SAP Server predmemorije je dodatne poslužiteljska komponenta za pristup (predmemorirani) dokumentima lokalno. SAP Server predmemorije predmemorira dokumenata SAP Server sadržaja. To je da biste optimizirali mrežni promet ako dokumenata moraju biti dohvaćeni više puta iz različitih mjesta. Općenito pravilo je da SAP Server predmemorije mora biti fizički blizu klijent koji pristupa SAP Server predmemoriju. 

Ovdje imate dvije mogućnosti:

1. **Klijent je pozadinski sustav SAP-a** Ako je pozadinski sustav SAP konfiguriran za pristup SAP Server sadržaja, taj sustava SAP je klijent. Kao što je sustava SAP i SAP Server sadržaja implementiran na istom području Azure – u istoj Azure Standard – su fizički blizu međusobno povezani. Stoga nema potrebe da bi namjenski SAP Server predmemoriju. Klijenti za SAP korisničko Sučelje (GUI SAP-a ili web-preglednik) izravno pristupiti sustavu SAP i sustava SAP dohvaća dokumente iz SAP Server sadržaja.
1. **Klijent je na lokalni web-pregledniku** SAP Server sadržaja moguće je konfigurirati da mogu pristupiti izravno na web-preglednik. U ovom slučaju web-pregledniku koji se izvodi lokalno – je klijent SAP sadržaja poslužitelja. Lokalni podatkovnog centra i Azure podatkovnog centra smještaju se na drugim lokacijama fizičke (najbolje blizu drugoga). Vaše lokalne podatkovnog centra povezan je s Azure putem Azure VPN-a web-mjesto ili ExpressRoute. Iako obje mogućnosti nude sigurne VPN mrežnu vezu za Azure, web-mjesto mrežne veze nude mreže propusnost i Latencija SLA između podatkovnog centra za lokalni i Azure podatkovnog centra. Da biste ubrzali pristup dokumentima, možete učiniti nešto od sljedećeg:
    1. Instalacija informacije o lokalnom poslužitelju SAP predmemorije, Zatvori da biste na lokalni web-pregledniku (mogućnost na slici [this][dbms-guide-900-sap-cache-server-on-premises])
    1. Konfiguriranje Azure ExpressRoute koji nudi velika brzina i niska kašnjenje namjenski mrežne veze između lokalnog podatkovnog centra i Azure podatkovnog centra.
 
![Mogućnost instalacije informacije o lokalnom poslužitelju predmemorije SAP-a][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Sigurnosno kopiranje / vraćanje
Ako konfigurirate SAP Server sadržaja za spremanje datoteka u bazi podataka za SAP MaxDB sigurnosnog kopiranja i vraćanja postupak i performanse razmatranja već je podrobnije opisan SAP MaxDB poglavlja [sigurnosno kopiranje i vraćanje] [dbms – vodič za-8.4.2] i poglavlje [performanse zahtjevi za sigurnosno kopiranje i vraćanje] [dbms – vodič za-8.4.3]. 

Ako konfigurirate SAP Server sadržaja za spremanje datoteka u datotečnom sustavu, jedan je mogućnost dopušta izvršavanje ručno sigurnosno kopiranje i vraćanje strukturom cijele datoteke gdje se nalaze u dokumentima. Slično SAP MaxDB sigurnosnog kopiranja i vraćanja, preporučuje se da bi jedinicu namjenski diska svrhu sigurnosne kopije. 

#### <a name="other"></a>Drugi
Ostale postavke određene SAP Server sadržaja su prozirni za Azure VMs i opisane u različite dokumente i SAP bilješke:

* <https://Service.SAP.com/contentserver> 
* Napomena SAP [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Specifičnosti za IBM DB2 za LUW u sustavu Windows
Uz Microsoft Azure jednostavno možete migrirati postojeću aplikacija SAP sustavom IBM DB2 Linux, UNIX i Windows (LUW) za Azure virtualnim strojevima. S SAP na IBM DB2 za LUW administratorima i razvojnim inženjerima i dalje možete koristiti isti razvoja i administratorskim alatima koji su dostupni na lokalni.
Opće informacije o pokretanju paket servisa Business SAP-a na IBM DB2 za LUW pronaći ćete u na SAP zajednice mreže (SKN) pri <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Dodatne informacije i ažuriranja o SAP-a na DB2 za LUW na Azure potražite u članku SAP bilješke [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX i Windows podrška
SAP-a na IBM DB2 za LUW na Microsoft Azure virtualnog računala Services podržano je od verzije DB2 10.5.

Informacije o podržani SAP proizvodi i vrste Azure VM, pogledajte bilješke SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX i Windows konfiguracije smjernice za SAP instalacije u Azure VMs

#### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
Sve datoteke baze podataka mora biti pohranjena na temelju VHD diskova datotečnim sustavom NTFS. Ove VHDs su postavljen da Azure VM i temelje se na stranici blobova platforme Azure (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Bilo koju vrstu mrežne pogone ili udaljenom dionice, kao što su sljedeći servisi Azure datoteka **nije** podržana za datoteke baze podataka: 

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Ako koristite Azure VHDs na temelju blobova platforme Azure stranice, naredbe napravili u ovom dokumentu poglavlja [strukturu implementacija RDBMS] [dbms vodič-2] primijeniti implementacijama s IBM DB2 LUW baze podataka. 

Kao što je opisano u Općenito dio dokumenta, kvota za IOPS propusnost za Azure VHDs postoji. Exact kvote ovise o vrsti VM koristi. Popis vrsta VM sa svojim kvotama možete pronaći [ovdje][virtual-machines-sizes].

Sve dok se trenutni kvote IOPS po disk je dovoljno, moguće je pohranjuje sve datoteke baze podataka na jednom VHD Azure postavljenu jedan. 

Performanse pitanja vezana uz i priručniku poglavlja "Podataka sigurnost i performanse pitanja vezana uz za bazu podataka direktorija" u vodilice instalacije SAP-a.

Umjesto toga koristite Windows grupe za pohranu (samo dostupne u sustavu Windows Server 2012 i noviji) ili podjela Windows za Windows 2008 R2 kao što je opisano u poglavlja [softver RAID] [dbms – vodič – 2.2] ovog dokumenta da biste stvorili veliki logička uređaja preko više postavljenu VHD diskova.
Za diskova koji sadrže DB2 putove za pohranu za web-mjesto sapdata i u okvir za saptmp direktorija, morate navesti veličinu sektor fizički disk 512 KB. Kada koristite Windows prostora za pohranu grupe, morate stvoriti grupe za pohranu ručno putem naredbenog retka sučelja pomoću parametar "-LogicalSectorSizeDefault". Detalje potražite u članku <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Sigurnosno kopiranje i vraćanje
Na isti način kao u standardni operacijski sustavi Windows Server i Hyper-V podržana funkcija sigurnosnog kopiranja i vraćanja za IBM DB2 za LUW.

Morate provjeriti imate li sigurnosnu strategije valjana baza podataka na mjestu. 

Kao implementacijama Gola metal performanse sigurnosnog kopiranja i vraćanja ovisi o količine koliko je moguće čitati paralelno i što bi moglo biti propusnost te količine. Osim toga, potrošnju procesora koristi sigurnosne kopije sažimanja možda isprobavati značajnu uloga na VMs samo do 8 niti procesora. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koristi za pohranu uređaja baze podataka, manji cjelokupan propusnost u načinu za čitanje
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja
* Manje ciljevi (pruga direktorija, VHDs) za pisanje sigurnosnog kopiranja, donjoj propusnost

Da biste povećali broj ciljnih web-mjesta za pisanje, dvije mogućnosti mogu koristiti/kombinirati ovisno o vašim potrebama:

* Striping glasnoće cilj sigurnosnog kopiranja preko više postavljenu VHDs da biste poboljšali propusnost IOPS na toj prugastim jedinici
* Korištenje više od jedne ciljne imenika pisati sigurnosnu kopiju

#### <a name="high-availability-and-disaster-recovery"></a>Visoke dostupnosti i Izrada oporavak
Microsoft klaster Server (MSCS) nije podržano.

Podržana je DB2 visoke dostupnosti Izrada oporavak (HADR). Ako virtualnim strojevima konfiguracije HA rad razlučivanje naziva, postavljanje u Azure ne se razlikovati od bilo kojeg postavljanje koje obavlja lokalnog. Ne preporučuje se oslanjate IP razlučivost samo.

Nemojte koristiti zemlj. – replikacije iz trgovine Azure. Dodatne informacije pogledajte poglavlja [Microsoft Azure Storage] [dbms – vodič – 2.3] i poglavlja [visoke dostupnosti i oporavak Izrada s Azure VMs] [dbms-vodič-3].

#### <a name="other"></a>Drugi
Sve druge općenite teme kao što su Azure dostupnost skupovima ili SAP nadzor primijeniti kao što je opisano u prva tri poglavlja dokumenta u implementacijama sustava VMs s IBM DB2 za LUW kao i. 

Pogledajte poglavlja [Općenito SQL Server za SAP-a na sažetak Azure] [dbms – vodič – 5,8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Specifičnosti za IBM DB2 za LUW na Linux
Uz Microsoft Azure jednostavno možete migrirati postojeću aplikacija SAP sustavom IBM DB2 Linux, UNIX i Windows (LUW) za Azure virtualnim strojevima. S SAP na IBM DB2 za LUW administratorima i razvojnim inženjerima i dalje možete koristiti isti razvoja i administratorskim alatima koji su dostupni na lokalni. Opće informacije o pokretanju paket servisa Business SAP-a na IBM DB2 za LUW pronaći ćete u na SAP zajednice mreže (SKN) pri <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Dodatne informacije i ažuriranja o SAP-a na DB2 za LUW na Azure potražite u članku SAP bilješke [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 Linux, UNIX i Windows podrška
SAP-a na IBM DB2 za LUW na Microsoft Azure virtualnog računala Services podržano je od verzije DB2 10.5.

Informacije o podržani SAP proizvodi i vrste Azure VM, pogledajte bilješke SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 Linux, UNIX i Windows konfiguracije smjernice za SAP instalacije u Azure VMs

#### <a name="storage-configuration"></a>Konfiguriranje prostora za pohranu
Sve datoteke baze podataka mora biti pohranjena u datotečnom sustavu na temelju VHD diskova. Ove VHDs su postavljen da Azure VM i temelje se na stranici blobova platforme Azure (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Bilo koju vrstu mrežne pogone ili udaljenom dionice, kao što su sljedeći servisi Azure datoteka **nije** podržana za datoteke baze podataka:

* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Ako koristite Azure VHDs na temelju blobova platforme Azure stranice, naredbe napravili u ovom dokumentu poglavlja [strukturu implementacija RDBMS] [dbms vodič-2] primijeniti implementacijama s IBM DB2 LUW baze podataka.

Kao što je opisano u Općenito dio dokumenta, kvota za IOPS propusnost za Azure VHDs postoji. Exact kvote ovise o vrsti VM koristi. Popis vrsta VM sa svojim kvotama možete pronaći [ovdje][virtual-machines-sizes].

Sve dok se trenutni kvote IOPS po disk je dovoljno, moguće je pohranjuje sve datoteke baze podataka na jednom VHD Azure postavljenu jedan.

Performanse pitanja vezana uz i priručniku poglavlja "Podataka sigurnost i performanse pitanja vezana uz za bazu podataka direktorija" u vodilice instalacije SAP-a.

Umjesto toga koristite LVM (Upravitelj logičkih glasnoću) ili MDADM kao što je opisano u poglavlja [softver RAID] [dbms – vodič – 2.2] ovog dokumenta da biste stvorili veliki logička uređaja preko više postavljenu VHD diskova.
Za diskova koji sadrže DB2 putove za pohranu za web-mjesto sapdata i u okvir za saptmp direktorija, morate navesti veličinu sektor fizički disk 512 KB.

#### <a name="backuprestore"></a>Sigurnosno kopiranje i vraćanje
Na isti način kao lokalnog standardne Linux instalacije podržana funkcija sigurnosnog kopiranja i vraćanja za IBM DB2 za LUW.

Morate provjeriti imate li sigurnosnu strategije valjana baza podataka na mjestu.

Kao implementacijama Gola metal performanse sigurnosnog kopiranja i vraćanja ovisi o količine koliko je moguće čitati paralelno i što bi moglo biti propusnost te količine. Osim toga, potrošnju procesora koristi sigurnosne kopije sažimanja možda isprobavati značajnu uloga na VMs samo do 8 niti procesora. Stoga nešto možete pretpostavlja da:

* Manje na broj VHDs koristi za pohranu uređaja baze podataka, manji cjelokupan propusnost u načinu za čitanje
* Što je manji broj procesora threads u VM, s raznih utjecaj sigurnosne kopije sažimanja
* Manje ciljevi (pruga direktorija, VHDs) za pisanje sigurnosnog kopiranja, donjoj propusnost

Da biste povećali broj ciljnih web-mjesta za pisanje, dvije mogućnosti mogu koristiti/kombinirati ovisno o vašim potrebama:

* Striping glasnoće cilj sigurnosnog kopiranja preko više postavljenu VHDs da biste poboljšali propusnost IOPS na toj prugastim jedinici
* Korištenje više od jedne ciljne imenika pisati sigurnosnu kopiju

#### <a name="high-availability-and-disaster-recovery"></a>Visoke dostupnosti i Izrada oporavak
Podržana je DB2 visoke dostupnosti Izrada oporavak (HADR). Ako virtualnim strojevima konfiguracije HA rad razlučivanje naziva, postavljanje u Azure ne se razlikovati od bilo kojeg postavljanje koje obavlja lokalnog. Ne preporučuje se oslanjate IP razlučivost samo.

Nemojte koristiti zemlj. – replikacije iz trgovine Azure. Dodatne informacije pogledajte poglavlja [Microsoft Azure Storage] [dbms – vodič – 2.3] i poglavlja [visoke dostupnosti i oporavak Izrada s Azure VMs] [dbms-vodič-3].

#### <a name="other"></a>Drugi
Sve druge općenite teme kao što su Azure dostupnost skupovima ili SAP nadzor primijeniti kao što je opisano u prva tri poglavlja dokumenta u implementacijama sustava VMs s IBM DB2 za LUW kao i.

Pogledajte poglavlja [Općenito SQL Server za SAP-a na sažetak Azure] [dbms – vodič – 5,8].