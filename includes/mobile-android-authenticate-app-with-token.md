
U prethodnom primjeru prikazivao standard prijavu, što zahtijeva klijenta za kontakt davatelja identiteta i pozadinskog Azure service prilikom svakog pokretanja aplikacije. Ne samo nije ovu metodu, možete naiđete na probleme vezane uz korištenje više klijenata trebao da biste pokrenuli aplikaciju u isto vrijeme. Bolje pristup je predmemoriju ovlaštenja vratio servisa Azure i pokušajte koristiti u ovom prvom prije korištenja je utemeljen na davatelja prijavu. 

>[AZURE.NOTE]Možete predmemoriju token izdala pozadinskog Azure service neovisno o tome koristite klijent upravlja ili servis upravlja provjere autentičnosti. Pomoću ovog praktičnog vodiča koristi servis upravlja provjere autentičnosti.


1. Otvorite datoteku ToDoActivity.java i dodajte sljedeće naredbe uvoz:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Dodavanje u sljedećim članovima na `ToDoActivity` predmete.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. U datoteci ToDoActivity.java dodajte na sljedeću definiciju za na `cacheUserToken` način.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Ova metoda pohranjuje korisnički id i token u datoteci preferenca koji je označen kao privatne. To biste trebali zaštititi pristup predmemorije tako da druge aplikacije na uređaju imaju pristup token jer je preferencama u memoriji za testiranje za aplikaciju. Međutim, ako netko poboljšava pristupa na uređaju, moguće je da može ostvariti pristup predmemoriju tokena do druge načine. 

    >[AZURE.NOTE]Možete dodatno zaštititi token šifriranjem ako se smatra vrlo osjetljive tokena pristupa podacima i netko može ostvariti pristup na uređaju. Međutim, potpuno sigurnom rješenje izlazi iz ovog praktičnog vodiča i ovisi o sigurnosne preduvjete.


4. U datoteci ToDoActivity.java dodajte na sljedeću definiciju za na `loadUserTokenCache` način.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. U datoteci *ToDoActivity.java* zamjena na `authenticate` postupak sa sljedećih metoda koji koristi tokena predmemoriju. Ako želite koristiti račun koji nije Google, promijenite davatelj prijave.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Stvaranje aplikacije i testiranje provjera autentičnosti pomoću valjane računa. Pokrenite najmanje dvaput. Tijekom prvog pokretanja, trebali biste dobiti upit za prijava i stvaranje tokena predmemoriju. Nakon toga svaki Pokreni će pokušati učitati tokena predmemoriju za provjeru autentičnosti i ne smije biti u potreban za prijavu.



