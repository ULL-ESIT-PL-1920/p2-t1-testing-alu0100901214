## p2-t1-testing-alu0100901214
### Sergio González Guerra   alu0100910214

# Tutorial practica 2

## Introducción.
Usaremos Node.js para convertir datos XML en formato JSON, haciendo uso de la metodología de desarrollo dirigida por pruebas (TDD). Para ello debemos instalar las dependencias necesarias.

## Creando nuestra carpeta de trabajo y cargando los archivos rdf.
- Primero debemos crearnos la carpeta '/database', donde guardaremos todo el código fuente:

  #mkdir databases
  
- También crearemos la carpeta '/data' dentro de '/database', donde guardaremos una serie de datos sobre libros en formato rdf:

  #mkdir data
  
- Descargaremos los archivos rdf de los libros dentro de la carpeta '/data':

  #curl -O http://www.gutenberg.org/cache/epub/feeds/rdf-files.tar.bz2
  
- Descomprimimos el contenido:

  #tar -xvjf rdf-files.tar.bz2
Esto nos creara una carpeta cache con todos los archivos rdf.

## Inicializar package.json .

- Uno de los primeros ficheros fuente que necesitaremos sera el package.json, lo inicializaremos con el siguiente código: 

![ini](./img2/packageJsonInicial.PNG)

- Instalamos chai para realizar los expect y mocha para los test y seguir la metodología TDD dentro de '/database':

  #npm init -y
  
  #npm install --save-dev --save-exact mocha@2.4.5 chai@3.5.0
  
- Añadimos una 'scripts' del package.json con lo siguiente:

![scriptMocha](./img2/scriptMocha.PNG)

- Ahora la entrada de los test invocaran a mocha.

## /Test y /Lib

- Primero crearemos la carpeta de '/test' con #mkdir y inicializamos dentro el archivo parse-rdf-test.js .
- Dentro de parse-rdf-test.js añadimos lo siguiente:

![function](./img2/function.PNG)

- Ahora podemos ver que dicho test nos falla, puesto que no hemos definido la función parseRDF correctamente: 

#npm test

![failed](./img2/failed.PNG)

- Creamos la carpeta '/lib' y inicializamos el fichero 'parse-rdf.js' donde declararemos los objetos necesarios para que nuestro test funcione correctamente:

![lib](./img2/lib.PNG)

- Realizando #npm test una vez mas, comprobaremos que nuestras expectativas funcionan correctamente:

![npm-test](./img2/npm-test.PNG)

## Hacer que los test se ejecuten continuamente

- Podemos aumentar la rapidez en la que ejecutamos los tests haciendo que se ejecuten continuamente en lugar de invocar #npm test cada vez.

- Para ello añadiremos otro script en el archivo package.json con lo siguiente:

![test-watch](./img2/test-watch.PNG)

- Ahora tan solo tendremos que ejecutar en una terminal aparte #npm run test:watch y se ejecutara automáticamente #npm test cada vez que modifiquemos un archivo.

## Extraer datos de XML con Cheerio

- Primero instalamos con npm.

  #npm install --save --save-exact cheerio@0.22.0

- Ejecutamos los tests continuamente con mocha usando el comando #npm run test:watch

- Modificamos el parse-rdf-test.js del /test para añadir un nuevo expect que detecte la id de un libro:

![expect-id](./img2/expect-id.PNG)

- Nos fallara la terminal de Mocha ya que no tenemos implementado el codigo que incluya el 'id' del objeto 'book'.

- Ahora en 'parse-rdf.js' de /lib añadimos el código necesario para poder identificar el id de un libro y formatearlo:

![book-id](./img2/book-id.PNG)

- Una vez modificado, la terminal de Mocha deberia decirnos que los test funcionan correctamente.

## Debugging tests con Chrome DevTools

- Es posible unir los DevTools de chrome con Node.js. Aqui podremos hacer breakpoints y esperar variables.

- Primero añadiremos en la sección de scripts del package.json lo siguiente:

![test-debug](./img2/test-debug.PNG)

- Ahora ejecutamos el comando #npm run test:debug

- Abrimos chrome y entramos en la URL especial  'chrome://inspect' y nos aparecera en la página de dispositivos de Chrome DevTools:

![chromeDebugger](./img2/chromeDebugger.PNG)

- En Remote Target, seleccionamos el link azul 'inspect'

- Y nos abrira los Devtools:

![DevTools](./img2/DevTools.PNG)

- Aqui podemos ir a la sección de 'Sources', abrir algun archivo y establecer breakpoints:

![breakpoint1](./img2/breakpoint1.PNG)

- Como Mocha esta ejecutando continuamente los tests, cada vez que cambie el fichero, ejecutara los test mirando los breakpoints.

