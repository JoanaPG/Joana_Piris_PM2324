## PRÀCTICA COMPTADOR
En aquest document s'explica el procediment seguit a la pràctica 1 comptador. 

#### ANÀLISI DE L'ESCTRUCTURA DEL PROJECTE
El primer pas de la pràctica és la creació del projecte al IDE d'android Studio. Hem partit d'una plantilla amb una activitat buida: "Empty Views Activity" i hem configurat el llenguatge Kotlin amb la SDK API 24.
Es genera una estructura de carpetes com la que es veu a la següent imatge:

| Podem veure que els fitxers dins del mòdul app s'agrupen en 4 capetes: manifests, java, res i Gradle scipts.  |![image](https://github.com/JoanaPG/Joana_Piris_PM2324/assets/78020595/6317c1a3-4823-47c3-8111-94f0dc01f8b0)|
| --- | --- |
| | |
- En la carpeta *manifest* trobem l'arxiu ```AndroidManifest.xml``` on és defineixen els aspectes més importants del nostre projecte, com el nom, paquet, icona, espais de noms.
- A a la carpeta *java* trobem el codi font de la nostra aplicació.
- En *res* hi ficarem tots els recursos associats, imatges, dissenys, colors, etc.
- I a la carpeta *Gradle Scripts* estan els fitxers de configuració de Gradle (tant a nivell d'aplicació com de mòdul)
Una activitat està composta per una vista (arxiu amb extensió xml que representa la part visual o layout, és a dir la pantalla), un fitxer amb el codi font on s'inclou la lògica de l'activitat (fitxer amb la classe i extesnió .kt).  A més d'aquests dos fitxers és necesari definir l'activitat en el fitxer de manifest, afegint una etiqueta activity com apareix al següent exemple:

![image](https://github.com/JoanaPG/Joana_Piris_PM2324/assets/78020595/6ecc7570-49eb-4898-8aca-dc10b36e7695)


#### ANÀLISI DEL CICLE DE VIDA I EL PROBLEMA DE PÈRDUA D'ESTAT
Per poder entendre el cicle de vida d'una activitat s'han ficat missatges de log en els mètodes onStart(), onResume(), onPause(), onStop(), onRestart() i onDestroy(). 
Al inciar l'activitat podem veure com apareix el missatge "onStart" i "onResume" (una vegada ja està visible per a l'usuari), però en fer el canvi d'orientació veiem com apareixen els altres missatges de les altres fases: onPause, onStop, onDestroy, i de nou "onStart" i "onResume", és a dir, torna a iniciar la vista amb la nova configuració horizontal:

![image](https://github.com/JoanaPG/Joana_Piris_PM2324/assets/78020595/cfa71aad-c649-47d4-ba3e-a1ad6210c6a9)


#### SOLUCIÓ A LA PÈRDUA D'ESTAT

Per defecte, android recrea la vista al canviar d'orientació. Aquest comportament es pot modificar de diverses formes: 

- canviant la **configuració de la Activity en el fitxer AndroidManifest.xml**, afegint una etiqueta android:configChanges="orientation|screensize", i sobreescrivint el mètode onConfigurationChanged de la nostra classe:
 
    ```@Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig); //altres acciones desitjades}
    ```


- guardant l'estat amb els **mètodes onSaveinstanceState / onRestoreInstanceState**, que ens permeten guardar l'estat en un 'Bundle'. (com anem a fer en el nostre cas)

    ```@Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        outState.putString("comptador", comptador);
    }
    
    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
        comptador = savedInstanceState.getString("comptador");
    }
    ```
- fent ús del **ViewModel**, que ens permet gestionar les dades de la nostra activitat encara que s'hi donen canvis en la configuració.
  
#### AMPLIANT LA FUNCIONALITAT AMB DECREMENTS I RESET

#### CANVIS PER IMPLEMENTAR EL VIEW BINDING
