# Déclarations CASE

### [](https://github-com.translate.goog/kplr-training/SQL-Course/blob/main/SQLite/6-Case_Statement.md?_x_tr_sl=ar&_x_tr_tl=fr&_x_tr_hl=en-US&_x_tr_pto=wapp#51-categorizing-wind-speed)5.1 Catégorisation de la vitesse du vent

Vous pouvez utiliser une `CASE`instruction pour transformer une valeur de colonne en une autre valeur basée sur des conditions. Par exemple, nous pouvons transformer différentes `wind_speed`plages en catégories `HIGH`, `MODERATE`et .`LOW`

SELECT report_code, year, month, day, wind_speed,

CASE
   WHEN wind_speed >= 40 THEN 'HIGH'
   WHEN wind_speed >= 30 THEN 'MODERATE'
   WHEN wind_speed >= 0 THEN 'LOW'
   ELSE 'N/A'
END as wind_severity

FROM station_data
ORDER by wind_speed DESC;

### [](https://github-com.translate.goog/kplr-training/SQL-Course/blob/main/SQLite/6-Case_Statement.md?_x_tr_sl=ar&_x_tr_tl=fr&_x_tr_hl=en-US&_x_tr_pto=wapp#52-more-efficient-way-to-categorize-wind-speed)5.2 Un moyen plus efficace de catégoriser la vitesse du vent

Nous pouvons en fait omettre `AND wind_speed < 40`l'exemple précédent car chacun `WHEN`est `THEN`évalué de haut en bas. Le premier qu'il trouve vrai est celui avec lequel il ira et cessera d'évaluer les conditions suivantes.