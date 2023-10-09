## PRÀCTICA COMPTADOR
En aquest document s'explica el procediment seguit a la pràctica 1 comptador. 

#### ANÀLISI DE L'ESCTRUCTURA DEL PROJECTE
El primer pas de la pràctica és la creació del projecte al IDE d'android Studio. Hem partit d'una plantilla amb una activitat buida: "Empty Views Activity" i hem configurat el llenguatge Kotlin amb la SDK API 24.
Es genera una estructura de carpetes com la que es veu a la següent imatge:

| Podem veure que els fitxers dins del mòdul app s'agrupen en 4 capetes: manifests, java, res i Gradle scipts.  |![image](https://github.com/JoanaPG/Joana_Piris_PM2324/assets/78020595/6317c1a3-4823-47c3-8111-94f0dc01f8b0)|
| --- | --- |
| | |
- En la carpeta *manifest* trovem l'arxiu ```AndroidManifest.xml``` on és defineixen els aspectes més importants del nostre projecte, com el nom, paquet, icona.
- A a la carpeta *java* trovem el codi font de la nostra aplicació.
- En *res* hi ficarem tots els recursos associats, imatges, dissenys, colors, etc.
- I a la carpeta *Gradle Scripts* estan els fitxers de configuració de Gradle (tant a nivell d'aplicació com de mòdul)  
Una activitat està composta per una vista (arxiu amb extensió xml que representa la part visual, és a dir la pantalla), un fitxer amb el codi font, funcionament lògic de la activitat, que es el fitxer amb la clase kotlin.

#### ANÀLISI DEL CICLE DE VIDA I EL PROBLEMA DE PÈRDUA D'ESTAT

#### SOLCUIÓ A LA PÈRDUA D'ESTAT

#### AMPLIANT LA FUNCIONALITAT AMB DECREMENTS I RESET

#### CANVIS PER IMPLEMENTAR EL VIEW BINDING
