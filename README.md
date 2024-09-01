## ¿Qué es Heroku? existen otras alternativas? hazme una tabla si existen similares, con los beneficios y desventajas que te da cada uno de ellos y que tenemos que tener en cuenta para la elección.

Según su página web “Heroku es una plataforma en la nube basada en contenedores como servicio (PaaS). Los desarrolladores utilizan Heroku para implementar, administrar y escalar aplicaciones modernas.”  Esto significa que podemos crear nuestros servidores de aplicaciones en la nube y administrarlos mientras los desplegamos a la vez para el uso por parte de los usuarios. Si creamos nuestra aplicación y enlazamos nuestro código de GitHub con el servicio de Heroku, la aplicación quedaría desplegada para su uso. En cuanto a la escalabilidad, nos brinda la ventaja de no estar preocupados por si nuestro servidor se ha quedado pequeño para el volumen de datos y usuarios, porque resulta más sencillo el escalado en plataformas en la nube que en servidores físicos.

Lamentablemente ahora es de pago desde que se hicieron las guías, así que habrá que buscar otras opciones.

Alternativas:

|**Nombre**|**Descripción**|**Ventajas**|**Desventajas**|
| :-: | :-: | :-: | :-: |
|Digital Ocean|Desarrollo e implementación para pymes y startups.|<p>Seguridad, disponibilidad de plataforma cercana al 100%. </p><p>Precios interesantes.</p>|Se requiere experiencia y poco soporte. |
|Vercel|<p>Desarrollo en equipo.</p><p>Gratis como hobby.</p>|Rápida y fácil gestión en grupos. Despliegue fluido con React.|<p>Versión de pago 20€ por usuario. </p><p>Limitada en aplicaciones complejas.</p>|
|Render|<p>Uso sencillo.</p><p>Gratuita para uso personal</p>|<p>Fácil de usar</p><p></p>|Menos documentación.|
|Firebase|Para el desarrollo de aplicaciones startups.|Escalable y BBDD en tiempo real.|Limitación de gratuidad según el tamaño de la aplicación.|
|Google Cloud|Sitios web simples o aplicaciones complejas (modular). IA y aprendizaje automático.|<p>Escalable y segura.</p><p>Integración con apps de Google.</p>|Aprendizaje exigente. Planes de pago basado en cuotas de espacio|
|AWS|Para aplicaciones de alto rendimiento y escalables.|Segura y flexible, prácticamente para el desarrollo de cualquier tipo de aplicación.|Complejo para principiantes. Difícil planificar costos.|

Estas son una muestra de las principales opciones, Render por su facilidad de uso y precio, puede ser una de las principales opciones en mi caso.




## ¿Qué es Redux? Quiero que me expliques que es, que beneficios tiene, cuando lo utilizarás, porque lo utilizarás, problemas comunes(riesgos). pon ejemplos

Una librería de JS que nos permite manejar el estado de la aplicación de manera centralizada con el almacenamiento en el Store y despreocuparnos de los estados descentralizados de cada componente. Lo que nos facilita el uso de cada dato sin tener que preocuparnos de jerarquías o traslado de datos entre componentes. Los cambios de estos estados se harían mediante funciones llamadas Reducers que tienen parametrizada una acción.

El principal beneficio es en la facilidad de manejo del código y su depuración. Dado que queda más simplificado, y su uso es recomendado en aplicaciones con muchos componentes y que necesiten sincronización, lo que facilitará su escalabilidad.

Puede haber problemas de rendimiento o ser contraproducente su uso en aplicaciones pequeñas por generar código innecesario.

En la guía hemos realizado el siguiente ejemplo con la api swapi.

```
//Acciones 
const SET_RECENT_POSTS = 'SET_RECENT_POSTS'; 
const SET_RESULTS_POSTS = 'SET_RESULTS_POSTS'; 

 
//Reducer: 
import { 
    SET_RECENT_POSTS, 
    SET_RESULTS_POSTS 
} from '../actions/types'; 

const INIT_STATE = { 
    resultsPosts: [], 
    recentPosts: [] 
} 

export default function (state = INIT_STATE, action) { 
    switch (action.type) { 
        case SET_RECENT_POSTS: 
            const recentPosts = action.payload 
            return { 
                ...state, 
                recentPosts 
            } 

        case SET_RESULTS_POSTS: 
            const resultsPosts = action.payload; 
            return { 
                ...state, 
                resultsPosts 
            } 

        default: 

            return state; 
    } 
} 
 
// Despachar la acción 
import { 
  SET_RECENT_POSTS, 
  SET_RESULTS_POSTS 
} from './types'; 


export function fetchRecentPosts() { 
  return function (dispatch) { 
    axios.get('https://swapi.dev/api/starships') 
      .then(response => { 
        dispatch({ 
          type: SET_RECENT_POSTS, 
          payload: response.data.results 
        }) 
      }).catch(error => { 
        console.log("mis errores", error); 
      }) 
  } 
} 

export function fetchPostsWithQuery(query, callBack) { 
  return function (dispatch) { 
    axios.get(`https://swapi.dev/api/starships/?search=${query}`) 
      .then(response => { 
        dispatch({ 
          type: SET_RESULTS_POSTS, 
          payload: response.data.results 
        }) 
        if(callBack){callBack()} 
      }) 
  } 
} 

//Store 
import { Provider } from "react-redux"; 
import { createStore, applyMiddleware, compose } from "redux"; 
import reducers from "./reducers"; 
import { thunk } from "redux-thunk"; 
 
const createStoreWithMiddleware = applyMiddleware(thunk)(compose((window.devToolsExtension ? window.devToolsExtension() : f => f)(createStore))); 

import "./style/main.scss"; 
import Home from './components/home'; 
import Results from "./components/results"; 

 
function main() { 
  ReactDOM.render( 
    <Provider store={createStoreWithMiddleware(reducers)}> 
      <BrowserRouter> 
        <Switch> 
          <Route path='/' exact component={Home} /> 
          <Route path='/results' component={Results} /> 
        </Switch> 
      </BrowserRouter> 
    </Provider>, 
    document.querySelector(".app-wrapper") 
```



¿Qué es un HOC? Quiero que me expliques que es, que beneficios tiene, cuando lo utilizarás, porque lo utilizarás, problemas comunes(riesgos). pon ejemplos

Es uns función que recibe un componente y devuelve un componente con otra capa o funciones adicionales (mejorado). 

El principal beneficio es reutilizar la lógica común en diferentes componentes sin generar código duplicado. También se separa la lógica de la presentación, lo que resulta más limpio. 

Y las desventajas pueden estar reclionadas con la pérdida de referencias en props, o tener un peor rendimiento unido a la complejidad de lectura si se generan demasiado de manera anidada. 

El ejemplo típico de uso es cuando varios componentes necesitan autentificarse. Es lógica puede estar separada de cada uno de ellos. 

En la guía hicimos uno en nuestro componente SearchBar para acceder a las propiedades del enrutador. 
```
import { withRouter } from 'reactor-router-dom';   
...
SearchBar = withRouter(SearchBar); 
```