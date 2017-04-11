<properties
    pageTitle="Jedinstvo snimljene fotografije Marko vodiča"
    description="Koraci za stvaranje klasični jedinstvo stariju verziju Marko utakmica koji je stara obavezna za sve vodiče jedinstvo radnje Mobile"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Stvaranje stariju verziju jedinstvo Marko igre

Pomoću ovog praktičnog vodiča vodi kroz glavni koraci za malo izmijenjene [jedinstvo snimljene fotografije Marko vodič](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Ova igra uzorka sastoji se od objekta spherical 'reproduktora' kojima se upravlja korisnik aplikacije i cilj igre je 'prikupljanje' collectible objekata po colliding reproduktora objekt s te collectible objekte. Pretpostavlja da se osnovni poznavanje čije je okruženje uređivač jedinstvo. Ako naiđete na probleme, a zatim pogledajte cijeli vodič. 

### <a name="setting-up-the-game"></a>Postavljanje igre
Koraci u nastavku su iz [jedinstvo vodiča](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Otvorite **Uređivač jedinstvo** , a zatim kliknite **Novo**. 
    
    ![][51] 
    
2. Navedite **Naziv projekta** & **mjesto**, odaberite **3D** i kliknite **Stvori projekt**.
    
    ![][52]

3. Spremanje scene zadani upravo stvorili kao dio novi projekt kao što je s nazivom **MiniGame** unutar novi ** \_scena** mape u mapi **Resursi** :
    
    ![][53]

4. Stvaranje u **ravnini -> 3D objekt** kao polje reprodukcija i preimenovati taj objekt ravnini kao **dna**

    ![][1]

5. Ponovno postavljanje pretvorbe komponenta za taj objekt **dna** tako da se nalazi na izvor. 

    ![][3]

6. Poništite potvrdni okvir **Pokaži rešetke** izbornika **sprave** **dna** objekta.

    ![][4]

7. Komponente **Promjena veličine** objekta **dna** biti [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Dodajte u novi **veliki raster -> 3D objekta** u projekt i preimenujte taj objekt veliki raster kao **reproduktora**. 

    ![][6]

9. Odaberite objekt **reproduktora** , a zatim kliknite **Vrati pretvoriti** slično ravnini objekt. 

10. Ažuriranje **transformacija -> položaj -> Koordinate Y** komponenta za Y reproduktora 0,5.  

    ![][7]

11. Stvorite novu mapu pod nazivom **materijale** u projektu kojem ćete stvoriti materijal boja reproduktora. 

12. Stvorite novi **materijal** naziva **pozadine** u ovu mapu. 

    ![][8]

13. Ažurirajte boja materijala ažuriranjem **Albedo** svojstvo je. Možete odabrati vrijednosti RGB [0,32,64]. 

    ![][9]

14. U ovom materijala odvučete scene prikaz da biste primijenili boju na objekt **dna** . 

    ![][10]

17. Na kraju ažuriranje na **transformacija -> zakretanje -> Y** u 60 na objekt izravnog osnovna verzija za prikaz. 

    ![][12]

### <a name="moving-the-player"></a>Premještanje reproduktora
Koraci u nastavku su iz [jedinstvo vodiča](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Dodajte komponentu **RigidBody** objekt **reproduktora** . 

    ![][13]

2. Stvorite novu mapu pod nazivom **skripte** u projektu. 

3. Kliknite **dodajte komponentu -> nove skripte -> C# skripte**. Naziv **PlayerController**pa kliknite **Stvaranje i dodavanje**. To će stvoriti i priložite skriptu objekt reproduktora.  

    ![][14]

5. Premještanje ovu skriptu u mapi **skripte** u projektu. 

6. Otvorite skripte za uređivanje u uređivaču omiljene skripte, ažurirajte kodu skripte sljedeći kod i spremite je. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Imajte na umu da iznad skripte koristi svojstvo **brzinu** . U uređivaču jedinstvo ažurirati svojstvo brzine 10.  

    ![][15]

9. Kliknite **Reproduciraj** u uređivaču jedinstvo. Sada ste trebali biste moći nadzirati Marko putem tipkovnice, a trebali biste zakretanje i kretanje. 

### <a name="moving-the-camera"></a>Premještanje na kameru
Korake u nastavku su iz [jedinstvo vodič](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) i će sve **Glavne kamere** objekt **reproduktora** . 

1. Ažuriranje **Transform.Position** da je X = 0, Y = 10.5, Z =-10.  
2. Ažuriranje **Transform.Rotation** da je X = 45, a zatim Y = 0, Z = 0.  

    ![][16]

2. Dodavanje nove skripte naziva **CameraController** **MainCamera** i premjestite u mapi skripti. 

    ![][17]

3. Otvorite skripte za uređivanje i u nju dodati sljedeći kod:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. U okruženju jedinstvo - odvučete varijabla reproduktora vremensko razdoblje reproduktora glavne kamere objekta tako da se međusobno povezuju dva. 

    ![][18]

6. Sada ako kliknete Reproduciraj u uređivaču jedinstvo i rotiranje objekta reproduktora Marko zatim vidjet ćete kamere pratiti u premještanje.  

### <a name="setting-up-the-play-area"></a>Postavljanje područja za reproduciranje
Koraci u nastavku su [jedinstvo vodič](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Ne možemo će stvoriti zidovi oko s dna tako da se objekt reproduktora Marko ne predaju područja za reproduciranje u svoje kretanje. 

1. Kliknite **Stvaranje -> stvorite prazan -> utakmica objekt** i nazovite ih **zidovi**

    ![][19]

2. U odjeljku taj objekt zidovi – stvaranje na nove **kocke -> 3D objekta** i nazovite ih "Zapad zida". 

    ![][20]

3. Ažurirajte **pretvorbe -> položaj** i **pretvorbu -> mjerilo** za regije zapada zida objekt. 

    ![][21]

4. Dupliciranje zida Zapad da biste stvorili **Istok zida** s položajem ažurirane pretvorbe i promjena veličine. 

    ![][22]

5. Dupliciranje zida Istok da biste stvorili **Sjeverna zida** ažurirane pretvorbe položaj i promjena veličine. 

    ![][23]

6. Dupliciranje zida Sjeverna i stvorite **Južnoafrička zida** sa ažurirane pretvorbe položaj i promjena veličine. 

    ![][24]

### <a name="creating-collectible-objects"></a>Stvaranje Collectible objekata
Koraci u nastavku su [jedinstvo vodič](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). Ne možemo će stvoriti neke privlačne izgledaju objekte koji će se obrazac skup collectible objekte koji objekt reproduktora Marko treba 'prikupljanje' po colliding s njima. 

1. Stvaranje novog **3D kocke objekta** i nazovite ih prijem. 

2. Prilagodba **pretvorbe -> zakretanje** & **transformacija -> skaliranje** prikupljanja objekta. 

    ![][25]

3. Stvaranje i priložite **Nove C# skripte** naziva **Rotator** prikupljanja objekt. Provjerite jeste li stavili skriptu u mapi skripte. 

    ![][26]

4. Otvorite ovaj skripte za uređivanje, a zatim ažurirajte ga da bi se sljedeće: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. Sada kliknite načina reprodukcije u uređivaču jedinstvo i na prikaz prikupljanja objekt se rotiranje na njegov osi.

6. Stvorite novu mapu pod nazivom **Prefabs** 

    ![][27]

7. Povucite i ispustite objekt **prijem** i spremite ga u mapi Prefabs.

    ![][28]

8. Stvorite novi **prazan utakmica objekt** pod nazivom **Pickups**. Vraćanje položaja polazište, a zatim povucite prikupljanja objekta u odjeljku igre objekt.  

    ![][29]

9. Dupliciranje **prijem** objekt, a zatim podjele na objektu **dna** oko objekta **reproduktora** ažuriranjem vrijednosti **X & Z Transform.Position-** komponenta. 

    ![][30]

10. Stvorite **novi materijal** naziva **prijem** i ažurirajte ga da bi se crvena u boji ažuriranjem **Albedo svojstvo** slični onima koje ćemo koristila za ažuriranje dna objekta. 

    ![][31]

11. Materijal primjenjuju se na sve objekte 4 prikupljanja.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>Prikupljanje prikupljanja objekata
Koraci u nastavku su [jedinstvo vodič](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Reproduktora ćemo ažurirati tako da se moći 'prikupljanje' prikupljanja objekata po colliding s njima. 

1. Otvorite gore skripte **PlayerController** priložiti objekt reproduktora za uređivanje, a zatim je ažurirajte sljedeće:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. Stvaranje nove **oznake** naziva **Odaberite gore** (mora podudarati što je u skripti)  

    ![][33]
    
    ![][34]

3. Primjena **oznake** na objekt Prefab prijem. 

    ![][35]

4. Omogućivanje potvrdni okvir **IsTrigger** Prefab objekta.

    ![][36]

5. Dodavanje čvrsta tijelo prijem Prefab objekt. Optimizaciju performansi ćemo ažurirati statične collider koji se koristi za dinamičku collider. 

    ![][37]
  
6. Na kraju potvrdite svojstvo **IsKinematic** prefab objekta. 

    ![][38]

7. Uspješnosti **reproducirati** u uređivaču jedinstvo, a bit će moći reproducirati ove igre **stariju verziju programa Marko** pomicanjem objekt reproduktora pomoću tipke na tipkovnici za unos smjer. 

### <a name="updating-the-game-for-mobile-play"></a>Ažuriranje igra za mobilne Reproduciraj
U odjeljcima iznad završena osnovni vodič s jedinstvo. Sada ćemo izmijeniti utakmica da biste ga mobilnog uređaja neslužbeni. Imajte na umu da koristi unosa na tipkovnici igre dosad za testiranje. Sada ćemo će ga izmijeniti tako da ćemo nadzor reproduktora odnosno pomoću kretanja telefona Korištenje Accelerometer kao unos. 

Otvorite **PlayerController** skripte za uređivanje i ažuriranje **FixedUpdate** način da biste koristili kretanja iz na accelerometer za pomicanje objekta reproduktora. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Pomoću ovog praktičnog vodiča završava osnovni igraće stvaranje pomoću jedinstvo, a to možete implementirati na uređaju po izboru da biste reproducirali igra. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
