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
* parlé de la machine qui crash quand il y a trop de charge

## Le plan

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
  + La suite de testes
  + Les résultats en attente active pour voire le sur coup
  + Les résultats du recouvreemnt.

  + **permette au MPI_Isend-post d'envoyer une interruption au MPI_Irecv-post pour qu'il puisse faire un MPI_Irecv-poll qui envois une interruption au MPI_Isend-poll. Ensuite le "MPI_Isend-poll" envois une interruption ainsi de suite jusqu'à la terminaison.**

On à déjà un recv post et quand l'utilisateur post on lui donne...

* Dans le bilan ou avant parlé du fonctionne du send uintr qui est en 2 parties, une qui consiste à écrire dans la mémoire UPID et la second qui consiste à déclancer le mécanisme de détection de l'interruption coté recepteur. Dans le cas ou le recepteur est endormie c'est qu kernel de déclancer la détection au moment du schedule. C'est la même pour le déclanchement uintr_notify à partir de kernel, la mémoire UPID est modifier puis la detection est déclancher directe ou par le schedule. Le déclanchement ce fait par `apic->send_UINTR()`. APIC c'est le gestionaire des interruption standard.
* 

* bilan

* consomation, pas d'attente active
