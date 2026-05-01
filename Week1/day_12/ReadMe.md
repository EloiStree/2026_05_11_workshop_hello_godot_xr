<img width="583" height="122" alt="image" src="https://github.com/user-attachments/assets/1665c8c3-bca5-4154-a8c2-756ebec15b4e" />


# **Matin : Continuation**

La vidéo de Brackeys est essentielle : animation, collisions, son, personnage… tout y passe.
Même en passant de la 2D à la 3D, vous retrouverez les mêmes concepts et une nomenclature similaire.

Prenons toute la matinée pour avancer le plus loin possible dans cette vidéo.

---

# **Après-midi : Input et InputMap**

Concentrons-nous davantage sur la gestion des inputs dans Godot.
Nous allons explorer deux approches : `_unhandled_input` et les **InputMap**.

Observons leur fonctionnement, à la fois pour le clavier et pour une manette.

---

# **Fin de journée : Input NES**

Mettons en pratique en ajoutant une manette NES à votre jeu.

* Utilisez les **InputMap** si vous débutez ou n’avez jamais vraiment codé.
* Utilisez `_unhandled_input` si vous avez déjà de l’expérience en programmation.

Les InputMap sont pratiques, mais ils ne s’exportent pas automatiquement entre projets ou vers une boîte à outils.
À l’inverse, `_unhandled_input` est plus flexible… mais peut poser des problèmes de compatibilité entre systèmes d’exploitation.
