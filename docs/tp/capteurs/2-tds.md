---
sidebar_position: 2
---

# Capteur TDS

Le capteur donne des valeurs en mV qu'il faut convertir avec les formules suivantes

Coefficient de compensation : $C_c = 1 + 0.2(T-25)$ \
Voltage de compensation : $C_v = \frac{V}{C_c}$ \
Valeur TDS : $tds = \frac{133.42C_v^3 - 255.46C_v^2 + 857.39C_v}{2} $ 