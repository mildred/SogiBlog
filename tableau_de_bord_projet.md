Tableau de bord Projet
======================

**Problème :** faire en sorte d'éviter la double saisie entre le tableau de bord Sogilis et le tableau de bord client.

**Contexte :** Projet Enalean. le reporting client est très léger. On leur fournir le tableau suivant par personne :

Date  | Temps | Description
----- | ----- | -----------
25/01 |      1| Import Artifact Links
26/01 |      1| Import Artifact Links
27/01 |      1| Import Artifact Links, Fix bugs in Artifact Links


Première étape : ajouter de l'info au tableau de bord Sogilis
-------------------------------------------------------------

Le tableau de bord Sogilis ne permet pas d'avoir d'information sur les tâches effectuées par quart de journée. Pour cela, j'ai modifié les formules afin de permettre d'ajouter cette information.

L'idée est la suivante : une ligne de la feuille de temps du tableau de bord peut être associée a un projet si elle correspond a un certain pattern. Ce pattern se trouve dans la colonne *Mot Clef* du tableau de bord. La formule de calcul par projet est un peu compliquée mais la voici :

    =IF(nom_projet="";
        0;
        IF(OR(COUNTIF(nom_projet;mot_clef_projet)=0;
              mot_clef_projet=0);
           COUNTIF(feuille_de_temps;nom_projet);
           0)
       +IF(mot_clef_projet<>0;
           COUNTIF(feuille_de_temps;mot_clef_projet);
           0)
       )/4

Cette formule va calculer le temps passé sur un projet en faisant une somme intelligente des cases contenant le nom du projet, et des cases correspondant au pattern du projet. 

Finalement, sur le projet Enalean, le mot clef a été configuré comme étant "`Enalean*`" ce qui permet de spécifier dans la feuille de temps :

| Janvier 2016| Mot Clef | ... | Shanti     |
| ----------- | -------- | --- | ---------- |
| Enalean     | Enalean* |     | =IF(...)/4 |
| ...         |
|             |
| 25/01 |  |  | Enalean - Import Artifact Links     |
| 25/01 |  |  | Enalean - Import Artifact Links     |
| 25/01 |  |  | Enalean - Import Artifact Links     |
| 25/01 |  |  | Enalean - Import Artifact Links     |
|       |  |  |                                     |
| 26/01 |  |  | Enalean - Import Artifact Links     |
| 26/01 |  |  | Enalean - Import Artifact Links     |
| 26/01 |  |  | Enalean - Import Artifact Links     |
| 26/01 |  |  | Enalean - Import Artifact Links     |
|       |  |  |                                     |
| 27/01 |  |  | Enalean - Import Artifact Links     |
| 27/01 |  |  | Enalean - Import Artifact Links     |
| 27/01 |  |  | Enalean - Fix bugs in Artifact Links|
| 27/01 |  |  | Enalean - Fix bugs in Artifact Links|

Maintenant, lors de l'export, il est possible de récupérer les informations sur la tâche en cours du tableau de bord.

Seconde étape : l'export
------------------------

L'export est géré par [sogiboard](http://sogiboard.herokuapp.com/). J'ai indiqué la clé secrète dans la case appropriée et ensuite rempli les champs suivants :

- Google Doc ID : `1cz...Z9w` (l'identifiant Google Doc du tableau de bord)
- Sheet ID, les identifiants des différentes feuilles (les différents mois) du tableau de bord:
    - `1510385237`
    - `779510020`
    - `215240968`
- Format: `csv`
- Project Regexp: `^Enalean`

Ceci va me générer [une URL](http://sogiboard.herokuapp.com/?s=293TarMmTs2eVbsckeihz8BPRYvaoYRaTWs3KM9eLECphY981B5JmccnMSNUwCGSZ1EyiMzmPfUaYe1aUpDKARtJ7JtAKi9dzoX74QuR7v9Q8WCzSwtSsu5R1mGy5R42wfTFZtbeyRkUHtYcpZrwDrDaz9wkVSVBEN3M4RV9NiA3Jo2xCA2uB4Bq1FdGTFkncJpp53LNMEUY3sa) qui exporte les données du projet Enalean en CSV, importable dans un autre document.

Troisième étape : l'import
--------------------------

L'import dans un document se fait via la formule `=IMPORTDATA("http://sogiboard.herokuapp.com/?s=...")`. Il en résulte un tableau ressemblant à:

Date  | Nom    | Durée | Tâche
------|--------|-------|-------
25/01 | Shanti |     1 | Import Artifact Links
26/01 | Shanti |     1 | Import Artifact Links
27/01 | Shanti |   0,5 | Import Artifact Links
27/01 | Shanti |   0,5 | Fix bugs in Artifact Links

Dernière étape : le traitement
------------------------------

Afin de pouvoir formatter cette feuille de temps comme l'original, il faut un post-traitement. Pour cela, j'ai défini une plage nommée `time_data` correspondant au tableau précédent (en excluant les titres).

J'ai ensuite défini une fonction Google Sheet (Outils -> Éditeur de scripts) :

    function activite(data, date, personne, prefix) {
      var duration = 0;
      var description = [];
      prefix = new RegExp(prefix|| "", "g");
      for(var i = 0; i < data.length; ++i) {
        if (data[i][0].toString() == date.toString() && data[i][1] == personne) {
          duration += data[i][2];
          var descr = data[i][3];
          prefix.exec(descr);
          description.push(descr.substring(prefix.lastIndex));
        }
      }
      return [[duration, description.join(", ")]];
    }

Je peux ensuite utiliser cette formule dans un tableau :

  | A     | B | C                                              | D
--|-------|---|------------------------------------------------|----------
1 |       |   | **Shanti**                                     |
2 |       |   | **Temps passé**                                | **Activité**
3 |       |   |                                                |
4 | 27/01 |   | `=activite(time_data; A4; C$1; "Enalean[ -]*")`|
5 | 28/01 |   | `=activite(time_data; A5; C$1; "Enalean[ -]*")`|
6 | 29/01 |   | `=activite(time_data; A6; C$1; "Enalean[ -]*")`|

Ce qui résulte en le tableau suivant :


A    | B | C               | D
-----|---|-----------------| -----------
     |   | **Shanti**      |
     |   | **Temps passé** | **Activité**
     |   |                 |
25/01|   |      1          | Import Artifact Links
26/01|   |      1          | Import Artifact Links
27/01|   |      1          | Import Artifact Links, Fix bugs in Artifact Links