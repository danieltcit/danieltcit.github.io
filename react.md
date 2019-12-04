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

* Siempre punto y coma al final

```js
  const variable = 3;
```

* Una línea al final de cada archivo

```js
...
...
export default MiComponente; //Line Break

```

* Los archivos al principio deben encontrarse las librerias oficiales y luego las importaciones locales

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

* Comentarios en inglés

```js
// There must be a better way
if(true){
  return false;
} else {
  return true;
}
```

* Funciones van afuera de la funcion principal, si se ocupa dentro de otras screens, va en utils functions.

```js

```

* Componentes en carpetas y dentro de ella un archivo index. De tener sub-componentes dentro de este componente se ha crear una nueva carpeta con su respectivo `index.js`.
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
* Los nombres de las carpetas han de ser de general a específico:
```
|_ components
   |_ Button
      |_ SmallButton
         |_ index.js
      |_ FabButton
         |_ index.js
      |_ index.js
```

* Procurar no usar clases si no es necesario

```javascript
// Prefer
const Component () => {
   return <Things />;
};
// Instead of
class Component extends React.Component {
  render(){
    return <Things />;
  }
};

```



* Las acctiones deben ser bindeadas utilizando en general,  con .bind(this) ya que el bind con arrow function es hasta 10 veces más lento.

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

* Otra alternativa es utilizar hooks (@diego completa porfa)

```js

```


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
