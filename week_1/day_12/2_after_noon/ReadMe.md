<img width="512" height="256" src="objectif.svg" alt="Objectif of the day"/>

**Objectif :** Apprendre à utiliser `_unhandled_input` et `InputMap`

Le but ici n’est pas de tout maîtriser, mais de comprendre le principe des inputs dans Godot et de créer des outils via les signaux pour les écouter.
On créera ensuite un simulateur de manette NES.

---

**Checklist :**

* [ ] Savoir écouter un clavier avec `_input`
* [ ] Comprendre la différence entre `_unhandled_input` et `_input`
* [ ] Écouter un bouton avec `InputMap`
* [ ] Écouter un trigger avec `InputMap`
* [ ] Gérer un axe 2D (gauche / droite) avec `InputMap`
  * [ ] Écouter un joystick (`Vector2`) avec `InputMap`
* [ ] Écouter un joystick (`Vector2`) avec `_unhandled_input`
* [ ] Écouter les boutons de la souris
  * [ ] Bonus : écouter le mouvement de la souris (delta)

---

Les inputs… ce n’est pas si simple en réalité.

C’est pour cela que, sur Unity, des outils comme *Rewired* ont été créés, puis repris dans le *Input Action System*.

L’idée est d’arrêter de dépendre directement du hardware en définissant des **actions** à écouter, puis de laisser le designer décider à quoi ces actions correspondent.

En résumé : un système d’actions.

Mais pour un prototype, surtout au début d’un projet, il est parfois plus simple et plus rapide d’écouter directement les inputs de base.

