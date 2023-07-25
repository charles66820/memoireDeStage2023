#

## Consignes

* analyse du stage
* clairement expliquer :
  + les objects
    - ce que j'avais à faire
  + le déroulement
    - Ce que j'ai fait (comment et pourquoi)
    - montré la difficulté du travail (dire que j'ai eu du mal ?)
    - montré que j'utilise mes connaissances et ce que j'ai vue en cours en M1 + premier semestre de M2
* Faire des citation
* une 30aine de pages
* parlé de la machine qui crash quand il y a trop de charge

* Petite description de INRIA
* Petite description d'Atos (Eviden)
* Environnement de travail immédiat (bureau dans l'open space TADaaM)
  + réunion de suivie du stage toute les semaine
* Le cadre :
  + Objectifs du service :
    - TADaaM : faire de la recherche... comm
    - BXI-LL : développé les NIC BXI... comm
  + type de personnel :
    - TADaaM : chercher, post-doc, doctorant et stagiaire
    - BXI-LL : ingénigeur ?
  + Culture informatique : très forte pour les 2 ?
  + Matériel et logiciels :
    - TADaaM : ordinateur portable linux, écran secondaire, service inria (email, mattermost, webex...)
    - BXI-LL : une machine avec 2 CPU SPR accessible via VPN.
  + contraintes : démarche scientifique ? ...


## Le plan

* introducion
  + presentation e/
  + presentation du plan

* context
  + Le HPC pour la simulation numérique...
  + interruption possible mais pas pour du os bypass
  + OS bypass
  + polling
  + présenté le résaux BXI
  + MPI
  + communication asynchrone (thread dédier, progression faite dans le calcule, progression opportuniste...).
  + problématique pas de réactivité, on dédie un thread ou on à besoin d'un système complex.
  + Présenté NewMadeleine : async ...

  + Les interuption semble intérécente mais on une grande latance... Mathieu à vue ça dans sont stage... en perspective il dit qu'un mécanisme d'interruption en espace utilisateur serai interraisant à exploré.

* problématiques / objectifs
  + sujet
    - un nouveau mecanisme d'interruption en sepace utilisateur
    - on cherche a éliminé les incovégants du polling
    - contentions...
    - revenir sur un mécanisme plus normal
  + objectifs
    - faire des communication a traver le réseau BXI
    - comprendre sont fonctionnement uintr (commen les utiliser pour)
    - Driver shm NewMadeline car ipc
    - Faire progresser les come
    <!-- - la même avec BXI
    - on utiliser les interruption pour lancer le interruptions -->
    - retirer les pb du polling (list des pb)

* Pris en main des uintr / Exploration de uintr

  + sur processeur récent
  + difficulté d'accés
  + naissaisite un patch de noyaux et une version récente de GCC
  + fonctionnement...
  + similaritude avec les signaux et interruption materiel
  + 
  + J'ai du compiler est installer le patch du noyau
  + 
  + comparaison avec les signaux (parler de tous les tests que j'ai fait. J'ai tester différante façon pour partager le FD)
  + problème corriger avec la alt_stack
  + Performance du mécanisme
    - Dans un premier temps j'ai pas bind les thread par pu donc j'avais de mauvais résultats.
    - J'ai fait différents bind (on vois que c'est similaire sauf quand on passe entre 2 NUMA)
    - si on monte la frequence c'est mieux (turbo boost)
    - 

* Intégration dans NewMadeleine
  + Présenté NewMadeleine details
  <!-- + revenir sur le problème que lon veut réglé est comment le réglé avec des interruption -->
  + progression ce fait par pioman ou attente active et là c'est directement du driver + contexte bizzard
  + parlé du changement dans le système de progression
  + parlé des problématiques de gestion des interruption
    - pas d'attente dans les handler (async safe)
    - difficulté avec le cycle de vie des p_pw
    - gestion des multiple interruptions qui s'écrase, (prob_any / pour les large si la progression à traiter le pipeline courant)
    - comment on fait progresser les partie critique des communications (lfqueue)
      * pb wait-free
      * recherche biblio
      * solution Alexandre
  + les cas ou on veut envoyer des interruptions (send/recv)
    - pour indiqué qu'un paquet est partie (le premier paquet part au moment de la creation de la request. Les suivent à partie du handler si large et que la communication n'est pas fini).
    - pour indiqué qu'un paquet à été reçus (quand on est dans le handler)
  + La gestion des gros paquet ce fait par rdv donc avec un petit paquet qui lance la premier interruption.
  + ajout des driver sig et uintr
  + La suite de tests
  + Les résultats en attente active pour voire le sur coup
  + Les résultats du recouvreemnt.

  + **permette au MPI_Isend-post d'envoyer une interruption au MPI_Irecv-post pour qu'il puisse faire un MPI_Irecv-poll qui envois une interruption au MPI_Isend-poll. Ensuite le "MPI_Isend-poll" envois une interruption ainsi de suite jusqu'à la terminaison.**

On à déjà un recv post et quand l'utilisateur post on lui donne...

* Dans le bilan ou avant parlé du fonctionne du send uintr qui est en 2 parties, une qui consiste à écrire dans la mémoire UPID et la second qui consiste à déclancer le mécanisme de détection de l'interruption coté recepteur. Dans le cas ou le recepteur est endormie c'est qu kernel de déclancer la détection au moment du schedule. C'est la même pour le déclanchement uintr_notify à partir de kernel, la mémoire UPID est modifier puis la detection est déclancher directe ou par le schedule. Le déclanchement ce fait par `apic->send_UINTR()`. APIC c'est le gestionaire des interruption standard.
* 

* bilan

* consomation, pas d'attente active
