# Energie thuis

Een webpagina om de data van HomeWizard Energy te bekijken: stroom, teruglevering, thuisbatterij, gas en water. De pagina rekent ook uit of een extra HomeWizard-thuisbatterij zichzelf terugverdient met je eigen stroomtarieven.

Alles draait in je browser. De pagina stuurt geen data naar internet.

## Gebruiken

1. Exporteer in de HomeWizard Energy-app per meter de grafiekdata met resolutie **per dag** (Instellingen → Grafieken → kies een grafiek → Grafiekdata exporteren). Doe dat voor de P1-meter (stroom en gas), de watermeter en de thuisbatterij.
2. Open `web/index.html` in je browser, of de GitHub Pages-versie van deze repository.
3. Sleep de CSV-bestanden op de pagina of klik op **CSV's inlezen**. De pagina herkent zelf welk bestand stroom, gas, water of batterij is.
4. Vul bij *Is een tweede thuisbatterij zinvol?* de tarieven van je eigen contract in.

Ingelezen bestanden en tarieven worden alleen in je eigen browser bewaard.

## Eigen data automatisch laden

Zet een bestand `web/data.js` naast de pagina:

```js
window.HW_DATA = [
  {name: "P1e-....csv", text: "time,Import T1 kWh,..."},
  {name: "Bat-....csv", text: "time,Import kWh,Export kWh\n..."}
];
```

`data.js` en alle `.csv`-bestanden staan in `.gitignore`, zodat je eigen meterdata niet in de repository terechtkomt.

## Hoe de batterijberekening werkt

- Verbruik per dag is het verschil tussen twee opeenvolgende dagstanden. Daalt een meterstand, bijvoorbeeld bij een metervervanging, dan telt de nieuwe stand vanaf nul.
- Een extra batterij laadt per dag hooguit 2,688 kWh, en nooit meer dan je die dag teruggeleverd hebt. Hij levert met het rendement dat je huidige batterij in de data haalt, en nooit meer dan je die dag van het net afnam.
- Met dagcijfers is niet te zien wanneer op een dag stroom teruggaat of wordt afgenomen. Daarom is de berekende opbrengst een bovengrens.
- Er is geen rekening gehouden met slijtage, prijsstijgingen of een dynamisch contract.
