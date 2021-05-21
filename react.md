---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: React
permalink: /react/
---

# React

Buenas prácticas de estilo y código para los projectos realizados con React.


### Buenas prácticas

Siempre punto y coma al final.

```js
  const variable = 3;
```

Una línea al final de cada archivo.

```js
...
...
export default MiComponente; //Line Break

```

Los archivos al principio deben encontrarse las librerias oficiales y luego las importaciones locales.

```js
// Global
import React from 'react';
import { connect } from 'react-redux';
// Local
import Table from '../../components/Commons/Table';
import InvoicesList from '../../components/Invoices/List';
...
...
```

Comentarios en inglés.

```js
// There must be a better way
if(true){
  return false;
} else {
  return true;
}
```

Para las funciones globales se ha de comentar de la siguiente manera:
*Más detalle en: https://jsdoc.app/tags-param.html*
```js

/**
 * Descripción de la función (en inglés):
 * Qué se pasará como parámetro, que realizará y lo que devolverá (No más de 64 caracteres)
 * 
 * Parámetros que recibe: tipo, nombre parámetro y descripción de ser necesario.
 * @param {string} key The key to match at the moment of finding the value.
 * @param {any} value The value to match the value of the key param.
 * 
 * Se ha de dar un ejemplo
 * // return found object in the array
 * @example [].find(valueByKey('id', id));
 */
function valueByKey(key, value) {
  // some logic
}
```

## Funciones

Las funciones que se usarán solo en un componente o container van en el mismo archivo de este, de lo contrario se moverán al archivo común `utils.js`.

Se ha de trabajar con programación funncinal más que orientada a objetos. Esto nos permite reutilizar más codigo y nos da simpleza al momento de escribir y entender el código.

Los nombres de las funciones han de ser simples y que conecten con la inteción al momento de usar la función. Ejemplo, quiero buscar `(find)` el id `(key)` de un valor `(value)` dado.

```js
function valueByKey(params) {}

function uniqueValue() {}

 [].find(valueByKey('id', value));
```
El ejemplo anterior podemos leer *buscar el valor por la clave* de un valor dado.

Las funciones que retornen valores booleanos se han de usar las palabras `is`, `are` y `exist` para permitir una mejor lectura.
```js

function areValidNumbers() {}

// Literalmente podemos leer "si son números válidos" - If are valid numbers
if (areValidNumbers()) { 
}

function userExist() {}

// Literalmente podemos leer "si usuario existe" - If user exist
if (userExist()) {
}

function isProccesCompleted() {}

// Literalmente podemos leer "si está completado el proceso" - If the proccess is completed
if (isProccessCompleted()) {
}

```

Usar programación funcional para saber *qué* estoy haciendo sin perder tiempo sabiendo el *cómo*.

```js
// Use this
[].map(valueByKey('name', value));

// Instead of this
[].map(value => value.name);

...

// Use this
[{isActive: true, name: 'Doe'}, {isActive: false, name: 'John'}]
  .map(pipe(
    String.toUpperCase,
    uniqueValue('name'),
  )); // result: ['DOE', 'JOHN'];
  
// Instead of this
[{isActive: true, name: 'Doe'}, {isActive: false, name: 'John'}]
  .map(value => {
     return value.name.toUpperCase();
  }); // result: ['DOE', 'JOHN'];
```

Reglas importantes
> * Las funciones que gatillan eventos del dom, hand de tener el sufijo `Handler`. *Ej.- onClick**Handler***
> * Hand de user el prefijo `on[NombreEvento]`. *Ej.- **onClick***
> * Si hay más de dos eventos iguales, ejemplo, dos `onClickHandler`, éste se ha de declarar usando un nombre que identifique qué gatilla la acción. *Ej. **onClickSummaryHandler**, **onClickModalCloseHandler***


## Estructura de carpetas

Componentes en carpetas y dentro de ella un archivo index. De tener sub-componentes dentro de este componente se ha crear una nueva carpeta con su respectivo `index.js`.

```
.
|_ components
   |_ MyComponent
      |_ OtherComponent
        |_ index.js
      |_ index.js

```

```
.
|_ components
   |_ MyComponent
      |_ index.js
```

Los nombres de las carpetas han de ser de general a específico:
```
|_ components
   |_ Button
      |_ SmallButton
         |_ index.js
      |_ FabButton
         |_ index.js
      |_ index.js
```

## Componentes

Procurar no usar clases si no es necesario.
> Cuando usar clases?
> * Cuando se ha de usar algún ciclo de vida como `constructor()`, `static getDerivedStateFromProps()`, `getSnapshotBeforeUpdate()`.
> * Manejadores de errores: `static getDerivedStateFromError()`, `componentDidCatch()`.
> * Cuando se tienen muchas funciones dentro del container y estas son difíciles de mantener usando los hooks `useCallback` o `useMemo`.

```javascript
// Use
const Component = () => {
   return <Things />;
};
// Instead of
class Component extends React.Component {
  render(){
    return <Things />;
  }
};

```

Las acciones deben ser bindeadas utilizando, en general, con .bind(this) ya que el bind con arrow function es hasta 10 veces más lento.

```js
class Component extends React.Component {
  action = this.action.bind(this);

  action(){
    ...
  }

  render(){
    return <Things />;
  }
};

```
Con las versiones actuales de CRA ya no es necesario hacer use de la función `bind`. Se han de seguir las siguientes reglas.
  * Declarar las funciones afuera del render con como `arrow function`.
  * Por lo general no declarar el constructor.
  * El estado es declarado como una variable más pero sin `const`, `let` o `var`

```js

class Component extends React.Component {
  state = {
  };

  action = () => {
    ...
  };

  render(){
    return <Things />;
  }
};

```
### containers vs components
* Los containers deben de tener todo lógica compleja y acciones que han de compartir uno o muchos componentes. Por lo general contiene contiene un estado y, manipula y pasa los props.
* Los componentes han solo de "pintar" la estructura html. Por lo general tienen poca lógica que es necesaria y propia para pintar el componente. Poco posibilidad de tener estado y manipular los props.

## Hooks



* Las funciones de reductor (Redux) deben ser escritas como *object* en lugar de *switch-case* de esta forma se reduce el overhead por exceso de comparaciones.

```js

...

// Reducer function
export default function reducer(state = initialState, action) {
  return (reducerObject[action.type] || reducerObject.default)(state, action);
}

...
...

// Get State From API
const reduceGetStateFromApi = (state, _) => state.setIn(['loading'], true);
const reduceGetStateFromApiSuccess = (state, action) => state
  .setIn(['farmDocumentIds'], action.result.farmDocumentIds)
  .setIn(['loading'], false);
const reduceCreateDocument = (state) => state.setIn(['form', 'acceptLoading'], false);
const reduceCreateDocumentSuccess = (state, action) => state
  .updateIn(['farmDocumentIds'], ids => [...new Set([...ids, action.result.farmDocumentId])])
  .setIn(['form', 'open'], false)
  .setIn(['form','acceptLoading'], true);
// Form & Controls
const reduceChangeControls = (state, action) => state.mergeIn(['controls'], action.payload);
const reduceChangeForm = (state, action) => state.mergeIn(['form'], action.payload);
const removeFarmDocumentId = (state, action) => state.updateIn(['farmDocumentIds'], ids => ids.filter(id => id !== action.result.farmDocumentId));

...
...

const reducerObject = {
  [CREATE_FARM_DOCUMENT]: reduceCreateDocument,
  [CREATE_FARM_DOCUMENT_SUCCESS]: reduceCreateDocumentSuccess,
  [GET_STATE_FROM_API_SUCCESS]: reduceGetStateFromApiSuccess,
  [DESTROY_FARM_DOCUMENT_SUCCESS]: removeFarmDocumentId,
  [GET_STATE_FROM_API]: reduceGetStateFromApi,
  [CHANGE_CONTROLS]: reduceChangeControls,
  [CHANGE_FORM]: reduceChangeForm,
  default(state, _) {
    return state;
  }
};

...

```

* Para generar la aciones usar makeActionActionCreator

```js
import makeActionCreator from '../../services/makeActionCreator';

...

export const CREATE_FARM_DOCUMENT = 'farm-manager/farmDocuments/CREATE_FARM_DOCUMENT';

...

export const createFarmDocument = makeActionCreator(CREATE_FARM_DOCUMENT, 'payload');

...

```

* Una acción múltiples sagas (No llamar a más de una saga en la misma línea de ejecución desde un componente)


* Utilizar Chart.js para gráficos

```js
import { Pie, Bar } from 'react-chartjs-2';
...
class PieGraph extends React.Component {
  render() {
    const options = {
      cutoutPercentage: 50,
      rotation: 270,
      animation: {
        animateRotate: true,
      }
    };

    const data = {
      datasets: [{
        data: series,
        backgroundColor: [
          "#43a047",
          "#00acc1",
        ],
        hoverBackgroundColor: [
          "#43a047",
          "#00acc1",
        ]
      }],
      labels: ['Presupuestado', 'Utilizado'],
    };

    return (
      <React.Fragment>
        <Pie data={data} options={options} />
      </React.Fragment>
    );
  }
}

export default PieGraph;

```

* Container usando una función `Stateless`
> * Hacer use del hook `useEffect` cuando necesitamos algo ~similar~ a: `componentDidMount()` y `componentDidUpdate()`.
  * **componentDidMount**
  ```js
  const Component = () => {
    useEffect(() => {
      // This act as a componentDidMount
    }, []);
  }
  ```

* Minimizar la cantidad de props al pasarlas al componente hijo


```js
// Avoid passign all props or big props
class Component extends React.Component {
  render(){
    return <Things props={this.props} />;
  }
};

// Pass only needed props
class Component extends React.Component {
  render(){
    const { prop1, prop2, prop3 } = this.props;
    return <Things prop1={prop1} />;
  }
};

```


* Uso de estados en Rayen:
Caso 1: Si un container tiene estados que se utilizan solo en el componente, se utiliza el estado del container
Para los containers que corresponden a clases se utiliza el estado de la clase
Cuando el container corresponde a un hook se utiliza el hook default de react useState
Caso 2: En caso que se utilice este valor en otra vista o módulo
Se guarda el valor en el estado de redux
Si por algún motivo el cambio del valor en redux es lento (en el caso de Rayen debido al persist que persiste los datos por medio de indexDB) se utiliza el estado de react antes mencionado y se guardan los datos al momento de sacar el focus del input, es decir en el onBlur


Pendiente de revisar: Re-renderización por immutable
