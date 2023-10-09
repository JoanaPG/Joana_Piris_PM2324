## PRÀCTICA COMPTADOR
En aquest document s'explica el procediment seguit a la pràctica 1 comptador. 

### ANÀLISI DE L'ESCTRUCTURA DEL PROJECTE
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


### ANÀLISI DEL CICLE DE VIDA I EL PROBLEMA DE PÈRDUA D'ESTAT
Per poder entendre el cicle de vida d'una activitat s'han ficat missatges de log en els mètodes onStart(), onResume(), onPause(), onStop(), onRestart() i onDestroy(). 
Al inciar l'activitat podem veure com apareix el missatge "onStart" i "onResume" (una vegada ja està visible per a l'usuari), però en fer el canvi d'orientació veiem com apareixen els altres missatges de les altres fases: onPause, onStop, onDestroy, i de nou "onStart" i "onResume", és a dir, torna a iniciar la vista amb la nova configuració horizontal:

![image](https://github.com/JoanaPG/Joana_Piris_PM2324/assets/78020595/cfa71aad-c649-47d4-ba3e-a1ad6210c6a9)


### SOLUCIÓ A LA PÈRDUA D'ESTAT

Per defecte, android recrea la vista al canviar d'orientació. Aquest comportament es pot modificar de diverses formes: 

- canviant la **configuració de la Activity en el fitxer AndroidManifest.xml**, afegint una etiqueta android:configChanges="orientation|screensize", i sobreescrivint el mètode onConfigurationChanged de la nostra classe:
 
    ```@Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig); //altres acciones desitjades}
    ```


- guardant l'estat amb els **mètodes onSaveinstanceState / onRestoreInstanceState**, que ens permeten guardar l'estat en un 'Bundle'. (com anem a fer en el nostre cas)

    ``` override fun onSaveInstanceState(estat: Bundle) {
        super.onSaveInstanceState(estat)
        outState.putInt("comptador", comptador)
    }
    
       override fun onRestoreInstanceState(estat: Bundle) {
       super.onRestoreInstanceState(estat)
       comptador=estat.getInt("comptador")
      }
    ```
- fent ús del **ViewModel**, que ens permet gestionar les dades de la nostra activitat encara que s'hi donen canvis en la configuració.

  En el nostre cas hem afegit el mètode onSaveinstanceState i en el mètode on create s'assigna valor al comptador sempre que aquest Bundle el continga. Amb aquests canvis la nostra activitat ja manté l'estat del comptador i per tant podem canviar d'orientació sense problema:
  
    ```override fun onCreate(savedInstanceState: Bundle?) {
          super.onCreate(savedInstanceState)
          setContentView(R.layout.activity_main)
  
          // Referencia al TextView
          val textViewContador=findViewById<TextView>(R.id.textViewComptador)
  
          if (savedInstanceState != null) {
              savedInstanceState.getInt("comptador", 0).also { comptador = it }
          }
    
    ...  *resta de codi*
    
    ```
  
### AMPLIANT LA FUNCIONALITAT AMB DECREMENTS I RESET

Per ampliar la funcionalitat s'inclouen nous botons a la vista (resta, add, rest) i en la classe es fiquen els corresponents listeners on implementem la lògica de increment, decrement o inicialització del comptador a 0.

```
        // Referencia al botón
        val btAdd=findViewById<Button>(R.id.btAdd)
        val btResta=findViewById<Button>(R.id.btResta)
        val btReset=findViewById<Button>(R.id.btReset)


        // Asociaciamos una expresióin lambda como
        // respuesta (callback) al evento Clic sobre
        // el botón
        btAdd.setOnClickListener {
            comptador++
            textViewContador.setText(comptador.toString())
        }

        btResta.setOnClickListener {
            comptador--
            textViewContador.setText(comptador.toString())
        }

        btReset.setOnClickListener {
            comptador=0
            textViewContador.setText(comptador.toString())
        }
```
(En el codi de la vista ActivityMain.xml, en la linia 54 hi ha una t de més: app:layout_constraintStart_toEnd**t**Of )

### CANVIS PER IMPLEMENTAR EL VIEW BINDING
Per a implementar el view binding s'ha modificar el codi del fitxer build.gradle.kt, afegint el 'viewBinding = true':

```plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
}

android {
    namespace = "com.ieseljust.pmdm.comptador"
    compileSdk = 33

    defaultConfig {
        applicationId = "com.ieseljust.pmdm.comptador"
        minSdk = 24
        targetSdk = 33
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = "1.8"
    }

    buildFeatures {
        viewBinding = true
    }
}

dependencies {

    implementation("androidx.core:core-ktx:1.9.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.9.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    implementation("androidx.media3:media3-common:1.1.1")
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
}
```

Una vegada fem el Build del projecte ja tenim disponible la classe ActivityMainBinding i ja no és necessari obtindre els components amb el findBy. Podem fer el import i modificar el mètode on create per a obtindre el binding de la següent forma:

```import com.ieseljust.pmdm.comptador.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding
    var comptador=0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        val view = binding.root
        setContentView(view)

        if (savedInstanceState != null) {
            savedInstanceState.getInt("comptador", 0).also { comptador = it }
        }

        binding.textViewComptador.text = comptador.toString()

        binding.btAdd.setOnClickListener {
            comptador++
            binding.textViewComptador.text = comptador.toString()
        }

        binding.btResta.setOnClickListener {
            comptador--
            binding.textViewComptador.text = comptador.toString()
        }

        binding.btReset.setOnClickListener {
            comptador = 0
            binding.textViewComptador.text = comptador.toString()
        }
    }
```

