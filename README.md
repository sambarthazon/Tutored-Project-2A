# Tutored-Project-2A

Implémentation d'un site sur un Raspberry-Pi

## Projet tuteuré deuxième année

Projet tuteuré de ma deuxième année à l'Université Clermont Auvergne (UCA).

Développement d'un site web dynamique permettant d'afficher des photos prises par un module à intervalle régulier.
Les photos devront remplir des critères tel qu'une certaines taille ainsi qu'une certaine extension afin de pouvoir s'afficher sur le site.

Utilisation de Apache ainsi que de RaspAp.

Voici le code implémenté dans le fichier index.php de RaspAp :

```
<!-- Code vérification sur la barre de navigation -->
    <?php if (RASPI_VERIF_ENABLED) : ?>
        <li class="nav-item">
            <a class="nav-link" href="verif"><i class="fas fa-key fa-fw mr-2"></i><span class="nav-label"><?php echo _("Verification"); ?></a>
        </li>
    <?php endif; ?>
<!-- Fin code vérification sur la barre de navigation -->
```

```
<!-- Code galerie sur la barre de navigation -->
    <?php if (RASPI_GALLERY_ENABLED) : ?>
        <li class="nav-item">
            <a class="nav-link" href="gallery"><i class="fas fa-paint-brush fa-fw mr-2"></i><span class="nav-label"><?php echo _("Gallery"); ?></a>
        </li>
    <?php endif; ?>
<!-- Fin code galerie sur la barre de navigation -->  
```

```
<!-- Programme de vérification -->
    <?php case "/verif": ?>
       	<form action="" method="POST" enctype="multipart/form-data">
            <label for="file"> Fichier </label>
            <input type="file" name="file">
            <button type="submit"> Envoyer </button>
        </form>
        <?php
            if(isset($_FILES['file'])){
                $tmpName = $_FILES['file']['tmp_name']; //Localisation temporaire
                $name = $_FILES['file']['name']; //Nom du fichier
                $size = $_FILES['file']['size']; //Taille en bit du fichier
                $type = $_FILES['file']['type']; //Extension du fichier
                if($type=='image/png'||$type=='image.jpg'||$type=='image/gif'||$type=='image/jpeg'){
                    if($size<=1000000000){
                        move_uploaded_file($tmpName, './app/img/uploads/'.$name); //Envoi du fichier vers uploads
                        echo 'Transféré';
                    } else {
                        echo 'Error taille';
                    }
                } else {
                echo 'Error extension';
                }
            }
    break;
//Fin du programme de vérification
```

```
//Programme d'affichage
    case "/gallery":
        $target_dir = 'app/img/uploads/'; //dossier que l'on veut traiter
        $dir = opendir($target_dir) or die('Erreur de listage : le répertoire n\'existe pas'); //on ouvre le contenu du dossier
    
        //boucle pour savoir si dans le dossier voulu (uploads) nous avons des fichiers ou d'autres dossiers
        while($element = readdir($dir)) { //$element = le nom du fichier lu dans $dir (donc avec opendir($target_dir) donc dans uploads)
            if($element != '.' && $element != '..') { //si le nom du fichier lu n'est pas '.' ou '..' faire : (car [0] et [1] du array $element = '.' et '..')
                echo "<img src='$target_dir/$element' class='content'>"; //affichage de chaque case du tableau $fichier
            }
        }
        //fin de la boucle de triage
        closedir($dir); //on ferme le dossier qui a été ouvert
    break;
//Fin du programme d'affichage
```