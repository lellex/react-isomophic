# Comment mettre en place le server-side rendering avec React ?

:warning: Article en cours de rédaction :warning:

On parle beaucoup de l'isomorphisme (ou server-side rendering) avec React ou Angular, mais comment le mettre en place concrètement ?

* Premier pré-requis : un serveur avec Node.js, nous utiliserons ici Heroku.

Avec React, ce qui risque d'être le plus impactant dans notre application sera :
* la génération du bundle : la configuration webpack devra être adaptée, le chargement des assets et des styles également.
* l'utilisation des routes avec [react-router](https://reacttraining.com/react-router/web/guides/server-rendering) - car le serveur va devoir déclarer ces routes pour les rendre accessibles avant que le javascript ne reprenne la main. 
* [redux](http://redux.js.org/docs/recipes/ServerRendering.html), les données initiales du states devront être fournies par le serveur afin de pouvoir afficher une première fois l'application dans son état d'origine.

## À partir d'un nouveau projet

Le plus simple est évidemment de prévoir le fait d'avoir un rendu serveur au démarrage du projet.

Pour celà, il éxiste différents boilerplates clé-en-main qui gèrent le server-side-rendering. Ce n'est hélas pas encore le cas des projets générés avec le CLI create-react-app. Néanmoins, [une liste de projets alternatifs](https://github.com/facebookincubator/create-react-app#alternatives) est disponible sur leur repo.

![Next.js](images/next.png)

Le plus populaire est **[Next.js](https://github.com/zeit/next.js)**, il s'agit d'un "framework" combinant React et Node, fournissant tout le nécéssaire pour faire un rendu serveur (CLI, routes).

L'utilisation de Next.js est très simple, il suffit d'installer le package en question et d'utiliser le composant *Link* qu'il propose afin de gérer les routes. Next dispose de tutos très simples ainsi que son propre service de déployement : [Now](https://zeit.co/now).

[Démo ici](https://react-isomorphic-01.herokuapp.com/)


## À partir d'un projet existant

### Avec Next (le plus simple)

Il est aussi possible d'utiliser Next sur un projet existant. Prenons ici l'exemple d'un projet généré via create-react-app. Ce qui va changer :
* Plus besoin de webpack ou react-scripts, next pends la main sur la génération du bundle.
* Par conséquent, toute configuration "custom" de webpack devra être adaptée, il faudra repenser l'utilisation des loaders.
* Next embarque [style-jsx](https://github.com/zeit/styled-jsx) qui permet d'écrire le css dans les composants jsx. Vos css devront donc être soit directement dans le jsx, soit complétement indépendant de React et appelés via une feuille de style de manière classique avec un [custom document](https://github.com/zeit/next.js#custom-document).

[Démo ici](https://react-isomorphic-02.herokuapp.com/)

![express](images/express.png)

### Pas à pas avec express

Une nouvelle fois, nous partirons d'un projet généré à partir de create-react-app.

Si ce n'est pas déjà fait, on extrait les sources afin de pouvoir adapter la configuration.

```bash
create-react-app react-isomorphic
cd react-isomorphic
npm run eject
```

On installe les dépendances nécéssaires au serveur

```bash
npm i -D request single-child webpack-hot-middleware babel-cli
npm i -S express
```

## Sources

https://github.com/facebookincubator/create-react-app/pull/172