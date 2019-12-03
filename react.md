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
// Use
const Component () => {
   return <Things />;
}
// Instead of
class Component extends React.Component {
  render(){
    return <Things />;
  }
}

```



Acciones bindeadas (en general,  con .bind(this) ya que el bind con arrow function es 10 veces más lento)
Reductor como objeto
Usar createAction
Una acción múltiples sagas (No llamar a más de una saga en la misma línea de ejecución desde un componente)
Utilizar Chart.js para gráficos
Minimizar la cantidad de props al pasarlas al componente hijo



Uso de estados en Rayen:
Caso 1: Si un container tiene estados que se utilizan solo en el componente, se utiliza el estado del container
Para los containers que corresponden a clases se utiliza el estado de la clase
Cuando el container corresponde a un hook se utiliza el hook default de react useState
Caso 2: En caso que se utilice este valor en otra vista o módulo
Se guarda el valor en el estado de redux
Si por algún motivo el cambio del valor en redux es lento (en el caso de Rayen debido al persist que persiste los datos por medio de indexDB) se utiliza el estado de react antes mencionado y se guardan los datos al momento de sacar el focus del input, es decir en el onBlur


Pendiente de revisar: Re-renderización por immutable
