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
style.css

* {
  box-sizing: border-box;
}

:root {
  --green: #075e54;
  --green-light: #e4f5ee;
  --background: #f3f7f6;
  --text: #172b28;
  --muted: #647570;
  --border: #dce7e3;
  --orange: #a64b00;
  --red: #b42318;
}

body {
  margin: 0;
  background: var(--background);
  color: var(--text);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  line-height: 1.5;
}

header {
  padding: 30px 20px 36px;
  background: var(--green);
  color: white;
  border-radius: 0 0 26px 26px;
}

header h1 {
  margin: 0 0 8px;
  font-size: 27px;
}

header p {
  margin: 0;
  color: #e0f2ec;
  font-size: 15px;
}

main {
  width: 100%;
  max-width: 560px;
  margin: -18px auto 0;
  padding: 0 16px 24px;
}

.card {
  background: white;
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 20px;
  margin-bottom: 18px;
  box-shadow: 0 5px 18px rgba(15, 60, 48, 0.05);
}

.card h2 {
  margin: 0 0 18px;
  font-size: 20px;
}

label {
  display: block;
  margin: 15px 0 7px;
  font-size: 14px;
  font-weight: 600;
}

input {
  display: block;
  width: 100%;
  min-height: 48px;
  padding: 12px;
  border: 1px solid #cbd9d4;
  border-radius: 10px;
  background: #fff;
  color: var(--text);
  font: inherit;
  font-size: 16px;
}

input:focus {
  outline: 2px solid #8acbb6;
  border-color: var(--green);
}

button {
  width: 100%;
  min-height: 48px;
  margin-top: 18px;
  padding: 12px 16px;
  border: 0;
  border-radius: 11px;
  background: var(--green);
  color: white;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
}

button:active {
  opacity: 0.85;
}

button.secondary {
  background: #edf3f0;
  color: var(--green);
}

.result {
  margin-top: 16px;
  padding: 15px;
  border-radius: 12px;
  background: var(--green-light);
  overflow-wrap: anywhere;
}

.result.warning {
  background: #fff1dc;
  color: var(--orange);
}

.result.danger {
  background: #feeceb;
  color: var(--red);
}

.result strong {
  font-size: 18px;
}

#history {
  margin: 0;
  padding-left: 20px;
}

#history li {
  padding: 8px 0;
  border-bottom: 1px solid #edf1ef;
  overflow-wrap: anywhere;
}

.notice {
  padding: 12px;
  color: var(--muted);
  font-size: 12px;
}

footer {
  padding: 12px 16px 24px;
  text-align: center;
  color: var(--muted);
  font-size: 13px;
}
