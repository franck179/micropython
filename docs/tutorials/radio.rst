Radio
-----

L'interaction à distance ressemble à de la magie.

La magie peut être utile si tu es un elfe, un sorcier ou une licorne, mais ces
choses n'existent que dans les histoires.

Cependant, il y a quelque chose de beaucoup mieux que la magie : la physique !

L'interaction sans fil n'est que de la physique : les ondes radio (une sorte de
radiation électromagnétique, un peu comme la lumière visible) ont certaines
propriétés (comme l'amplitude, la pulsation ou la période) modulées par un
émetteur de façon à ce que cette information puisse être encodée et ainsi diffusée.
Lorsque des ondes radio rencontrent un conducteur électrique (c'est-à-dire une
antenne), elles provoquent l'apparition d'un courant alternatif duquel
l'information contenue dans les ondes peut être extraite et retraduite dans sa
forme originale.

Couches après couches
+++++++++++++++++++++

Si tu te rappelles bien, les réseaux sont fabriqués en couches.

Le besoin le plus fondamental d'un réseau sont des sortes de chemins pour
qu'un signal se rende d'un appareil à un autre. Dans notre tutoriel sur les
réseaux nous utilisions des câbles connectés au pins d'E/S. Grâce au module radio
on peut se passer des câbles et utiliser la physique résumée plus haut comme
l'invisible connexion entre les appareils.

La couche suivante dans la pile réseau est également différente par rapport à
notre exemple dans le tutoriel sur les réseaux. Dans l'exemple câblé on utilisait
le *on* et le *off* digital (1 ou 0) pour envoyer et recevoir un signal. Avec le
module radio inclu sur le micro:bit la plus petite partie utilisable d'un signal
est l'octet.

Octets
++++++

Un octet est une unité d'information qui est (habituellement) constituée de huit
bits. Un bit est la plus petite unité d'information possible puisqu'il ne peut
être que dans deux états : éteint ou allumé (*on* ou *off* ~ 1 ou 0)

Les octets fonctionnent comme des sortes d'abaques : chaque position dans l'octet
est comme une colonne dans un abaque - elle représente un nombre qui lui est
associé. Dans un abaque il y a habituellement une colonne pour les milliers, une
pour les centaines, une pour les dizaines et une pour les unités. Dans un octet
la position la plus à gauche représente 128, puis viennent 64, 32, 16, 8, 4, 2 et
1. Au fur et à mesure que les bits (les signaux on/off) sont envoyés dans l'air,
ils sont recombinés en octets par le récepteur.

As-tu repéré le schéma ? ( Indice: base 2.)

En ajoutant les nombres associés aux positions mises sur *on* dans un octet on
peut représenter tous les nombres entre 0 et 255. L'image ci-dessous montre
comment ça marche avec cinq bits pour compter de 0 à 31:

.. image:: binary_count.gif

Si on se met d'accord sur ce que représente chacun des 256 nombres (encodé par
un octet) ~ comme des caractère par exemple ! ~ alors on peut commencer à
envoyer du texte, un caractère par octet.

Et en fait, certains `y ont déjà pensé <https://en.wikipedia.org/wiki/ASCII>`_  !
Utiliser des octets pour encoder et décoder de l'information est très habituel.
Cela correspond à peu près à la couche de protocole du code Morse dans notre
exemple de réseau câblé.

Une super série d'explications claires à destination des enfants (et des enseignants)
sur "tout est octet" peut être trouvée sur le site web de `CS unplugged <http://csunplugged.org/binary-numbers/>`_

Addressage
++++++++++

Le problème avec les ondes radio c'est qu'on ne peut pas transmettre directement
à une personne. Toute personne avec l'antenne appropriée peut recevoir les messages
que tu envoies. Il est donc important d'être capable de déterminer qui devrait
recevoir les diffusions.

La façon dont est conçu le module radio dans le micro:bit règle le problème de
façon assez simple:

* Il est possible de régler la radio sur des canaux différents (numérotés 0-100).
Ça marche exactement comme les Talkie-Walkie d'enfants : tout le monde se met sur le même
canal et tout le monde entend ce que quiconque émet sur ce canal. Tout comme
avec les Talkie-Walkie, il peut y avoir de petites interférences entre canaux
adjacents.
* Le module radio te permet de préciser deux informations: une adresse et un
groupe. L'adresse est comme une adresse postale tandis qu'un groupe est comme un
destinataire spécifique à cette adresse. La chose importante est que la radio va
filtrer les messages qu'elle reçoit qui ne correspondent pas à *ton* adresse et à
*ton* groupe. Il faut donc préparer l'adresse et le groupe que ton application va
utiliser.

Bien sûr, le micro:bit reçoit aussi les messages diffusés pour les autres
combinaisons d'adresse et de groupe. Mais ce qui est important c'est que tu n'as
pas besoin de t'occuper de les filtrer. Néanmoins, si quelqu'un était suffisament
intelligent, il pourrait lire * tout le trafic du réseau sans fils* quelque soit
la cible adresse/groupe à laquelle il est destiné. Dans ce cas il est indispensable
d'utiliser des moyens de communication cryptés pour que seul le destinataire
souhaité soit capable de lire le message diffusé. La cryptographie est un sujet
fascinant mais, malheureusement, au-delà de la portée de ce tutoriel.

Lucioles
+++++++++

Ceci est une luciole:

.. image:: firefly.gif

C'est une sorte d'insecte qui utilise la bioluminescence pour signaler (sans fil)
sa présence à ses amis. Voilà ce que ça donne lorsqu'ils communiquent:

.. image:: fireflies.gif

La BBC a `une  video plutôt sympa <http://www.bbc.com/earth/story/20160224-worlds-largest-gathering-of-synchronised-fireflies>`_
de lucioles disponible en ligne.

Nous allons utiliser le module radio pour créer quelque chose se rapprochant d'un
essaim de lucioles émettant des signaux les unes vers les autres.

Tout d'abord on ``import radio`` pour rendre les fonctions disponibles pour ton
programme en Python. Ensuite on fait appel à la fonction ``radio.on()`` pour
allumer la radio. Puisque la radio consomme de l'énergie et de la mémoire on a
fait en sorte que *tu* décides quand elle est allumée ou pas (il y a, évidemment,
une fonction ``radio.off()``).


A ce moment là, le module radio est configuré avec des paramètres par défaut qui
lui permettent d'être compatible avec d'autres plateforme qui pourrait vouloir
interagir avec le BBC micro:bit. Il est possible de contrôler plusieurs des
caractéristiques évoquées plus haut (comme les canaux et l'adressage) ainsi que
la quantité d'énergie utilisée pour la diffusion des messages et la quantité de
RAM que les message entrant dans la queue occupent. La documentation de l'API
contient toutes les informations dont tu as besoin pour configurer ta radio selon
tes besoins.

En supposant que nous soyons contents des réglages par défaut, le moyen le plus
simple d'envoyer un message est comme ceci::

    radio.send("un message")

L'exemple utilise la fonction ``send`` pour simplement diffuser la chaîne de
caractères "un message". Recevoir un message est encore plus simple::

    nouveau_message = radio.receive()

Au fur et à mesure que les messages arrivent, il sont mis dans une file de
messages. La fonction ``receive`` renvoie le plus ancien message de la queue sous
la forme d'une chaîne de caractères, faisant ainsi de la place pour les nouveaux
messages entrant. Si la queue de messages est pleine, alors les nouveaux messages
entrant sont ignorés.

C'est vraiment tout ce que ça fait! (Bien que le module radio soit également
suffisament puissant pour te permettre d'envoyer n'importe quel type de données,
pas seulement des chaînes de caractères. Regarde la documentation de l'API pour
savoir comment ça marche.)

Armé de ces connaissances, il est facile de faire des lucioles micro:bit comme ça::

    import radio
    import random
    from microbit import display, Image, button_a, sleep

    # Création de la liste "flash" contenant les images de l'animation
    # Comprends-tu comment ça fonctionne ?
    flash = [Image().invert()*(i/9) for i in range(9, -1, -1)]

    # La radio ne marchera pas sauf si on l'allume !
    radio.on()

    # Boucle événementielle.
    while True:
        # Le bouton A envoie un message "flash"
        if button_a.was_pressed():
            radio.send('flash')  # a-ha
        # On lit tous les messages entrant
        incoming = radio.receive()
        if incoming == 'flash':
            # Si il y a un message "flash" entrant
            # on affiche l'animation du flash de luciole après une petite
            # pause de durée aléatoire.
            sleep(random.randint(50, 350))
            display.show(flash, delay=100, wait=False)
            # On re-diffuse aléatoirement le message flash après une petite
            # pause
            if random.randint(0, 9) == 0:
                sleep(500)
                radio.send('flash')  # a-ha

Les choses importantes se passent dans la boucle événementielle. Tout d'abord,
on vérifie si le bouton A a été pressé et, si c'est le cas, on utilise la radio
pour envoyer le message "flash". Puis on lit tous les messages de la queue avec
``radio.receive()``. Si il y a un message, on dort pendant un court instant
aléatoire (pour rendre l'affichage plus intéressant) et on utilise
``display.show()`` pour animer le flash de la luciole. Enfin, pour rendre le tout
un peu excitant, on choisit un nombre au hasard de sorte qu'il y ait une chance
sur dix de rediffuser le message "flash" à tous le monde (c'est ce qui rend
possible la propagation de l'affichage des lucioles sur plusieurs appareils).
Si la re-diffusion est choisie alors on attend une demie seconde (pour être sûrs
que l'affichage initial du message flash soit terminé) avant de renvoyer le
signal "flash". Puisque ce code est inséré dans une instruction ``while True``,
il se répète en boucle indéfiniment.

Le résultat final (en utilisant un groupe de micro:bit) devrait ressembler à ça::

.. image:: mb-firefly.gif

.. footer:: The image of binary counting is released under the licensing details listed here: https://en.wikipedia.org/wiki/File:Binary_counter.gif
