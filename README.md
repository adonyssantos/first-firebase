# Firestore para SQL Developers

Este proyecto es del curso en [YouTube](https://www.youtube.com/playlist?list=PLCKuOXG0bPi29EkcAuVCln9ISbExcQk66) de Fernado Herrera.

**API Documentation** [https://googleapis.dev/nodejs/firestore/latest/CollectionReference.html](https://googleapis.dev/nodejs/firestore/latest/CollectionReference.html)

## Scripts:

Recuerden reconstruir los módulos de Node

```
npm install
```

Y para construir el build, recueren:

```
npm run build
```

# Resumen

## Conexión a Firebase

Para conectar Firebase con nuestra Aplicación Web debemos de crear una App en la configuración de proyecto de Firebase.

Hay varias formas de conectar nuestro proyecto con Firebase, la que usamos en este caso fue con un archivo `/config/firebase.ts` para almacenar la configuracion de Firebase.

```ts
import firebase from "firebase/app";
import "firebase/firestore";

const firebaseConfig = {
  apiKey: "AIzaSyCELZUvvnBQsxYfCb9Bl-p8QB3uQKuUWJc",
  authDomain: "first-firebase-4cd37.firebaseapp.com",
  projectId: "first-firebase-4cd37",
  storageBucket: "first-firebase-4cd37.appspot.com",
  messagingSenderId: "116369059692",
  appId: "1:116369059692:web:adb10ff3f756c51a520d49",
  measurementId: "G-6D5B5Q5L10",
};

firebase.initializeApp(firebaseConfig);

export default firebase.firestore();
```

_En este caso las variables estan publica porque eliminare este proyecto una vez termiine el curso, pero la forma correcta de hacer esto es utilizando Variables de entorno, ya que estos datos son muy sensibles y cualquier persona con esto datos podria modificar nuestra BD._

Ahora debemops importar nuestra configuracion a nuestro archivo principal:

```ts
import "./config/firebase";
```

En este caso agregamos `firebase.firestore()` dentro de una variable para poderla reutilizar.

```ts
const db = firebase.firestore();

export default db;
```

## Insertar datos en una colección

Podemos relacionar **colección** de Firebase con una Tabla de SQL. Esto no es correcto pero para entenderlo mejor.

**Nota:** _Para insertar datos debemos enviarle un objecto literal/sencillo. No podemos pasarle funciones, por ejemplo._

```ts
import db from "./config/firebase";

const user = {
  name: "Adonys",
  age: 17,
  title: "Frontend Developer",
  student: true,
};

// Insertar datos en una colección
db.collection("users")
  .add(user)
  .then((docRef) => {
    console.log(docRef);
  })
  .catch((error) => console.log("error", error));
```

## Actualizar datos de una colección

### Referencia a una colección

Si vamos a reutilizar una colección podemos almacenarla en una variable:

```ts
// Referencia a la colección users
const usersRef = db.collection("users");
```

De esta forma no tenemos que llamar la misma colección varias veces.

```ts
usersRef.add({...})
```

No existen un metodo `update` para la referencia de una colección, por lo que hay que llamarla por el `id`.

```ts
// Referencia a la colección users
const usersRef = db.collection("users");

// Actualizar datos de una colección
usersRef.doc("SdFvvlTZsiHs9KrNnEre").update({ student: true });

// También podemos utilizar .set, pero es una forma destructiva, lo que quiere decir que borrara los otros datos
usersRef.doc("SdFvvlTZsiHs9KrNnEre").set({ student: true });
```

_Se recomienda utilizar `then` y `catch` para evitar errores._

## Borrar datos de una colección

Para borrar datos de una colección se hace de la misma forma que con [update](#actualizar-datos-de-una-colección), se le pasa la referencia a la colección, el id del cocumento que se desea borrar y el metodo `.delete()`.

```ts
// Borrar datos de una colección
usersRef
  .doc("SdFvvlTZsiHs9KrNnEre")
  .delete()
  .then(() => console.log("Borrado"))
  .catch((error) => console.log(error));
```

_Utilizar `then` y `catch` no es necesario pero ayuda a evitar errores._
