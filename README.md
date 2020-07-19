# backend-node-platzi
Proyecto del curso de Backend con Node 2019


# BackendNode
build an instagram chat whit plazti

### Peticiones HTTP

es un lenguaje común para todas la comunicaciones
entre cualquier cliente/servidores

clienete envia una petición a internet y desde ahi la revibe el servidor para luego devolver una respuesta

### Cómo es una petición

GET /index.html HTTP/1.1
Host:www.example.com --a donde lo estoy pidiendo
Referer:www.google.com -- de donde
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10) --navegador usuario
Connection:keep-alive --información de la conexion..que se mantenga viva mientras..

### Cómo es una Respuesta
HTTP/1.1 200 OK
Date: wed,8 jul 2020
23:59:59 GMT
Content-Type: text/htmlEscape
Content-Length:1221

<html></html>

## Puntos claves

#### Métodos
que quieres hacer

GET
recoger informacion del servidor

PUT
reemplaza información en el servidor

POST
Añadir información al servidor

DELETE
eliminar información del servidor

PATCH
Actualzia parte de la información en el servidor

OPTIONS
Pedir información sobre los métodos, apra saber si podemos usar POST,PUT,PATCH o DELETE



### Cabeceras
Cómo quieres hacer la petición

Authotization--asegurar que puedes pedir cosas al servidor
Indicaciones
condiciones
Cors--solo se pueda pedir de ciertas cabeceras podemos o no manejar información desde afuera de nuestro servicio
cookies--compartir informacion entre peticiones te identifica como usario
Accept--tipo de contenido que acepta
cache--almacenamiento temporal la respuesta será la misma durante cierto tiempo

#### Estado
Cómo ha ido la operacion
https://http.cat/

200:ok
201:created

peticion redirigida
3xx

errores del cliente
4xx

errores del servidor
500:internal server error


### Cuerpo
lo que el servidor devuelve
    la informacion que buscamos el servidor nos dé

    depende de las cabeceras

    Content-type --que tipo de contenido en

    ejemplos

    text/html
    text/css
    application/javascript
    image/jpeg
    application/json
    application/xml

 ### parámetros query
 orden de las Peticiones
 parámetros que quieres medir

 api.com/personas?orderBy=name&age=25

 (nunca envies información sensible dentro de parametros de query)


estructura
 -añadir ? al final de la url
 -nombre=valor
 -separarlos con &
# Crear un servidor HTTP en js y comenzar a escuchar y responder peticioned desde un cliente

## creando el server HTTP desde nodeJS

node package manager (npm) permite trabajar con paquetes en js

`npm init` iniciar el proyecto js desde el servidor esto crea el package.json con las config inicial

## usando el router de express
 `const router = express.Router();`

 nos permite separar cabeceras por method, url ...

 y se añade con

 `app.use(router);`

 para añadir peticiones sería:

 router.method('/',function(req, res){
     //respuesta
     res.send(); //esto nos permite mostrar la respuesta al cliente
 })



 ## Recibir informacion del cliente: Body y query des

 instalamos 
 body-parser

importamos el modulo con
`const bodyParser = requiere('body-parser');`
el body lo encontramos en la requests
cuando hagamos el request es importante no enviar el bodyParser entero si no usar
los métodos que contiene ej:
 `app.use(bodyParser.json());`

 podemos ver la respuesta con un console.log(req.body)
 ó en vez de body usar (req.query) en la peticion del mensaje.
 ej:

  router.post('/', function (req, res){
      console.log(req.query);
  }):

## cómo leer las cabeceras
    estas vienen en el request pero se pueden enviar en el response tambien y se consulta en headers cabeceras especificas para el cliente y las que quieran actualizar ej:
    res.headers({
        'custom-header':'header personalizado';
    })

    las ubicamos dentro del router.get('/message', function (req, res){
        console.log(req.headers) //esta información varia segun el navegador usado en ////si usamos insomniaó postman será diferente
    });

 dentro de los headers hay unos que recomienda usar y que son muy útiles, cómo
 cache-control: para poder configurar caché especifico para imagenes, arcivos js,...
ej :
    `'cache-control':'max-age=0'`

 user-agent: para desde donde nos vienen
 y en casos concretos la cabeceras

 connect y accept


## tipos de respuesta	: vacia, plana, con datos y estructurada

para lograr un mejor protocolo de comunicación es importante manejar una misma formade responder 

se puede devolver un mensaje solo planos
un status vacio
res.status(201).send();

de una forma estructurada enviar un objeto

res.status(400).send({
    "error":"hola mundo fixed",
})
pueden ser arrays
res.status(201).send([{
    "status-response":"array value",
}])

## Respuestas coherentes #
se recomienda crear un modulo el cual responda las peticiones

exports.success = function (req, res,message,status ){
    //response
}

exports.error = function (req, res){
    //response
}

se recomienda crear una carpeta "network" aqui es donde se integrarán todas las partes que tengan
uan aplicacion de red completa

## servir archivos estáticos
html,css,js  todos los archivos necesarios para le frontend de nuestra aplicación
node tiene la ventaja de que no se bloquea

usarremos el servidor de estaticos de express

    `app.use('/app', express.static('nombre de carpeta donde queremos pasar la información')) // 

    creamos index.thm, style.css y lo traemos en el server


## ERROREs:cómo presentarlos e  implicaciones en la seguridad

cuando algo suceda mal es bueno comunicar error si algo falla
pero es muy importante tener cuidado para no dar inforamción sensible

para el personal que mantiene la aplicación si podemos respoder errores con details
para tener un log de lo que ha sucedido esto lo veremos en el backend

pero el cliente no verá esto

podemos usar esto en la peticion del response

exports.error = function(req, res,message,status,details){
    console.error(details);

    })

}
 
# comprender y desarrollar la arquitectura básica de un backend en Node.JS y comunicarse entre nmódulos

### Rutas, controladores, bases de datos

de donde nos van a venir las peticiones? desde internet (nube)

el pruimeto que recibe las peticiones es el `server.js`
este se encarga de que las peticiones son correctas o cancelarlas, este tambien configura toda la informacion importante del server, BD , cabeceras... este envia la informacion a un archivo donde gestionamos todas las rutas...

`routes.js` este se encarga de revisar ahcia donde va la peticion y llamar al componente adecuado

los componentes se encuentran en carpetas dentro la carpeta `components`y estas contiene el nombre del componente eje:

la carpeta `message` contiene toda la informacion de los mensajes
    -cuales son los endpoints de los mensajes
    -acciones que queremos hacer
    -logica relacionada

dentro de esta carpeta tenemos al archivo de rutas en este caso llamado `network` este se encarga de
  gestioanr todas las rutas
  endpoints http

tambien tenemos al controlador el archivo `controller` este contiene toda la lógica
    si el mensaje tiene una fecha
    si el mensaje necesita llamar a otro componente para hacer más cosas
    todo lo que sea modificar le mensaje, hacer comprobaciones se hace aqui lo que se conoce cómo lógica de negocio

y para guardar los datos lo tenemos gestionado en el archivo `store`
    se encarga de indicar donde y cómo se guarda la información
    en que BD 

componente `users`
    network,controller,store

fichero `response.js` cada vez que una peticion sea correcta el network se lo devuelve al response y este se encarga de emitir los  mensajes

al dividir nuestro servidor de esta manera lo hacemos más escalable



### definiendo la logica de negocio
 que es lo que va suceder dentro de nuestros arcchivos
    

