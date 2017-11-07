# Utiliser Chrome Dev Tools CPU Profiler ou Memory Profiler

## Prérequis:

- Chrome ou Chromium version au moins 55+
- nodejs version supérieur à 6.3

#### Quelques notes sur le chrome prérequis :

La dernière version de Chrome officiel (61.0.3163.100 sur ma machine) devrait fonctionner directement.

Mais pour un Chromium pas mis à jour en utilisant par example [ungoogled-chromium](https://github.com/Eloston/ungoogled-chromium), il faut suivre ces étapes:
- Activer le flag chrome #enable-devtools-experiments: chrome://flags/#enable-devtools-experiments
- Redémarrer Chromium
- Ouvrir les Dev Tools
- Aller dans les Settings des Dev Tools
- Catégorie Experiments
- Maintenir la touche 'Shift' enfoncée ou la taper 6 fois de suite
- Cocher "Node debugging"
- Se plaindre parce que cela peux ne pas marcher

![Activer les dev tools pour nodejs](https://github.com/antonin-lebrard/nodejs-profiling-tutos/blob/master/enableExperiments.gif)

## Profilling

En prenant pour exemple une application node `index.js`:
- Lancer node avec le parametre --inspect (docs [here](https://nodejs.org/api/cli.html#cli_inspect_host_port)) : 

```
$ node --inspect index.js
Debugger listening on port 9229.
```

- Si la ligne `Debugger listening on port 9229.` apparait, tout fonctionne pour node, maintenant on peut ouvrir chrome à l'URL chrome://inspect
- Si Chrome a bien été configuré, l'appli `index.js` devrait apparaitre dans 'Remote Target' avec l'icone ![image nodejs icon](https://nodejs.org/static/favicon.png) et son path formatté avec des `_`
- Cliquer sur `inspect` ouvrira une fenetre Dev Tools spécialisée pour node, dont les catégories qui nous intéressent sont 'Profiler' et 'Memory'

![Ouvrir les dev tools sur une appli exemple](https://github.com/antonin-lebrard/nodejs-profiling-tutos/blob/master/openNodeDevTools.gif)

### CPU Profiler

Correspond à la catégorie 'Profiler' des Dev Tools.

Fonctionne de la même manière que l'équivalent front-end 'Record Javascript CPU Profile' :

Cliquer sur 'Start' commencera la capture CPU et après quelques intéractions avec l'appli node, cliquer sur 'Stop' nous ouvrira une fenêtre nous donnant le temps d'execution pour chaque function, ainsi qu'un mode 'Chart' nous montrant un flame chart sur lequel on peut zoomer et intéragir pour aller au code correspondant (par example fonction anonyme)

![Utiliser le CPU Profiler](https://github.com/antonin-lebrard/nodejs-profiling-tutos/blob/master/recordCPU.gif)

### Memory Profiler

Un développeur Front-end aurait sans doute préferer utiliser la Timeline des Dev Tools pour cette utilisation, mais ce n'est pas disponible pour les Dev Tools nodejs.

Ce qui s'en rapproche le plus c'est l'option 'Record allocation profile' de la Catégorie 'Memory' des Dev Tools.

Même fonctionnement que le CPU Profiler, et même résultat visuel.

Mais attention à une petite confusion, là où le CPU Profiler utilise le temps en abscisse, le Memory Profiler n'utilise pas le temps, et mets à plat l'utilisation de la mémoire par les différentes fonctions. La courbe qui ressemble à une timeline au dessus montre bien l'évolution de la mémoire dans le temps, mais elle semble suivre une échelle variable.

![Utiliser le Memory Profiler](https://github.com/antonin-lebrard/nodejs-profiling-tutos/blob/master/recordMemory.gif)

## Notes

L'idéal serait d'avoir l'utilisation du CPU et de la mémoire associées, mais cela ne semble pas possible avec les Dev Tools actuels

Des liens utiles pour aller plus loin :

- https://github.com/ChromeDevTools/awesome-chrome-devtools <= all kinds of good ressources here
- https://github.com/thlorenz/v8-perf/issues/4
- https://github.com/node-inspector/node-inspector
- https://github.com/node-inspector/v8-profiler
- https://github.com/tomgco/cpu-profiler
- https://github.com/tomgco/chrome-cpu-profiler
- https://github.com/deepak1556/node-memwatch
