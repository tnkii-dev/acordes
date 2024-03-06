#Acordes

Acordes es una herramienta interactiva diseñada para facilitar el estudio y la práctica de acordes musicales. Esta aplicación, desarrollada en JavaScript, está destinada a ayudar a estudiantes principiantes o intermedios, que desean mejorar sus habilidades en la identificación y reproducción de acordes.

El propósito es ofrecer una plataforma dinámica que haga del aprendizaje de acordes una experiencia atractiva y memorable. A través de la generación aleatoria de acordes y la visualización opcional de notas en un piano virtual, la aplicación utiliza la técnica de repetición espaciada para optimizar el proceso de aprendizaje, para permitir consolidar conocimientos de manera progresiva y efectiva.

##Instalación

La instalación de Acordes es opconal, puedes probarla en linea [aquí](http://www.niupleis.com/projects/acordes/)

Si deseas descargarlo localmente simplemente clona este repositorio o descarga el [zip](https://github.com/tnkii-dev/acordes/archive/refs/heads/main.zip), Se ejecuta directamente en tu navegador sin necesidad de instalaciones adicionales o de un servidor, arrastrando el archivo [index.html](/index.html/).

##Uso

La interfaz permite configurar el acorde generado para ajustarse al nivel de habilidad del usuario.

####Configuración

En esta seccíón se configura la clase de acordes que se mostraran en el display, abarca una gran cantidad de simbolos de acordes. Los de Triadas simples, los de Septimas, de PowerChords, De Sextas, notas Suspendidas, Miscalaneas, Extensiones y Alteraciones. Aparte una configuración especial para estudiar escalas modales.

![configuracion](/res/screenshoots/cofiguración.webp)

####Display

EL display tiene dos partes: El nombre de acorde y las notas del acorde.

![display](/res/screenshoots/display.webp)

La primera muestra el acorde generado cada que se corre el programa al hacer click en el boton `Generar acorde` o al presionar la tecla `space`.

![display](/res/screenshoots/display1.webp)

La segunda parte es el piano virtual que muestra las notas que corresponden al acorde generado, está oculto por defecto, y se puede cambiar su visibilidad al hacer click el botón `Mostrar acorde` o al presionar la tecla `enter`.

####Interfaz

**Casilla 'Escalas'.**
Esta casilla simplemente se encarga de activar los simbolos de escala en el display, si se activa sola sin ningún otra casilla, a aplicaión interpretará la configuración com si fuera una triada mayor, en música un simbolo de nota sin acorde se interpreta como un acorde mayor.
Se activa por defecto y cada que se activa otra opción, perfectamente se puede desactivar pero la aplicaión será inutilizable.

`C, C#, D♭, D, D#, E♭, E, F, F#, G♭, G, G#, A♭, A, A#, B♭, B`

**Casilla 'Triada'.**
Esta casilla activa los simolos de cuatro tipos de acorde:
*Acordes mayores*
`maj, M`
*Acordes menores*
`min, m, -`
*Acordes disminuídos*
`dim, °`
*Acordes aumentados*
`aug, (#5), +`

**Casilla 'Septima'.**
Esta casilla activa los simbolos de acorde con septima mayor, menor, disminuída y semi-disminuída.
`7, △, M7, ⌀7, 7(♭5)`
Y también los simbolos de novenas, onceavas y  treceavas singulares (Las que no necesitan añadir el acorde se septima completo).
`add9, add11, add13`

**Casilla 'Power Chords'**
Añade simbolos de powerchords (acordes de dos notas)
`5, 3`

**Casilla 'Sextas'**
Añade todos los simbolos que contienen o añaden sextas al acorde
`6, (add♭6), (♭6), ⁶₉`
El simbolo `⁶₉` añade también una novena al acorde

**Casilla 'Suspensiones'**
Añade los simbolos de acordes con notas suspendidas
`sus, sus4, sus2`

**Casilla 'Miscelaneas'**
Aveces hay convinaciónes de notas que no tienen una escritura estandarizada y lo que hace es encontrar una manera lógica de describirla, para eso se usan estos símbolos oscuros
`2, (add2)`

**Casilla 'Extensiones'**
Esta casilla activa los simbolos de acordes extendidos, novenas, onceavas y treceavas completas (Las que si añaden septimas, novenas y onceavas)
`9, 7(add9), 11 y 13`
Estos hacen terceras y quintas opcionales, ya que a menudo el interprete omite notas, tambien se omiten con los simbolos de la siguiente casilla.

**Casilla 'Alteraciones'**
Esta añade simbolos que alteran un acorde.
Las que piden al interprete omitir notas
`(omit5), (no5), (omit7), (no7)`
Y las que piden aumentar o disminuír notas un semitono
`(#9), (♭9), (#11), (♭11), (#13) y (♭13)`

**Casilla 'Escala modal'**
Esta casilla se utiliza independientemente de todas las demás, esta no genera acordes, esta genera una escala modal, es decir, enseña todas las notas de la escala a partir de la nota generada. Y puede generar escalas en todos los modos:
`jonico, dorico, frigio, lidio, mixolidio, eolico, locrio`

##Como funciona

###Funciones

####**`generarAcorde()`**

La primera parte contiene look-up tables de todos los simbolos que se pueden usar en el display
```javascript
  const escalas = ["C", "C#", "D", "D#", "E", "F", "F#", "G", "G#", "A", "A#", "B"];
  const triadas = ["maj", "M", "m", "min", "-", "aug", "+", "(#5)", "dim", "°"];
  const septimas = ["7", "△", "△7", "M7", "⌀7", "7(♭5)", "(add9)", "(add11)", "(add13)"];
  const powerChords = ["5", "3"];
  const sextas = ["6", "(add♭6)", "(♭6)", "⁶₉"];
  const miscelaneas = ["2", "(add2)"];
  const alts = ["b9", "#9", "b5", "#5", "b13"];
  const suspendidas = ["sus", "sus4", "sus2"];
  const extensiones = ["9", "7(add9)", "11", "13"];
  const alteraciones = ["(omit5)", "(no5)", "(omit7)", "(no7)", "(#9)", "(♭9)", "(#11)", "(♭11)", "(#13)", "(♭13)"];
  const invertidas = ["/C", "/D", "/E", "/F", "/G", "/A", "B/", "/C#", "/D#", "/F#", "/G#", "/A#"];
  const arpegiosModales = [" jonico", " dorico", " frigio", " lidio", " mixolidio", " eolico", " locrio"];
```
La siguiente guarda en variables el estatus de cada checkkbox
```javascript
  const escalaCheckbox = document.getElementById("escalasCheckbox").checked;
  const triadasCheckbox = document.getElementById("triadasCheckbox").checked;
  const septimasCheckbox = document.getElementById("septimasCheckbox").checked;
  const powerChordsCheckbox = document.getElementById("powerChordsCheckbox").checked;
  const sextasCheckbox = document.getElementById("sextasCheckbox").checked;
  //const alteradasCheckbox = document.getElementById("alteradasCheckbox").checked;
  const suspendidasCheckbox = document.getElementById("suspendidasCheckbox").checked;
  const extensionesCheckbox = document.getElementById("extensionesCheckbox").checked;
  const alteracionesCheckbox = document.getElementById("alteracionesCheckbox").checked;
  const inversionesCheckbox = document.getElementById("inversionesCheckbox").checked;
  const miscelaneasCheckbox = document.getElementById("miscelaneasCheckbox").checked;
  const arpegioModalCheckbox = document.getElementById("arpegioModalCheckbox").checked;
```
Luego se valida que al menos una checkbox esté activada
```javascript
  if (!(escalaCheckbox || arpegioModalCheckbox || triadasCheckbox || septimasCheckbox ||
        powerChordsCheckbox || sextasCheckbox || /*alteradasCheckbox ||*/ suspendidasCheckbox ||
        extensionesCheckbox|| alteracionesCheckbox || inversionesCheckbox || miscelaneasCheckbox)) {
    alert("Selecciona almenos una ._.");
    return;
  }
```
Lo siguiente es crear la tabla donde se guardarán todas los simbolos para juntar con `.join` al final.

```javascript
  let acorde = [];
  ```

*A partir de aquí empieza la geneación del acorde y la union de simbolos*

Primero se selecciona un simbolo random de la tabla de simbolos de escalas y si es un simbolo de sostenido `#` se cambia a uno de `♭` el 50% de la veces.
```javascript
  if (escalaCheckbox) {
    escala = escalas[Math.floor(Math.random() * escalas.length)];
    if(escala == "C#" && Math.floor(Math.random()*2)==1){
        escala="D♭"
    }; // Esto es un fragmento de ejemplo
    acorde.push(escala);
  }
```

Luego se generan simbolos para triadas, septimas, powerChords, sextas, suspendidas y miscelaneas con probabilidad de 1 a 4,
```javascript
  if (triadasCheckbox && Math.floor(Math.random()*4)+1!=2) {
    triada = triadas[Math.floor(Math.random() * triadas.length)]
    // Aqui se comprueban los simbolos especificos para aplicar condiciones especiales "if (triada == "'maj'){}"
    acorde.push(triada);
  }
  // Este ejemplo es de 'triadas', aplica de manera similar para los tipos de acorde mencionados
```

*Las extensiones y alteraciones funcionan diferente ya que requieren condiciones especiales para funcionar pero comparten probabilidades y comprobaciones especificas.*

*El modo Arpegio Modal tambien requiere condiciones especificas ya que funciona diferente.*

Finalmente se juntan los acordes resultantes y se actualiza el objeto en la página para mostrar el acorde.
```javascript
  const resultadoElement = document.getElementById("resultado");
  resultadoElement.textContent = acorde.join("");
```

```html
  <button onclick="generarAcorde()">Generar Acorde (space)</button>
  <div style="font-size: 48px;" id="resultado"></div>
```

**Condiciones especiales y simbolos especificos**

Al generar triadas no se requieren condiciones especiales

####Display de acorde

####Display de notas