<properties
   pageTitle="SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju i planiranje | Microsoft Azure"
   description="SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju i planiranje"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-windows-virtual-machines-vms--planning-and-implementation-guide"></a>SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju i planiranje

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

[dbms-guide]:virtual-machines-windows-sap-dbms-guide.md (SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju DBMS) [dbms-guide-2.1]:virtual-machines-windows-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (Međuspremanje za VMs i VHDs) [dbms-guide-2.2]:virtual-machines-windows-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (softver RAID) [dbms-guide-2.3]:virtual-machines-windows-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (Microsoft Azure spremište) [dbms-guide-2]:virtual-machines-windows-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (strukturu RDBMS implementacije) [dbms-guide-3]:virtual-machines-windows-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (visoke dostupnosti i oporavak Izrada s Azure VMs) [dbms-guide-5.5.1]:virtual-machines-windows-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 i novijim verzijama) [dbms-guide-5.5.2]:virtual-machines-windows-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 i prethodnih izdanja) [dbms-guide-5.6]:virtual-machines-windows-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (pomoću SQL Server slike iz trgovine Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-windows-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Općenito SQL Server za SAP-a na sažetak Azure) [dbms-guide-5]:virtual-machines-windows-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (specifičnosti za SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-windows-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (prostora za pohranu konfiguracije) [dbms-guide-8.4.2]:virtual-machines-windows-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (sigurnosnog kopiranja i vraćanja) [dbms-guide-8.4.3]:virtual-machines-windows-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (performanse zahtjevi za sigurnosno kopiranje i vraćanje) [dbms-guide-8.4.4]:virtual-machines-windows-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (ostalo) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-windows-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-windows-sap-deployment-guide.md (SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju) [deployment-guide-2.2]:virtual-machines-windows-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP resursi) [deployment-guide-3.1.2]:virtual-machines-windows-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (implementacija VM prilagođene slikom) [deployment-guide-3.2]:virtual-machines-windows-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (scenarij 1: implementacija VM iz trgovine Azure za SAP-a) [deployment-guide-3.3]:virtual-machines-windows-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (scenarij 2: implementacija VM s prilagođenu sliku za SAP-a) [deployment-guide-3.4]:virtual-machines-windows-sap-deployment-guide.md# a9a60133-a763-4de8-8986-ac0fa33aa8c1 (scenarij 3: premještanje na VM iz lokalnog VHD Azure za koje nisu GRG generalizirano pomoću SAP-a) [deployment-guide-3]:virtual-machines-windows-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (implementacije scenariji od VMs za SAP-a na Microsoft Azure) [deployment-guide-4.1]:virtual-machines-windows-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (implementacija Azure PowerShell cmdleti) [deployment-guide-4.2]:virtual-machines-windows-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (Preuzimanje i uvoz SAP odgovarajuću cmdleta ljuske PowerShell) [deployment-guide-4.3]:virtual-machines-windows-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (uključivanje VM u lokalne domene – samo u sustavu Windows) [deployment-guide-4.4.2]:virtual-machines-windows-sap-deployment-guide.md# 6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [deployment-guide-4.4]:virtual-machines-windows-sap-deployment-guide.md#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (preuzimanje, instaliranje i Omogući Azure VM Agent) [deployment-guide-4.5.1]:virtual-machines-windows-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-windows-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure EŽA) [deployment-guide-4.5]:virtual-machines-windows-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (Konfiguriranje Azure Poboljšana nadzor proširenja za SAP-a) [deployment-guide-5.1]:virtual-machines-windows-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (spremnosti potražiti Azure Poboljšana nadzor za SAP-a) [implementacije – vodič za-5,2]: Virtual-machines-Windows-SAP-Deployment-Guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Provjera stanja za konfiguraciju infrastrukture nadzor Azure) [deployment-guide-5.3]:virtual-machines-windows-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (daljnje rješavanje problema Azure nadzor infrastrukture za SAP-a)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-windows-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-windows-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-windows-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-windows-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-windows-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-windows-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-windows-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-windows-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-windows-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-windows-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-windows-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-windows-sap-get-started.md
[getting-started-dbms]:virtual-machines-windows-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-windows-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-windows-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[ha-guide]:virtual-machines-windows-sap-high-availability-guide.md

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-windows-sap-planning-guide.md (SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju i planiranje) [planning-guide-1.2]:virtual-machines-windows-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (Resursi) [planning-guide-11.4]:virtual-machines-windows-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (visoke dostupnosti (HA) i Izrada oporavak (DR) za SAP NetWeaver koji se izvodi na Azure virtualnim strojevima) [planning-guide-11.4.1]:virtual-machines-windows-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (visoke dostupnosti za poslužitelje aplikacija SAP-a) [planning-guide-11.5]:virtual-machines-windows-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (pomoću automatsko pokrenite instanci SAP-a) [planning-guide-2.1]:virtual-machines-windows-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (samo za oblak - implementacijama virtualnog računala u Azure bez međuzavisnosti lokalne mreže klijenta) [planning-guide-2.2]:virtual-machines-windows-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (unakrsno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalne mreže) [planning-guide-3.1]:virtual-machines-windows-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure regije) [planning-guide-3.2.1]:virtual-machines-windows-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (kvara domena) [planning-guide-3.2.2]:virtual-machines-windows-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Nadogradnja domena) [planning-guide-3.2.3]:virtual-machines-windows-sap-planning-guide.md#18810088- f9be - 4c 97-958a - 27996255c 665 (Azure dostupnost skupovima) [planning-guide-3.2]:virtual-machines-windows-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (u pojam Microsoft Azure virtualnog računala) [planning-guide-3.3.2]:virtual-machines-windows-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium spremište) [planning-guide-5.1.1]:virtual-machines-windows-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Premještanje na VM s lokalnim Azure s diskom koji nisu GRG generalizirano) [planning-guide-5.1.2]:virtual-machines-windows-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (implementacija VM sa slikom za određene korisnike) [planning-guide-5.2.1]:virtual-machines-windows-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Priprema za premještanje na VM Azure s lokalnim na nisu GRG generalizirano disk) [planning-guide-5.2.2]:virtual-machines-windows-sap-planning-guide.md#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Priprema za implementaciju VM sa slikom za određene korisnike za SAP-a) [planning-guide-5.2]:virtual-machines-windows-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Priprema VMs s SAP-a za Azure) [planning-guide-5.3.1]:virtual-machines-windows-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (razlika između programa Azure na disku i slika Azure) [planning-guide-5.3.2]:virtual-machines-windows-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (prijenos VHD lokalnim za Azure) [planning-guide-5.4.2]:virtual-machines-windows-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopiranje diskova između računa za pohranu za Azure) [planiranja – vodič za-5.5.1]: Virtual-machines-Windows-SAP-planning-Guide.md#4efec401-91e0-40c0-8e64-f2dceadff646 (VM/VHD strukturu implementacijama SAP-a) [planning-guide-5.5.3]:virtual-machines-windows-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (postavka automount za priložene diskova) [planning-guide-7.1]:virtual-machines-windows-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (jedan VM s NetWeaver SAP pokazni videozapis/obuka scenarij) [planning-guide-7]:virtual-machines-windows-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (koncepata Cloud-Only implementacija instanci SAP-a) [planning-guide-9.1]:virtual-machines-windows-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure nadzor rješenje za SAP-a) [planning-guide-azure-premium-storage]:virtual-machines-windows-sap-planning-guide.md# ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium spremište)

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
[planning-guide-microsoft-azure-networking]:virtual-machines-windows-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-windows-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

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
[virtual-machines-windows-attach-disk-portal]:virtual-machines-windows-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-windows-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-windows-capture-image]:virtual-machines-windows-create-vm-generalized.md
[virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture]:virtual-machines-windows-create-vm-generalized.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-windows-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-ps-create.md
[virtual-machines-sizes]:virtual-machines-windows-sizes.md
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
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

Microsoft Azure omogućuje tvrtki dobiti računalnim i pohranu resursa u minimalnog vremenu bez ciklusa dugotrajan Nabava. Azure virtualnim strojevima Dopusti tvrtke za implementaciju klasični aplikacija, kao što su SAP NetWeaver temelji aplikacije u Azure i proširivanje njihove pouzdanosti i dostupnost bez dodatne resurse dostupne lokalnog. Azure virtualnog računala servisa podržava i povezivanje više lokacija, koja omogućuje tvrtki aktivno integracije virtualnim računalima sustava Azure u svoje domene lokalnog, njihove privatno oblaka i njihovih vodoravno sustava SAP.
Ova studija opisuju osnove sustava Microsoft Azure virtualnog računala i nudi walk-through planiranje i implementacija pitanja koja se odnose SAP NetWeaver instalacija u Azure i kao mora biti dokument da biste pročitali prije pokretanja stvarni implementacijama NetWeaver SAP-a na Azure.
Papira dopuna dokumentaciju o instalaciji SAP i bilješke SAP-a koji predstavljaju primarni resursi za instalacijama i implementacijama SAP softvera na navedeni platformama.

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="summary"></a>Sažetak
Cloud računalstvo široku pojam koji se pokušavaju više važnost unutar industrijskih IT s malim tvrtkama najviše velikih i multinacionalnoj korporacije.

Microsoft Azure je u oblak usluge tvrtke Microsoft koji nudi širok razni načini nove mogućnosti. Sada korisnici će moći hitro dodjele resursa i de-dodjele aplikacijama kao servis u oblaku, tako da se ne nalaze ograničeni na tehničke ili izrade proračuna ograničenja. Umjesto morate uložiti vrijeme i proračun u infrastrukture hardver, tvrtke možete fokus na aplikaciju, poslovnih procesa i njegov pogodnosti Kupci i korisnicima.

Microsoft Azure virtualnog računala Services, Microsoft nudi opsežne infrastrukture kao platforma servisa (IaaS). Podržane su aplikacija SAP NetWeaver koji se temelji na virtualnim računalima sustava Azure (IaaS). U ovom koje će opisuju kako planiranje i implementacija SAP NetWeaver temelji aplikacijama u Microsoft Azure kao platformu po izboru.

Papira sam usredotočite se na dva glavna aspekata:

* Prvi dio opisane su dva podržana implementacija obrazaca za aplikacije NetWeaver SAP-a koji se temelji na Azure. Će opisuju i Općenito rukovanje Azure s implementacijama SAP-a na umu.
* Drugi dio će detaljno implementacijom dvije različite scenarije opisane u prvi dio.

Dodatni resursi potražite u članku poglavlja [resursi] [planiranja – vodič za-1,2] u ovom dokumentu.

### <a name="definitions-upfront"></a>Definicija upfront
Cijelom dokumentu koristit ćemo sljedeće uvjete:

* IaaS: Infrastruktura kao usluga.
* PaaS: Platforme kao usluga.
* SaaS: Softver kao usluga.
* OKVIRA: Azure Voditelj resursa
* SAP komponente: pojedinačne SAP programa kao što su ECC, BW, Upravitelj rješenja ili EP.  Komponente SAP-a mogu se temeljiti na tradicionalni ABAP ili Java tehnologije ili aplikacije koje nisu-NetWeaver temelji kao što su poslovnih objekata.
* SAP okruženja: neke komponente SAP logički grupirane da biste izvršili Poslovna funkcija, kao što su razvoj, QAS, obuka, DR ili radnog.
* Vodoravno SAP: Odnosi se na cijelu resursima SAP-a u klijentu IT vodoravno. Vodoravno SAP uključuje sve proizvodnje i koje nisu radnog okruženja.
* SAP sustav: Kombinacije DBMS sloja i aplikacijskom sloju – primjerice SAP ERP razvoj sustava, SAP BW testiranje sustava, SAP CRM operativnom sustavu, itd... U implementacijama Azure nije podržan podjele te dvije slojeve između lokalnog i Azure. To znači upit sustava SAP-a ili je implementiran na lokalni ili ga je uveden u Azure. Možete uvesti i druge sustave programa SAP vodoravno u Azure ili na lokalnim poslužiteljima. Na primjer, možete implementirati razvoj SAP CRM i testirali sustavi u Azure, ali u SAP CRM radnog sustava lokalnog.
* Samo oblak implementacije: implementacija gdje Azure pretplate je povezan putem web-mjesto ili ExpressRoute vezu s lokalnim mrežne infrastrukture. Zajednički Azure dokumentaciju ove vrste implementacijama su i što je opisano kao "Samo za oblaka" implementacije. Virtualnim strojevima implementiran ako koristite taj način je moguće pristupiti putem Interneta i javnu ip adresa i/ili javno DNS naziv dodijeljen VMs u Azure. Za Microsoft Windows lokalni Active Directory (AD) i DNS-a nije proširena za Azure u tim vrstama implementacije. Dakle u VMs nisu dio lokalnog servisa Active Directory. Isto vrijedi za Linux implementacije pomoću npr OpenLDAP + Kerberos.

> [AZURE.NOTE] Samo oblak implementacije u ovom dokumentu definirana je kao dovršen landscapes SAP pokrenutih isključivo u Azure bez nastavka servisa Active Directory / OpenLDAP ili naziv razlučivosti lokalnim u javnom oblaka. Konfiguracija samo oblak nisu podržani za sustavima SAP proizvodnje ili konfiguracije kojima STMS SAP-a ili drugih resursa na lokalno morati koristiti između sustavima SAP hostirane na Azure i resursima residing lokalni.

* Više lokacija: Opisuje scenarij u kojem VMs uvode se Azure pretplatu koja ima web-mjesto, više web-mjesta ili ExpressRoute veze između lokalnog datacenter(s) i Azure. Zajednički Azure dokumentaciju ove vrste implementacijama su i što je opisano kao scenariji više lokacija. Razlog za vezu da biste proširili lokalne domene lokalnog servisa Active Directory / OpenLDAP i lokalnih DNS-a u Azure. Vodoravno lokalnog prošireni Azure imovini pretplate. Imate ovaj kućni broj, na VMs može biti dio lokalne domene. Korisnici domene lokalne domene možete pristupiti na poslužitelje, a mogu se izvoditi usluge na te VMs (kao što je DBMS services). Moguće je komunikacije i naziv razlučivost između VMs implementiran na lokalni i Azure distribuiranih VMs. Ovo je scenarij očekivanog Većina imovine SAP-a uvesti u.  Pogledajte [ovaj] [ vpn-gateway-cross-premises-options] članak i [u ovom] [ vpn-gateway-site-to-site-create] dodatne informacije.

> [AZURE.NOTE] Za sustavima SAP radnog podržane su više lokacija implementacijama sustava SAP gdje Azure virtualnim strojevima sa sustavima SAP su članovi lokalne domene. Konfiguracija više lokacija podržani za implementaciju dijelova ili dovršiti landscapes SAP-a u Azure. Čak i radi dovršeno vodoravno SAP-a u Azure zahtijeva pojavljuju one VMs dio lokalne domene i reklame / OpenLDAP. U bivšeg verzijama u dokumentaciji smo bila riječ o scenarijima Hibridne IT, gdje je s termina 'Hibridnog' korijenom u činjenica da postoji više lokacija povezivanje između lokalnog i Azure. Osim toga, činjenica da VMs u Azure su dio lokalnog servisa Active Directory / OpenLDAP.

Neke Microsoft dokumentaciji se opisuje više lokacija scenariji malo drugačije, posebice za DBMS SLIKA konfiguracije. Slučaju u SAP povezane dokumente, samo scenarij više lokacija boils prema dolje da biste nemaju web-mjesto ili privatnih (ExpressRoute) s povezivanjem i fact vodoravno SAP distribuira između lokalnog i Azure.  

### <a name="e55d1e22-c2c8-460b-9897-64622a34fdff"></a>Resursi
Za temu implementacijama SAP-a na Azure dostupne su sljedeće dodatne vodilice:

* [SAP NetWeaver na Windows virtualnim strojevima (VMs) – planiranja i vodič za implementaciju sustava (ovaj dokument)] [– vodič za planiranje]
* [SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju] [– vodič za implementaciju]
* [SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju DBMS] [dbms vodič]
* [SAP NetWeaver na Windows virtualnim strojevima (VMs) – vodič za implementaciju visoke dostupnosti][ha-guide]

> [AZURE.IMPORTANT] Gdje se koristi moguće vezu Referentni vodič za instalaciju SAP-a (referenca InstGuide-01, potražite u članku <http://service.sap.com/instguides>). Kada je riječ o preduvjetima i procesa instalacije, vodiče za instalaciju NetWeaver SAP uvijek biti pažljivo pročitajte, kao što je ovaj dokument pokriva samo određene zadatke za sustavima SAP NetWeaver instaliranima u u programu Microsoft Azure virtualnog računala.

Sljedeće SAP bilješke se odnose na temu SAP-a na Azure:

| Broj bilješke | Naslov |
|--------------|-------|
| [1928533] | Aplikacija za SAP na Azure: podržani proizvodi i promjenu veličine |
| [2015553] | SAP-a na Microsoft Azure: podržava preduvjeti |
| [1999351] | Rješavanje problema poboljšanom Azure nadzor za SAP |
| [2178632] | Ključ nadzor metriku za SAP-a na Microsoft Azure |
| [1409604] | Virtualizacija u sustavu Windows: Poboljšana nadzora |
| [2191498] | SAP-a na Linux s Azure: Poboljšana nadzora
| [2243692] | Linux na Microsoft Azure (IaaS) VM: problema licence SAP-a
| [1984787] | SUSE LINUX Enterprise Server 12: Napomene o instalaciji
| [2002167] | Crvena je vaša Enterprise Linux 7.x: instalacija i nadogradnja

I pročitajte [SKN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) koja sadrži sve bilješke SAP-a za Linux.

Općenito zadani ograničenja i maksimalna ograničenja Azure pretplate pronaći ćete u [ovom članku][azure-subscription-service-limits-subscription] 

## <a name="possible-scenarios"></a>Mogući scenariji
SAP često prikazivati kao jedna od najčešće zaštita njihove privatnosti ovise ključnih aplikacija unutar tvrtke. Arhitektura te operacije te aplikacije uglavnom je vrlo složen i jamči da ispunjavate uvjete na dostupnost i performanse je važno.

Stoga korporacije morati razmišljati pažljivo o aplikacije mogu se izvoditi u okruženju javno oblaka, neovisno o odabranom oblaka davatelja.

Vrste mogućih sustava za implementaciju SAP NetWeaver temeljiti aplikacijama u javnim oblaka okruženja navedena su u nastavku:

1. Srednje veličine radnog sustavi
1. Sustavi za razvoj
1. Testiranje sustavi
1. Brisanje knjižne sustavi
1. Učenje i pokazni sustavi

Da biste uspješno uvođenje sustavima SAP u Azure IaaS ili IaaS Općenito, važno je da značajan razlike između ponude tradicionalni outsourcers ili davatelji usluge i IaaS ponude. Dok tradicionalni za hostiranje ili vanjske suradnike će prilagoditi infrastrukturu (Vrsta mreže, za pohranu i server) za radno opterećenje klijent želi glavno računalo, umjesto toga je odgovornosti kupca da biste odabrali desnom radno opterećenje za IaaS implementacije.

Kao prvi korak, korisnici morati provjerite sljedeće stavke:

* U SAP podržane vrste VM Azure
* U SAP podržava proizvodi/izdanja Azure
* Podržane OS i DBMS izdaje za određene izdanja SAP servisu Azure
* SAPS propusnost nudi različite Azure SKU-ove

Odgovori na ta pitanja je moguće čitati u bilješku SAP [1928533]. 

Kao drugi korak Azure ograničenja resursa i propusnosti morati može usporediti s stvarni resursa utrošak lokalnog sustava. Dakle, korisnici moraju biti upoznati s različitim mogućnostima Azure vrste podržava SAP-a u području:

* Opterećenje procesora i memorije resursi različitih vrsta VM i
* IOPS propusnosti različitih vrsta VM i 
* Mogućnosti mrežni različitih vrsta VM.

Većinu tih podataka nalazi se [ovdje][virtual-machines-sizes]

Imajte na umu da se navedena ograničenja u gornje veze su gornjem ograničenja. To znači da se ograničenja za neki od resursa, npr. IOPS može pružati sve okolnostima. Iznimke su kroz resurse procesora i memorije odabranom VM vrste. Za VM koje podržava SAP, resursi procesora i memorije su rezervirana i kao što su dostupne u bilo kojem trenutku u vremenu za potrošnju unutar na VM.

Microsoft Azure platforme kao što su druge platforme IaaS je više klijenta. To znači da su zajedničke između klijenata za pohranu, mreže i ostale resurse. Inteligentna iskorištenosti i ograničavanje logike se koristi da biste spriječili koje utječu na performanse drugi klijent (oscilirajuće susjednog) drastične način jednog klijenta. Iako logike u Azure pokušava Zadrži varijance u propusnosti iskustvom small, Visoko zajedničke platforme obično predstavljanje veće varijance u dostupnost resursa/propusnosti od mnogo korisnici koriste se za u svoje lokalnim implementacijama. Zbog toga koje se mogu pojaviti različite razine propusnosti u vas pozdravljamo za povezivanje s mrežom ili prostora za pohranu/i (u glasnoću kao i Latencija) iz minute za minute. Vjerojatnost da bi sustava SAP-a na Azure može doći do varijanci veće od u lokalnim sustavom treba uzeti u obzir.

Posljednji korak se treba vrednovati preduvjeti dostupnost. To se može dogoditi, temeljni Azure infrastrukture mora se ažurirati, a zahtijeva hosts pokrenut VMs ponovno pokrenuti. U tim slučajevima VMs koji se izvode na te domaćini će se isključiti i ponovno pokrenuti kao i. Tempiranje takve održavanja obavlja tijekom radno vrijeme koje nisu core za određenu regiju, ali je prozor potencijalne od nekoliko sati tijekom kojeg će se izvršiti ponovno pokretanje računala relativno širine. Postoje različite tehnologije unutar Azure platformu koja možete konfigurirati da biste smanjili neke ili sve utjecaj takva ažuriranja. Poboljšanja za buduće od platforme Azure aplikacije DBMS i SAP osmišljene su da biste minimizirali utjecaj takve ponovnog pokretanja. 

Da biste uspješno uvođenje sustava SAP-a na Azure, na lokalni SAP system(s) operacijski sustav baze podataka i aplikacija SAP moraju se pojaviti u podršku matrice SAP Azure stane unutar resursa možete unijeti Azure Infrastruktura i koje možete raditi s ponuda za dostupnost SLA Microsoft Azure. Kao što su označena te sustavima, morat ćete odlučiti na jedan od sljedeća dva implementacije scenarija.

### <a name="1625df66-4cc6-4d60-9202-de8a0b77f803"></a>Samo za oblak - implementacijama virtualnog računala u Azure bez međuzavisnosti lokalne mreže klijenta
 
![Jedan VM SAP pokazni videozapis ili scenarij obuka u uveden u Azure][planning-guide-figure-100]

Taj se scenarij uobičajeni obuka koje ili sustavi pokazni videozapis instaliranim sve komponente SAP i softver koji nisu SAP unutar jedne VM. U ovom scenariju implementacije sustavima SAP radnog nisu podržani. Općenito govoreći, u ovom scenariju zadovoljava sljedeće preduvjete:

* VMs same može se pristupiti putem javne mreže. Izravni mrežne veze za aplikacije radi unutar na VMs za lokalnu mrežu ili tvrtke vlasnik pokazni programi ili obuka koje sadržaja ili kupac nije potreban. 
* U slučaju više VMs koji predstavlja obuka koje ili scenarij pokazni videozapis, mora rad s datotekama na VMs razlučivost komunikacije i naziv mreže. No komunikaciju između skup VMs moraju biti Izolirani tako da se nekoliko skupova VMs može biti implementirano usporedno bez smetnje.  
* Za krajnjeg korisnika za udaljene prijavu u VMs smješten u Azure potreban je mogućnost povezivanja s Internetom. Ovisno o tome na goste OS, Terminal Services/ZAPISI ili VNC/ssh koristi se za pristup VM ispunjavanje zadaci obuka ili izvođenje na pokazni programi. Ako se SAP priključaka kao što su 3200, 3300 i 3600 mogu i izložiti instancu aplikacija SAP možete pristupiti s bilo kojeg povezanog površine Internet.
* System(s) SAP-a (i VM(s)) predstavljaju samostalne scenarija u Azure koji samo zahtijeva javna internetska veza za krajnjeg korisnika pristup i ne zahtijeva vezu s drugim VMs u Azure.
* Instalirati i pokrenuti izravno na VM SAPGUI i web-preglednik. 
* Brzo vraćanje VM u izvorno stanje i novi implementacije tog izvorno stanje ponovno je obavezan. 
* U slučaju pokazni videozapis i scenariji za obuku koji su Rač u više VMs, servisa Active Directory / OpenLDAP i/ili DNS servis potreban je za svaki skup VMs.


![Grupa VM korisnika koji predstavlja jednu pokazni videozapis ili obuka scenarija u Oblaku programa Azure][planning-guide-figure-200]

Važno Imajte na umu da VM(s) u svakoj od skupova moraju biti implementirano paralelno, gdje VM su imena u svakom skupu isti je.

### <a name="f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10"></a>Unakrsno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalnu mrežu
 
![VPN-a s povezivanjem na web-mjesto (unakrsno lokaciji)][planning-guide-figure-300]

Taj se scenarij scenarij više lokacija uzorcima više mogućih implementacije. To se može opisati jednostavno kao što je pokrenut neki dijelovi u SAP vodoravno lokalnog i druge dijelove sustava SAP vodoravno na Azure. Svim aspektima fact koje su dio komponente SAP pokrenute na Azure mora biti prozirni za krajnje korisnike. Dakle SAP prijenosa ispravak sustava (STMS), RFC komunikacije, ispisa, sigurnost (kao što je SSO), itd funkcionirat će jednostavno za sustavima SAP sustavom Azure. No više lokacija scenarij opisuje i scenarij gdje dovršeno vodoravno SAP pokreće se u Azure s domenom klijenta i DNS prošireni u Azure. 

> [AZURE.NOTE] Ovo je scenarij implementaciju koja je podržana za pokretanje produktivni sustavima SAP.

U [ovom se članku] [ vpn-gateway-create-site-to-site-rm-powershell] dodatne informacije o povezivanju lokalne mreže Microsoft Azure

> [AZURE.IMPORTANT] Kada smo razgovor o scenarijima više lokacija između Azure i lokalnih implementacijama klijenta, ne možemo gledate preciznosti cijeli sustavima SAP. Koje su _nisu podržani_ za više lokacija scenariji scenarija:
> 
> * Različite slojeve aplikacija SAP izvodi u različitim načinima. Npr. pokrenuti na DBMS sloja lokalni, ali aplikacijskom sloju SAP-a u VMs implementiran kao Azure VMs ili obrnuto.
> * Neke komponente programa SAP sloj u Azure i neke lokalnog. Npr. Podjela pojavljivanja aplikacijskom sloju SAP između lokalnog i Azure VMs. 
> * Distribucija VMs instance SAP-a sustava preko više Azure regija nije podržana.
> 
> Razlog ta ograničenja je obavezu mreža visoke performanse nisku Latencija unutar SAP-a sustava, osobito između instanci aplikacije i sloj DBMS sustava SAP-a.



### <a name="supported-os-and-database-releases"></a>Podržane OS i izdanja baze podataka

* Microsoft poslužiteljski softver podržana za servisa Azure virtualnog računala je navedena u ovom se članku: <http://support.microsoft.com/kb/2721672>. 
* Podržani operacijski sustav izdanja, ali baza podataka sustava izdanja Azure virtualnog računala Services podržava zajedno sa softverom za SAP su zabilježeni u bilješku SAP [1928533]. 
* Aplikacija SAP i izdanja Azure virtualnog računala Services podržava su zabilježeni u bilješku SAP [1928533].
* Da biste pokrenuli kao gost VMs u Azure SAP scenarijima za podržani su samo 64-bitni slike. To također znači da su podržane samo 64-bitni aplikacija SAP i baza podataka.

## <a name="microsoft-azure-virtual-machine-services"></a>Servisi Microsoft Azure virtualnog računala
Platforme Microsoft Azure je internet skaliranje oblaka servisa platforme hostira i kojim se upravlja u programu Microsoft podataka. Platforme obuhvaća Microsoft Azure virtualnog računala Services (Infrastruktura kao usluga ili IaaS) i skup obogaćenog platforme kao mogućnosti usluge (PaaS).

Azure platforme smanjuje potrebe za unaprijed tehnologije i infrastrukture kupi. Ga pojednostavnjuje održavanje i operacijski aplikacije unosom računalnim na zahtjev i pohranu glavno računalo, promjena veličine i upravljanje web-aplikacije i povezani aplikacije. Upravljanje Infrastruktura je automatizirana s platformu koji je namijenjen visoke dostupnosti i dinamički skaliranje tako da odgovara potrebama za korištenje s mogućnošću pay-as-you-go cijene modela.


 
![Razmještaj servisa Microsoft Azure virtualnog računala][planning-guide-figure-400]

Pomoću servisa Azure virtualnog računala Services Microsoft je što vam omogućuje da implementacija slike prilagođene server Azure kao IaaS instance (pogledajte slika 4). Virtualnim strojevima servisu Azure temelje se na Hyper-V virtualne tvrdog diska (VHD) i će moći pokrenuti drugi operacijski sustavi kao gost OS.

Iz perspektive radu, virtualnog računala servisa Azure nudi slične sučelja kao virtualnim strojevima postavila lokalno. Međutim, ima značajan prednost da ne morate nabavu, upravljati i upravljanje Infrastruktura. Razvojni inženjeri i administratori imaju potpunu kontrolu nad operacijski sustav slike unutar te virtualnih računala. Administratori mogu prijaviti daljinski u tim virtualnim strojevima izvođenje održavanja i zadatke, kao i zadataka uvođenja softver za otklanjanje poteškoća. O implementaciji u samo su ograničenja veličine i mogućnostima Azure VMs. To možda neće biti sljedeći precizno zrnastog u konfiguraciji kao što je to možete učiniti lokalno. Odabir vrste VM koji predstavljaju kombinacija je:

* Broj vCPUs,
* Memorija
* Broj VHDs koji se može priložiti
* Mreža i pohranu bandwidths.

Veličina i ograničenja različite različite virtualnim strojevima veličine nudi mogu vidjeti u tablici u [ovom članku][virtual-machines-sizes]

Kako će shvatite da postoje drugi linije ili niz virtualnim računalima. Sljedeće linije od VMs razlikuje od 2015 Pro:

* Vrste VM A0 A7: neke od tih su certificirani za SAP-a. Prvi niz VM koji imate uvodi Azure IaaS.
* Vrste a8 A11 VM: visoke performanse računalstvo instance. Pokretanje na različitim bolje izvođenje izračunati domaćini od drugih VMs odgovora niz.
* Vrste VM D niz: bolje izvođenje od A0 A7. Sve vrste VM potvrđen s SAP-a.
* Vrste VM DS niz: koristiti isti domaćini kao D niz, ali se mogu povezati sa spremištem Premium Azure (pogledajte poglavlja [Azure Premium prostora za pohranu] [planiranja – vodič za-3.3.2] ovog dokumenta). Ponovno sve vrste VM posjeduju s SAP-a.
* Vrste VM G niz: vrste VM opterećenje memorije. 
* Vrste VM Oznaka niz: sviđa G niz, ali obuhvaća mogućnost da biste koristili Azure Premium prostora za pohranu (pogledajte poglavlja [Azure Premium prostora za pohranu] [planiranja – vodič za-3.3.2] ovog dokumenta). Prilikom korištenja VMs Oznaka niz kao poslužitelja baze podataka je obavezna da biste koristili Premium prostora za pohranu za DB podatke i transakcije datoteke zapisnika


Isti procesora i memorije konfiguracija može pronaći u različite nizove VM. Ipak, kada tražite propusnost performanse te VMs iz različitih niz možda se razlikuju znatno. Bez obzira imate ista konfiguracija opterećenje procesora i memorije. Razlog je podlozi hardver glavnog računala poslužitelja pri Uvod različitih vrsta VM imao je propusnost različitih karakteristikama.  Obično razlika prikazano propusnost performanse i prikazuje se cijena različite VMs.

Napominjemo da se ne svi drugi niz VM možda nudi u svakom od njih područja Azure (za Azure područja potražite u članku sljedeće poglavlje). Također imajte na umu da su certificirani za SAP-a ne i svih VMs ili VM niz.

> [AZURE.IMPORTANT] Za korištenje aplikacije NetWeaver SAP-a koji se temelji, podržani su samo podskupa VM vrste i konfiguracija SAP bilješke [1928533] na popisu.

### <a name="be80d1b9-a463-4845-bd35-f4cebdb5424a"></a>Azure regije
Microsoft omogućuje za implementaciju virtualnim strojevima pa pod nazivom "Azure regije". U područje Azure možda jedan ili više centrima podataka koje se nalaze uz. Za većinu Geopolitički područja svijeta Microsoft ima najmanje dva područja Azure. Npr. u Europi postoji u regiji Azure "Sjeverna Europa" i jedan od "Zapad Europe". Takve dva područja Azure unutar područja Geopolitički razdvojene su znakom dovoljno značajan udaljenost tako da se prirodnim ili tehničkom disasters ne utječu na obje regije Azure u istom Geopolitički regiji. Budući da Microsoft steadily sastavlja out nova Azure područja u različitim područjima Geopolitički globalno, broj ove područja steadily rast i od Pro 2015 unijeli najveći broj 20 područja Azure s dodatnim regijama objaviti već. Kao klijenta možete implementirati sustavima SAP u sve te regije, uključujući tih dviju regija Azure u Kini. Trenutni ažuran informacije o Azure područja potražite u članku ovo web-mjesto: <https://azure.microsoft.com/regions/>

### <a name="8d8ad4b8-6093-4b91-ac36-ea56d80dbf77"></a>Koncept virtualnog računala Microsoft Azure
Microsoft Azure nudi infrastrukturu na razini kao rješenje servisa (IaaS) glavno računalo za virtualnim strojevima sa slične radovi kao rješenje virtualizacije lokalnog. Vi ste moći stvoriti virtualnim strojevima iz programa Azure Portal, PowerShell ili EŽA, koje nude implementacije i mogućnosti upravljanja.

Azure Voditelj resursa omogućuje dodjeljivanje aplikacije pomoću predloška za deklarativno. U jedan predložak možete implementirati većem broju servisa uz njihove ovisnosti. Koristiti isti predložak za više puta implementaciju aplikacije tijekom svakoj fazi životnog ciklusa aplikacije.

Dodatne informacije o korištenju predložaka ARM nalazi se ovdje:

* [Upravljanje virtualnim strojevima pomoću predložaka Voditelj resursa Azure i EŽA Azure i][virtual-machines-linux-cli-deploy-templates]
* [Upravljanje virtualnim strojevima pomoću upravitelja resursa Azure i komponente PowerShell][virtual-machines-deploy-rmtemplates-powershell]
* <https://Azure.microsoft.com/Documentation/Templates/>

Drugi zanimljive značajka je mogućnost da biste stvorili slike iz virtualnim strojevima koji omogućuje vam da biste pripremili određene spremištima iz koje vam se brzo uvesti instance virtualnog računala koje zadovoljavaju svojim potrebama.

Dodatne informacije o stvaranju slike iz virtualnih računala pronaći ćete u [ovom članku (Windows)] [ virtual-machines-windows-capture-image] ili u [ovom članku (Linux)][virtual-machines-linux-capture-image].

#### <a name="df49dc09-141b-4f34-a4a2-990913b30358"></a>Kvara domene
Domena kvara predstavljaju fizičke jedinica pogreške, vrlo usko povezan s fizičke infrastrukture koje se nalaze u podataka i tijekom fizičke plohu ili za bicikle možete smatrati kvara domene, nema Izravni ikone mapiranja između dva. 

Ako pokrenete više virtualnim strojevima kao dio SAP-a sustava Microsoft Azure virtualnog računala Services, možete utjecati kontrolerom tkanina Azure za implementaciju aplikacije u različitim domenama kvara time sastanak preduvjeti SLA za Microsoft Azure. Međutim, raspodjele kvara domene putem Azure jedinica mjerilo (zbirka stotine računalnim čvorove ili čvorove prostora za pohranu i rad s mrežom) ili Dodjela VMs na određenu domenu kvara je nešto putem kojeg nemate Izravni kontrolu. Da biste izravno kontrolerom Azure tkanina uvesti skup VMs preko različitih kvara domena, morate dodijeliti Azure dostupnost skup na VMs trenutku implementacije. Dodatne informacije o skupovima dostupnost Azure potražite u članku poglavlja [Azure dostupnost skupove] [planiranja – vodič za-3.2.3] u ovom dokumentu.

#### <a name="fc1ac8b2-e54a-487c-8581-d3cc6625e560"></a>Nadogradnja domene
Logičke jedinicu koju pomoć da biste odredili koliko će se ažurirati VM sustava SAP-a koji se sastoji od SAP instance izvodi u više VMs, nadogradite predstavljaju domene. Kada se pojavi nadogradnju, Microsoft Azure prolazi kroz postupak ažuriranja te nadogradnje domene jedan po jedan. Širenjem VMs trenutku implementacije preko različitih domena nadogradnje možete zaštititi sustava SAP djelomično iz potencijalne isključiti. Da biste nametnuli Azure za implementaciju VMs sustava SAP-a proširite preko različitih domena za nadogradnju, prvo morate postaviti određeni atribut implementacije vrijeme svaki VM. Slično kvara domena, se jedinica skaliranje Azure je podijeljen u više domena za nadogradnju. Da biste izravno kontrolerom Azure tkanina uvesti skup VMs preko različitih domena za nadogradnju, morate dodijeliti Azure dostupnost skup na VMs trenutku implementacije. Dodatne informacije o skupovima dostupnost Azure potražite u članku poglavlja [Azure dostupnost skupove] [planiranja – vodič za-3.2.3] ispod.

#### <a name="18810088-f9be-4c97-958a-27996255c665"></a>Skupovi Azure dostupnosti
Azure virtualnim strojevima u jedan dostupnost Azure skupu će distribuirati kontrolerom tkanina Azure preko različitih kvara i nadogradnja domene. Svrha raspodjele preko različitih kvara i nadogradnja domena je da biste spriječili isključuje slučaju infrastrukture održavanja ili pogreške unutar jedne domene kvara sve VMs sustava za SAP-a. Prema zadanim postavkama VMs nisu dio dostupnosti skupa. Sudjelovanje u VM u dostupnost skupu definirana je vrijeme za implementaciju ili noviji na ponovno konfiguriranje i ponovno implementacije u VM.

Da biste shvatili pojam Azure dostupnost skupova i način dostupnost skupove odnose se na kvara i nadogradnja domena, pročitajte [Ovaj članak][virtual-machines-manage-availability]

Da biste definirali dostupnost skupovi za ARM putem predloška json potražite u članku [Specifikacija rest api](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-compute/2015-06-15/swagger/compute.json) i traženje "dostupnosti".

### <a name="a72afa26-4bf4-4a25-8cf7-855d6032157f"></a>Prostor za pohranu: Microsoft Azure prostora za pohranu i diskova podataka
Virtualnim računalima sustava Microsoft Azure korištenje prostora za pohranu za različite vrste. Kada implementacijom SAP-a na web-servisa Azure virtualnog računala Services važno je da biste razumjeli razlike između ove dvije glavne vrste prostora za pohranu:

* Osobe koje nisu stalni, promjenjive prostora za pohranu.
* Stalnih prostor za pohranu.

Nisu stalnih prostor za pohranu izvodi virtualnim strojevima izravno priložiti, a nalazi na čvorove računalnim same – lokalnu instancu prostora za pohranu (privremenu pohranu). Veličina ovisi o veličini virtualnog računala odabrali kada implementacijskih pokreće. Ta vrsta prostora za pohranu je promjenjive i stoga disk se pokreće prilikom ponovnog pokretanja instancu komponente virtualnog računala. Obično stranične za operacijski sustav nalazi se na privremene disk.

___

> ![Windows][Logo_Windows] Windows
>
> Na Windows VMs temp je postavljen kao pogon D:\ u distribuiranih VM.
>
> ![Linux][Logo_Linux] Linux
> 
> Na Linux VMs ga je postavljen kao /mnt/resource ili /mnt. Potražite dodatne informacije u nastavku:
> 
> * [Kako priložiti podatkovni Disk Linux virtualnog računala][virtual-machines-linux-how-to-attach-disk]
> * <http://blogs.msdn.com/b/mast/Archive/2013/12/07/Understanding-the-Temporary-Drive-on-Windows-Azure-Virtual-machines.aspx>

___

Stvarni pogon je promjenjive jer je početak pohranjen na poslužitelju glavno računalo sam. Ako se na VM Premjesti u je ponovno uvođenje (npr. zbog održavanja na glavno računalo ili isključivanje i ponovno pokretanje) sadržaj pogon se gube. Zbog toga nije mogućnost za pohranu važne podatke na ovom pogonu. Vrsta medija koji se koristi za tu vrstu prostora za pohranu razlikuje između različitih grupa VM s karakteristikama vrlo različito performanse koji od lipnja 2015 izgledati kao što su:

* A5 do A7: Vrlo ograničeni performansi. Ne preporučuje se za sve osim datoteka stranice
* A8 A11: Vrlo dobre performanse karakteristike s nekim IOPS deset tisuće i > 1GB/sec propusnost.
* D-niz: Vrlo dobre performanse karakteristike s nekim, a zatim tisućica IOPS i > 1GB/sec propusnost.
* DS-niz: Vrlo dobre performanse karakteristikama s nekim IOPS deset tisuće i > 1GB/sec propusnost.
* G nizom: Vrlo dobre performanse karakteristike s nekim IOPS deset tisuće i > 1GB/sec propusnost.
* Serija Oznaka: Vrlo dobre performanse karakteristikama s nekim IOPS deset tisuće i > 1GB/sec propusnost.

Naredbe iznad primjene su vrste VM koje posjeduju SAP-a. VM niz s izvrstan IOPS i propusnost zadovoljavate leverage neke značajke DBMS. Pogledajte na [DBMS vodič za implementaciju] [dbms vodič] više pojedinosti.

Spremište na platformi Microsoft Azure nudi postojanog prostora za pohranu i uobičajene razine zaštite i zalihosti vidjeti na SAN prostora za pohranu. Diskova Azure prostora za pohranu na temelju su virtualne na tvrdom disku (VHDs) koja se nalazi u Azure servise za pohranu. Lokalni Disk OS (Windows C:\, Linux / (/ razvojni/sda1)) je pohranjen na Azure prostora za pohranu i dodatnih količine/diskova postavljen na VM se tamo spremljene, previše.

Moguće je prijenos postojeće VHD lokalnim ili stvorite prazan one iz Azure i priložite s distribuiranih VMs. Te VHDs se pozivati kao Azure diskova. 

Nakon stvaranja ili prijenos na VHD u Azure prostora za pohranu, moguće je da biste dostupnosti i priložite s postojećeg virtualnog računala, a da biste kopirali postojeći VHD (Nepostavljen).

Kao što su te VHDs dosljedan, podataka i promjena unutar one sigurno prilikom ponovnog pokretanja i vraćanju instancu komponente virtualnog računala. Čak i ako se briše instance, te VHDs sigurnosti i možete ponovno implementirati ili u slučaju diskova koje nisu OS možete postavljen da druge VMs.

Unutar mreže prostora za pohranu Azure moguće je konfigurirati zalihosti različite razine:

* Minimalna razina koje je moguće odabrati je "lokalni zalihosti", koja je jednaka tri replike podataka unutar iste podatkovnog centra područja za Azure (pogledajte poglavlja [Azure Regions][planning-guide-3.1]). 
* Zone suvišnih prostora za pohranu koji će podjele tri slike preko različitih vrsta centara unutar iste Azure područja.
* Zadana razina zalihosti je geografske zalihosti koji asinkrono replicira sadržaj u drugom 3 slike s podacima u drugoj regiji Azure koji se nalazi u istom Geopolitički regiji.

Također potražite u tablici pri vrhu ovog članka u vas pozdravljamo mogućnosti različite zalihosti: <https://azure.microsoft.com/pricing/details/storage/> 

Dodatne informacije potražite u vas pozdravljamo za pohranu Azure nalazi se ovdje: 

* <https://Azure.microsoft.com/Documentation/Services/Storage/>
* <https://Azure.microsoft.com/Services/site-Recovery>
* <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>
* <https://blogs.msdn.com/b/azuresecurity/Archive/2015/11/17/Azure-disk-Encryption-for-Linux-and-Windows-Virtual-machines-public-Preview.aspx>


#### <a name="azure-standard-storage"></a>Azure standardne prostora za pohranu
Azure standardne blobova nije vrstu spremišta kada Azure IaaS objavljen. Pojavile su se IOPS kvote nametnuo po jedan VHD. Latencija iskustvom nije isti predmet kao SAN/NAS uređaji obično uvesti za visokokvalitetni sustavima SAP hostira lokalnog. Ipak, proved dovoljni za mnoge stotine standardne prostora za pohranu Azure sustavima SAP-a u međuvremenu implementiran u Azure.

Azure standardne prostora za pohranu se naplaćuje ovisno o stvarnih podataka koja je spremljena, količinu prostora za pohranu transakcije, prijenosi izlazne podatke i mogućnosti zalihosti odabrali. Mnoge VHDs mogu stvoriti na taj 1TB Maksimalna veličina, ali sve dok se oni ostati prazan postoji bez troškova. Ako zatim unesite jednu VHD sa 100GB, koji će se naplatiti za 100GB za pohranu, a ne za veličinu nominal u VHD imate stvorene pomoću.

#### <a name="ff5ad0f9-f7f4-4022-9102-af07aef3bc92"></a>Prostor za pohranu Azure Premium
Travanj 2015 Microsoft uvedena Azure Premium prostora za pohranu. Prostor za pohranu Premium imate uvedene s ciljem možete unijeti:

* Bolje kašnjenje/i.
* Bolje propusnost.
* Manje varijabilnosti kašnjenje/i.

Za tu svrhu velik broj promjena uvedene od koji su dvije najčešće značajan:

* Korištenje SSD diskova u čvorove Azure prostora za pohranu
* Novi čitanja predmemorije koje se sigurnosno putem lokalnog SSD od Azure računalnim čvora

U nasuprot standardne prostora za pohranu gdje mogućnosti niste promijenili ovisi o veličini diska (ili VHD), Premium prostora za pohranu trenutno sadrži 3 drugi disk kategorije koje se prikazuju na kraju ovog članka prije u odjeljku najčešća Pitanja: <https://azure.microsoft.com/pricing/details/storage/>

Pogledajte IOPS/VHD i diska propusnost/VHD ovise o kategoriji veličina od diskova

Troškova temelj slučaju prostora za pohranu Premium nije glasnoće stvarnih podataka pohranjene u takvim VHDs, ali veličinu kategoriju kao što je VHD, neovisno o količinu podataka koji je pohranjen u na VHD.

Također možete stvoriti VHDs na Premium prostora za pohranu koji nisu izravno mapiranja u kategorije veličina prikazano. To može biti predmet, osobito kada kopirate VHDs iz standardnog prostora za pohranu u Premium prostora za pohranu. U tim slučajevima mapiranje na sljedeću najveću Premium prostora za pohranu na disku mogućnost se izvodi. 

Imajte na umu samo određeni niz VM možete im Azure Premium prostora za pohranu. Od 2015 Pro, ovo su DS - a Oznaka-niz. DS-niz je isti kao D-niza uz iznimku da DS niz ima mogućnost postavljanja Premium prostora za pohranu na temelju VMs dodatno da biste VHDs koji se nalaze Azure standardne prostora za pohranu. Isto vrijedi za G niz u usporedbi s Oznaka niz.

Ako su odjavljivanje dio VMs DS niza u [ovom članku] [ virtual-machines-sizes] također će shvatite da nema podataka glasnoću ograničenja za VHDs Premium prostora za pohranu u preciznosti VM razine. Različite DS nizove ili VMs Oznaka niza i imaju različite ograničenja u vas pozdravljamo broj VHDs koji se može postaviti. Ta ograničenja su navedenih u članku prethodno navedenim kao i. No zapravo znači ako npr dostupnosti 32 x P30 diskova/VHDs na jednom VM DS14 ne možete dobiti 32 x Maksimalna propusnost P30 diska. Umjesto toga Maksimalna propusnost na razini VM kao navedenih u članku će ograničiti propusnost podataka. 

Dodatne informacije o Premium prostora za pohranu nalazi se ovdje: <http://azure.microsoft.com/blog/2015/04/16/azure-premium-storage-now-generally-available-2>

#### <a name="azure-storage-accounts"></a>Računi za Azure prostora za pohranu
Kada implementirate services ili VMs u Azure, implementaciju VM slike i VHDs mora biti organiziran jedinicama pod nazivom računi servisa Azure prostora za pohranu. Prilikom planiranja Azure implementaciju, morate razmisliti o ograničenja Azure. Na jednoj strani postoji ograničen broj računa za pohranu po pretplati Azure. Iako svaki račun za pohranu Azure mogu sadržavati velik broj datoteka VHD, fixed ograničen je na ukupni IOPS po računu za pohranu. Kada implementirate stotine SAP VMs sa sustavima DBMS stvaranje značajnu IO pozive, preporučuje se distribuirati visoke VMs DBMS IOPS između više računa Azure prostora za pohranu. Potrebno je poduzeti da nećete biti dulji od trenutnog ograničenja računi servisa Azure prostora za pohranu po pretplati. Budući da je prostor za pohranu Ključni dio implementacije baze podataka za sustav SAP, tog koncepta opisan je u detaljnije na već referencirani [DBMS vodič za implementaciju] [dbms vodič].

Dodatne informacije o računima za pohranu Azure pronaći ćete u [ovom članku][storage-scalability-targets]. Pročitajte ovaj članak će shvatite da postoje razlike u ograničenja Azure standardne račune za pohranu i račune za pohranu Premium. Glavne razlike su količinu podataka koji se mogu nalaziti unutar kao što je račun za pohranu. U standardnom prostora za pohranu Glasnoća je veličina koji je veće od s Premium prostora za pohranu. Na drugoj strani standardni račun za pohranu je uzrokuje ograničen u IOPS (pogledajte stupac "Ukupno zahtjev stopa"), dok je račun za pohranu Azure Premium sadrži Nema takve ograničenja. Ne možemo obrađuje pojedinosti i rezultata te razlike kada razgovarate o implementacijama sustava SAP, osobito DBMS poslužiteljima.

U račun za pohranu imate mogućnost da biste stvorili drugu spremnika radi organiziranja i kategoriziranje različite VHDs. Ove spremnika obično koriste se za npr zasebnom VHDs različite VMs. Postoje bez posljedice performanse u spremnik za samo jednu ili više spremnika ispod jednog računa za pohranu za Azure.

Unutar Azure naziv VHD slijedi sljedeće imenovanja veze koje je potrebno Navedite jedinstveni naziv VHD unutar Azure:

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

Kao što je rečeno iznad niz treba identificirati samo VHD koji je pohranjen na Azure prostora za pohranu.

### <a name="61678387-8868-435d-9f8c-450b2424f5bd"></a>Mreže Microsoft Azure
Microsoft Azure dat će mrežne infrastrukture koji omogućuje uređivanje mapiranja sve scenarija koji rado bismo shvatili sa softverom za SAP-a. Nekoliko mogućnosti:

* Pristup s vanjski, izravno VMs putem Windows Terminal Services ili ssh/VNC
* Pristup servisima i određene priključke koriste aplikacije unutar na VMs
* Interna komunikacije i naziv razlučivost između grupe VMs u uveden kao Azure VMs
* Povezivanje s više lokacija između lokalne mreže klijenta i Azure mreže
* Unakrsni Azure regija ili povezivanje s podacima centra između Azure web-mjesta 

Dodatne informacije možete pronaći ovdje: <https://azure.microsoft.com/documentation/services/virtual-network/>

Postoji mnogo različitih mogućnosti da biste konfigurirali naziv i IP razlučivost u Azure. U ovom dokumentu samo oblak scenariji oslanjate zadani upotrebe Azure DNS-a (za razliku od definiranje vlastite DNS servis). Postoji novi Azure DNS servis koji može se koristiti umjesto postavljanju vlastite DNS poslužitelj. Dodatne informacije pronaći ćete u [ovom članku] [ virtual-networks-manage-dns-in-vnet] i na [ovoj stranici](https://azure.microsoft.com/services/dns/).

Za scenarije više lokacija ne možemo su potrebe za oslanjanjem na činjenica koji na lokalni AD/OpenLDAP/DNS proširen putem VPN-a ili privatne veze za Azure. Za određene scenarije kao što je ovdje navedenih, možda je potrebno da bi je replike AD-OpenLDAP instalirali u drugom Azure.

Budući da se s mrežom i razlučivanje naziva je tehnika dio uvođenje baze podataka za sustav SAP, tog koncepta opisan u detaljnije [DBMS upute za uvođenje] [dbms vodič].


##### <a name="azure-virtual-networks"></a>Azure virtualne mreže

Uz izgradnji Azure virtualne mreže možete odrediti adresu raspon privatne IP adrese koje funkcija Azure DHCP. U slučajevima više lokacija rasponu IP adresa definirani će i dalje se dodijeliti koristeći DHCP po Azure. Međutim, naziv domene razlučivost će se izvršiti lokalnog (pod pretpostavkom da su u VMs dio lokalne domene) i zato mogli biste riješiti adrese izvan različite servise u Oblaku Azure.

[komentar]: <>  (MSSedusch i dalje potrebna? Popis obveza izvorno programa Azure virtualne mreže bila povezana afinitet grupi. Uz to virtualne mreže u Azure dobio ograničena jedinica skaliranje Azure koje imate dodijeljene grupi afinitet. Na kraju, to namijenjena je ograničeno na dostupni u jedinici skaliranje Azure resursi virtualne mreže. To je od promijenio i sada možete Azure virtualne mreže proširila više jedinica skaliranje Azure. Koji zauzima Azure virtualne mreže jesu li ** ne ** pridružene afinitet grupe više na vrijeme stvaranja. Ne možemo već ranije spomenutih da u nasuprot preporuke godišnje prije, trebali biste ** nije više pod utjecajem Azure afinitet grupe **. Dodatne informacije potražite u < https://azure.microsoft.com/blog/regional-virtual-networks/>)

Svaki virtualnog računala u Azure mora biti povezani s virtualne mreže.

Dodatne informacije pronaći ćete u [ovom članku] [ resource-groups-networking] i na [ovoj stranici](https://azure.microsoft.com/documentation/services/virtual-network/).

[komentar]: <>  (MShermannd obveze ne može pronaći članak koja sadrži temi OpenLDAP + ARM;)
[komentar]: <>  (MSSedusch < https://channel9.msdn.com/Blogs/Open/Load-balancing-highly-available-Linux-services-on-Windows-Azure-OpenLDAP-and-MySQL>)

> [AZURE.NOTE] Prema zadanim postavkama, kada je implementiran u VM nije moguće promijeniti Konfiguracija virtualne mreže. Postavke TCP/IP mora ostati na poslužitelj za Azure DHCP. Zadano ponašanje je dinamičke IP dodjele.

MAC adresa kartice virtualne mreže npr može promijeniti nakon ponovnog veličina i goste Windows ili Linux OS će obraditi novu karticu mreže i automatski koristiti DHCP dodijeliti adrese IP-a i DNS- a u ovom slučaju.

##### <a name="static-ip-assignment"></a>Dodjela statičke IP
Moguće je da biste dodijelili fixed ili rezervirana IP adresa VMs unutar Azure virtualne mreže. Izvodi se VMs u Azure virtualne mreže otvorit će se sjajno mogućnost odražava ta je funkcija ako je potrebno ili potreban za neke scenarije. Dodjela IP ostaje valjan cijeloj postojanje parametra VM, neovisno o tome je li u VM pokretanje ili isključivanje. Zbog toga trebate ponijeti ukupni broj VMs (pokrenut i zaustavljena VMS) u obzir prilikom definiranja raspona IP adresa za virtualne mreže. IP adresa ostaje dodijeljen dok u VM i njegov mrežno sučelje izbriše ili dok IP adresu dobiva njezini ponovno dodijeliti. Pročitajte članak detaljne informacije u [ovom članku][virtual-networks-static-private-ip-arm-pportal].

##### <a name="multiple-nics-per-vm"></a>Više NIC-ovi po VM
Možete definirati više virtualne mrežne kartice (vNIC) za programa Azure virtualnog računala. Uz mogućnost više vNICs možete pokrenuti da biste postavili mrežni promet odvojenosti gdje – primjerice klijent promet je usmjerena kroz jedan vNIC i pozadinskog promet je usmjerena kroz drugi vNIC. O njima ovisne o vrsti VM postoji su različite ograničenja u vas pozdravljamo broj vNICs. EXACT detalje funkcionalnost i ograničenja nalazi se u sljedećim člancima:
 
* [Stvaranje na VM s više NIC-ovi][virtual-networks-multiple-nics]
* [Implementacija višestruki NIC VMs pomoću predloška][virtual-network-deploy-multinic-arm-template]
* [Implementacija višestruki NIC VMs pomoću komponente PowerShell][virtual-network-deploy-multinic-arm-ps]
* [Implementacija višestruki NIC VMs pomoću EŽA Azure][virtual-network-deploy-multinic-arm-cli]

#### <a name="site-to-site-connectivity"></a>Povezivanje web-mjesto
Više lokacija je Azure VMs i lokalnih povezane s prozirnim i trajna VPN veza. Očekuje postati najčešće SAP implementacije uzorak Azure. Pretpostavlja se da radu postupke i procesa pomoću SAP instance Azure surađivati prozirna. Ti znači trebali biste moći ispis iz tih sustava kao i koristi na SAP prijenosa upravljanja sustava (TMS) za prijenos mijenja iz razvoj sustavu Azure za testiranje sustava koji je implementiran na lokalni. Dodatne dokumentacija oko web-mjesto može se pronaći u [ovom članku][vpn-gateway-create-site-to-site-rm-powershell]

##### <a name="vpn-tunnel-device"></a>Uređaj tunelom VPN-a
Da biste stvorili vezu web-mjesto (lokalni podatkovnog centra u Centar za Azure podataka), morat ćete dobiti i konfiguriranje VPN uređaja ili usmjeravanje i daljinski pristup servisa (RRAS) koji je uvedena upotrijebili softver komponentom Windows Server 2012. 

* [Stvaranje virtualne mreže s web-mjesto VPN veza pomoću komponente PowerShell][vpn-gateway-create-site-to-site-rm-powershell]
* [O uređajima VPN-a za web-mjesto VPN pristupnika veze][vpn-gateway-about-vpn-devices]
* [Najčešća pitanja vezana uz pristupnik za VPN-a][vpn-gateway-vpn-faq]

![Web-mjesto veze između lokalnog poslužitelja i Azure][planning-guide-figure-600]

Iznad slika prikazuje dvije Azure pretplate imaju IP adresa subranges rezervirano za korištenje u virtualne mreže u Azure. Veza iz lokalne mreže s Azure je uspostavljena putem VPN-a.

#### <a name="point-to-site-vpn"></a>Točka web VPN-a
VPN točke web zahtijeva svaki klijentskom računalu da biste se povezali s vlastitom VPN-a u Azure. Za scenarije SAP smo gledate, točke web povezivanje nije praktično. Zbog toga ne daljnje reference će dobiti za povezivanje VPN točke na web.

[komentar]: <>  (MSSedusch – dodatne informacije Ovdje možete pronaći)
[komentar]: <>  (MShermannd obveze veza više nije valjana; Ali ARM ipak nije podržan – pogledajte sljedeću vezu ispod)
[komentar]: <>  (MSSedusch – < http://msdn.microsoft.com/library/azure/dn133798.aspx>.)
[komentar]: <>  (MShermannd obveze pokažite na web-mjesta ne podržava još ARM)
[komentar]: <>  (MSSedusch – < https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/>)

#### <a name="multi-site-vpn"></a>VPN više web-mjesta
Azure danas također nudi mogućnost da biste stvorili povezivanje VPN više web-mjesta za jednu Azure pretplatu. Prethodno jedne pretplate je ograničeno na VPN vezu s jednog web-mjesto. To ograničenje nije Odsutan s više web-mjesta VPN veze za jednu pretplatu. To omogućuje odražava više područja Azure za određenu pretplatu putem konfiguracije više lokacija.

Dodatne dokumentaciju potražite [u ovom se članku][vpn-gateway-create-site-to-site-rm-powershell]
[komentar]: <> (MShermannd obveze pronaći veze nema OBLAK oznake strukture doku)

#### <a name="vnet-to-vnet-connection"></a>VNet VNet vezu
Korištenje više web-mjesta VPN, morate konfigurirati zasebnom Azure virtualne mreže u svakoj od tih regija. No često imate obavezne komponente softver u različitim područjima treba komunikaciju. Najbolje ovu komunikaciju ne usmjeravati na određenu regiju Azure na lokalni i iz nje na područje Azure. Prečac Azure nudi mogućnost konfigurirati vezu s jedne mreže virtualne Azure u jednom području s drugom mrežom virtualne Azure smješten u drugoj regiji. Ta je funkcija zove VNet VNet veze. Dodatne informacije o ta je funkcija nalazi se ovdje: <https://azure.microsoft.com/documentation/articles/vpn-gateway-vnet-vnet-rm-ps/>.

#### <a name="private-connection-to-azure--expressroute"></a>Privatne veze Azure – ExpressRoute
Microsoft Azure ExpressRoute omogućuje stvaranje privatne veze između centre za Azure podataka i infrastrukture lokalnog ili kupca ili u okruženju zajednički mjesto. ExpressRoute je nudi razne MPLS davatelji VPN-a (paketa pojavljivali) ili drugim davateljima usluga mreže. ExpressRoute veze ne otvorite putem javnog Interneta. ExpressRoute veze nude veću sigurnost, više pouzdanosti kroz više paralelnih krugova, brže brzine i donjem latencies od standardne veze putem Interneta. 

Potražite dodatne informacije o Azure ExpressRoute i ponuda ovdje:

* <https://Azure.microsoft.com/Documentation/Services/expressroute/>
* <https://Azure.microsoft.com/pricing/details/expressroute/>
* <https://Azure.microsoft.com/Documentation/articles/expressroute-FAQs/>

Usmjeravanje eksplicitnih omogućuje višestruke pretplate Azure putem jednog elektronička ExpressRoute kao što je ovdje navedenih 

* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-linkvnet-arm/> 
* <https://Azure.microsoft.com/Documentation/articles/expressroute-howto-circuit-arm/>


#### <a name="forced-tunneling-in-case-of-cross-premise"></a>Prisilno tuneliranja u slučaju unakrsno lokaciji
Za pridruživanje lokalne domene putem web-mjesto, mjesta točke ili ExpressRoute VMs, morate da biste bili sigurni da postavke proxy poslužitelja Internet početak uvode se za sve korisnike te VMs. Prema zadanim postavkama, softver koji se izvodi u tim VMs ili korisnika putem preglednika za pristup Internetu bi proći kroz proxy poslužitelj tvrtke, ali se želite povezati izravno putem Azure s Internetom. No čak i postavka proxyja nije rješenje 100% da biste usmjerili promet putem proxyja tvrtke jer je odgovornosti softvera i usluga da biste provjerili proxy poslužitelj. Ako to je činite softver koji se izvodi u na VM ili administrator upravlja postavkama, možete promet s Internetom ponovno se detoured izravno putem Azure s Internetom. 

Da biste izbjegli ovo, možete konfigurirati prisilno tuneliranje s povezivanjem na web-mjesto između lokalnog i Azure. Detaljan opis značajke prisilno tuneliranje je objavljeni ovdje <https://azure.microsoft.com/documentation/articles/vpn-gateway-forced-tunneling-rm/> 

Prisilne Tunneling s ExpressRoute omogućena je prema klijentima oglašavanje zadani smjer putem sesije peering ExpressRoute BGP.

#### <a name="summary-of-azure-networking"></a>Sažetak Azure s mrežom
Ovo poglavlje sadrži važne detalje o Azure s mrežom. Ovo je sažetak glavnih točaka:


* Azure virtualne mreže omogućuje postavljanje mreže u skladu s vlastitim potrebama
* Azure virtualne mreže koje se mogu leveraged da biste dodijelili rasponi IP adresa za VMs ili fiksnim IP adrese dodijeliti VMs
* Da biste Postavljanje veze web-mjesto ili točke na mjestu morate najprije stvorite Azure virtualne mreže
* Kada je uveden virtualnog računala više nije moguće promijeniti virtualne mreže dodijeljene u VM

### <a name="quotas-in-azure-virtual-machine-services"></a>Kvota u servisa Azure virtualnog računala
Ne možemo moraju biti Očisti o fact prostora za pohranu i mrežne infrastrukture se razmjenjuju VMs radi različite usluge u Azure infrastrukture. I kao centre za klijenta vlastite podatke previše dodjeljivanje nekih resursa infrastrukture odvija da biste diploma. Azure platforme Microsoft koristi disk, procesora, mreže i druge kvote da biste ograničili potrošnje resursa i da biste sačuvali dosljedne i deterministic performanse.  Različite vrste VM (A5, A6 itd.) imaju različite kvote za broj diskova procesora, RAM-a i mrežne.

> [AZURE.NOTE] Opterećenje procesora i memorije resursa koje VM podržava SAP su unaprijed dodijeljene na čvorove glavnog računala. To znači da kada je implementiran u VM, resursa na glavnom računalu bit će dostupni onako kako su definirana vrsta VM.

Prilikom planiranja i SAP-a za promjenu veličine na rješenja Azure smatra mora se kvote za svaki veličina virtualnog računala.  Kvota VM opisane [u nastavku][virtual-machines-sizes].

Kvota opisane predstavljaju theoretical maksimalne vrijednosti.  Ograničenje od IOPS po VHD je moguće postići s malim IOs (8kb), no vjerojatno je nije moguće postići s velikim IOs (1Mb).  Ograničenje IOPS Poboljšana je preciznosti jedan VHDs.

Kao stabla odlučivanja gruba odlučite li upit sustava SAP pristaju servisa Azure virtualnog računala i njegovim mogućnostima ili li postojeći sustav potrebno je konfigurirati drugačije za implementaciju sustava na Azure, ispod stabla odlučivanja mogu koristiti:
 
![Stabla odlučivanja odluči mogućnost za implementaciju SAP-a na Azure][planning-guide-figure-700]

**Korak 1**: najvažnije informacije za početak s je SAPS obavezu navedeni sustava SAP. Preduvjeti za SAPS moraju biti odvojeni u dijelu DBMS i dio aplikacije SAP, čak i ako je sustava SAP već distribuiranih lokalnog u konfiguraciji 2 sloja. Za postojeće sustave SAPS vezane uz hardver koristi često se može odrediti ili Procijenjena na temelju postojeće jednonitnih SAP. Rezultati nalazi se ovdje: <http://global.sap.com/campaigns/benchmark/index.epx>. Za nove distribuiranih sustavima SAP, trebali biste imati nestalo kroz vježbu za promjenu veličine koji treba odrediti preduvjeti SAPS sustava.
Vidi također ovom blogu i priloženi dokument za promjenu veličine SAP-a na Azure: <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

**Korak 2**: za postojeće sustave/i glasnoću i operacije/i sekundi na poslužitelju DBMS mjeri se. Za nove planiranog sustave vježbu promjenu veličine za novi sustav i mora dati gruba ideje preduvjeti/i na strani DBMS. Ako je sigurni, ipak ćete morati vođenje dokaz pojam.

**Korak 3**: Usporedba SAPS obavezu DBMS poslužitelj s SAPS pružaju različite vrste VM Azure. Informacije o SAPS različitih vrsta Azure VM navedenih u bilješku SAP [1928533]. Žarište mora biti na DBMS VM najprije jer je baza podataka sloj sloj u sustavu NetWeaver SAP-a za skaliranje izgleda u većini implementacije. Nasuprot tome, aplikacijskom sloju SAP možete skalirana odgovor. Ako nijedna u SAP nije podržana Azure VM vrste možete održati potrebna SAPS, radno opterećenje planiranog SAP-a sustava nije moguće pokrenuti na Azure. Koje bilo potrebno za implementaciju sustava lokalnog ili ćete morati promijeniti glasnoću radno opterećenje za sustav.

**Korak 4**: kao dokumentirane [ovdje][virtual-machines-sizes], Azure nameće kvota za IOPS po VHD neovisno bez obzira koristite li standardne prostora za pohranu ili Premium prostora za pohranu. Ovisi o vrsti VM, broj VHDs koji se može postaviti mijenja se. Zbog toga možete izračunati maksimalni broj IOPS koju možete postići sa svakom različitih vrsta VM. Ovisno o tome izgleda datoteke baze podataka, možete stripe VHDs postane jedna jedinica u goste OS. Međutim, ako trenutne jedinice IOPS distribuiranih sustava SAP premašuje izračunati ograničenja najveću VM vrste Azure i ako je riječ o nema mogućnost vaše s dodatnu memoriju, radno opterećenje sustava SAP možete neće utjecati uzrokuje. U tim slučajevima kliknete na točku gdje treba ne implementacija sustava na Azure.

**Korak 5**: osobito u SAP sustave koji imaju implementiran lokalno u konfiguracije sloja 2, vjerojatnost su sustav morati konfiguriran na Azure u konfiguraciji 3 sloja. U ovom ćete koraku morate provjeriti postoji li komponenta korisničkog sučelja u aplikacijskom sloju SAP-a koji nije moguće omjera izgleda i koje bi uklapaju u opterećenje procesora i memorije resursi nude različitih vrsta Azure VM. Ako uistinu postoji takvo komponentu, sustava SAP i njegov radno opterećenje nije moguće implementirati u Azure. No ako ste možete skaliranje iz aplikacije komponente SAP-a u više VMs Azure, sustav može uvesti u Azure. 

**Korak 6**: Ako komponente sloja aplikacije DBMS i SAP, a mogu se izvoditi u Azure VMs, konfiguraciju mora biti definiran s regard za:

* Broj Azure VMs
* Vrste VM za pojedinih komponenti
* Broj VHDs u DBMS VM omogućuju dovoljno IOPS

## <a name="managing-azure-assets"></a>Upravljanje Azure resursi

### <a name="azure-portal"></a>Portal za Azure
Portal za Azure jedan je od tri sučelja za upravljanje implementacijama Azure VM. Zadatke osnovni upravljanja, kao što su implementacija VMs sa slike, možete učiniti putem portala za Azure. Uz to, stvaranje računa za pohranu, virtualne mreže i drugih komponenti za Azure su i zadaci Portal za Azure možete rukovati vrlo dobro. Međutim, funkciju kao što su prijenos VHDs iz lokalnog za Azure ili kopiranje VHD unutar Azure su zadaci koje zahtijevaju Alati drugih proizvođača ili administracije putem komponente PowerShell ili EŽA.
 
![Portal Microsoft Azure – pregled virtualnog računala][planning-guide-figure-800]

[komentar]: <>  (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-networks-create-vnet-arm-pportal/>)
[komentar]: <>  (MSSedusch * < https://azure.microsoft.com/documentation/articles/virtual-machines-windows-tutorial/>)

Moguće iz unutar portala Azure su Administracija i konfiguraciji zadataka za instancu virtualnog računala. 

Osim ponovnog pokretanja i isključivanja virtualnog računala možete priložiti, odvajanje i stvaranje disketa podataka za instancu virtualnog računala da biste zabilježili instancu za pripremu slike i konfiguracija veličine instance virtualnog računala.

Portal za Azure predstavlja osnovne funkcije možete implementirati i konfigurirati VMs i mnoge druge servise Azure. No ne svi su dostupni funkcionalnost je prekriven Portal za Azure. Na portalu za Azure nije moguće obavljati zadatke kao što su:

* Prijenos VHDs za Azure
* Kopiranje VMs

[komentar]: <>  (MShermannd obveze što o Automatizacija usluga za SAP VMs?)
[komentar]: <>  (Implementacija MSSedusch više VMs OS-a u međuvremenu moguće)
[komentar]: <>  (MSSedusch i bilo koju vrstu Automatizacija vezane uz implementaciju nije moguće uz portal za Azure. Zadatke kao što su skriptiranih implementaciju VMs više nije moguće putem portala za Azure.) 

### <a name="management-via-microsoft-azure-powershell-cmdlets"></a>Upravljanje putem cmdleta ljuske PowerShell za Microsoft Azure
Komponente Windows PowerShell je Napredna i extensible framework koje sadrži kupci implementacija veći broj sustavi u Azure je široko prihvatile. Nakon instalacije cmdleta ljuske PowerShell na radnu površinu, prijenosno računalo ili namjenski upravljanje stanice, cmdleta ljuske PowerShell možete pokrenuti daljinski.

Postupak da biste omogućili lokalnu radnu površinu/prijenosno računalo za korištenje cmdleta ljuske PowerShell Azure te o tome kako konfigurirati one za korištenje s Azure subscription(s) je opisano u [ovom članku][powershell-install-configure]. 

Detaljnije upute o tome kako instalirati, ažuriranje i konfiguriranje Azure PowerShell cmdleti i nalazi se u [to ovog poglavlja od vodič za implementaciju] [implementacije – vodič – 4.1].

Korisnička sučelja dosad nije je li PowerShell (PS) certainly jače alat za implementaciju VMs i da biste stvorili prilagođeni korake implementacije VMs. Sve klijente pokrenute instance SAP-a u Azure pomoću cmdleta PS dodatku izjavi zadatke upravljanja njihovu učinite na portalu za Azure ili čak i pomoću cmdleta PS isključivo upravljanje postupak implementacije u Azure. Budući da se Azure određene cmdleti za zajedničko korištenje iste konvencija imenovanja kao više od 2000 povezane cmdleta sustava Windows, je jednostavan zadatak za administratore sustava Windows odražava tih cmdleta.

Pogledajte primjer ovdje: <http://blogs.technet.com/b/keithmayer/archive/2015/07/07/18-steps-for-end-to-end-iaas-provisioning-in-the-cloud-with-azure-resource-manager-arm-powershell-and-desired-state-configuration-dsc.aspx>

[komentar]: <>  (Popis obveza MShermannd opisuju naredba nova EŽA kada testirati)
Uvođenje Azure nadzor proširenje za SAP-a (potražite u članku poglavlja [Azure nadzor rješenje za SAP] [planiranja – vodič – 9,1] u ovom dokumentu) je moguće samo putem komponente PowerShell ili EŽA. Stoga je obavezna za instalaciju i konfiguriranje PowerShell ili EŽA pri implementaciji ili administriranje SAP NetWeaver sustavu Azure.  

Kao što je Azure pruža dodatne funkcije, novi cmdleti za PS prelaska potrebno dodati koje je potrebno ažuriranje programa s cmdletima. Stoga je li bolje da biste provjerili web-mjesto za preuzimanje Azure barem jednom mjesec <https://azure.microsoft.com/downloads/> za novu verziju s cmdletima. Novu verziju instalirat će se samo pri vrhu starije verzije.

Za Opći popis stavki Azure povezane naredbe ljuske PowerShell potražite ovdje: <https://msdn.microsoft.com/library/azure/dn708514.aspx>. 

### <a name="management-via-microsoft-azure-cli-commands"></a>Upravljanje putem naredbe EŽA Microsoft Azure

Za korisnike koji koristite Linux i želite upravljati Azure resursi Powershell možda neće biti mogućnost. Microsoft nudi Azure EŽA kao alternativu.
Azure EŽA pruža skup Otvori izvor različite platforme naredbi za rad s platforme Azure. Azure EŽA nudi velik broj isti funkcionalnosti pronaći na portalu za Azure.

Informacije o instalacija, konfiguracija i upute za korištenje EŽA naredbi za obavljanje zadatka Azure potražite u članku

* [Instalacija Azure EŽA][xplat-cli]
* [Upravljanje virtualnim strojevima pomoću predložaka Voditelj resursa Azure i EŽA Azure i][virtual-machines-linux-cli-deploy-templates]
* [Korištenje Azure EŽA za Mac i Linux Windows s Azure Voditelj resursa][xplat-cli-azure-resource-manager]

Pročitajte i poglavlja [Azure EŽA za Linux VMs] [implementacije – vodič za-4.5.2] u [upute za uvođenje] [– vodič za planiranje] o korištenju Azure EŽA za implementaciju Azure nadzor proširenje za SAP-a.

## <a name="different-ways-to-deploy-vms-for-sap-in-azure"></a>Različite načine za implementaciju VMs za SAP servisu Azure
Ovo poglavlje će saznat ćete različite načine za implementaciju VM u Azure. Postupke pripreme dodatne, kao i rukovanje VHDs i VMs u Azure možete pronaći u Ovo poglavlje.

### <a name="deployment-of-vms-for-sap"></a>Uvođenje VMs za SAP
Microsoft Azure nudi različite načine za implementaciju VMs i pridruženi diskova. Stoga je važno da biste razumjeli razlike jer pripreme na VMs može se razlikovati od ovisno o način implementacije. Općenito govoreći, ne možemo će pogledajte sljedeće scenarije:

#### <a name="4d175f1b-7353-4137-9d2f-817683c26e53"></a>Premještanje na VM s lokalnim Azure s diskom koji nisu GRG generalizirano
Planiranje da biste premjestili određene sustava SAP lokalnim Azure. To možete učiniti tako da prenesete VHD koji sadrži binarne datoteke OS, binarne datoteke za SAP i DBMS plus na VHDs s podacima i zapisnika datotekama DBMS za Azure. Za razliku [scenarij #2 ispod] [planiranja – vodič za-5.1.2], držite naziv glavnog računala, SAP SID i SAP korisničkih računa u Azure VM oni su konfigurirano u lokalnog okruženja. Stoga generalizing slike nije potreban. Pročitajte članak poglavlja [Priprema za premještanje na VM lokalnim Azure s diskom koji nisu GRG generalizirano] [planiranja – vodič za-5.2.1] ovog dokumenta za lokalni pripremnih koraka i prijenos nije GRG generalizirano VMs ili VHDs za Azure. Pročitajte poglavlja [scenarij 3: premještanje na VM iz lokalnog VHD Azure za koje nisu GRG generalizirano pomoću SAP] [implementacije – vodič – 3.4] u [upute za uvođenje] [– vodič za implementaciju] detaljne upute implementacije takve slike u Azure.

#### <a name="e18f7839-c0e2-4385-b1e6-4538453a285c"></a>Implementacija VM sa slikom za određene korisnike
Zbog određene zakrpu preduvjeti za verzija OS-a ili DBMS navedeni slike u trgovine Windows Azure možda odgovara vašim potrebama. Stoga ćete morati stvoriti VM pomoću vlastite 'privatne OS/DBMS VM slike koje možete uvesti naknadno nekoliko puta. Priprema takve 'privatne sliku za dupliciranje, imaju smatra se sljedeće stavke:

___

> ![Windows][Logo_Windows] Windows
>
> Postavke sustava Windows (kao što je Windows SID i naziv glavnog računala) mora biti izvučene/GRG generalizirano na lokalni VM putem naredbu sysprep.
[komentar]: <>  (MSSedusch > vidjeli više detalja ovdje:)
[komentar]: <> Prva veza  (MShermannd obveze je klasični model. Niste pronašli članak Azure oznake strukture doku)
[komentar]: <>  (MSSedusch >< https://azure.microsoft.com/documentation/articles/virtual-machines-create-upload-vhd-windows-server/>)
[komentar]: <>  (MSSedusch >< http://blogs.technet.com/b/blainbar/archive/2014/09/12/modernizing-your-infrastructure-with-hybrid-cloud-using-custom-vm-images-and-resource-groups-in-microsoft-azure-part-21-blain-barton.aspx>)
>
> ![Linux][Logo_Linux] Linux
>
> Slijedite korake opisane u sljedećim člancima za [SUSE] [ virtual-machines-linux-create-upload-vhd-suse] ili [Je vaša crveno] [ virtual-machines-linux-redhat-create-upload-vhd] da biste pripremili VHD prenijeti Azure.

___

Ako ste već instalirali SAP sadržaj u vaše lokalne VM (osobito za sustave sloja 2), možete prilagoditi postavke sustava SAP nakon implementacije VM Azure kroz instanci Preimenovanje postupak podržava SAP softver dodjeljivanje Upravitelj (SAP bilješke [1619720]). Potražite u članku poglavlja [Priprema za implementaciju VM sa slikom za određene korisnike za SAP] [planiranja – vodič za-5.2.2] i [prijenosu VHD lokalnim i Azure] [planiranja – vodič za-5.3.2] ovog dokumenta za lokalni pripremnih koraka i prijenos generalizirano VM za Azure. Pročitajte poglavlja [scenarij 2: implementacija VM s prilagođenu sliku za SAP] [implementacije – vodič – 3,3] u [upute za uvođenje] [– vodič za implementaciju] detaljne upute implementacije takve sliku u programu Azure.

#### <a name="deploying-a-vm-out-of-the-azure-marketplace"></a>Implementacija VM iz trgovine Windows Azure
Želite li koristiti Microsoft ili 3 strana dobili VM sliku iz trgovine Azure za implementaciju sustava VM. Nakon implementiran na VM u Azure, slijedite isti smjernice i alati da biste instalirali softver za SAP i/ili DBMS unutar vaše VM koje biste slijedili u okruženju na lokaciji. Detaljnije opis implementaciju, pročitajte članak poglavlja [scenarij 1: implementacija VM iz trgovine Azure za SAP] [implementacije – vodič za-3,2] u [upute za uvođenje] [– vodič za implementaciju].

### <a name="6ffb9f41-a292-40bf-9e70-8204448559e7"></a>Priprema VMs s SAP-a za Azure
Prije prijenosa VMs u Azure potrebno da biste bili sigurni VMs i VHDs ispunjavaju određene preduvjeti. Postoje razlike small ovisno o način implementacije koji se koristi. 

#### <a name="1b287330-944b-495d-9ea7-94b83aff73ef"></a>Priprema za premještanje na VM lokalnim Azure s diskom koji nisu GRG generalizirano
Uobičajeni način implementacije je da biste premjestili na postojeće VM koji se izvodi upit sustava SAP lokalnim Azure. Da VM i SAP sustavu na VM samo trebale bi funkcionirati u Azure pomoću isti naziv glavnog računala i vrlo je vjerojatno isti SID SAP-a. U ovom slučaju goste OS VM treba neće biti GRG generalizirano za više implementacije. Ako imate lokalne mreže proširen u Azure (potražite u članku poglavlja [izdvojeno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalnu mrežu] [planiranja – vodič – 2.2] u ovom dokumentu), a zatim čak i isti domenski računi se može koristiti unutar na VM kao one su se koristile prije lokalnog. 

Preduvjeti prilikom pripreme vlastite Disk VM Azure su:

* Izvorno VHD koja sadrži operacijski sustav nije imaju maksimalnu veličinu 127 GB samo. To ograničenje imate ukloniti na kraju ožujak 2015. Sada VHD koja sadrži operacijski sustav može biti do 1TB po veličini kao Azure pohranu hostira VHD kao i.
[komentar]: <>  (Popis obveza MShermannd ste da biste provjerili EŽA i pretvara u statični)
* Ona mora biti u nepromjenjivi oblik VHD. Dinamični VHDs ili VHDs u obliku VHDx su još nije podržana na Azure. Dinamični VHDs pretvorit će se u statični VHDs prilikom učitavanja VHD s PowerShell Cmdlete ili EŽA
* VHDs koja su postavljena za na VM i mora biti postavljen ponovno Azure VM će se moraju biti u nepromjenjivi VHD oblik. Isti ograničenje veličine diska OS odnosi se na kao i diskova podataka. VHDs mogu sadržavati maksimalnu veličinu 1 TB. Dinamični VHDs pretvorit će se u statični VHDs prilikom učitavanja VHD s PowerShell Cmdlete ili EŽA
* Dodajte drugi lokalni račun s administratorskim ovlastima koje se mogu koristiti Microsoftovoj službi za podršku ili koji se mogu dodijeliti kao kontekst za usluge i aplikacije da biste pokrenuli u dok je implementiran u VM i prikladnije korisnici mogu koristiti.
* U slučaju korištenja scenarij samo oblak implementacije (potražite u članku poglavlja [samo za oblak – implementacijama virtualnog računala u Azure bez međuzavisnosti lokalne mreže kupca] [planiranja-vodič – 2.1] ovog dokumenta) u kombinaciji s Ova metoda uvođenja računa domene možda neće funkcionirati nakon Azure Disk je uveden u Azure. Ovo je posebno točno za račune koji se koriste za pokretanje servisa kao što je aplikacija DBMS ili SAP-a. Stoga ćete morati zamijenite takve domenski računi VM lokalni računi i brisanje računa domene lokalnog u na VM. Zadržavanje lokalne domene korisnike na slici VM nije problem kada se VM implementiran u scenariju više lokacija kao što je opisano u poglavlja [izdvojeno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalnu mrežu] [planiranja – vodič – 2.2] u ovom dokumentu.
* Ako domenski računi su koristi kao DBMS prijave ili pojedinačnim korisnicima kada je pokrenut na sustavu lokalnog poslužitelja i one VMs su bi trebao biti implementirano u scenarijima samo oblak, korisnici domene moraju biti izbrisane. Morate da biste bili sigurni da lokalni administrator i drugi VM lokalni korisnik dodaju se kao prijava/korisnik u DBMS kao administratori.
* Dodajte druge lokalni računi kao one će možda biti potrebno scenarija određene implementacije.

___

> ![Windows][Logo_Windows] Windows
>
> U ovom slučaju ne generalizacije (sysprep) na VM potreban je da biste prenijeli i implementacija VM na Azure.
> Provjerite pogon D:\ nije koristi li automount disk skup za priložene diskova kao što je opisano u poglavlja [postavka automount za priložene diskova] [planiranja – vodič za-5.5.3] u ovom dokumentu.
> 
> ![Linux][Logo_Linux] Linux
>
> U ovom slučaju ne generalizacije (waagent-deprovision) od na VM potreban je da biste prenijeli i implementacija VM na Azure.
> Provjerite je li se ne koristi/mnt/resursa i da se svi diskova postavljen putem uuid. Za OS disk provjerite je li unos učitavač pokretanja i odražava postavljanja utemeljenu na uuid.

___

#### <a name="57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3"></a>Priprema za implementaciju VM sa slikom za određene korisnike za SAP
VHD datoteke koje sadrže generalizirano OS također se pohranjuju u spremnika na računi servisa Azure prostora za pohranu. Možete implementirati novi VM takve slike VHD tako da upućuju na VHD kao izvor VHD u datotekama sustava uvođenje predloška kao što je opisano u poglavlja [scenarij 2: implementacija VM prilagođene slikom za SAP] [implementacije – vodič – 3,3] [implementacije vodiča] [– vodič za implementaciju]. 

Preduvjeti prilikom pripreme vlastite slike VM Azure su:

* Izvorno VHD koja sadrži operacijski sustav nije imaju maksimalnu veličinu 127 GB samo. To ograničenje imate ukloniti na kraju ožujak 2015. Sada VHD koja sadrži operacijski sustav može biti do 1TB po veličini kao Azure pohranu hostira VHD kao i.
[komentar]: <>  (Popis obveza MShermannd ste da biste provjerili EŽA i pretvara u statični)
* Ona mora biti u nepromjenjivi oblik VHD. Dinamični VHDs ili VHDs u obliku VHDx su još nije podržana na Azure. Dinamični VHDs pretvorit će se u statični VHDs prilikom učitavanja VHD s PowerShell Cmdlete ili EŽA
* VHDs koja su postavljena za na VM i mora biti postavljen ponovno Azure VM će se moraju biti u nepromjenjivi VHD oblik. Isti ograničenje veličine diska OS odnosi se na kao i diskova podataka. VHDs mogu sadržavati maksimalnu veličinu 1 TB. Dinamični VHDs pretvorit će se u statični VHDs prilikom učitavanja VHD s PowerShell Cmdlete ili EŽA
* Budući da svi korisnici domena registrirana kao korisnika u na VM će ne postoji u scenariju samo oblak (potražite u članku poglavlja [samo za oblak – implementacijama virtualnog računala u Azure bez međuzavisnosti lokalne mreže kupca] [planiranja-vodič – 2.1] ovog dokumenta), servise pomoću takve domene računa ne funkcioniraju kad je na slici u uveden u Azure. Ovo je posebno točno za račune koji se koriste za pokretanje servisa kao što je DBMS ili SAP aplikacija. Stoga ćete morati zamijenite takve domenski računi VM lokalni računi i brisanje računa domene lokalnog u na VM. Zadržavanje lokalne domene korisnike na slici VM možda neće biti problem prilikom na VM implementiran u scenariju unakrsno lokaciji kao što je opisano u poglavlja [izdvojeno lokaciji - implementacije jednu ili više VMs SAP-a u Azure s preduvjet se potpuno integrirani u lokalnu mrežu] [planiranja – vodič – 2.2] u ovom dokumentu.
* Dodajte drugi lokalni račun s administratorskim ovlastima koje se mogu koristiti Microsoftovoj podršci u istrage problem ili koji se mogu dodijeliti kao kontekst za usluge i aplikacije da biste pokrenuli u dok je implementiran u VM i prikladnije korisnici mogu koristiti.
* U implementacijama samo oblak i gdje domenski računi su se koristile kao DBMS prijave ili pojedinačnim korisnicima kada se pokrene u sustavu Lokalni korisnici domene treba izbrisati. Morate provjerite je li lokalni administrator plus drugi VM lokalni korisnik dodan kao prijava/korisnik DBMS kao administratori.
* Dodajte druge lokalni računi kao one će možda biti potrebno scenarija određene implementacije.
* Ako slika sadrži instalacija programa SAP NetWeaver i preimenovanje naziv glavnog računala s izvornim nazivom mjestu Azure implementacije je vjerojatno, preporučuje se da biste kopirali najnoviju verziju sustava SAP softver dodjeljivanje Upravitelj DVD-a u predlošku. To će vam omogućiti da biste lakše koristiti funkciju Preimenuj SAP dobili prilagoditi promijenjene naziv glavnog računala i/ili promijenite SID sustava SAP unutar distribuiranih slika VM čim se pokreće novu kopiju.

___

> ![Windows][Logo_Windows] Windows
>
> Provjerite pogon D:\ nije koristi li automount disk skup za priložene diskova kao što je opisano u poglavlja [postavka automount za priložene diskova] [planiranja – vodič za-5.5.3] u ovom dokumentu.
> 
> ![Linux][Logo_Linux] Linux
>
> Provjerite je li se ne koristi/mnt/resursa i da se svi diskova postavljen putem uuid. Za na OS na disku Provjerite je li unos učitavač pokretanja odražava postavljanja uuid-poštu.

___

* SAP GUI (za administratora i postavljanje svrhe) biti unaprijed instalirana na takav predloška.
* Drugi softver koji je potrebno je uspješno pokrenuti na VMs u više lokacija scenarijima moguće je instalirati sve dok se ovaj softver možete raditi s Preimenuj u VM.

Ako na VM pripremljeni potpuno generički i naposljetku neovisno o računi/korisnika nije dostupno u scenariju ciljano Azure implementaciju, obavljaju je posljednji korak pripreme generalizing pretraživanje kao sliku.

##### <a name="generalizing-a-vm"></a>Generalizing na VM 

___

[komentar]: <>  (MShermannd obveze ste da biste pronašli članke bolje / oznake strukture doku o generalizing VMs za ARM)
> ![Windows][Logo_Windows] Windows
>
> Posljednji korak je da biste se prijavili VM pomoću administratorskog računa. Otvorite prozor naredbenog Windows kao "administrator". Idite na ...\windows\system32\sysprep i izvršavanje sysprep.exe.
> Pojavit će se maleni prozor. Važno je da potvrdite mogućnost 'Konf' (zadano je isključeno), a zatim promijenite mogućnost isključivanja iz zadane od "Nakon ponovnog pokretanja" u 'Isključivanje'. Ovaj postupak pretpostavlja da je postupak sysprep izvedena lokalno u OS-a goste na VM.
> Ako želite, slijedite postupak s VM izvodi Azure, slijedite korake opisane u [ovom članku][virtual-machines-windows-capture-image].
> 
> ![Linux][Logo_Linux] Linux
>
> [Kako snimiti Linux virtualnog računala da biste koristili kao predložak Voditelj resursa][virtual-machines-linux-capture-image]

___

### <a name="transferring-vms-and-vhds-between-on-premises-to-azure"></a>Prijenos VMs i VHDs između lokalnog za Azure
Budući da se prenosi VM slike i diskova Azure nije moguće putem portala za Azure, morate koristiti Azure PowerShell cmdleti ili EŽA. Moguće je i pomoću alata za 'AzCopy'. Alat za možete kopirati VHDs između lokalnog i Azure (u oba smjera). To možete kopirati i VHDs među područjima Azure. Pogledajte dokumentaciju [ovaj] [ storage-use-azcopy] za preuzimanje i korištenje funkcije AzCopy.

Treći alternativu bi raznih alata GUI usmjerena drugih proizvođača. Međutim, provjerite ove alate su podrške Azure stranice blob-ova. Naš svrhe moramo da koristi spremište blobova platforme Azure stranice (ovdje opisuje razlike: <https://msdn.microsoft.com/library/windowsazure/ee691964.aspx>). I alati nudi Azure su vrlo učinkovitog u sažimanje VMs i VHDs koji moraju prenijeti. To je važno jer ovo učinkovitosti u sažimanja smanjuje vrijeme učitavanja (čime se mijenja se ipak ovisno o prijenos vezu s Internetom iz lokalnog pogona i regija Azure implementacije ciljani). To je sajma pretpostavci da prijenos VM ili VHD s europskom mjesta Sjedinjenih Država temelje Azure podataka centrima će trajati dulje nego prijenos iste VMs/VHDs centrima europskom Azure podataka. 

#### <a name="a43e40e6-1acc-4633-9816-8f095d5a7b6a"></a>Prijenos VHD lokalnim za Azure
Da biste prenijeli postojeći VM ili VHD iz lokalne mreže kao što je VM ili VHD treba zadovoljava preduvjete, kao što je navedeno u poglavlja [Priprema za premještanje na VM lokalnim Azure s diskom koji nisu GRG generalizirano] [planiranja – vodič za-5.2.1] ovog dokumenta.

Takve VM ne moraju biti GRG generalizirano i moguće ih je prenijeti u stanje i oblik ima nakon isključivanja s lokalnim strane. Isto vrijedi i za dodatne VHDs koji ne sadrže operacijske sustave. 

##### <a name="uploading-a-vhd-and-making-it-an-azure-disk"></a>Prijenos na VHD ili upućivanje na Disk Azure
U ovom slučaju želimo prijenos VHD, sa ili bez OS-a u njemu, pa ga postaviti VM kao podatkovni disk ili ga koristiti kao OS disk. Ovo je s više koraka 

__PowerShell__

* Prijavite se na pretplatu za _Prijavu AzureRmAccount_
* Postavljanje pretplatu na svom kontekstu s _Skup AzureRmContext_ i parametar SubscriptionId ili SubscriptionName - potražite u članku <https://msdn.microsoft.com/library/mt619263.aspx>
* Prijenos VHD s _Dodaj AzureRmVhd_ s računom za pohranu Azure – pogledajte <https://msdn.microsoft.com/library/mt603554.aspx>
* Postavite diska OS novi VM config VHD s _Postavljanje AzureRmVMOSDisk_ – pogledajte <https://msdn.microsoft.com/library/mt603746.aspx>
* Stvaranje nove VM iz config VM s _Novo AzureRmVM_ – pogledajte <https://msdn.microsoft.com/library/mt603754.aspx>
* Podatkovni disk dodati nove VM s _Dodaj AzureRmVMDataDisk_ – pogledajte <https://msdn.microsoft.com/library/mt603673.aspx>

__Azure EŽA__

* Prebacivanje u način rada Voditelj resursa Azure s _azure config način arm_
* Prijavite se na pretplatu _azure prijava_
* Odaberite pretplatu _skup račun za azure `<subscription name or id` _
* Prijenos VHD s _azure spremišta blobova prijenos_ – potražite u odjeljku [Korištenje EŽA Azure s Azure prostora za pohranu][storage-azure-cli]
* Stvaranje nove VM navodeći prenesene VHD kao OS disk s _azure vm stvaranje_ i parametar -d
* Podatkovni disk dodati nove VM s _vm diska priložite – novi_

__Predložak__

* Prijenos VHD s EŽA Powershell ili Azure
* Implementacija u VM s predloškom JSON koji se odnose na VHD kao što je prikazano u [ovom primjeru JSON predlošku](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-from-specialized-vhd/azuredeploy.json).

#### <a name="deployment-of-a-vm-image"></a>Uvođenje VM slike
Da biste prenijeli postojeći VM ili VHD iz lokalne mreže da biste upotrijebili sliku Azure VM kao što je VM ili VHD morati zadovoljava preduvjete navedene u poglavlja [Priprema za implementaciju VM sa slikom za određene korisnike za SAP] [planiranja – vodič za-5.2.2] ovog dokumenta.

* U sustavu Windows koristiti _sysprep_ ili _waagent-deprovision_ na Linux da biste generalize VM - potražite [u] članku snimanje virtualnog računala za Windows u modelu implementaciju upravljanja resursima[ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] ili [kako snimiti Linux virtualnog računala da biste koristili kao predložak Voditelj resursa][virtual-machines-linux-capture-image-capture]
* Prijavite se na pretplatu za _Prijavu AzureRmAccount_
* Postavljanje pretplatu na svom kontekstu s _Skup AzureRmContext_ i parametar SubscriptionId ili SubscriptionName - potražite u članku <https://msdn.microsoft.com/library/mt619263.aspx>
* Prijenos VHD s _Dodaj AzureRmVhd_ s računom za pohranu Azure – pogledajte <https://msdn.microsoft.com/library/mt603554.aspx>
* Postavite diska OS novi VM config VHD s _skup AzureRmVMOSDisk - SourceImageUri - CreateOption fromImage_ -potražite u članku <https://msdn.microsoft.com/library/mt603746.aspx>
* Stvaranje nove VM iz config VM s _Novo AzureRmVM_ – pogledajte <https://msdn.microsoft.com/library/mt603754.aspx>

__Azure EŽA__

* U sustavu Windows koristiti _sysprep_ ili _waagent-deprovision_ na Linux da biste generalize VM - potražite [u] članku snimanje virtualnog računala za Windows u modelu implementaciju upravljanja resursima[ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] ili [kako snimiti Linux virtualnog računala da biste koristili kao predložak resursima] [ virtual-machines-linux-capture-image-capture] za Linux
* Prebacivanje u način rada Voditelj resursa Azure s _azure config način arm_
* Prijavite se na pretplatu _azure prijava_
* Odaberite pretplatu _skup račun za azure `<subscription name or id` _
* Prijenos VHD s _azure spremišta blobova prijenos_ – potražite u odjeljku [Korištenje EŽA Azure s Azure prostora za pohranu][storage-azure-cli]
* Stvaranje nove VM navodeći prenesene VHD kao OS disk s _azure vm stvaranje_ i parametar -pitanja

__Predložak__

* U sustavu Windows koristiti _sysprep_ ili _waagent-deprovision_ na Linux da biste generalize VM - potražite [u] članku snimanje virtualnog računala za Windows u modelu implementaciju upravljanja resursima[ virtual-machines-windows-capture-image-prepare-the-vm-for-image-capture] ili [kako snimiti Linux virtualnog računala da biste koristili kao predložak resursima] [ virtual-machines-linux-capture-image-capture] za Linux
* Prijenos VHD s EŽA Powershell ili Azure
* Implementacija u VM s predloškom JSON pozivanju slika VHD kao što je prikazano u [ovom primjeru JSON predlošku](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).

#### <a name="downloading-vhds-to-on-premises"></a>Preuzimanje VHDs na lokalni
Azure infrastrukture kao servis nije jednosmjerna ulica su samo moći prenositi VHDs i SAP proizvod sustava. Možete premjestiti SAP sustavi iz Azure natrag u svijetu lokalnog.

Tijekom preuzimanja u VHDs ne može biti aktivna. Čak i kad je preuzimanje VHDs koja su postavljena na VMs, na VM mora biti zatvaranja. Ako samo želite preuzeti sadržaj baze podataka koji moraju se koristiti za postavljanje novog sustava lokalnog i ako je prihvatljiva da tijekom vremena preuzimanje i instalaciju novi sustav da sustavu Azure i dalje moći radu, nije moguće izbjegli dugo nedostupnost izvođenjem sigurnosnu kopiju Komprimirana baze podataka u na VHD i samo preuzeti tu VHD umjesto i preuzimanje VM osnovni OS.

#### <a name="powershell"></a>PowerShell

Kada se zaustavio sustava SAP i sustava VM je isključen, možete pomoću cmdleta PowerShell Spremi AzureRmVhd na odredištu lokalnog da biste preuzeli diskova VHD natrag na lokalni svijeta. Da biste mogli provesti, morate URL VHD koje možete pronaći u 'za pohranu dio"Azure Portal (potrebe da biste prešli na račun za pohranu i spremnik za pohranu gdje je stvoren u VHD), a morate znati gdje se VHD trebaju biti kopirane.

Zatim možete koristiti naredbu definiranjem jednostavno parametar SourceUri kao URL VHD da biste preuzeli i na LocalFilePath kao fizičke mjesto VHD (uključujući njegov naziv). Naredba može izgledati kao što su:

```powerhell
Save-AzureRmVhd -ResourceGroupName <resource group name of storage account> -SourceUri http://<storage account name>.blob.core.windows.net/<container name>/sapidedata.vhd -LocalFilePath E:\Azure_downloads\sapidesdata.vhd
```

Dodatne informacije o cmdletu Spremi AzureRmVhd, provjerite je li ovo <https://msdn.microsoft.com/library/mt622705.aspx>. 

#### <a name="cli"></a>EŽA

Kada je zaustavljena sustava SAP, a na VM je isključen, možete koristiti preuzimanje blobova platforme azure prostora za pohranu naredba Azure EŽA na odredištu lokalnog da biste preuzeli diskova VHD natrag na lokalni svijeta. Da biste to učinili, potrebno je naziv, a zatim spremnik VHD koje možete pronaći u 'Za pohranu odjeljku' portala za Azure (potrebe da biste prešli na račun za pohranu i spremnik za pohranu gdje je stvoren u VHD), a morate znati gdje se VHD trebaju biti kopirane.

Zatim možete koristiti naredbu definiranjem jednostavno parametara blob i spremnik na VHD za preuzimanje i odredište kao fizičke odredištu VHD (uključujući njegov naziv). Naredba može izgledati kao što su:

```
azure storage blob download --blob <name of the VHD to download> --container <container of the VHD to download> --account-name <storage account name of the VHD to download> --account-key <storage account key> --destination <destination of the VHD to download> 
```

### <a name="transferring-vms-and-vhds-within-azure"></a>Prijenos VMs i VHDs unutar Azure

#### <a name="copying-sap-systems-within-azure"></a>Kopiranje sustavima SAP unutar Azure

SAP-a sustava ili čak i namjenski DBMS poslužitelj podrške SAP aplikacijskom sloju će se vjerojatno sastojati od nekoliko VHDs koji sadrže ili OS binarnih ili datoteke podataka i zapisnika SAP baze podataka. Ni Azure funkcije kopiranja VHDs ni Azure funkcionalnost spremanja VHDs na disk je mehanizam za sinkronizaciju koji bi snimke više VHDs sinkronizirano. Stoga stanje VHDs kopirane ili spremljenu čak i ako se one su postavljena na temelju isti VM bi drugi. To znači da u konkretni slučaja pamtiti različite podatke i logfile(s) sadržani u različitim VHDs baze podataka na kraju biti koje nisu usklađene. 

**Zaključak: Da bi se kopirajte ili spremite VHDs koje su dio programa SAP podatke o konfiguraciji sustava morate zaustaviti sustava SAP i morati zatvoriti distribuiranih VM. Samo zatim možete kopirati ili preuzimanje skup VHDs ili stvaranje kopije sustava SAP-a u Azure ili na lokalnim poslužiteljima.**

Diskova podataka spremaju se kao VHD datoteke u račun za Azure prostora za pohranu i može biti izravno prilaganje virtualnog računala ili se koristi kao sliku. U ovom slučaju na VHD se kopira na drugo mjesto prije beeing priložiti virtualnog računala. Puni naziv datoteke VHD u Azure mora biti jedinstveno unutar Azure. Kao što je rečeno prethodno već, naziv je vrsta tri dijela naziv koji izgleda kao: 

    http(s)://<storage account name>.blob.core.windows.net/<container name>/<vhd name>

##### <a name="powershell"></a>PowerShell
Pomoću cmdleta ljuske PowerShell Azure možete kopirati u VHD kao što je prikazano u [ovom članku][storage-powershell-guide-full-copy-vhd].

##### <a name="cli"></a>EŽA
Azure EŽA možete koristiti da biste kopirali u VHD kao što je prikazano u [ovom članku][storage-azure-cli-copy-blobs]

##### <a name="azure-storage-tools"></a>Alati za pohranu za Azure

* <http://azurestorageexplorer.codeplex.com/releases/View/125870>

Također postoje Profesionalna izdanja Azure prostora za pohranu programu Software Explorer koju je moguće pronaći ovdje:

* <http://www.cerebrata.com/>
* <http://clumsyleaf.com/products/cloudxplorer> 


Kopiraj na VHD sam prostora za pohranu subjekta je postupak koji traje samo nekoliko sekundi (slično SAN hardver stvaranja brze snimke s drži kopiju i Kopiraj na pisanje). Nakon što ste kopiju datoteke VHD možete priložiti virtualnog računala ili koristiti kao sliku da biste priložili kopije na VHD virtualnim strojevima.

##### <a name="powershell"></a>PowerShell

```powershell
# attach a vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <path to vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption attach
$vm | Update-AzureRmVM

# attach a copy of the vhd to a vm
$vm = Get-AzureRmVM -ResourceGroupName <resource group name> -Name <vm name>
$vm = Add-AzureRmVMDataDisk -VM $vm -Name newdatadisk -VhdUri <new path of vhd> -SourceImageUri <path to image vhd> -Caching <caching option> -DiskSizeInGB $null -Lun <lun e.g. 0> -CreateOption fromImage
$vm | Update-AzureRmVM
```
##### <a name="cli"></a>EŽA
```
azure config mode arm 

# attach a vhd to a vm
azure vm disk attach <resource group name> <vm name> <path to vhd>

# attach a copy of the vhd to a vm
# this scenario is currently not possible with Azure CLI. A workaround is to manually copy the vhd to the destination.
```

#### <a name="9789b076-2011-4afa-b2fe-b07a8aba58a1"></a>Kopiranje diskova između računa za pohranu za Azure
Ovaj zadatak nije moguće izvesti na portalu Azure. Možete filtar Azure PowerShell Cmdlete, Azure EŽA ili nekog drugog proizvođača za pohranu preglednika. PowerShell cmdleti ili EŽA naredbe možete stvoriti i upravljati blob-ova, što obuhvaća mogućnost asinkrono kopiranja blob-ova račune za pohranu i područja unutar Azure pretplate.

##### <a name="powershell"></a>PowerShell 

Kopiranje VHDs između pretplate moguće je. Za dodatne informacije pročitajte [Ovaj članak][storage-powershell-guide-full-copy-vhd]. 

Osnovni tok logike cmdlet PS izgleda ovako:

* Stvaranje kontekstu računa za pohranu za račun za pohranu izvora s _Novo AzureStorageContext_ - potražite u članku <https://msdn.microsoft.com/library/dn806380.aspx>
* Stvaranje računa kontekst za pohranu za račun prostora za pohranu za ciljnu s _Novo AzureStorageContext_ - potražite u članku <https://msdn.microsoft.com/library/dn806380.aspx>
* Pokretanje kopiju

```powershell
Start-AzureStorageBlobCopy -SrcBlob <source blob name> -SrcContainer <source container name> -SrcContext <variable containing context of source storage account> -DestBlob <target blob name> -DestContainer <target container name> -DestContext <variable containing context of target storage account>
```

* Provjera statusa Kopiraj u petlji s
 
```powershell
Get-AzureStorageBlobCopyState -Blob <target blob name> -Container <target container name> -Context <variable containing context of target storage account>
```

* Prilaganje novi VHD virtualnog računala prethodno opisan.

Primjeri potražite u [članku][storage-powershell-guide-full-copy-vhd]

##### <a name="cli"></a>EŽA
* Pokretanje kopiju

```
  azure storage blob copy start --source-blob <source blob name> --source-container <source container name> --account-name <source storage account name> --account-key <source storage account key> --dest-container <target container name> --dest-blob <target blob name> --dest-account-name <target storage account name> --dest-account-key <target storage account name>
```

* Provjera stanja ako kopiju u petlji s

```
azure storage blob copy show --blob <target blob name> --container <target container name> --account-name <target storage account name> --account-key <target storage account name>
```
  
* Prilaganje novi VHD virtualnog računala prethodno opisan.

Primjeri potražite u [članku][storage-azure-cli-copy-blobs]

### <a name="disk-handling"></a>Rukovanje na disku
#### <a name="4efec401-91e0-40c0-8e64-f2dceadff646"></a>Struktura VM/VHD za SAP implementacije
Najbolje rukovanje strukture na VM i pridruženi VHDs mora biti vrlo jednostavne. U lokalne instalacije kupci razvio mnoštvo načina strukturiranja instalaciju poslužitelja. 

* Jedan osnovni VHD koji sadrži os-a i sve binarne datoteke DBMS i/ili SAP-a. Od 2015 ožujak, ova VHD može biti do 1TB veličina umjesto starijim ograničenja koja ograničen na 127 GB. 
* Jedan ili više VHDs koja sadrži datoteke zapisnika DBMS SAP baze podataka i datoteka zapisnika DBMS temp memoriju (Ako je na DBMS to podržava). Ako su IOPS preduvjeti za baze podataka zapisnika visoka, morate stripe više VHDs da bi se dosegne glasnoće IOPS obavezno. 
* Broj VHDs koja sadrži jednu ili dvije datoteke baze podataka baze podataka za SAP i DBMS temp podatkovne datoteke i (Ako je na DBMS to podržava).

![Konfiguriranje referenca Azure IaaS VM za SAP][planning-guide-figure-1300]

[komentar]: <>  (Popis obveza MShermannd opisuju Linux strukture)

___

> ![Windows][Logo_Windows] Windows
>
> S više od jednog klijenta smo vidjeli konfiguracije gdje, na primjer, SAP i DBMS binarne datoteke nisu instalirane na pogon c:\ na kojem je instaliran OS-a. Došlo je do razloge za to, ali kada smo nije natrag u korijen, obično je da su pogone small i OS nadogradnje potreban dodatan prostor prije godine 10 do 15. Oba uvjeta primijeniti previše često više ovih dana. Danas pogon c:\ mogu mapirati na diskova veliku ili VMs. Da biste zadržali implementacijama jednostavne u strukturu, preporučuje se da biste pratili sljedeći uzorak implementaciju sustava SAP NetWeaver servisu Azure
>
> Stranične operacijski sustav Windows mora biti na disku D: (koji nisu stalni disk) 
> 
> ![Linux][Logo_Linux] Linux
>
> Postavite swapfile Linux u odjeljku /mnt/mnt/resursa na Linux kao što je opisano u [ovom članku][virtual-machines-linux-agent-user-guide]. Zamjena datoteke moguće je konfigurirati u konfiguracijskoj datoteci /etc/waagent.conf Linux Agent. Dodajte ili promijenite sljedeće postavke:

```
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=30720
```

Da biste aktivirali promjene, morate ponovno pokrenuti Linux Agent s

```
sudo service waagent restart
```

Pročitajte SAP bilješke [1597355] više pojedinosti o preporučene zamjena veličina datoteke

___

Broj VHDs koji se koriste za DBMS podatkovne datoteke i vrstu Azure prostora za pohranu te VHDs nalaze se na mora biti određen preduvjeti IOPS i Latencija obavezno. U [ovom članku] opisane su točno kvote[virtual-machines-sizes]

Sučelje za SAP implementacijama tijekom posljednjeg 2 godina pokazala si nam neke za početnike koji se zbrajati kao:

* Promet IOPS u različitim podatkovne datoteke nije uvijek isti jer postojeće korisnike sustava možda imaju drugačije veličine podatkovne datoteke koji predstavlja njihove SAP baze podataka. Rezultat je izgleda da biste bolje koristiti RAID konfiguracije preko više VHDs da biste postavili podatkovne datoteke LUNs carved izvan onih. Postoje situacije, osobito s Azure standardne pohranom gdje stopu IOPS pritisnete kvote jedan VHD protiv zapisnik transakcija DBMS. U takvim slučajevima preporučuje se korištenje Premium prostora za pohranu ili umjesto toga Zbrajanje više standardnih prostora za pohranu VHDs sa softverom RAID.

___

> ![Windows][Logo_Windows] Windows
>
> * [Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure][virtual-machines-sql-server-performance-best-practices]
> 
> ![Linux][Logo_Linux] Linux
>
> * [Konfiguriranje softver RAID na Linux][virtual-machines-linux-configure-raid]
> * [Konfiguriranje LVM na Linux VM servisu Azure][virtual-machines-linux-configure-lvm]
> * [Azure tajne prostora za pohranu i optimizacije/Linux i](http://blogs.msdn.com/b/igorpag/archive/2014/10/23/azure-storage-secrets-and-linux-i-o-optimizations.aspx)

___

* Prostor za pohranu Premium prikazuju značajan bolje performanse, posebice za zapisivanje zapisnika ključnih transakcije. Za scenarije SAP-a koji se očekuje izlaganje radni kao što su performanse, preporučljivo je da biste koristili VM niz koji možete koristiti za pohranu Premium Azure.

Imajte na umu da VHD koja sadrži OS, a preporučujemo, binarne datoteke programa SAP i baza podataka (baza VM) kao i nije više ograničeni na 127GB. Sada može imati do 1TB veličine. Trebali biste dovoljno prostora potrebno datoteka uključujući npr SAP grupe za posao zapisnika.

Za dodatne prijedloge i više detalja, namijenjenu DBMS VMs, obratite se u [DBMS vodič za implementaciju] [dbms vodič]


#### <a name="disk-handling"></a>Rukovanje na disku
U većini slučajeva potrebnih za stvaranje dodatnih diskova da bi se implementacija SAP bazi podataka u na VM. Ne možemo bila riječ o na pitanja vezana uz broj VHDs u poglavlja [VM/VHD strukturu za SAP implementacijama] [planiranja – vodič za-5.5.1] ovog dokumenta. Portal za Azure omogućuje priložite te odvajanje diskova kada je implementiran osnovni VM. Na diskova može biti priložene/odvojene kada je na VM prema gore i pokrenuta kao i kada je zaustavljena. Prilikom pridruživanja na disku, portala za Azure nudi priložiti prazan disk ili postojeći disk koji tom trenutku u trenutku nije priključen drugi VM. 

**Napomena**: VHDs mogu samo priložiti jedan VM u bilo kojem trenutku.
 
![Prilaganje / odvajanje diskova s Azure standardne prostora za pohranu][planning-guide-figure-1400]

Morate odlučiti želite li stvoriti novi i prazne VHD (koji će se stvoriti u isti prostor za pohranu računa kao osnovni VM se) ili želite da biste odabrali na postojeće VHD koji je prethodno prenijeli i mora priložiti u VM sada. 

**Važno**: **ne** želite koristiti predmemoriranje glavno računalo s Azure standardne prostora za pohranu. Ostavite preferenca predmemorije glavnog računala na zadani od NIŠTA. S Azure Premium pohranom trebate omogućiti čitanje predmemoriranje ako karakteristikama/i uglavnom čitati kao što su uobičajeni/i promet na temelju podatkovne datoteke baze podataka. U slučaju datoteke zapisnika transakcije baze podataka bez predmemoriranja preporučuje se.

___

> ![Windows][Logo_Windows] Windows
>
> [Kako priložiti podatkovni disk na portalu za Azure][virtual-machines-windows-attach-disk-portal]
>
> Ako su priložene diskova, morate prijaviti u VM da biste otvorili Upravitelj diska u sustavu Windows. Ako automount nije omogućen kao što je preporučeno u poglavlja [postavka automount za priložene diskova] [planiranja – vodič za-5.5.3], upravo priložene glasnoću treba poduzeti online i inicijalizirana.
>
> ![Linux][Logo_Linux] Linux
>
> Ako su priložene diskova, morate prijaviti na VM i pokretanje u diskova kao što je opisano u [ovom članku][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]

___

Ako novi disk je prazan disk, morate oblikovati na disku. Za oblikovanje, posebice za DBMS podataka i datoteka zapisnika isti preporuke kao Gola metal implementacijama sustava na DBMS odnose.

Spomenuti već u poglavlja [u programu Microsoft Azure virtualnog računala pojam] [planiranja – vodič za-3,2], račun za Azure pohranu daje beskonačno resurse pomoću glasnoću/i glasnoću IOPS i podatke. Obično DBMS VMs najčešće utječe to. Možda je najbolje koristiti zaseban račun za pohranu za svaku VM ako imate nekoliko/i velik VMs za implementaciju da bi se Ostanite unutar ograničenja glasnoće Azure prostora za pohranu računa. U suprotnom, morate da biste vidjeli kako uskladiti te VMs između različitih računa za pohranu bez odlazak ograničenje od svakog jednog računa za pohranu. Više pojedinosti se spominju u [DBMS upute za uvođenje] [dbms vodič]. Također, ta ograničenja treba Imajte na umu čisto aplikacije SAP server VMs ili druge VMs koja možda ipak zahtijeva dodatne VHDs.

Druge teme koje su relevantne za račune za pohranu je VHDs na računu za pohranu dobivate zemlj replicirati. Zemlj replikacije je omogućiti ili onemogućiti na razini prostora za pohranu računa, a ne na razini VM. Ako je omogućen zemlj replikacije, VHDs subjekta prostora za pohranu će je replicirati u Azure podatkovnog centra u drugu unutar iste područja. Prije nego što odlučite na to, trebali biste razmisliti o sljedeća ograničenja:

Azure zemlj replikacije radi lokalno na svakom VHD u na VM i replicirati IOs kronološkim redoslijedom preko više VHDs u na VM. Stoga VHD koja predstavlja osnovni VM, kao i sve dodatne VHDs priložiti u VM su replicirati međusobno nezavisni. To znači da se ne sinkroniziraju promjene u različitim VHDs. Fact da se na IOs replicirati neovisno o redoslijedom u kojem su se sastavljene znači da zemlj replikacija nije vrijednosti za bazu podataka poslužitelja koji imaju svoje baze podataka tijekom više VHDs. Osim DBMS, također mogu postojati druge programe koji se procesi pisanje ili obradu podataka u različitim VHDs i gdje je važno stalno redoslijed promjene. Ako je zahtjev, trebali biste nije omogućen zemlj replikacije u Azure. Ovisno o tome je li potrebno ili želite zemlj replikacije skupa VMs, ali ne i za drugi skup, možete već kategorizacije VMs i njihove povezane VHDs u različite račune za pohranu koji imaju zemlj. – replikacije omogućiti ili onemogućiti.

#### <a name="17e0d543-7e8c-4160-a7da-dd7117a1ad9d"></a>Postavljanje automount za priložene diskova

___


> ![Windows][Logo_Windows] Windows
> 
> Za VMs koje su stvorene iz vlastite slike ili diskova je potrebno provjeriti i vjerojatno postaviti parametar automount. Postavljanje taj parametar da bi se VM nakon ponovnog pokretanja ili ponovno uvođenje u Azure automatski ponovno postavljanje pogona priložene/postavljen. 
> Parametar postavljen za slike koji nudi Microsoft u Azure Marketplace.
>
> Da biste postavili na automount, provjerite dokumentaciju u naredbenom retku izvršna diskpart.exe ovdje: 
> 
> * [Mogućnosti naredbenog retka DiskPart](https://technet.microsoft.com/library/cc766465.aspx)
> * [Automount](http://technet.microsoft.com/library/cc753703.aspx)
> 
> Prozor naredbenog retka za Windows treba otvoriti kao administrator.
> 
> Ako su priložene diskova, morate prijaviti u VM da biste otvorili Upravitelj diska u sustavu Windows. Ako automount nije omogućen kao što je preporučeno u poglavlja [postavka automount za priložene diskova] [planiranja – vodič za-5.5.3], upravo priložene glasnoću > treba poduzeti online i inicijalizirana.
>
> ![Linux][Logo_Linux] Linux
>
> Morate pokrenuti upravo priložene prazan disk, kao što je opisano u [ovom članku][virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux].
> Morate dodati novih diskova na /etc/fstab.

___


### <a name="final-deployment"></a>Konačni implementacije
Konačni implementacije i točne korake, osobito koja se odnosi na implementaciju sustava SAP prošireni nadzor, potražite u [vodič za implementaciju] [– vodič za implementaciju].

## <a name="accessing-sap-systems-running-within-azure-vms"></a>Pristup sustavima SAP radi unutar Azure VMs
Za scenarije samo oblak, možda ćete povezati te sustavima SAP s Interneta javno pomoću GUI SAP-a. Tim se slučajevima sljedećih postupaka morate zatvoriti.

Dalje u dokumentu ne možemo obrađuje druge glavne scenarij, povezivanje sustavima SAP u unakrsno lokalnim implementacijama koji imaju na web-mjesto veze (VPN-a tunelom) ili Azure ExpressRoute između lokalnog sustava i Azure sustavi.


### <a name="remote-access-to-sap-systems"></a>Daljinski pristup sustavima SAP

Pomoću upravitelja za Azure resursa koje nisu zadane krajnje točke više kao što je bivši klasični modela. Svi priključci programa Azure ARM VM otvoreno dok god:

1. Nema mreže sigurnosne grupe definiran za podmreži ili mrežnog sučelja. Mrežni promet za Azure VMs mogu zaštiti putem so-called "mrežni sigurnosne grupe". Dodatne informacije potražite u članku [što je mreža grupe sigurnosti (NSG)?][virtual-networks-nsg]
1. Nema Azure opterećenja definiran za mrežno sučelje   
 
Kao što je opisano u [ovom članku]u odjeljku arhitektura razlikuju klasični modela i ARM[virtual-machines-azure-resource-manager-architecture].
 
#### <a name="configuration-of-the-sap-system-and-sap-gui-connectivity-for-cloud-only-scenario"></a>Konfiguracija sustava SAP i SAP GUI povezivanje samo oblak scenariju
Pogledajte članak koji opisuje pojedinosti u ovoj se temi: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/06/24/sap-gui-connection-closed-when-connecting-to-sap-system-in-azure.aspx> 

#### <a name="changing-firewall-settings-within-vm"></a>Promjena postavki vatrozida unutar VM
Možda je potrebno za konfiguriranje vatrozida na virtualnim strojevima da dopušta unutarnje promet sa sustavom SAP-a. 

___

> ![Windows][Logo_Windows] Windows
>
> Prema zadanim postavkama Vatrozid za Windows unutar Azure distribuiranih VM uključena. Sada morate dopustiti priključak SAP da se otvori, u suprotnom SAP GUI nećete moći povezati.
> Da biste to učinili:
>
>  * Otvaranje upravljačke Panel\System i Security\Windows Vatrozid za 'Napredne postavke".
>  * Sada desnom tipkom miša kliknite ulazna pravila, a zatim učinite novo pravilo.
>  * U sljedeće čarobnjaka odaberite da biste stvorili novo pravilo "Priključak".
>  * U sljedećem koraku čarobnjaka, ostavite postavku TCP i upišite broj priključka koji želite otvoriti. Budući da naš ID instance SAP je 00, ne možemo trajalo 3200. Ako je vaš instance broj drugu instancu, priključak ste definirali ranije ovisno o broju instancu treba otvoriti.
>  * U sljedeće dio čarobnjaka, morate ostaviti stavku "Dopusti veze" potvrđen.
>  * U sljedećem koraku čarobnjaka potrebno definirati primjenjuje li se pravilo za domene, privatni i javni mrežu. Prilagodite ga po potrebi vašim potrebama. Međutim, povezujete s SAP GUI vanjski putem javne mreže, morate koristiti pravilo primjenjuje se na javnim mrežama.
>  * U posljednjem koraku čarobnjaka, morate naziv pravila, a zatim spremite pravilo tako da pritisnete 'Završi
>
>  Pravilo stupa na snagu odmah.
>
> ![Definicija pravila za priključak][planning-guide-figure-1600]
>
> ![Linux][Logo_Linux] Linux
>
> Slike Linux u trgovine Windows Azure omogućite vatrozid iptables po zadanom i surađivati veze sa sustavom SAP-a. Ako ste omogućili iptables ili neki drugi vatrozid, potražite u dokumentaciji iptables ili korištenih vatrozid da biste dopustili ulaznog tcp promet 32xx priključka (gdje je xx broj sustava sustava SAP-a). 

___

#### <a name="security-recommendations"></a>Preporuke o sigurnosti

SAP GUI odmah povezivanje svih instanci SAP-a (priključak 32xx) koji se izvode, no najprije povezuje putem priključka otvoriti SAP poruke poslužiteljski proces (priključak 36xx). U prošlosti vrlo isti priključak je koristio poruka poslužitelj za internu komunikaciju u instance aplikacije. Da biste spriječili lokalnog poslužitelja aplikacije iz slučajno komunikacije s poslužiteljem poruku u Azure priključke za internu komunikaciju mogu se promijeniti. Preporučujemo da biste promijenili internu komunikaciju između SAP server poruka i njezin instanci aplikacije u drugom broju priključka na sustavima koji imaju omogućeno klonirana iz lokalnog sustava, kao što su Kloniraj razvoja za projekt testiranja itd. To se može učiniti s parametrom zadanog profila:

>   rdisp/msserv_internal

kao što je navedenih u: <https://help.sap.com/saphelp_nwpi71/helpdata/en/47/c56a6938fb2d65e10000000a42189c/content.htm> 

## <a name="96a77628-a05e-475d-9df3-fb82217e8f14"></a>Pojmovi samo oblak implementacije instanci SAP-a

### <a name="3e9c3690-da67-421a-bc3f-12c520d99a30"></a>Jedan VM s NetWeaver SAP pokazni videozapis/obuka scenarija
 
![Pokrenuti jednu pokazni videozapis SAP VM sustavi s istim nazivima VM, Izolirani u Azure servise u Oblaku][planning-guide-figure-1700]

U ovom scenariju (pogledajte poglavlja [samo za oblak] [planiranja-vodič – 2.1] ovog dokumenta) ne možemo su implementacijom scenarij sustava uobičajeni obuka/pokazni videozapis kojem scenarij dovršeno obuka/pokazni videozapis nalaze jednu VM. Pretpostavimo da implementacijskih obavlja putem VM predložaka slika. Također pretpostavkom da je taj višekratnik od tih pokazni videozapis/obuka koje VMs treba biti implementirano s VMs istog naziva.

Pretpostavlja se da ste stvorili VM slike kao što je opisano u nekim sekcijama poglavlja [Priprema VMs s SAP-a za Azure] [planiranja – vodič za-5,2] u ovom dokumentu.

Niz događaja možete implementirati scenarij izgleda ovako:

[komentar]: <>  (MShermannd obveze morate unijeti ARM uzorka opis pomoću predloška json + upit o jedinstveni naziv VM unutar OKVIRA virtualne mreže)   
##### <a name="powershell"></a>PowerShell

* Stvaranje nove grupe nasljeđivanje vrste za svaku vodoravno obuka/pokazni videozapis

```powershell
$rgName = "SAPERPDemo1"
New-AzureRmResourceGroup -Name $rgName -Location "North Europe"
```

* Stvaranje novog računa za pohranu

```powershell
$suffix = Get-Random -Minimum 100000 -Maximum 999999
$account = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name "saperpdemo$suffix" -SkuName Standard_LRS -Kind "Storage" -Location "North Europe"
```

* Stvorite novi virtualne mreže za svaku vodoravno obuka/pokazni videozapis da biste omogućili korištenje isti naziv glavnog računala i IP adrese. Virtualne mreže zaštićen sigurnosne grupe mreže koja samo omogućuje promet na web-mjesto priključak 3389 da biste omogućili pristup udaljene radne površine i u okvir za priključak 22 SSH. 

```powershell
# Create a new Virtual Network
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGRDP -Protocol * -SourcePortRange * -DestinationPortRange 3389 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 100
$sshRule = New-AzureRmNetworkSecurityRuleConfig -Name SAPERPDemoNSGSSH -Protocol * -SourcePortRange * -DestinationPortRange 22 -Access Allow -Direction Inbound -SourceAddressPrefix * -DestinationAddressPrefix * -Priority 101
$nsg = New-AzureRmNetworkSecurityGroup -Name SAPERPDemoNSG -ResourceGroupName $rgName -Location  "North Europe" -SecurityRules $rdpRule,$sshRule

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name Subnet1 -AddressPrefix  10.0.1.0/24 -NetworkSecurityGroup $nsg
$vnet = New-AzureRmVirtualNetwork -Name SAPERPDemoVNet -ResourceGroupName $rgName -Location "North Europe"  -AddressPrefix 10.0.1.0/24 -Subnet $subnetConfig
```

* Stvaranje nove javna IP adresa koji se mogu koristiti za pristup virtualnog računala s Interneta

```powershell
# Create a public IP address with a DNS name
$pip = New-AzureRmPublicIpAddress -Name SAPERPDemoPIP -ResourceGroupName $rgName -Location "North Europe" -DomainNameLabel $rgName.ToLower() -AllocationMethod Dynamic
```

* Stvorite novo sučelje mreže za virtualnog računala

```powershell 
# Create a new Network Interface
$nic = New-AzureRmNetworkInterface -Name SAPERPDemoNIC -ResourceGroupName $rgName -Location "North Europe" -Subnet $vnet.Subnets[0] -PublicIpAddress $pip 
```

* Stvaranje virtualnog računala. Scenarij samo oblak svaki VM će imati isti naziv. SAP SID instanci SAP NetWeaver u tim VMs će biti isti kao i. U grupi resursa Azure naziv u VM mora biti jedinstvena, ali u različitim grupama Azure resursa možete pokrenuti VMs s istim nazivom. Zadani račun "Administrator" sustava Windows ili korijenski za Linux nisu valjani. Stoga novo korisničko ime administratora potrebno je definirati zajedno s lozinku. Veličina u VM i potrebno definirati.

```powershell
#####
# Create a new virtual machine with an official image from the Azure Marketplace
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

# select image
$vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "SUSE" -Offer "SLES" -Skus "12" -Version "latest"
# $vmconfig = Set-AzureRmVMSourceImage -VM $vmconfig -PublisherName "RedHat" -Offer "RHEL" -Skus "7.2" -Version "latest"
# $vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="os"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"
$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

```powershell
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vmconfig = New-AzureRmVMConfig -VMName SAPERPDemo -VMSize Standard_D11

$vmconfig = Add-AzureRmVMNetworkInterface -VM $vmconfig -Id $nic.Id

$diskName="osfromimage"
$osDiskUri=$account.PrimaryEndpoints.Blob.ToString() + "vhds/" + $diskName  + ".vhd"

$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Windows
$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Windows -ComputerName "SAPERPDemo" -Credential $cred
#$vmconfig = Set-AzureRmVMOSDisk -VM $vmconfig -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri <path to VHD that contains the OS image> -Linux
#$vmconfig = Set-AzureRmVMOperatingSystem -VM $vmconfig -Linux -ComputerName "SAPERPDemo" -Credential $cred

$vm = New-AzureRmVM -ResourceGroupName $rgName -Location "North Europe" -VM $vmconfig
```

* Po želji dodavanje dodatnih diskova i vraćanje potrebne sadržaja. Imajte na umu sva imena blob (URL-ovi s blob-ova) mora biti jedinstvena u Azure.

```powershell
# Optional: Attach additional data disks
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name SAPERPDemo
$dataDiskUri = $account.PrimaryEndpoints.Blob.ToString() + "vhds/datadisk.vhd"
Add-AzureRmVMDataDisk -VM $vm -Name datadisk -VhdUri $dataDiskUri -DiskSizeInGB 1023 -CreateOption empty | Update-AzureRmVM
```

##### <a name="cli"></a>EŽA

Sljedeći primjer kod može se koristiti na Linux. Za Windows, pročitajte ili pomoću komponente PowerShell prethodno opisan ili prilagoditi primjer koristite rgName % umjesto $rgName i postavite varijablu okruženja pomoću sustava Windows naredba _Postavljanje_.

* Stvaranje nove grupe nasljeđivanje vrste za svaku vodoravno obuka/pokazni videozapis

```
rgName=SAPERPDemo1
rgNameLower=saperpdemo1
azure group create $rgName "North Europe"
```

* Stvaranje novog računa za pohranu

```
azure storage account create --resource-group $rgName --location "North Europe" --kind Storage --sku-name LRS $rgNameLower
```

* Stvorite novi virtualne mreže za svaku vodoravno obuka/pokazni videozapis da biste omogućili korištenje isti naziv glavnog računala i IP adrese. Virtualne mreže zaštićen sigurnosne grupe mreže koja samo omogućuje promet na web-mjesto priključak 3389 da biste omogućili pristup udaljene radne površine i u okvir za priključak 22 SSH. 

```
azure network nsg create --resource-group $rgName --location "North Europe" --name SAPERPDemoNSG
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGRDP --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 3389 --access Allow --priority 100 --direction Inbound
azure network nsg rule create --resource-group $rgName --nsg-name SAPERPDemoNSG --name SAPERPDemoNSGSSH --protocol \* --source-address-prefix \* --source-port-range \* --destination-address-prefix \* --destination-port-range 22 --access Allow --priority 101 --direction Inbound

azure network vnet create --resource-group $rgName --name SAPERPDemoVNet --location "North Europe" --address-prefixes 10.0.1.0/24
azure network vnet subnet create --resource-group $rgName --vnet-name SAPERPDemoVNet --name Subnet1 --address-prefix 10.0.1.0/24 --network-security-group-name SAPERPDemoNSG
```

* Stvaranje nove javna IP adresa koji se mogu koristiti za pristup virtualnog računala s Interneta

```
azure network public-ip create --resource-group $rgName --name SAPERPDemoPIP --location "North Europe" --domain-name-label $rgNameLower --allocation-method Dynamic
```

* Stvorite novo sučelje mreže za virtualnog računala

```
azure network nic create --resource-group $rgName --location "North Europe" --name SAPERPDemoNIC --public-ip-name SAPERPDemoPIP --subnet-name Subnet1 --subnet-vnet-name SAPERPDemoVNet 
```

* Stvaranje virtualnog računala. Scenarij samo oblak svaki VM će imati isti naziv. SAP SID instanci SAP NetWeaver u tim VMs će biti isti kao i. U grupi resursa Azure naziv u VM mora biti jedinstvena, ali u različitim grupama Azure resursa možete pokrenuti VMs s istim nazivom. Zadani račun "Administrator" sustava Windows ili korijenski za Linux nisu valjani. Stoga novo korisničko ime administratora potrebno je definirati zajedno s lozinku. Veličina u VM i potrebno definirati.

```
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn SUSE:SLES:12:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
# azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --image-urn RedHat:RHEL:7.2:latest --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd --disable-boot-diagnostics
```

```
#####
# Create a new virtual machine with a VHD that contains the private image that you want to use
#####
azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Windows --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
#azure vm create --resource-group $rgName --location "North Europe" --name SAPERPDemo --nic-name SAPERPDemoNIC --os-type Linux --admin-username <username> --admin-password <password> --vm-size Standard_D11 --os-disk-vhd https://$rgNameLower.blob.core.windows.net/vhds/os.vhd -Q <path to image vhd> --disable-boot-diagnostics
```

* Po želji dodavanje dodatnih diskova i vraćanje potrebne sadržaja. Imajte na umu sva imena blob (URL-ovi s blob-ova) mora biti jedinstvena u Azure.

```
# Optional: Attach additional data disks
azure vm disk attach-new --resource-group $rgName --vm-name SAPERPDemo --size-in-gb 1023 --vhd-name datadisk
```

##### <a name="template"></a>Predložak
Ogledni predlošci možete koristiti na spremište azure, brzi početak rada i predloške na github.

* [Jednostavan Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* [Jednostavan Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
* [VM sa slike](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

### <a name="implement-a-set-of-vms-which-need-to-communicate-within-azure"></a>Implementacija skup VMs koje treba komunikaciju unutar Azure
Scenarij samo oblak je uobičajeni scenarij za obuku i pokazni videozapis potrebe gdje softver koji predstavlja pokazni videozapis/obuka scenarij je šire više VMs. Različite komponente instalirane u različitim VMs moraju međusobno komunicirati. Ponovno u ovom slučaju ne lokalne mreže komunikacije ili je potrebno više lokacija scenarij.

Taj se scenarij datotečni nastavak instalacije opisane u poglavlja [jedan VM s NetWeaver SAP pokazni videozapis/obuka scenarij] [planiranja – vodič – 7.1] ovog dokumenta. U ovom slučaju više virtualnim strojevima dodaje se u postojeću grupu resursa. U sljedećem primjeru vodoravno obuka sastoji se od SAP ASCS/SCS VM, VM pokreće na DBMS ili instancu poslužitelja aplikacija SAP VM.

Prije nego što se izgraditi scenarij ćete morati razmišljati osnovne postavke kao već exercised u scenariju prije.

#### <a name="resource-group-and-virtual-machine-naming"></a>Grupa resursa i imenovanja virtualnog računala
Svi nazivi grupa resursa mora biti jedinstvena. Razvijate vlastite imenovanja sheme resursa, kao što su `<rg-name`>-sufiks. 

Naziv virtualnog računala mora biti jedinstvena u grupu resursa. 

#### <a name="setup-network-for-communication-between-the-different-vms"></a>Postavljanje mreže za komunikaciju između različitih VMs
 
![Skup VMs unutar Azure virtualne mreže][planning-guide-figure-1900]

Da biste spriječili imenovanja sudara s clones iste landscapes obuka/pokazni videozapis, morate stvoriti Azure virtualne mreže za svaku vodoravno. Razrješavanje DNS naziva će nudi Azure ili možete konfigurirati vlastite DNS poslužitelj izvan Azure (ne da biste se dodatno opisan u nastavku). U ovom slučaju ne možemo konfigurirati našim DNS-a. Za sve virtualnim strojevima unutar jedne mreže virtualne Azure komunikaciju putem hostnames će biti omogućene. 

Razlozi za razdvajanje obuka ili pokazni videozapis landscapes putem virtualne mreže, a ne samo grupe resursa nije moguće:

* Vodoravno SAP kako je postavljeno mora vlastitu AD/OpenLDAP i poslužitelja domene mora biti dio svakog na landscapes.  
* Vodoravno SAP kako je postavljeno je komponente koje su potrebne za rad s fiksnim IP adrese.

Dodatne informacije o Azure virtualne mreže i kako ih definirati pronaći ćete u [ovom članku][virtual-networks-create-vnet-arm-pportal].

## <a name="deploying-sap-vms-with-corporate-network-connectivity-cross-premises"></a>Implementacija SAP VMs s povezivanjem s korporacijskom mrežom (-lokacija)

Pokrenite SAP vodoravno i podjele implementacije između Gola-metal za visokokvalitetni DBMS poslužitelje, lokalni virtualiziranom okruženju za web-mjesto slojeve aplikacije i u okvir za manje 2 sloja konfiguriran sustavima SAP i Azure IaaS. Osnovni pretpostavlja se da sustavima SAP unutar jedne SAP vodoravno moraju komunikaciju s međusobno i mnoge druge softver komponente implementiran u tvrtki, neovisno o obrascu implementacije. Također bi se trebala prikazati nema razlike uvedene implementacije obrazac za povezivanje s GUI SAP-a ili druge sučelja krajnjeg korisnika. Sljedeće uvjete može biti zadovoljen kada imamo lokalni Active Directory/OpenLDAP i DNS servise prošireni Azure sustavi putem web-mjesta-na-web-mjesta/višestruka-site povezivanje ili privatne veze, kao što su Azure ExpressRoute.

Da biste dobili dodatne pozadine na Detalji o implementaciji sustava SAP-a na Azure, preporučujemo da biste pročitali poglavlja [koncepata Cloud-Only implementaciju SAP instanci] [planiranja – vodič za-7] ovog dokumenta koja objašnjava neke od konstrukta osnove Azure i kako ih treba koristiti s aplikacijama SAP-a u Azure.

### <a name="scenario-of-a-sap-landscape"></a>Scenarij SAP vodoravno

Scenarij više lokacija možete otprilike opisani kao što su u nastavku grafike:
 
![Povezivanje web-mjesto između lokalnog i Azure resursi][planning-guide-figure-2100]

Scenarij gore navedenoj sintaksi opisuje scenarij u kojem na lokalni AD-OpenLDAP i DNS proširen je Azure. Na strani lokalnog na određene rasponu IP adresa je rezervirana po Azure pretplate. Azure virtualne mreže na strani Azure će se dodijeliti rasponu IP adresa.

#### <a name="security-considerations"></a>Sigurnosna pitanja vezana uz

Minimalni preduvjet je korištenje protokola sigurne komunikacije kao što su SSL/TLS za pristup putem preglednika ili utemeljen na VPN veza za pristup sustavu Azure servisima. Pretpostavlja se da tvrtke rukovati VPN veza između njihove korporacijskom mrežom i Azure vrlo različito. Neke tvrtke blankly može otvoriti sve priključke. Neke tvrtke možda želite da vas se preciznu u koje priključke koje su im potrebne za otvaranje itd. 

U tablici ispod standardne SAP navedene su priključke za komunikaciju. Zapravo je dovoljno da biste otvorili priključak pristupnika SAP-a.

| Servis | Naziv priključka | Primjer `<nn`> = 01 | Zadani raspon (min max) | Komentar |
|---------|-----------|-------------------|-------------------------|---------|
| Otpremnik | sapdp`<nn>` potražite u članku * | 3201 | 3200 – 3299 | SAP otpremnik koriste GUI SAP-a za Windows i Java |
| Poslužitelj za poruke | sapms`<sid`> potražite u članku ** | 3600 | Besplatni sapms`<anySID`> | SID = SAP ID-sustava |
| Pristupnik | sapgw`<nn`> potražite u članku * | 3301 | slobodno | SAP pristupnik koristi za CPIC i RFC komunikacije |
| SAP usmjerivač | sapdp99 | 3299 | slobodno | Samo nazivi servisa CI (središnje instance) možete ponovno u /etc/services proizvoljne vrijednosti nakon instalacije. |

*) nn = broj instanci SAP-a

*) sid = SAP ID-sustava

Detaljnije informacije o priključke potrebne za različite SAP proizvode ili usluge po proizvodima SAP-a nalazi se ovdje <http://scn.sap.com/docs/DOC-17124>. Uz ovaj dokument mora biti da biste otvorili Namjenska priključke nužne za proizvode SAP i scenariji uređaja za VPN-a.

Ostale sigurnosne mjeri pri implementaciji VMs u takvim scenariju može biti da biste stvorili [Mrežni sigurnosne grupe] [ virtual-networks-nsg] da biste definirali pravila za pristup.

### <a name="dealing-with-different-virtual-machine-series"></a>Postupanje s nizom različitih virtualnog računala

Tijeku zadnjih 12 mjeseci dodao Microsoft mnogo više vrsta VM koje se razlikuju u broj vCPUs, memorije ili važnijih na hardveru se izvodi. Podržane su sve te VMs s SAP-a (potražite u članku podržane vrste VM u bilješku SAP [1928533]). Neke od tih VMs se izvoditi na drugo glavno hardver generations. U preciznosti od programa Azure skaliranje jedinicom su početak implementirana te generations hardver glavnog računala. Srednje slučajevima može pojaviti gdje se različitim veličinama VM koje ste odabrali ne može pokrenuti na istoj jedinici mjerilo. Dostupnost postavljanje je ograničen mogućnost obuhvaćaju skaliranje jedinice temelji različite hardvera.  Npr. Ako želite pokrenuti na DBMS A5 A11 VMs i aplikacijskom sloju SAP-a na VMs G niza koji želite prisilno za implementaciju jedan sustav SAP-a ili drugu sustavima SAP unutar različitih skupova dostupnost.


#### <a name="printing-on-a-local-network-printer-from-sap-instance-in-azure"></a>Ispis na pisaču u lokalnoj mreži iz SAP instance servisu Azure
##### <a name="printing-over-tcpip-in-cross-premises-scenario"></a>Ispis putem TCP/IP u scenariju više lokacija


Postavljanje lokalnog TCP/IP temelji mrežnih pisača u programa Azure VM je ukupni jednaki onima s korporacijskom mrežom, uz pretpostavku da imate tunelom VPN-a web-mjesta-na-web-mjesta ili ExpressRoute veza. 

___

> ![Windows][Logo_Windows] Windows
>
> Da biste to učinili:
> - Neke mrežne pisače isporučuje se s Čarobnjak za konfiguraciju koji olakšava postavili pisač u VM programa Azure. Ako je softver nije Čarobnjak za distribuirane s pisačem u "ručni" da biste postavili pisač tako da biste stvorili novi TCP/IP priključka za pisač.
> - Otvorite upravljačku ploču -> uređaji i pisači -> Dodaj pisač 
> - Odaberite Dodaj pisač TCP/IP adrese ili naziv glavnog računala
> - Upišite IP adresu pisača
> - Standardni 9100 priključka za pisač
> - Ako je potrebno ručno instalirati upravljački program za odgovarajući pisač. 
> 
> ![Linux][Logo_Linux] Linux
>
> - kao što su za sustav Windows samo daljnji standardni postupci za instalaciju mrežnog pisača
> - Samo slijedite javno vodilice Linux [SUSE](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_y2_hw_print.html) ili [Crveno razgovor](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Printer_Configuration.html) o tome kako dodati pisač.

___

 
![Mrežni ispis][planning-guide-figure-2200]



##### <a name="host-based-printer-over-smb-shared-printer-in-cross-premises-scenario"></a>Na pisač putem SMB (zajednički pisač) u scenariju više lokacija

Utemeljen na glavno računalo pisača nisu mreže kompatibilan s dizajnom. No pisač utemeljen na glavno računalo mogu zajednički koristiti računala na mreži dok god je povezan s računalom pokreće na. Povežite svoje tvrtke mreže web-mjesto ili ExpressRoute i zajedničko korištenje lokalni pisač. Protokol SMB koristi NetBIOS umjesto DNS servisa. Naziv glavnog računala NetBIOS može se razlikovati od DNS naziv glavnog računala. Standardni slučaj još NetBIOS naziv glavnog računala i naziv glavnog računala DNS jednaki. DNS domena provjerite uvid u prostoru za NetBIOS naziv. Sukladno tome, potpuno kvalificiran naziv glavnog računala DNS-a koji se sastoji od DNS naziv glavnog računala i DNS-a domene ne smije koristiti u prostoru za NetBIOS naziv.

Zajedničko korištenje pisača označena jedinstveni naziv u mreži:

* Naziv glavnog računala voditelju SMB (uvijek potrebno). 
* Naziv koji se zajednički koristi (uvijek potrebno). 
* Naziv domene Ako pisač za zajedničko korištenje nije u istom domenu sustava SAP-a. 
* Uz to, korisničko ime i lozinka možda morati pristup zajedničko korištenje pisača.

kako:

___

> ![Windows][Logo_Windows] Windows
>
> Zajedničko korištenje lokalni pisač.
> Azure VM otvorite Windows Explorer i upišite naziv za zajedničko korištenje pisača.
> Čarobnjak za instalaciju pisača će vas voditi kroz postupak instalacije.
>
> ![Linux][Logo_Linux] Linux
>
> Evo nekoliko primjera dokumentaciju o konfiguraciji mrežnih pisača u Linux ili uključujući poglavlje vezano uz ispis u Linux. Funkcionirat će na isti način kao u programa Azure Linux VM pod uvjetom da se VM je dio VPN:
>
> * SLES <_Share_or_Windows_Share https://en.opensuse.org/SDB:Printing_via_SMB_ (Samba)>
> * RHEL <https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s1-printing-smb-printer.html>

___


##### <a name="usb-printer-printer-forwarding"></a>USB pisač (pisač prosljeđivanje) 

U Azure mogućnost udaljene radne površine i korisnicima omogućuju pristup svoje uređaje lokalni pisač u udaljenu sesiju nije dostupna.

___

> ![Windows][Logo_Windows] Windows
>
> Dodatne informacije o ispisu sa sustavom Windows nalazi se ovdje: <http://technet.microsoft.com/library/jj590748.aspx>.

___

 
#### <a name="integration-of-sap-azure-systems-into-correction-and-transport-system-tms-in-cross-premises"></a>Integracija programa SAP Azure sustavi u ispravke i prijenosa sustava (TMS) u više lokacija

Promjena SAP i sustava prijenosa (TMS) potrebno je konfigurirati za izvoz i uvoz zahtjev za prijenos preko sustavi u u vodoravno. Pretpostavimo da instance razvoj sustava SAP-a (Razvojni) nalaze se u Azure dok kvalitete (značajke pitanja i odgovora) i produktivni sustavi (PRD) su lokalni. Osim toga, pretpostavimo da postoji direktorij središnje prijenosa.

##### <a name="configuring-the-transport-domain"></a>Konfiguriranje domena prijenosa
Konfiguriranje prijenosa domene u sustavu označen kao kontrolerom domene prijenosa kao što je opisano u [konfiguriranju kontrolerom domene prijenosa](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm). Sistemski korisnik TMSADM stvorit će se i potrebne RFC odredište bit će načinjena. Možda Provjerite ove veze RFC pomoću transakcije SM59. Naziv glavnog računala razlučivost mora biti omogućen preko prijenosa domene. 

kako:

* U našem scenariju smo odlučili lokalnim sustavom za QAS bit će kontrolerom domene CTS. Nazovite transakcije STMS. Pojavit će se dijaloški okvir TMS. Prikazat će se dijaloški okvir za konfiguriranje domene prijenosa. (Ovo dijaloški okvir pojavljuje samo ako još niste konfigurirali prijenosa domeni.)
* Provjerite da korisnik automatski stvoreni TMSADM ovlašten (SM59 -> ABAP veza -> TMSADM@E61.DOMAIN_E61 -> pojedinosti -> Utilities(M) -> autorizacija Test). Početni zaslon transakcije STMS trebao pokazati da sustav SAP sada funkcionira kao kontrolerom domene prijenosa kao što je prikazano ovdje:
 
![Početni zaslon transakcija STMS kontrolerom domene][planning-guide-figure-2300]

#### <a name="including-sap-systems-in-the-transport-domain"></a>Uključujući sustavima SAP u domeni prijenosa

Slijed uključujući upit sustava SAP-a domene prijenosa izgleda na sljedeći način:

* U sustavu Razvojni u Azure idite na sustav prijenosa (klijent 000) i nazovite transakcije STMS. Odaberite drugi konfiguracije iz dijaloškog okvira i nastavite s obuhvaćaju sustava u domeni. Odredite kontrolerom domene kao ciljno glavno računalo ([Uključujući sustavima SAP prijenosa domenu](http://help.sap.com/erp2005_ehp_04/helpdata/en/44/b4a0c17acc11d1899e0000e829fbbd/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm)). Sustav sada čeka se da bi bio uvršten u domeni prijenosa.
* Sigurnosnih vam razloga, zatim imate da biste se vratili kontrolerom domene da biste potvrdili vaš zahtjev. Odaberite sistemski pregled i odobravanje sustava na čekanju. Potvrda upit i konfiguraciji distributed.

Taj sustav SAP-a sada sadrži potrebne informacije o svim drugim sustavima SAP u domeni prijenosa. U isto vrijeme podatke o adresi novog sustava SAP šalju se u svim drugim sustavima SAP i sustava SAP nije unesena u profilu prijenosa programa kontrole prijenosa. Provjerite je li RFCs i pristup direktoriju prijenosa domene funkcioniraju.

Nastavite s konfiguracijom prijenosa sustava kao i obično kao što je opisano u dokumentaciji o [promjenama i prijenosa sustava](http://help.sap.com/saphelp_nw70ehp3/helpdata/en/48/c4300fca5d581ce10000000a42189c/content.htm?frameset=/en/44/b4a0b47acc11d1899e0000e829fbbd/frameset.htm).

kako:

* Provjerite je li ispravno konfigurirano vaše STMS lokalno.
* Provjerite je li naziv glavnog računala s kontrolerom domene prijenosa može riješiti tako da virtualnog računala na Azure i Potpredsjednik visa.
* Poziv transakcije STMS -> drugih konfiguracije -> Uključi sustava u domeni.
* Provjerite vezu u sustavu na lokalni TMS.
* Konfiguriranje usmjerava prijenosa, grupe i slojeve kao i obično.

U web-mjesto povezanog više lokacija scenarija latencije između lokalnog poslužitelja i Azure i dalje mogu biti znatno. Ako ne možemo slijedite slijed prijenos objekte kroz razvoj i testiranje sustavi radnog ili razmislite o primjeni prijenosa ili podržava paketa u druge sustave, shvatite da, ovisi o mjestu središnje prijenosa direktorija, neke od sustava naići ćete visokom latencijom čitanje ili pisanje podataka u imeniku središnje prijenosa. Situaciji slično je SAP vodoravno konfiguracije gdje su različite sustave podjele kroz centre za različite podatke s znatno udaljenost između centre za podatke.

Da biste zaobišli takvo Latencija, a imate sustavi se brzo u čitanje ili pisanje ili iz imenika prijenosa možete postaviti dva STMS prijenos domene (jedan za lokalni i sa sustavima u Azure i povezivanje prijenosa domene. Provjerite ove dokumentacije koja objašnjava načela iza tog koncepta u SAP TMS: <http://help.sap.com/saphelp_me60/helpdata/en/c4/6045377b52253de10000009b38f889/content.htm?frameset=/en/57/38dd924eb711d182bf0000e829fbfe/frameset.htm>. 

kako:

* Postavljanje domene prijenosa na svakom mjestu (lokalni i Azure) pomoću transakcije STMS <http://help.sap.com/saphelp_nw70ehp3/helpdata/en/44/b4a0b47acc11d1899e0000e829fbbd/content.htm>
* Veza na domene s vezom za domenu i potvrdite vezu između dvije domene. 
  <http://help.SAP.com/saphelp_nw73ehp1/helpdata/en/a3/139838280c4f18e10000009b38f8cf/Content.htm>
* Distribucija konfiguraciju povezane sustav.

#### <a name="rfc-traffic-between-sap-instances-located-in-azure-and-on-premises-cross-premises"></a>Promet RFC između SAP instance koja se nalazi u Azure i lokalnih (-lokacija)

Promet RFC između sustava koji su lokalni i Azure mora raditi. Postavljanje veze poziva transakcije SM59 u izvorišnom sustavu tamo gdje vam je potreban da biste definirali vezu s RFC pri ciljni sustav. Konfiguracija je slična standardnog postavu RFC veze.

Ne možemo pretpostavimo da u scenariju više lokacija VMs koje su izvođenja sustavima SAP koje su potrebne za komunikaciju na istom domenu. Stoga postavljanje RFC veze između sustava SAP-a ne razlikuju se od korake za postavljanje i unose u lokalni scenarijima.

#### <a name="accessing-local-fileshares-from-sap-instances-located-in-azure-or-vice-versa"></a>Pristupanju 'lokalni fileshares iz SAP instance koja se nalazi u Azure i obrnuto

SAP instance koja se nalazi u Azure moraju imati pristup zajedničke datoteke koje su unutar tvrtke lokalno. Uz to, lokalnog SAP instance moraju imati pristup zajedničkim datotekama koje se nalaze u Azure prikazati. Da biste omogućili zajedničko korištenje datoteka dozvole i mogućnosti zajedničkog korištenja na lokalnom sustavu morate konfigurirati. Provjerite jeste li otvorili priključke za VPN-a ili ExpressRoute vezu između Azure i vaše podatkovnog centra.

## <a name="supportability"></a>Pružanje podrške
### <a name="6f0a47f3-a289-4090-a053-2521618a28c3"></a>Azure nadzor rješenja za SAP
Da biste omogućili za nadzor sustava zaštita njihove privatnosti ovise ključnih SAP-a na Azure SAP Alati SAPOSCOL ili SAP Pribavljanje podataka izvan voditelju servisa Azure virtualnog računala putem nastavkom nadzor Azure za SAP Agent za glavno računalo za nadzor. Budući da se na zahtjeve po SAP su određene SAP aplikacije, Microsoft odlučili ne generically implementirati potrebna integrira u Azure, ali ostavili ga za klijente za implementaciju potrebne komponente za nadzor i konfiguracija njihove virtualnim strojevima sa servisu Azure. Međutim, implementacije i životni ciklus upravljanje komponenti za nadzor će se uglavnom automatizirati tako da Azure.

#### <a name="solution-design"></a>Rješenja dizajna

Rješenje razvio da biste omogućili nadzor SAP temelji se na arhitektura Azure VM Agent i nastavak framework. Ideju framework Azure VM Agent i nastavak je omogućiti instalaciju softvera aplikacije dostupni u galeriji Azure VM proširenje unutar na VM. Načelo iza tog koncepta je da bi se (u slučajevima kao što su Azure nadzor proširenje za SAP-a) implementaciju posebnih integrira u na VM i konfiguraciji uključio trenutku implementacije. 

Nakon veljača 2014., "Azure VM agenta' koji omogućuje rukovanje određene proširenja VM Azure unutar na VM je umetnutog u Windows VMs po zadanom na stvaranje VM na portalu za Azure. U slučaju SUSE ili Linux je vaša crvene boje na VM agent je već dio slike trgovine Windows Azure. U slučaju da nešto želite prenijeti Linux VM lokalnim Azure VM agent je ručno instalirati.


Osnovni sastavni blokovi rješenje praćenja u Azure za SAP izgleda ovako:
 
![Komponente Microsoftova proširenja za Azure][planning-guide-figure-2400]

Kao što je prikazano u blok dijagrama gore, jedan dio nadzora rješenja za SAP-a nalazi se u sliku VM Azure i galeriji proširenje Azure koji se globalno repliciranu spremniku, čime se upravlja Azure operacije. To je odgovornosti joint SAP-MS tim za rad na Azure implementaciji sustava SAP-a za rad s Azure operacije da biste objavili nove verzije Azure nadzor proširenje za SAP-a. Ovaj Azure nadzor kućni broj za SAP koristit će nastavak Microsoft Azure Dijagnostika (WAD) ili Linux Azure Dijagnostika (LAD) da biste dobili potrebne informacije. 

Ako pokrenete novi VM Windows, u Azure Agent-VM automatski se dodaju u na VM. Funkcija Ovaj agent je koordiniranje učitavanje i konfiguracija Azure proširenja za nadzor NetWeaver sustava SAP-a. Linux VMs Azure VM Agent je već dio slike Azure Marketplace OS.

No postoji koraka koje je još potrebno izvršiti koje je korisnik odabrao. Ovo je omogućenja i konfiguraciji zbirke performansi. Postupak vezane uz "konfiguracija" je automatizirano skriptu PowerShell ili EŽA naredbe. U značajci Microsoft Azure Script Center kao što je opisano u [vodič za implementaciju] [– vodič za implementaciju] se može preuzeti skriptu PowerShell.

Cjelokupan arhitekturu rješenja Azure nadzora za SAP izgleda ovako:
 
![Azure nadzor rješenja za NetWeaver SAP-a][planning-guide-figure-2500]

**Exact upute i detaljne upute korištenja tih cmdleta ljuske PowerShell ili naredbe EŽA tijekom implementacije slijedite upute navedene u [upute za uvođenje] [– vodič za implementaciju].**

### <a name="integration-of-azure-located-sap-instance-into-saprouter"></a>Integracija programa Azure nalazi instancu SAP-a u SAProuter

SAP instance izvodi u Azure moraju biti dostupni iz SAProuter kao i.
 
![SAP usmjerivač mrežne veze][planning-guide-figure-2600]

Na SAProuter omogućuje TCP/IP komunikacije između sustava koji sudjeluju ako ne postoji Izravni IP veza. To daje prednost završetka do kraja veze između partnere komunikacije je potrebno na razini mreže. Na SAProuter priključuje na priključak 3299 prema zadanim postavkama.
Povezivanje SAP instance putem programa SAProuter morate dati SAProuter niz glavno računalo naziv i s bilo kojeg prilikom pokušaja povezivanja.

## <a name="sap-netweaver-as-java"></a>SAP kao NetWeaver Java

Dosad fokus dokument je SAP NetWeaver Općenito ili posložiti NetWeaver ABAP SAP-a. U ovom odjeljku small nalaze se određene zahtjevi za SAP Java stogu. Jedna od najvažnijih aplikacija SAP NetWeaver Java isključivo temelji je Portal Enterprise SAP-a. Drugi NetWeaver SAP temelji aplikacije kao što su SAP PI i SAP rješenje Upravitelj koriste SAP NetWeaver ABAP i Java snop. Stoga certainly je potrebno uzeti u obzir određene aspekte vezane uz stog NetWeaver Java SAP-a.

### <a name="sap-enterprise-portal"></a>Portal za Enterprise SAP-a

Postavljanje portala za SAP-a u programa Azure virtualnog računala razlikuju se od o instalacija lokalni Ako implementirate u više lokacija scenarijima. Budući da se DNS-om obavlja tvrtka lokalnog, priključak postavki za pojedinačne instance mogu se izvršiti kao konfiguriranog lokalnog. Preporuke i ograničenja opisana u ovom dokumentu dosad Općenito primijeniti programa kao što su SAP Portal za Enterprise ili stog NetWeaver Java SAP-a. 

![Portal prikazanog SAP][planning-guide-figure-2700]

Scenarij posebno implementacije tako da neki klijenti je Izravni izlaganje portala za Enterprise SAP-a s Internetom dok virtualnog računala glavno računalo povezano s mrežom tvrtke putem tunelom VPN-a web-mjesto ili ExpressRoute. Za pretraživanje kao scenarija imate da biste bili sigurni da su određene priključke otvorene i nije blokirano po vatrozida ili mreže sigurnosne grupe. Isti mehanika morate zatvoriti kada se želite povezati s instancom SAP Java lokalnim u scenariju samo oblak.

Početna portal URI je http(s):`<Portalserver`>: 5XX00/irj gdje je priključak oblikovana 50000 znakom plus (Systemnumber × 100). Zadani portala URI SAP sustav 00 je `<dns name`>. `<azure region`>.Cloudapp.azure.com:PublicPort/irj. Dodatne pojedinosti imaju izgled <http://help.sap.com/saphelp_nw70ehp1/helpdata/de/a2/f9d7fed2adc340ab462ae159d19509/frameset.htm>. 
 
![Konfiguraciju krajnje točke][planning-guide-figure-2800]

Ako želite da biste prilagodili URL-a i/ili priključci Portal za Enterprise SAP-a, provjerite ove dokumentacije:

* [Promjena URL portala](http://wiki.scn.sap.com/wiki/display/EP/Change+Portal+URL) 
* [Promjena zadanog brojeva priključaka, brojeve priključaka na portalu](http://wiki.scn.sap.com/wiki/display/NWTech/Change+Default++port+numbers%2C+Portal+port+numbers) 


## <a name="high-availability-ha-and-disaster-recovery-dr-for-sap-netweaver-running-on-azure-virtual-machines"></a>Visoke dostupnosti (HA) i Izrada oporavak (DR) za SAP NetWeaver koji se izvodi na virtualnim strojevima sa sustavom Azure
### <a name="definition-of-terminologies"></a>Definicija terminologies

Termina **visoke dostupnosti (HA)** obično vezane uz skup tehnologija koje minimizira to izbjeglo IT unosom poslovanje od usluge putem suvišnih, pogreške ili prebacivanje zaštićena komponente unutar **iste** podatkovnog centra. U našem slučaja u jednom području Azure.

**Izrada oporavak (DR)** i određivanje minimiziranje prekidu IT servisa i njihovih oporavak, ali preko **različitih** vrsta centri koji se obično nalazi stotine kilometrima odmah. U našem slučaju obično između različitih područja Azure unutar iste Geopolitički područja ili kao utvrđene vi kao kupca.

### <a name="overview-of-high-availability"></a>Pregled visoke dostupnosti
Odvojiti raspravu o SAP visoke dostupnosti u Azure u dva dijela:

* **Infrastruktura za azure visoke dostupnosti**, npr HA računalnim (VMs), mreže, za pohranu itd. i njegov pogodnosti za povećanje dostupnosti aplikacija za SAP-a.
* **Visoke dostupnosti aplikacija za SAP**, primjerice SLIKA sustava SAP softverske komponente:
    * Poslužitelji aplikacija za SAP
    * Instance ASCS SCS SAP-a 
    * Poslužitelj baze podataka

i kako ga može se kombinirati s Azure infrastrukture HA.

SAP visoke dostupnosti u Azure sadrži neke razlike u okruženju na lokaciji fizički ili virtualni u usporedbi s SAP visoke dostupnosti. Sljedeće papira iz SAP opisuju standardne SAP visoke dostupnosti konfiguracije u virtualiziranom okruženju u sustavu Windows: <http://scn.sap.com/docs/DOC-44415>. Postoji bez sapinst integriran SAP-SLIKA konfiguracija Linux kao da se ona nalazi za Windows. O SAP SLIKA lokalnog za Linux Saznajte više o tome ovdje: <http://scn.sap.com/docs/DOC-8541>.

### <a name="azure-infrastructure-high-availability"></a>Infrastruktura za Azure visoke dostupnosti
Nema jedne VM SLA dostupna je na Azure virtualnim strojevima odmah. Da biste dobili dojam kako dostupnost jedan VM može izgledati jednostavno možete izraditi proizvoda različite dostupne Azure SLA: <https://azure.microsoft.com/support/legal/sla/>.

Osnovica za izračun je 30 dana mjesečno ili 43200 minuta. Stoga 0,05% nedostupnost odgovara 21.6 minuta. Kao i uvijek će pomnožiti dostupnost u različitim servisima na sljedeći način:

(Servis dostupnosti #1/100) *(Servis dostupnosti #2/100)* (Servis dostupnosti #3/100) *...

Na sljedeći način:

(99.95/100) *(99.9/100)* (99.9/100) = 0.9975 ili cjelokupan raspoloživost 99.75%.

#### <a name="virtual-machine-vm-high-availability"></a>Visoke dostupnosti virtualnog računala (VM)

Postoje dvije vrste platforme Azure događaja koji mogu utjecati na dostupnost virtualnim strojevima: Planirano održavanje i Neplanirana održavanja.

* Događaji planiranog održavanja su periodičku ažuriranjima Microsoft podlozi platforme Azure da biste poboljšali cjelokupan pouzdanosti, performanse i sigurnost infrastrukture platformu koji se izvoditi na virtualnim računalima.
* Neplanirano održavanja događaja pojaviti kada hardver ili fizičke infrastrukture podlozi virtualnog računala pojavila na mobitel. To se može obuhvaćati pogrešaka u lokalnoj mreži, pogreške na lokalni disk ili druge razine pogrešaka za bicikle. Kada se otkrije takve pogreške, Azure platforme automatski migrirati virtualnog računala iz dobro fizičke poslužitelj koji hostira virtualnog računala dobar fizičke poslužitelj. Takve događaji se rijetko, ali može uzrokovati virtualnog računala da biste ponovno pokrenuti računalo.

Dodatne informacije možete pronaći u ove dokumentacije: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>

#### <a name="azure-storage-redundancy"></a>Zalihosti Azure prostora za pohranu

Podatke na računu sustava Microsoft Azure prostora za pohranu uvijek replicirati da bi se rok trajanja i visoke dostupnosti SLA prostora za pohranu Azure čak i in the face of tranzitne hardverske pogreške za sastanak

Jer je za pohranu Azure je evidencije 3 slike podataka prema zadanim postavkama, RAID5 jedinici ili RAID1 na više Azure diskova nisu potrebni.

Dodatne informacije pronaći ćete u ovom članku: <http://azure.microsoft.com/documentation/articles/storage-redundancy/> 

#### <a name="utilizing-azure-infrastructure-vm-restart-to-achieve-higher-availability-of-sap-applications"></a>Korištenja Azure infrastrukture VM ponovno pokretanje računala da biste postigli "Veća dostupnost" aplikacija SAP

Ako odlučite da nećete koristiti radovi kao što su Windows Server prebacivanje Klasteriranje (WSFC) ili Linux ekvivalentan (drugu mogućnost nešto nije podržana još na Azure u kombinaciji sa softverom za SAP-a), ponovno pokrenite Azure VM se koristi da biste zaštitili sustava SAP protiv planirane i Neplanirana nedostupnost Infrastruktura Azure fizičke server i cjelokupan podlozi platforme Azure. 
 
> [AZURE.NOTE] Važno je da spomenuti da ponovno pokrenite Azure VM prvenstveno štiti VMs, a ne na aplikacije. Ponovno pokrenite VM nude visoke dostupnosti za SAP aplikacije, ali nuditi na dostupnost infrastrukture razinu i stoga neizravno "veća dostupnost" sustavima SAP. Postoji i bez SLA za vrijeme potrebno da biste ponovno pokrenuli u VM nakon planiranog ili neplanirano glavno računalo prekida. Stoga ovu metodu visoke dostupnosti nije prikladna za ključne komponente sustava SAP kao što su (A) SCS ili DBMS.

Neki drugi element važne infrastrukture za visoke dostupnosti je prostora za pohranu. Npr. Azure prostora za pohranu SLA je 99,9% dostupnost. Ako na jednom Azure prostora za pohranu račun, potencijalne Azure prostora za pohranu nedostupnosti uzrokovat će nedostupnosti sve VMs smještene u taj račun za Azure prostora za pohranu i i sve SAP komponente izvodi unutar te VMs uvodi sve VMs s njegova diskova.  

Umjesto stavljanja sve VMs jedan jedan račun Azure prostora za pohranu, možete koristiti i namjenski prostor za pohranu računa za svaki VM i na taj način povećati cjelokupan VM i SAP, a aplikacija dostupnost pomoću više neovisno računa Azure prostora za pohranu. 

Ogledna arhitektura sustava SAP NetWeaver koji koristi Azure infrastrukture HA može izgledati ovako:
 
![Korištenja Azure infrastrukture HA da biste postigli "više" dostupnosti aplikacija za SAP][planning-guide-figure-2900]

Za ključne komponente SAP smo postići sljedeće dosad:

* Visoke dostupnosti poslužitelji aplikacija SAP-a (AS)

SAP aplikacije server instancama su suvišnih komponente. Svaki SAP kao što je instanca implementiran na vlastitu VM koja se izvodi u neku drugu kvara Azure i nadogradnja domene (pogledajte poglavlja [kvara domene] [planiranja – vodič za-3.2.1] i [Domains][planning-guide-3.2.2]) nadogradnju. To je ensured pomoću Azure dostupnost skupovima (pogledajte poglavlja [Azure dostupnost Sets][planning-guide-3.2.3]). Potencijalne planiranog ili neplanirano nedostupnosti kvara Azure ili nadogradnja domene uzrokovat će nedostupnosti ograničeni broj VMs sa svojim SAP kao instance. Svaki SAP kao što je instanca nalazi se u vlastiti račun za pohranu Azure – potencijalne nedostupnosti od jednog računa za pohranu Azure uzrokovat će nedostupnosti samo jedan VM s njegova kao SAP instance. Međutim, imajte na umu da ne postoji ograničenje računa za pohranu Azure Azure pretplata. Da biste bili sigurni automatskog pokretanja (A) SCS instance nakon ponovnog pokretanja VM, provjerite jeste li postaviti automatsko pokrenite parametar (A) SCS instancu start profilu opisane u poglavlja [pomoću automatsko pokrenite SAP instanci] [planiranja – vodič za-11,5].
I pročitajte poglavlja [visoke dostupnosti za poslužitelji aplikacija SAP] [planiranja – vodič za-11.4.1] više pojedinosti.

* _Veća_ Dostupnost instance SCS SAP-a (A)
 
Ne možemo ovdje koristi funkciju Azure VM ponovno da biste zaštitili na VM pomoću instaliranih instance SCS SAP-a (A). U slučaju planiranog ili neplanirano nedostupnost Azure severs, VMs će se ponovno pokrenuti na neki drugi poslužitelj dostupan. Kao što je rečeno ranije, ponovno pokrenite Azure VM prvenstveno štiti na VMs, a ne na aplikacije, u ovoj instanci SCS slova (A). Putem VM ponovno pokrenite smo ćete doći do neizravno "veća dostupnost" instance SCS SAP-a (A). Da biste osigurali automatskog pokretanja (A) SCS instance nakon ponovnog pokretanja VM, provjerite da biste postavili automatsko pokrenite parametar () SCS instancu start profil opisane u poglavlja [pomoću automatsko pokrenite SAP instanci] [planiranja – vodič za-11,5]. To znači SCS (A) instancu kao jednu točku od pogreške (SPOF) radi u jednom VM bit će determinative faktor za raspoloživost cijelog vodoravno SAP-a. 

* _Veća_ Dostupnost DBMS poslužitelja

Ovdje, slično kutije za korištenje instancu SCS SAP-a (A), ne možemo koristiti ponovno pokrenite Azure VM da biste zaštitili VM uz instaliran softver DBMS, a zatim ćemo postigli "veća dostupnost" DBMS softvera putem VM ponovno pokrenite. DBMS izvodi u jednom VM je na SPOF i je determinative faktor za raspoloživost cijelog vodoravno SAP-a. 

### <a name="sap-application-high-availability-on-azure-iaas"></a>SAP visoke dostupnosti aplikacija na Azure IaaS
Da biste postigli punu SAP-a sustava visoke dostupnosti, moramo zaštita sve ključnih SAP komponente sustava, npr suvišnih SAP poslužitelja aplikacije, i jedinstvene komponente (npr. jednu točku od pogreške) kao što su instancu SCS SAP-a (A) i DBMS. 

#### <a name="5d9d36f9-9058-435d-8367-5ad05f00de77"></a>Visoke dostupnosti za poslužitelji aplikacija za SAP
Instanci poslužitelja/dijaloški aplikacija za SAP nije potrebno razmislite o određenim visoke dostupnosti rješenja. Visoke dostupnosti jednostavno postići zalihosti i time imate dovoljno ih u različitim virtualnim računalima. Oni sve smjestiti u istom Azure dostupnost skupu da biste izbjegli na VMs može se ažurirati u isto vrijeme tijekom nedostupnost planiranog održavanja. Osnovne funkcije koji stvara na različitim nadogradnje i kvara domene unutar programa Azure skaliranje jedinice već je uvedena u poglavlja [nadograditi domene] [planiranja – vodič za-3.2.2]. Azure dostupnost skupovima prikazane su u poglavlja [Azure dostupnost skupovima] [planiranja – vodič za-3.2.3] ovog dokumenta. 

Postoji bez neograničen broj kvara i nadogradnja domene koje mogu koristiti Azure dostupnost skup unutar programa Azure skaliranje jedinice. To znači da stavljanje broj VMs u jedan dostupnost postavljanje ranije ili kasnije u činjenica da više VM završava u istom kvara ili nadogradnja domene

Implementacija nekoliko SAP aplikacije instanci poslužitelja u svojim sažecima VMs i uz pretpostavku da ne možemo imate 5 nadograditi domene, na kraju pojavi na sljedećoj slici. Stvarni maksimalni broj kvara i ažuriranje domene unutar skupa dostupnost u budućnosti može promijeniti:
 
![SLIKA poslužitelja aplikacija SAP-a u Azure][planning-guide-figure-3000]

Dodatne informacije možete pronaći u ove dokumentacije: <http://azure.microsoft.com/documentation/articles/virtual-machines-manage-availability>


#### <a name="high-availability-for-the-sap-ascs-instance-on-windows"></a>Visoke dostupnosti za instancu SCS SAP-a (A) u sustavu Windows

Windows Server prebacivanje klaster (WSFC) je najpopularnijih rješenja za zaštitu instancu SCS SAP-a (A). Također je integriran u sapinst u obliku "HA instalacija". Tom trenutku u trenutku Azure infrastrukture nije uspio funkcionalnosti da biste postavili obavezno klaster prebacivanje Windows Server na isti način kao što je proveo lokalnog.

Od siječnja 2016 platforme Azure oblaka operacijskog sustava Windows ne nudi mogućnost nastanka klaster zajednički glasnoće na disku zajednički koriste dvije Azure VMs.

Korištenje 3 proizvođača koji omogućuje zajedničko glasnoću po replikacije sinkrono i prozirne disk koji možete integrirati u WSFC kroz je valjan rješenje. Taj se način podrazumijeva da je samo čvor aktivni klaster moći pristupiti jednu od njih disk u trenutku u vremenu. Od siječnja 2016 ovaj HA konfiguracije podržano je da biste zaštitili instancu SCS SAP-a (A) na Windows goste OS na Azure VMs u kombinaciji s 3 proizvođača SIOS DataKeeper.

Rješenje SIOS DataKeeper omogućuje resursa klaster zajedničke disk klastere prebacivanje Windows tako da vas:

* Dodatne VHD za Azure pridružen svakom virtualnim strojevima (VMs) koje su u konfiguraciji Windows klaster
* Na oba čvorove VM SIOS DataKeeper klaster Edition
* Imate SIOS DataKeeper klaster Edition konfiguriran tako da sinkronizirano Zrcali sadržaj dodatne VHD priložene jedinice iz izvora VMs dodatne VHD priložiti količinu cilj VM.
* SIOS DataKeeper je abstracting izvorišno i odredišno lokalne jedinice i pregledava Windows prebacivanje klaster kao jedan zajedničke disk.
 
Kako instalirati Windows klaster prebacivanje s SIOS Datakeeper i SAP [Klasteriranje instancu ASCS SAP-a pomoću Windows Server prebacivanje klaster na Azure s SIOS DataKeeper] možete pronaći sve pojedinosti[ ha-guide-classic] studiju. 

#### <a name="high-availability-for-the-sap-ascs-instance-on-linux"></a>Visoke dostupnosti za instancu SCS SAP-a (A) na Linux
 
Od Pro 2015 postoji i bez ekvivalent zajedničke disk WSFC za Linux VMs na Azure. Druga solutions pomoću 3 proizvođača softvera kao što su SIOS za Windows su još provjeriti radi SAP-a na Linux na Azure.



#### <a name="high-availability-for-the-sap-database-instance"></a>Visoke dostupnosti za SAP instanci baze podataka
Uobičajeni SAP DBMS SLIKA postavljanje temelji se na dva DBMS VMs kojima se koristi funkcija visoku dostupnost DBMS za replikaciju podataka iz aktivne instance DBMS drugi VM u pasivni DBMS instance.

Visoke dostupnosti i Izrada oporavak funkcije za DBMS Općenito, kao i određene DBMS je podrobnije opisan u [DBMS vodič za implementaciju] [dbms vodič].


#### <a name="end-to-end-high-availability-for-the-complete-sap-system"></a>Kraj do kraja visoke dostupnosti za sustav dovršeno SAP

Dva primjera dovršeno SAP NetWeaver SLIKA arhitekturu u Azure – jedan za Windows i jedan za Linux.
Koncepte kao što je opisano u nastavku možda morati biti ugrožena malo kada implementacija mnogim sustavima SAP, a broj VMs uvedena su premašuju maksimalno ograničenje od račune za pohranu po pretplati. U tim slučajevima VHDs VMs moraju se kombinirati unutar jednog računa za pohranu. Obično činite to kombiniranjem VHDs SAP aplikacijskom sloju VMs različitih sustava SAP-a.  Ne možemo kombinirati i različite VHDs različite DBMS VMs različite sustava SAP-a u jedan račun za Azure prostora za pohranu. Taj način čuvanja IOPS ograničenje prostora za pohranu računi servisa Azure na umu ( <https://azure.microsoft.com/documentation/articles/storage-scalability-targets> )

##### <a name="windowslogowindows-ha-on-windows"></a>![Windows][Logo_Windows] SLIKA u sustavu Windows

![SAP NetWeaver aplikacije HA arhitektura sa sustavom SQL Server u Azure IaaS][planning-guide-figure-3200]

Sljedećih konstrukta Azure koriste se za sustav NetWeaver SAP-a za minimiziranje utjecaj po infrastrukture problema i hostira zakrpa:

* Cjelokupnog sustava je implementiran na Azure (obavezno - DBMS sloja, (A) SCS instancu i dovršavanje aplikacijskom sloju je potrebno za pokretanje na istom mjestu).
* Pokreće cjelokupnog sustava unutar Azure pretplata (obavezno).
* Pokreće cjelokupnog sustava unutar jedne Azure virtualne mreže (obavezno).
* Odvojenosti VMs sustava SAP-a u tri dostupnost skupove moguće je čak i uz sve VMs pripadaju isti virtualne mreže.
* Sve VMs instance DBMS sustava SAP su u jednom skupu dostupnost. Pretpostavimo da postoji više VM pokrenute instance DBMS po sustava od nativni DBMS visoke dostupnosti koriste značajke, kao što su SQL Server AlwaysOn ili Oracle podataka straža.
* Sve VMs pokrenute instance DBMS koristite vlastiti račun za pohranu. DBMS podataka i datoteka zapisnika su replicirati s jednog računa za pohranu na drugi račun za pohranu koji se pomoću funkcije visoke dostupnosti DBMS koji se sinkroniziraju s podacima. Nedostupnosti od jednog računa za pohranu uzrokovat će nedostupnosti jedan čvor klaster SQL Windows, ali ne cijele servis sustava SQL Server. 
* Sve VMs pokretanja instance sustava SAP-a (A) SCS su u jednom skupu dostupnost. Unutar te VMs je konfigurirati Windows klaster poslužitelja prebacivanje (WSFC) da biste zaštitili (A) SCS instance.
* Sve VMs pokrenute instance (A) SCS koristiti svoj poštanski račun za pohranu. (A) SCS instancu datoteke i SAP globalni mapu na drugi račun za pohranu koji se pomoću SIOS DataKeeper replikacije su replicirati s jednog računa za pohranu. Nedostupnosti od jednog računa za pohranu uzrokovat će nedostupnosti jedne () SCS Windows klaster čvor, ali ne i cjeline () SCS servisa. 
* SVE VMs koji predstavlja SAP aplikacijskom sloju poslužitelja u trećoj dostupnost postavljene.
* SVE VMs izvodi poslužitelji aplikacija SAP koristite vlastiti račun za pohranu. Nedostupnosti od jednog računa za pohranu uzrokovat će nedostupnosti jedan poslužitelj aplikacije SAP, gdje druge kao SAP-a i dalje pokretati.

##### <a name="linuxlogolinux-ha-on-linux"></a>![Linux][Logo_Linux] SLIKA na Linux

Arhitektura za SAP SLIKA na Linux na Azure zapravo ista je kao Windows prethodno opisan. Od Sij 2016 postoje dva ograničenja kroz:

* na Linux na Azure bez replikacije značajke elika i mala slova, trenutno podržava samo SAP 16 elika i mala slova. 
* postoji nijedno SAP-a (A) SCS SLIKA rješenje podržava još Linux na Azure

Kao consequence od siječnja 2016 SAP Linux Azure sustava nije moguće postići isti dostupnost kao SAP Windows Azure sustav zbog nedostaje HA za SCS (A) instancu i baze podataka za jednokratni SAP elika i mala slova.

### <a name="4e165b58-74ca-474f-a7f4-5e695a93204f"></a>Korištenje automatsko pokrenite instanci SAP-a

SAP-a nudi funkcije da biste pokrenuli SAP instance odmah nakon početka OS unutar na VM. Točne korake su navedenih u članku iz baze znanja SAP [1909114] – kako pokrenuti SAP instance automatski pomoću parametar automatsko pokrenite. Međutim, SAP je preporučiti koristiti postavku više jer nema kontrole u redoslijedu ponovnog pokretanja instance, uz pretpostavku da više VM imate utječe na ili više instanci pokrenuli po VM. Pretpostavkom uobičajeni Azure scenarij instance jedan SAP aplikacije poslužitelja u web-mjesto s VM i u okvir za slučaj jedan VM naposljetku početak ponovnog pokretanja na automatsko pokrenite nije zaista ključnih i možete omogućiti tako da dodate taj parametar:

    Autostart = 1

U profil start SAP ABAP i/ili Java instance.

> [AZURE.NOTE] 
> Automatsko pokrenite parametar može sadržavati neke downfalls. Detaljno, parametar pokreće početak ABAP SAP-a ili Java instancu kada se pokrene povezane servisa Windows/Linux instance. Certainly je tako kada operacijski sustavi pokretati prema gore. Međutim, ponovnog pokretanja servisa SAP su uobičajeni stvar za upravljanje životni ciklus softverom za SAP funkcije kao što je zbroj ili druge ažurira ili Nadogradi. Ove radovi se očekuje instancu ponovno pokrenuti automatski uopće. Zbog toga parametar automatsko pokrenite mora biti onemogućena prije pokretanja zadatke kao što su. Automatsko pokrenite parametar također se ne smiju koristiti za SAP instance koje su grupirane, kao što su SCS/ASCS/CI.

Potražite dodatne informacije vezane uz automatsko pokrenite za SAP instance ovdje:

* [Početak i kraj SAP uz vaše Unix poslužitelja početak i kraj](http://scn.sap.com/community/unix/blog/2012/08/07/startstop-sap-along-with-your-unix-server-startstop)
* [Pokretanje i zaustavljanje SAP NetWeaver upravljanja agenata](https://help.sap.com/saphelp_nwpi711/helpdata/en/49/9a15525b20423ee10000000a421938/content.htm)
* [Kako omogućiti automatsko pokretanje programa HANA baze podataka](http://www.freehanatutorials.com/2012/10/how-to-enable-auto-start-of-hana.html)


### <a name="larger-3-tier-sap-systems"></a>Veće sustavima SAP 3 sloja
Visoku dostupnost aspekte 3 sloja SAP konfiguracije imate spominju u starijim sekcija već. No što je s sustavi gdje su prevelik da bi se ona nalazi u Azure, ali aplikacijskom sloju SAP poslužiteljski preduvjeti DBMS može biti implementirano u Azure?

#### <a name="location-of-3-tier-sap-configurations"></a>Mjesto konfiguracija SAP 3 sloja

Nije podržana za podjelu razina aplikacije sam ili aplikacije i DBMS sloju između lokalnog i Azure. SAP-a sustava je bilo potpuno distribuiranih lokalno ili na Azure. Također nije podržan da bi neki poslužitelji aplikacija pokretanje lokalnog poslužitelja i druge u Azure. To je početnu točku rasprave. Smo također se ne podržava da bi DBMS komponenti sustava SAP i sloj poslužitelja aplikacija SAP implementiran u dvije različite regije Azure. Npr. DBMS u Zapad SAD-a i SAP, a na aplikacijskom sloju u središnjoj SAD-a. Razlog ne podržava takve konfiguracije je osjetljivost Latencija arhitektura NetWeaver SAP-a.

Međutim, tijekom dvotjednog prošle godine podataka centar za partnere razvio suradnika mjesta Azure područja. Ove suradnika mjesta uz vrlo fizičke Azure podataka često su centrima unutar područja programa Azure. Kratki udaljenost i veze imovine zajednički mjestu kroz ExpressRoute u Azure može uzrokovati Latencija koji je manji od 2ms. U tim slučajevima, da biste pronašli sloj DBMS (uključujući prostora za pohranu SAN/NAS) u takvim zajednički mjesto i na SAP Azure ili na aplikacijskom sloju moguće je. Od 2015 Pro ne imamo sve implementacijama tako. No drugi korisnici s implementacijama aplikacije koje nisu SAP već koristite takvog pristupa. 

### <a name="offline-backup-of-sap-systems"></a>Izvanmrežna sustavi za sigurnosno kopiranje SAP

Zavisne o konfiguraciji SAP postoji odabrali (sloja 2 ili 3 sloju) može biti potrebno sigurnosno kopiranje. Sadržaj VM sam plus da biste imali sigurnosnu kopiju baze podataka. Na DBMS povezane sigurnosne kopije se očekuje da napraviti pomoću metode baze podataka. Detaljan opis za različite baza podataka nalazi se u [DBMS vodič] [dbms vodič]. S druge strane, SAP podataka mogu se sigurnosno na izvanmrežni način (uključujući sadržaj baze podataka) kao što je opisano u ovom odjeljku ili na mreži kao što je opisano u sljedećem odjeljku.

Izvanmrežna sigurnosne kopije zapravo je potrebna isključivanje VM putem portala za Azure i kopiju osnovni disk VM plus sve priložene VHDs na VM. To će zadržati točku vrijeme slici na VM i njegove pridružene disk. Preporučuje se da biste kopirali 'sigurnosne kopije' u neki drugi račun za pohranu Azure. Zato na koji se postupak opisan u poglavlja [kopiranje diskova između računa za pohranu Azure] [planiranja – vodič za-5.4.2] te primjenjivati dokumenta.
Osim isključivanje računala pomoću portala za Azure nešto možete učiniti ga putem komponente Powershell ili EŽA kao što je opisano u nastavku: <https://azure.microsoft.com/documentation/articles/virtual-machines-deploy-rmtemplates-powershell/>

Vraćanje to stanje će se sastojati od brisanje osnovni VM, kao i izvorni diskova s osnovnim VM i postavljen VHDs, kopiranje natrag spremljenu VHDs izvornu račun za pohranu i redeploying u sustavu.
U ovom se članku objašnjava primjer skripte postupak u ljusci Powershell: <http://www.westerndevs.com/azure-snapshots/>

Provjerite je li instalirati novu licencu za SAP jer restoing sigurnosnu kopiju VM prethodno opisan stvara novi ključ hardvera.

### <a name="online-backup-of-an-sap-system"></a>Online sigurnosnu kopiju sustava za SAP

Sigurnosna kopija na DBMS se izvodi s određenim načinima DBMS kao što je opisano u [DBMS vodiču] [dbms vodič]. 

Drugi VMs sustava SAP-a mogu se sigurnosno pomoću funkcije sigurnosne kopije Azure virtualnog računala. Azure virtualnog računala sigurnosne kopije imate uvedene na početku 2015, a u međuvremenu je standardni način za sigurnosno kopiranje dovrši VM u Azure. Azure sigurnosne kopije pohranjuje sigurnosnih kopija u Azure i omogućuje vraćanje od na VM ponovno. 

> [AZURE.NOTE] 
> Od Pro 2015 korištenju sigurnosnih kopija VM ne čuvaju jedinstveni ID VM koji se koristi za SAP licenciranje. To znači da vraćanje iz sigurnosne kopije VM potrebna je instalacija novi ključ licence SAP kao vraćene VM smatra se novi VM i ne zamjenski bivši onu koja je spremljena. Od Sij 2016 sigurnosne kopije VM Azure ne podržava VMs koje su uvedene s Azure Resourc Upravitelj još.

> ![Windows][Logo_Windows] Windows
>
> Theoretically VMs pokretanja baze podataka mogu se sigurnosno dosljedan način kao i ako DBMS sustave podržava Windows VSS (glasnoću sjene Kopiraj servisa <https://msdn.microsoft.com/library/windows/desktop/bb968832(v=vs.85).aspx> ) kao npr SQL Server ne.
> Međutim, imajte na umu koji na temelju sigurnosne kopije Azure VM vraća točke u vrijeme baza podataka nisu moguće. Stoga je na preporuka za izvođenje sigurnosne kopije baze podataka pomoću funkcije DBMS tipkanje sigurnosne kopije VM Azure
>
> Upoznati s sigurnosne kopije virtualnog računala Azure započnite ovdje: <https://azure.microsoft.com/documentation/articles/backup-azure-vms/>.
>
> Druge mogućnosti su poslužite se kombinacijom Microsoftova podataka zaštitu upravitelja instalirana u poruku Azure VM i Azure sigurnosne kopije za sigurnosno kopiranje i vraćanje baze podataka. Dodatne informacije možete pronaći ovdje: <https://azure.microsoft.com/documentation/articles/backup-azure-dpm-introduction/>.  


> ![Linux][Logo_Linux] Linux
> 
> Postoji bez jednaku vrijednost u Windows VSS u Linux. Stoga samo dosljedan datoteku sigurnosne kopije su moguće, ali ne aplikacije-dosljedan sigurnosne kopije. Sigurnosno kopiranje SAP DBMS računajući pomoću funkcije DBMS. Datotečni sustav koji uključuje u SAP povezane podatke moguće je spremiti npr pomoću tar kao što je opisano u nastavku: <http://help.sap.com/saphelp_nw70ehp2/helpdata/en/d3/c0da3ccbb04d35b186041ba6ac301f/content.htm>


### <a name="azure-as-dr-site-for-production-sap-landscapes"></a>Azure kao DR web-mjesto za landscapes radnog SAP-a

Nakon Mid 2014 proširenja za razne komponente oko Hyper-V, centar za sustav i Azure omogućiti korištenje Azure kao DR web-mjesta za VMs koji se izvodi na lokaciji na temelju Hyper-V. 

Blog dovršenog kako implementirati rješenje ovdje navedenih: <http://blogs.msdn.com/b/saponsqlserver/archive/2014/11/19/protecting-sap-solutions-with-azure-site-recovery.aspx>

## <a name="summary"></a>Sažetak
Ključne točke visoke dostupnosti za sustavima SAP u Azure su:

* Tom trenutku u trenutku, SAP jednoj točki kvara nije moguće osigurati na isti način kao što možete učiniti u lokalnim implementacijama. Razlog je taj zajednički Disk klastere nije još moguće napraviti u Azure bez korištenja 3 proizvođača softvera.
* Za sloj DBMS morate koristiti funkcije DBMS koje ne ovise o tehnologije klaster zajedničke disk. Detalji o su zabilježeni u [DBMS vodič] [dbms vodič].
* Da biste minimizirali utjecaj probleme unutar kvara domena u Azure održavanje infrastrukture ili glavno računalo, trebali biste koristiti Azure dostupnost skupova:
    * Preporučuje se da bi jedan dostupnost postavljena za SAP-a na aplikacijskom sloju.
    * Preporučuje se da bi zasebnom dostupnosti skupa za sloj DBMS SAP-a.
    * NE preporučuje se da biste primijenili istu dostupnosti za VMs različitih sustava SAP.
* Svrhu sigurnosne kopije sloj DBMS SAP-a, provjerite je li [DBMS vodič] [dbms vodič].
* Sigurnosno kopiranje instance SAP dijaloški smisla malo Budući da je obično brže implementirati instance jednostavne dijaloški okvir.
* Sigurnosno kopiranje VM koja sadrži globalni direktorija sustava SAP i s njom profili različite instance smisla i trebaju izvesti pomoću sigurnosnog kopiranja Windows ili npr ciljni na Linux. Budući da postoje razlike između Windows Server 2008 (R2) i Windows Server 2012 (R2), koje olakšavaju sigurnosne kopije pomoću novije Windows Server izdavanje, preporučujemo da se pokreće Windows Server 2012 (R2) kao gost operacijski sustav. 
