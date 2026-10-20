# Marathon Pace Band & Race Fuel Calculator

Reference algorithms for landmark-calibrated marathon pacing splits, printable wristband generation, and dual-transporter (1:0.8 maltodextrin to fructose) endurance fuel modeling for distance runners.

## Live Applications & Calculators

Access the interactive web applications, printable wristband generator, and course elevation profiles at **[Pace & Fuel](https://paceandfuel.com/)**:

- **[Printable Marathon Pace Bands](https://paceandfuel.com/pacing/)** – Custom, landmark-calibrated wristband splits (miles & km) for major UK courses including London, Manchester, Brighton, and the Great North Run.
- **[Race Fuel & Sodium Planner](https://paceandfuel.com/#calculator)** – Hourly carbohydrate oxidation modeling (SGLT1 and GLUT5 transporter saturation), hydration targets, and electrolyte requirements by finish time.
- **[DIY Running Gel Recipe & Batch Calculator](https://paceandfuel.com/blog/how-to-make-diy-running-gels/)** – Formulates 90g/hr citrus pectin and sodium alginate hydrogels for reusable soft flasks.
- **[London Marathon Hub](https://paceandfuel.com/london-marathon/)** – Course elevation breakdown, official Maurten/Lucozade station mapping, and split strategies.

---

## 1. Pacing & Split Calculations

The core pacing engine calculates target landmark checkpoints based on goal finish time, adjusting for course topography and expected fatigue curves.

```javascript
/**
 * Generates landmark split times for a target endurance event.
 * @param {number} finishSeconds - Total target finish time in seconds.
 * @param {number[]} landmarkDistances - Array of milestone distances (miles or km).
 * @param {number} totalDistance - Total race distance (e.g., 42.195 km or 26.21875 miles).
 * @param {number[]} [gradeMultipliers] - Course topography weighting factors (1.0 = flat).
 * @returns {string[]} Array of formatted HH:MM:SS split checkpoints.
 */
function calculateLandmarkSplits(finishSeconds, landmarkDistances, totalDistance, gradeMultipliers = []) {
  const basePacePerUnit = finishSeconds / totalDistance;

  return landmarkDistances.map((distance, idx) => {
    const factor = gradeMultipliers[idx] || 1.0;
    const checkpointSeconds = Math.round(distance * basePacePerUnit * factor);

    const hours = Math.floor(checkpointSeconds / 3600);
    const minutes = Math.floor((checkpointSeconds % 3600) / 60);
    const seconds = checkpointSeconds % 60;

    return [hours, minutes, seconds]
      .map(v => v.toString().padStart(2, '0'))
      .filter((v, i) => v !== '00' || i > 0)
      .join(':');
  });
}
```

---

## 2. Dual-Transporter Carbohydrate Modeling

Endurance fueling past 60 grams of carbohydrate per hour is physiologically limited by intestinal absorption channels:

* **SGLT1 Intestinal Transporters (Glucose / Maltodextrin):**
  * Saturates at approximately **60g/hr**.
  * Excess glucose beyond saturation remains in the gut lumen, drawing water via osmosis and causing GI distress.
* **GLUT5 Transporters (Fructose):**
  * Utilizes an independent, non-competitive facilitated diffusion pathway.
  * Can oxidize an additional **30g to 50g/hr**.
* **Optimal Dual-Source Ratio:**
  * **1 : 0.8 (Maltodextrin to Fructose)** optimizes total exogenous carbohydrate oxidation up to **90g–120g/hr** with minimal gastrointestinal discomfort.

---

## Terms & Reference

The algorithms and nutritional formulas outlined in this repository are published for educational, research, and technical reference. 

The web applications, design systems, and software implementations at [paceandfuel.com](https://paceandfuel.com/) are proprietary.

Copyright © [Pace & Fuel](https://paceandfuel.com/). All rights reserved.
