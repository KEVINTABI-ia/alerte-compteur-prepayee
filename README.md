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
app.js

const creditInput = document.getElementById("credit");
const seuilInput = document.getElementById("seuil");
const consoInput = document.getElementById("conso");
const result = document.getElementById("result");
const historyList = document.getElementById("history");
const saveButton = document.getElementById("save");
const clearButton = document.getElementById("clear");

const STORAGE_KEY = "alerteCompteurHistorique";
const SETTINGS_KEY = "alerteCompteurReglages";

function lireHistorique() {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
  } catch {
    return [];
  }
}

function lireReglages() {
  try {
    return JSON.parse(localStorage.getItem(SETTINGS_KEY)) || {};
  } catch {
    return {};
  }
}

function afficherMontant(valeur) {
  return new Intl.NumberFormat("fr-FR", {
    maximumFractionDigits: 0
  }).format(valeur) + " FCFA";
}

function actualiserResultat() {
  const credit = Number(creditInput.value);
  const seuil = Number(seuilInput.value);
  const conso = Number(consoInput.value);

  result.className = "result";

  if (creditInput.value.trim() === "" || !Number.isFinite(credit) || credit < 0) {
    result.textContent = "Saisissez un crédit restant valide.";
    return;
  }

  let message = "<strong>Crédit restant : " + afficherMontant(credit) + "</strong>";

  if (seuilInput.value !== "" && seuil >= 0 && credit <= seuil) {
    result.classList.add(credit === 0 ? "danger" : "warning");
    message += "<p>⚠️ Votre crédit a atteint le seuil d'alerte.</p>";
  } else {
    message += "<p>✅ Votre crédit est au-dessus du seuil d'alerte.</p>";
  }

  if (consoInput.value.trim() !== "" && Number.isFinite(conso) && conso > 0) {
    const jours = credit / conso;
    message += "<p>Autonomie estimée : <strong>" +
      (jours < 1
        ? "moins d'une journée"
        : jours.toLocaleString("fr-FR", { maximumFractionDigits: 1 }) + " jour(s)") +
      "</strong></p>";
  } else {
    message += "<p>Renseignez votre consommation quotidienne moyenne pour estimer l'autonomie.</p>";
  }

  result.innerHTML = message;
}

function afficherHistorique() {
  const historique = lireHistorique();
  historyList.replaceChildren();

  if (historique.length === 0) {
    const item = document.createElement("li");
    item.textContent = "Aucun relevé enregistré.";
    historyList.appendChild(item);
    return;
  }

  historique.slice().reverse().forEach((releve) => {
    const item = document.createElement("li");
    item.textContent = releve.date + " — " + afficherMontant(releve.credit);
    historyList.appendChild(item);
  });
}

function sauvegarderReglages() {
  const reglages = {
    seuil: seuilInput.value,
    conso: consoInput.value
  };
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(reglages));
}

function restaurerReglages() {
  const reglages = lireReglages();

  if (reglages.seuil !== undefined) seuilInput.value = reglages.seuil;
  if (reglages.conso !== undefined) consoInput.value = reglages.conso;
}

saveButton.addEventListener("click", () => {
  const credit = Number(creditInput.value);

  if (creditInput.value.trim() === "" || !Number.isFinite(credit) || credit < 0) {
    result.className = "result danger";
    result.textContent = "Entre un montant de crédit valide avant d'enregistrer.";
    return;
  }

  const historique = lireHistorique();
  historique.push({
    credit,
    date: new Date().toLocaleString("fr-FR")
  });

  localStorage.setItem(STORAGE_KEY, JSON.stringify(historique));
  sauvegarderReglages();
  afficherHistorique();
  actualiserResultat();
});

[creditInput, seuilInput, consoInput].forEach((champ) => {
  champ.addEventListener("input", () => {
    actualiserResultat();
    if (champ !== creditInput) sauvegarderReglages();
  });
});

clearButton.addEventListener("click", () => {
  if (confirm("Veux-tu vraiment effacer tout l'historique des relevés ?")) {
    localStorage.removeItem(STORAGE_KEY);
    afficherHistorique();
  }
});

restaurerReglages();
afficherHistorique();
actualiserResultat();
manifest.json

const creditInput = document.getElementById("credit");
const seuilInput = document.getElementById("seuil");
const consoInput = document.getElementById("conso");
const result = document.getElementById("result");
const historyList = document.getElementById("history");
const saveButton = document.getElementById("save");
const clearButton = document.getElementById("clear");

const STORAGE_KEY = "alerteCompteurHistorique";
const SETTINGS_KEY = "alerteCompteurReglages";

function lireHistorique() {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY)) || [];
  } catch {
    return [];
  }
}

function lireReglages() {
  try {
    return JSON.parse(localStorage.getItem(SETTINGS_KEY)) || {};
  } catch {
    return {};
  }
}

function afficherMontant(valeur) {
  return new Intl.NumberFormat("fr-FR", {
    maximumFractionDigits: 0
  }).format(valeur) + " FCFA";
}

function actualiserResultat() {
  const credit = Number(creditInput.value);
  const seuil = Number(seuilInput.value);
  const conso = Number(consoInput.value);

  result.className = "result";

  if (creditInput.value.trim() === "" || !Number.isFinite(credit) || credit < 0) {
    result.textContent = "Saisissez un crédit restant valide.";
    return;
  }

  let message = "<strong>Crédit restant : " + afficherMontant(credit) + "</strong>";

  if (seuilInput.value !== "" && seuil >= 0 && credit <= seuil) {
    result.classList.add(credit === 0 ? "danger" : "warning");
    message += "<p>⚠️ Votre crédit a atteint le seuil d'alerte.</p>";
  } else {
    message += "<p>✅ Votre crédit est au-dessus du seuil d'alerte.</p>";
  }

  if (consoInput.value.trim() !== "" && Number.isFinite(conso) && conso > 0) {
    const jours = credit / conso;
    message += "<p>Autonomie estimée : <strong>" +
      (jours < 1
        ? "moins d'une journée"
        : jours.toLocaleString("fr-FR", { maximumFractionDigits: 1 }) + " jour(s)") +
      "</strong></p>";
  } else {
    message += "<p>Renseignez votre consommation quotidienne moyenne pour estimer l'autonomie.</p>";
  }

  result.innerHTML = message;
}

function afficherHistorique() {
  const historique = lireHistorique();
  historyList.replaceChildren();

  if (historique.length === 0) {
    const item = document.createElement("li");
    item.textContent = "Aucun relevé enregistré.";
    historyList.appendChild(item);
    return;
  }

  historique.slice().reverse().forEach((releve) => {
    const item = document.createElement("li");
    item.textContent = releve.date + " — " + afficherMontant(releve.credit);
    historyList.appendChild(item);
  });
}

function sauvegarderReglages() {
  const reglages = {
    seuil: seuilInput.value,
    conso: consoInput.value
  };
  localStorage.setItem(SETTINGS_KEY, JSON.stringify(reglages));
}

function restaurerReglages() {
  const reglages = lireReglages();

  if (reglages.seuil !== undefined) seuilInput.value = reglages.seuil;
  if (reglages.conso !== undefined) consoInput.value = reglages.conso;
}

saveButton.addEventListener("click", () => {
  const credit = Number(creditInput.value);

  if (creditInput.value.trim() === "" || !Number.isFinite(credit) || credit < 0) {
    result.className = "result danger";
    result.textContent = "Entre un montant de crédit valide avant d'enregistrer.";
    return;
  }

  const historique = lireHistorique();
  historique.push({
    credit,
    date: new Date().toLocaleString("fr-FR")
  });

  localStorage.setItem(STORAGE_KEY, JSON.stringify(historique));
  sauvegarderReglages();
  afficherHistorique();
  actualiserResultat();
});

[creditInput, seuilInput, consoInput].forEach((champ) => {
  champ.addEventListener("input", () => {
    actualiserResultat();
    if (champ !== creditInput) sauvegarderReglages();
  });
});

clearButton.
