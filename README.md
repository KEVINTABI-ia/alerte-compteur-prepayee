# alerte-compteur-prepayee
 Application de suivi du crédit des compteurs prépayés
index.html

<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="theme-color" content="#075e54">
  <meta name="description" content="Suivez le crédit de votre compteur prépayé et estimez votre autonomie.">
  <title>Alerte compteur prépayée</title>
  <link rel="manifest" href="manifest.json">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>⚡ Alerte compteur</h1>
    <p>Anticipez l'épuisement de votre crédit électrique.</p>
  </header>

  <main>
    <section class="card">
      <h2>Mon compteur</h2>
      <label for="credit">Crédit restant (FCFA)</label>
      <input id="credit" type="number" min="0" step="1" placeholder="Ex. : 5000">

      <label for="seuil">Seuil d'alerte (FCFA)</label>
      <input id="seuil" type="number" min="0" step="100" value="1000">

      <label for="conso">Consommation moyenne par jour (FCFA)</label>
      <input id="conso" type="number" min="1" step="100" placeholder="Ex. : 1000">

      <button id="save">Enregistrer mon relevé</button>
      <div id="result" class="result" aria-live="polite">
        Saisissez votre crédit pour commencer.
      </div>
    </section>

    <section class="card">
      <h2>Historique</h2>
      <ul id="history">
        <li>Aucun relevé enregistré.</li>
      </ul>
      <button id="clear" class="secondary">Effacer l'historique</button>
    </section>

    <p class="notice">
      Cette application n'est pas officielle CIE. Le crédit est saisi
      manuellement. Les estimations dépendent de votre consommation et
      ne constituent pas une garantie contre les coupures.
    </p>
  </main>

  <footer>Votre consommation, mieux suivie.</footer>
  <script src="app.js"></script>
</body>
</html>
