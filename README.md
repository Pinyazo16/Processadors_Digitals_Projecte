# Processadors Digitals: Projecte

## Idea del projecte:

---

La idea del projecte és recrear el clàssic joc “******************************Simon says******************************” utilitzant el microcontrolador **********ESP32**********.
Tot i que aquest joc té una gran quantitat de variacions, per aquest projecte ens basarem en la versió més bàsica del joc.

El projecte consistirà a preparar un codi per a l'ESP32 que ens generi una seqüència aleatòria de llums i que comprovi si la seqüència introduïda pel jugador és correcta.

![Untitled](Processadors%20Digitals%20Projecte%208a7a2f58c7ab4830bf3f5eb059f80f9a/Untitled.png)

Per al projecte també comptavem amb certs requeriments com poden ser l’utilització d’un *******display*******, o l’implementació i ús d’una pàgina web.

# El codi:

---

El codi consta de dues parts o funcions principals per al funcionament del joc: 

## El generador de seqüències:

---

Aquesta funció és la que fa que cada partida sigui completament diferent, generant una seqüència aleatòria cada cop que s’inicia el joc.

```cpp
void generar_sequencia(){ 
  randomSeed(millis());
  for (int i = 0; i < nivel_max; i++)
  {
    sequencia[i]  = random(16,20); 
  }
}
```

En aquest cas generem un nombre aleatori entre $16$ i $19$. Aquests valors corresponen als pins de l’**********ESP32********** als que estan connectats els LEDs. D’aquesta manera podem utilitzar la variable `sequencia[i]` per encendre els LEDs corresponents a la funció `mostrar_sequencia` , que (com indica el nom) és la que s’encarrega de mostrar la seqüència pels LEDs del joc.

## La comprovació de la seqüència:

---

Aquesta segona funció és fonamental pel funcionament del joc, ja que gestiona l’entrada de la seqüència a través del botons i comprova que aquesta coincideixi amb la seqüència mostrada pels LEDs. 
En cas que la seqüència introduïda pel jugador sigui incorrecta, aquesta funció fa una crida a una altra funció anomenada `secuencia_incorrecta` que gestiona els efectes de llum i so en cas d’error i reinicia els paràmetres del joc per a la següent partida.

```cpp
      if(digitalRead(32) == LOW){ //Boton rojo
        digitalWrite(pin_rojo, HIGH);   
        sequencia_actual[i] = pin_rojo;
        marcador = 1;
        delay(200);
        if(sequencia_actual[i] != sequencia[i]){
          secuencia_incorrecta();
          return;
        }
        digitalWrite(pin_rojo, LOW);
      }
```

Aquesta funció està dividida en quatre blocs de codi que comproven cadascun dels botons. En aquest cas tenim el bloc de comprovació del botó corresponent al LED vermell.
Tots quatre blocs de comprovació (per tots quatre botons) estan dins una funció `while()` que té com a condició que la variable `marcador` sigui igual a $0$. Si aquesta variable canvia a $1$, significa que la seqüència és correcta i per tant acaba el bucle i crida a la funció `secuencia_correcta` que gestiona els efectes en cas d’encertar la seqüència.

## Altres funcions del codi:

---

El codi també compta amb un sistema que augmenta lleugerament la velocitat del joc progressivament; així com més dura la partida, més augmenta la dificultat del joc. Aquest paràmetre de dificultat el gestionen les funcions `secuencia_correcta` i `secuencia_incorrecta` que augmenten i reinicien la dificultat respectivament.

# El muntatge:

---

El muntatge consisteix principalment de cinc botons (un per cada color i un altre per iniciar el joc), quatre LEDs, un display i un ******buzzer******.

![Sin título-1.jpg](Processadors%20Digitals%20Projecte%208a7a2f58c7ab4830bf3f5eb059f80f9a/Sin_ttulo-1.jpg)

Aquí podem veure un esquema del cablejat del joc. Com podem veure els LEDs estan connectats a pins amb nombres consecutius per assegurar el funcionament de la funció que genera la seqüència de cada partida.
El display també està connectat a uns pins específics del microprocessador, ja que utilitza el bus **I2C** per la comunicació amb l’**********ESP32**********.

Per això el display està connectat als pins ******G21****** i ******G22******; que són els pins de comunicació ******I2C****** per defecte.
Per tal de millorar l’estètica del projecte i la comoditat a l’hora de jugar, hem creat una caixa on poder amagar tots els components electrònics i l’**********ESP32.**********

Per millorar la posició dels botons a l’hora de jugar, la caixa compta amb unes perforacions d’on surten els botons, els LEDs i el display.
Al disseny també hi consta una ranura per on poder accedir al port micro USB de l’**ESP32** que utilitzem per alimentar tot el joc.
També, i tot i que no apareix al disseny, hi ha una altra perforació per al botó d’inici del joc i una petita perforació a un dels laterals per on pot sortir el so del ***************************buzzer*************************** mentre es reprodueixen els efectes. 

![Untitled](Processadors%20Digitals%20Projecte%208a7a2f58c7ab4830bf3f5eb059f80f9a/Untitled%201.png)

A l’interior de la caixa hi ha una petita PCB que conté l’**********ESP32********** amb totes les connexions necessàries que apareixen a l’esquema de dalt, però sense utilitzar la protoboard.

# Conclusions:

---

En conclusió, creiem que és un projecte que permet millores; com podria ser la substitució del ******buzzer *********************per un altaveu que reproduís un so específic per a cada color (com fa el joc real) o la millora de la part de la web implementant un scoreboard on poder visualitzar les puntuacions dels últims jugadors o les millors puntuacions obtingudes.
També, i com hem comentat al principi, el joc original té un gran nombre de variacions, que van des d’afegir més colors, a afegir diferents modes de joc.
En definitiva és un projecte que replica el joc més clàsic de la millor manera que hem pogut amb les limitacions dels nostres recursos i dels requisits del projecte. I tot i que sempre es pot millorar, estem bastant satisfets amb el resultat.
