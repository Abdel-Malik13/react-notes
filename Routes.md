Fonctionnement des routes avec **React Router DOM** :

1. Routage de navigation de base
2. Routage imbriqué
3. Routage imbriqué avec des paramètres de chemin
4. Routage protégé

**React Router basique :**

Ci-dessous un exemple d'une route de base pour une application (sans sous-route)

<Router>
    <Route exact path="/" component={Home} />
    <Route path="/category" component={Category} />
    <Route path="/login" component={Login} />
    <Route path="/products" component={Products} />
</Router>

Router :
Pour configurer nos routes, nous avons tout d'abord besoin d'un composant qui
permettra d'englober notre application pour permettre aux autres pages de l'application
de pouvoir utiliser les composants Route, Link etc.

ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>,
    document.getElementById('root)
);

Il faut placer le composant <BrowserRouter> (qui est notre Routeur) le plus haut dans l'application.
Ici j'ai importé <BrowserRouter> dans index.js (qui est le composant le plus haut), avec lequel j'ai englober le composant <App>
qui est le point d'entrée de notre application. Ainsi le composant App et et ses enfants auront accès au éléments Route, Link etc.

Note : un composant Routeur (BrowserRouter) ne peut avoir qu'un seul élément enfant. Cet élément enfant peut-être
un élément HTML (tel que div) ou un composant React (comme ici avec <App>).

Le code ci-dessus va créé une instance de 'history' (historique) pour l'ensemble de notre composant <App>.

History :

History est une bibliothèque JavaScript qui nous permet de gérer facilement l'historique des sessions partout où JavaScript est exécuté.
History fourni une API minimale nous permettant de gérer la pile d'historique, de naviguer, de confirmer la navigation et de conserver l'état
entre les sessions.

Chaque composant de routeur crée un objet 'history' qui garde la trace de l'emplacement actuel (history.location) et des emplacements précédents dans une pile.
Lorsque l'emplacement actuel change, la vue est restituée et nous obtenons une impression de navigation.

Comment change l'emplacement actuel ? L'objet 'history possède des méthodes telles que 'history.push()' et 'history.replace()' pour s'en occuper.
'history.push() est appelé lorsque nous cliquons sur un composant <Link> et 'history.replace()' est appelé lorsque nous utilisons <Redirect>.
D'autres méthodes, telles history.goBack() et history.goForward() permettent de naviguer dans l'historique en retournant en arrière ou en avant.


Links et Routes :

Le composant <Route> est le composant le plus important de React router. Il rend certaines interfaces utilisateurs si l'emplacement actuel correspond au chemin (path) de la route.
Idéalement, un composant <Route> doit avoir une propriété nommée 'path', et si le pathname correspond (match) avec l'emplacement actuel, il est rendu.

Le composant <Link>, d'autre part, permet de naviguer entre les pages. Il est comparable à l'élément d'ancrage HTML (<a>). Cependant, l'utilisation de liens d'ancrage entraînerait une actualisation du navigateur, ce que nous ne souhaitons pas.
A la place, nous pouvons utiliser <Link> pour naviguer vers une URL particulière et afficher la vue sans actualisation du navigateur.


Demo 1 : Routing de base

const Home = () => (
    <div>
        <h2>Home</h2>
    </div>
)

const Category = () => (
    <div>
        <h2>Category</h2>
    </div>
)

const Products = () => (
    <div>
        <h2>Products</h2>
    </div>
)

Ici je créer 3 composants d'interface utilisateur, ce nos pages qui seront afficher lorsqu'ils correspondront au path de l'url. Par exemple, si l'url est '/category' alors le composant (la page) Category sera afficher.

Ensuite dans notre application nous avons des url pour naviguer entre les différentes pages, pour faire cela nous avons besoin de <Link>

<ul>
    <li><Link to="/">Home</Link></li>
    <li><Link to="/category">Category</Link></li>
    <li><Link to="/products">Products</Link></li>
</ul>

Ensuite, les composants de route son rendus si la propriété 'path' correspond à l'URL actuelle

<Route path="/" component={Home} />
<Route path="/category" component={Category} />
<Route path="/products" component={Products} />

En gros, dans notre application nous avons une navigation comme une page HTML standard avec la balise <a> sauf que là c'est <Link>.
Donc quand l'utilisateur clique sur <li><Link to="/category">Category</Link></li> la barre d'adresse de son navigateur devient monapp.com/category
la Route prend le relais et vérifie si dans son path il y a un '/category', si c'est le cas, il va afficher le composant qui est définini dans component={Category},
ici si ça correspond c'est le composant Category qui sera afficher.

Dans le composant <App> nous avons déclarer ces routes de bases car c'est le point d'entrée de notre application, ensuite pour chaque sous-routes il faudra les 
déclarer dans le composant parent de ces sous-routes.

Ici, / correspond à / et /category. Par conséquent ces deux routes seront matcher et rendus. Comment éviter que deux composants ne soient rendu en même temps ?
Pour cela nous devons passer 'exact = {true}' comme propriété à notre route qui possède le 'path='/''.

<Route exact={true} path="/" component={Home} />

Si on veut qu'une route soit rendu uniquement si le path est exactement le même, nous devons utiliser la propriété 'exact'.

ROUTAGE IMBRIQUE

Pour créer des routes imbriquées, nous devons mieux comprendre comment fonctionne <Route>.

<Route> a trois propriétés que nous pouvons utiliser pour définir ce qui est rendu :

* component. Nous l'avons déjà vu en action, lorsque l'URL correspond, le routeur crée un élément React à partir du composant donné en utilisant React.createElement.
* render. C'est pratique pour le rendu en ligne. La propriété render attend une fonction qui renvoie un élément lorsque l'emplacement (location) correspond au path de la route.
* children. La propriété children est similaire à render en ce sens qu'elle attend une fonction qui retourne un élément React. Cependant, les enfants sont rendus indépendamment du fait que le chemin correspond ou non à l'emplacement (location).


Path et match
