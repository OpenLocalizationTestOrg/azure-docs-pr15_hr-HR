
1. U programu Explorer Visual Studio u rješenje, desnom tipkom miša kliknite projekt aplikacija iz Windows trgovine, kliknite **trgovina** > **Aplikacije za povezivanje s trgovinom...**.

    ![Pridruživanje aplikacija iz Windows trgovine](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. U čarobnjaku kliknite **Dalje**, prijavite se pomoću Microsoftova računa, upišite naziv aplikacije u **rezervnih novi naziv aplikacije**, a zatim kliknite **rezervnih**.

3. Nakon registracije za aplikaciju uspješno stvorili, odaberite novi naziv aplikacije, kliknite **Dalje**, a zatim **povezati**. To su potrebne informacije o registraciji iz Windows trgovine dodaje programski manifest.

7. Ponovite korake 1 i 3 za Windows Phone trgovine aplikacija projekta isti Registracija ste prethodno stvorili za aplikaciju iz Windows trgovine.  

7. Otvorite [Windows Razvojni centar](https://dev.windows.com/en-us/overview)za prijavu pomoću Microsoftova računa kliknite novi Registracija aplikacije u **Moje aplikacije**, a zatim proširite **servisa** > **automatske obavijesti**.

8. Na stranici **automatske obavijesti** kliknite **Live bilježaka i brošura** u odjeljku **Windows automatske obavijesti Services (WNS) i Microsoft Azure mobilne aplikacije**i zabilježite vrijednosti **SID paketa** i *trenutnu* vrijednost u **Aplikaciji tajna**. 

    ![Postavke za aplikaciju u centru za razvojne inženjere](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Tajna aplikacije i paket SID su važne sigurnosne vjerodajnice. Zajedničko korištenje ove vrijednosti sa svima i distribucija ih s aplikacijom.
