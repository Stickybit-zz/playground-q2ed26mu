# Méthodes du cycle de vie React : render et ComponentDidMount

Quand nous parlons des méthodes de cycle de vie dans React.js on fait référence à la méthode <b>render()</b> la plupart du temps. JSX est utilisé quand un composant React doit afficher des données. React utilise JSX pour créer des modèles plutôt que du JavaScript classique.

# Méthode render()

<b>render()</b> est la méthode la plus utilisée pour tout composant alimenté par React qui retourne un JSX avec des données back-end. la fonction <b>render()</b> est considérée comme une fonction normale mais en réalité elle doit toujours retourner quelque chose. Lorsque le fichier composant est appelé, il appelle par défaut la méthode <b>render()</b> parce que ce composant doit afficher le balisage HTML (qu'on peut qualifier de syntaxe JSX).

```javascript
import React, { Component } from 'react';


class App extends Component {
  render() {
    return (
      <div>
          <h1 className="App-title">Welcome to React</h1>
      </div>
    );
  }
}

export default App;
```
Notez qu'il faut retourner quelque chose, s'il n'y a pas de <b>JSX</b> pour le retour alors null fera l'affaire. Quoi qu'il en soit il faut renvoyer quelque chose. Dans ce scénario, vous pouvez faire ça :

```javascript
import { Component } from 'react';


class App extends Component {
  render() {
    return null;
  }
}

export default App;
```

<b>Souvenez-vous, vous ne pouvez pas définir <b>setState()</b> dans la fonction <b>render()</b>. Pourquoi ?</b>
Parce que la fonction <b>setState()</b> change l'état de l'application qui, par conséquent, déclenche l'appel de la fonction <b>render()</b>. Donc si vous écrivez quelque chose comme ceci, vous entrerez dans une boucle infinie et l'application crashera.

Vous pouvez définir certaines variables, effectuer certaines opérations dans la fonction <b>render()</b>, mais n'utilisez jamais la fonction setState. D'une manière générale, souvent on affiche la valeur de certaines variables dans la méthode <b>render()</b>. C'est la fonction qui fait appel aux méthodes de montage du cycle de vie.

# La méthode componentDidMount()

Comme son nom l'indique, cette méthode est appelée une fois que tous les éléments de la page sont rendus correctement. Une fois le balisage défini sur la page. Cette méthode est appelée par React lui-même, soit pour récupérer les données depuis une API externe, soit pour effectuer des opérations uniques qui nécessitent des éléments <b>JSX</b>.

La méthode <b>componentDidMount()</b> est l'endroit parfait pour appeler la méthode <b>setState()</b> afin de changer l'état de l'application tandis que <b>render()</b> se charge des données JSX mise à jour. Par exemple, si nous récupérons toutes les données d'une API, alors l'appel à l'API doit être placé dans cette méthode du cycle de vie. Une fois la réponse obtenue, nous pouvons appeler la méthode <b>setState()</b> et rendre l'élément avec les données mises à jour.

```javascript
import React, { Component } from 'react';

class App extends Component {

  constructor(props){
    super(props);
    this.state = {
      data: 'Jordan Belfort'
    }
  }

  getData(){
    setTimeout(() => {
      console.log('Our data is fetched');
      this.setState({
        data: 'Hello WallStreet'
      })
    }, 1000)
  }

  componentDidMount(){
    this.getData();
  }

  render() {
    return(
      <div>
      {this.state.data}
    </div>
    )
  }
}

export default App;
```

Dans l'exemple ci-dessous j'ai simulé un appel à une API via la fonction setTimeOut puis je récupère les données. Une fois le composant rendu correctement, la fonction <b>componentDidMount()</b> est appelée qui enchaine l'appel à la fonction <b>getData()</b>.

# La méthode componentWillMount()

La méthode <b>componentWillMount()</b> est la méthode de cycle de vie la moins utilisée, elle est appelée avant que tout élément HTML soit rendu. Si vous voulez voir un aperçu, jetez un coup d'oeil à l'exemple précédent, il suffit d'ajouter une méthode de plus :

```javascript
import React, { Component } from 'react';

class App extends Component {

  constructor(props){
    super(props);
    this.state = {
      data: 'Jordan Belfort'
    }
  }
  componentWillMount(){
    console.log('First this called');
  }

  getData(){
    setTimeout(() => {
      console.log('Our data is fetched');
      this.setState({
        data: 'Hello WallStreet'
      })
    }, 1000)
  }

  componentDidMount(){
    this.getData();
  }

  render() {
    return(
      <div>
      {this.state.data}
    </div>
    )
  }
}

export default App;
```

Si vous regardez attentivement la sortie console, il est d'abord affiché "First this called" et ensuite notre état initial est défini, puis la méthode <b>render()</b> est appelée, ensuite la méthode <b>componentDidMount()</b> est appelée pour qu'enfin les données nouvellement récupérées soient affichées dans le composant.

## L’ordre d’appel des méthodes

1. componentWillMount()
2. définir l'état initial dans le constructeur
3. render()
4. componentDidMount()
5. setState()
6. render()

L'exemple suivant est un cas d'utilisation de <b>componentDidMount()</b>. Les exécutions sont effectuées dans l'ordre ci-dessus. Au bout d'une seconde, l'état change et la mise à jour s'affiche.

L'état initial est "Jordan Belfort" mais après 1 seconde il est mis à jour avec "Hello WallStreet".

# Démonstration pratique

@[Exemple d'application React]({"stubs": ["src/app/app.jsx", "src/main.js"], "command": "./run.sh"})