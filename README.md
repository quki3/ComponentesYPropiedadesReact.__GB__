Componentes y propiedades
Los componentes permiten separar la interfaz de usuario en piezas independientes, reutilizables y pensar en cada pieza de forma aislada. Esta página proporciona una introducción a la idea de los componentes
Conceptualmente, los componentes son como las funciones de JavaScript. Aceptan entradas arbitrarias (llamadas “props”) y devuelven a React elementos que describen lo que debe aparecer en la pantalla.
Componentes funcionales y de clase
La forma más sencilla de definir un componente es escribir una función de JavaScript:

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
Esta función es un componente de React válido porque acepta un solo argumento de objeto “props” (que proviene de propiedades) con datos y devuelve un elemento de React. Llamamos a dichos componentes “funcionales” porque literalmente son funciones JavaScript.

También puedes utilizar una clase de ES6 para definir un componente:

class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
Los dos componentes anteriores son equivalentes desde el punto de vista de React.
Renderizando un componente
Anteriormente, sólo encontramos elementos de React que representan las etiquetas del DOM:

const element = <div />;
Sin embargo, los elementos también pueden representar componentes definidos por el usuario:

const element = <Welcome name="Sara" />;
Cuando React ve un elemento representando un componente definido por el usuario, pasa atributos JSX e hijos a este componente como un solo objeto. Llamamos a este objeto “props”.
Por ejemplo, este código muestra “Hello, Sara” en la página:
{/*PRIMERO??*/}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
Nota: Comienza siempre los nombres de componentes con una letra mayúscula.

Composición de componentes
Los componentes pueden referirse a otros componentes en su salida. Esto nos permite utilizar la misma abstracción de componente para cualquier nivel de detalle. Un botón, un cuadro de diálogo, un formulario, una pantalla: en aplicaciones de React, todos son expresados comúnmente como componentes.
Por ejemplo, podemos crear un componente App que renderiza Welcome muchas veces:
{/*SEGUNDO??*/}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

Extracción de componentes
No tengas miedo de dividir los componentes en otros más pequeños.

Por ejemplo, considera este componente Comment:
{/*TERCERO??*/}
function Commen(props) {
  return (
    <div className="Commen">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Commen-text">
        {props.text}
      </div>
      <div className="Commen-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

Primero, vamos a extraer Avatar:
{/*EXTRAER AVATAR??*/}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.avatarUrl}
      alt={props.name}
    />
  );
}

Ahora podemos simplificar Commen un poquito:

function Commen(props) {
  return (
    <div className="Commen">
      <div className="UserInfo">
        <Avatar />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Commen-text">
        {props.text}
      </div>
      <div className="Commen-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

A continuación, vamos a extraer un componente UserInfo que renderiza un Avatar al lado del nombre del usuario:
{/*EXTRAER USER_INFO??*/}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}

Esto nos permite simplificar Comment aún más:

function Commen(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

Las props son de solo lectura
Ya sea que declares un componente como una función o como una clase, este nunca debe modificar sus props. Considera esta función sum :
function sum(a, b) {
  return a + b;
}
Tales funciones son llamadas “puras” porque no tratan de cambiar sus entradas, y siempre devuelven el mismo resultado para las mismas entradas.

En contraste, esta función es impura porque cambia su propia entrada:

function withdraw(account, amount) {
  account.total -= amount;
}
React es bastante flexible pero tiene una sola regla estricta:

Todos los componentes de React deben actuar como funciones puras con respecto a sus props.