<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. debut d'un fichier yaml</a>
<ul>
<li><a href="#sec-1-1">1.1. variables</a></li>
<li><a href="#sec-1-2">1.2. tableaux</a></li>
<li><a href="#sec-1-3">1.3. structure clef/valeur</a></li>
<li><a href="#sec-1-4">1.4. tableaux de table de hachage</a></li>
</ul>
</li>
<li><a href="#sec-2">2. playbook</a></li>
<li><a href="#sec-3">3. inventaire</a></li>
</ul>
</div>
</div>

# debut d'un fichier yaml<a id="sec-1" name="sec-1"></a>

    ---

## variables<a id="sec-1-1" name="sec-1-1"></a>

## tableaux<a id="sec-1-2" name="sec-1-2"></a>

    tableau:
      - "un"
      - "deux"
      - 3

ou:

    tableau: ["un", "deux", 3]

## structure clef/valeur<a id="sec-1-3" name="sec-1-3"></a>

    utilisateur1:
      - nom: pierre
      - prenom: jean

ou:

    utilisateur1: {nom: pierre, prenom: jean}

## tableaux de table de hachage<a id="sec-1-4" name="sec-1-4"></a>

    liste_utilisateurs:
      - nom: pierre
        prenom: pierre
      - nom : pierre
        prenom: sarah

ou:

    liste_utilisateurs:
      - utilisateur1: {nom: pierre, prenom: jean}
      - utilisateur2: {nom: pierre, prenom: sarah}

ou:

    users: [{nom: perre, prenom: yannig},{nom: perre, prenom: sarah}]

# playbook<a id="sec-2" name="sec-2"></a>

    ---
    - name: "Apache installation"
      hosts: all
      tasks:
        - name: "Install apache package"
          yum:
            name: "httpd"
            state: "present"
        - name: "Start apache service"
          service:
            name: "httpd"
            state: "started"
            enabled: yes
        - name: "Copy test.html"
          copy:
            src: "test.html"
            dest: "/var/www/html"
            owner: "apache"
            group: "apache"

    curl http://localhost/test.html

ça marche en local, mais inaccessible depuis l'extérieur: problème de firewall
on autorise le service https

    - name: "autoriser le service https"
      firewalld:
        service: https
        permanent: true
        state: enabled

ou bien on ouvre le port 443:

    - name: "ouvrir le port 443"
      firewalld:
        port: 443
        permanent: true
        state: enabled

# inventaire<a id="sec-3" name="sec-3"></a>

groupe: entre crochets

    [un_groupe]
    machine1
    machine2
