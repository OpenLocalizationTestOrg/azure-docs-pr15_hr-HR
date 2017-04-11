## <a name="send-messages-to-event-hubs"></a>Slanje poruke s koncentratorima događaja

U ovom odjeljku smo će pisanje u aplikaciji C da biste poslali događaje koncentratora za događaj. Koristit ćemo biblioteku Proton AMQP iz [Apache Qpid projekta](http://qpid.apache.org/). Ovo je analogno pomoću servisa Bus redovima i teme AMQP iz C kao što je prikazano [ovdje](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Dodatne informacije potražite u članku [Qpid Proton dokumentacije](http://qpid.apache.org/proton/index.html).

1. Sa [stranice Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html), kliknite vezu **Instalacije Qpid Proton** i slijedite upute ovisno o okruženju. Ne možemo će da okruženju Linux; na primjer, u [Azure Linux VM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) s Ubuntu 14.04.

2. Da biste Kompiliranje Proton biblioteke, instalirajte paketi za sljedeće:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Preuzmite biblioteku [Qpid Proton biblioteke](http://qpid.apache.org/proton/index.html) i izdvajanje, npr.:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Stvaranje Sastavi direktorija, Kompiliranje i instalirajte:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. U imeniku tvrtke, stvorite novu datoteku pod nazivom **sender.c** sa sljedećim sadržajem. Imajte na umu da zamijenite vrijednost za naziv događaja koncentratora i polja naziva (drugu mogućnost je obično `{event hub name}-ns`). Morate zamijeniti i kodiran URL-om verziju tipku za **SendRule** ste ranije stvorili. Možete URL kodiranje je [ovdje](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Kompiliranje datoteku, uz pretpostavku da **gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] U kodu, da biste nametnuli poruke izvan čim koristimo odlazne prozor 1. Općenito govoreći, aplikacija trebao poruke grupe da biste povećali propusnost. Posjetite [stranicu Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html) dodatne informacije o korištenju biblioteka Qpid Proton u ovom i drugim okruženja, kao i iz platforme za koje se nude povezivanja (trenutno Perl, PHP, Python i Ruby).
