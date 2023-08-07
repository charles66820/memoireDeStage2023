#

## Consignes

* analyse du stage
* clairement expliquer :
  + les objectifs
    - ce que j'avais à faire
  + le déroulement
    - Ce que j'ai fait (comment et pourquoi)
    - montré la difficulté du travail (dire que j'ai eu du mal ?)
    - montré que j'utilise mes connaissances et ce que j'ai vue en cours en M1 + premier semestre de M2
* Faire des citation

## Le plan

  + **permette au MPI_Isend-post d'envoyer une interruption au MPI_Irecv-post pour qu'il puisse faire un MPI_Irecv-poll qui envois une interruption au MPI_Isend-poll. Ensuite le "MPI_Isend-poll" envois une interruption ainsi de suite jusqu'à la terminaison.**

On à déjà un recv post et quand l'utilisateur post on lui donne...

* Dans le bilan ou avant parlé du fonctionne du send uintr qui est en 2 parties, une qui consiste à écrire dans la mémoire UPID et la second qui consiste à déclancer le mécanisme de détection de l'interruption coté recepteur. Dans le cas ou le recepteur est endormie c'est qu kernel de déclancer la détection au moment du schedule. C'est la même pour le déclanchement uintr_notify à partir de kernel, la mémoire UPID est modifier puis la detection est déclancher directe ou par le schedule. Le déclanchement ce fait par `apic->send_UINTR()`. APIC c'est le gestionaire des interruption standard.

* bilan

* consommation, pas d'attente active
