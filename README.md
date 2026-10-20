# Marathon Pace Band & Race Fuel Calculator

Open-source algorithms for landmark-calibrated marathon pacing splits, printable wristband generation, and dual-transporter (1:0.8 maltodextrin to fructose) endurance fuel modeling for distance runners.

## Live Applications & Calculators
Access the interactive web tools, printable wristband generator, and course elevation profiles at **[Pace & Fuel](https://paceandfuel.com/)**:

* **[Printable Marathon Pace Bands](https://paceandfuel.com/pacing/)** – Generates custom, landmark-calibrated wristband splits (miles & km) for major UK courses including London, Manchester, Brighton, and the Great North Run.
* **[Race Fuel & Sodium Planner](https://paceandfuel.com/#calculator)** – Calculates hourly carbohydrate oxidation rates (SGLT1 and GLUT5 transporter saturation), hydration targets, and electrolyte requirements by finish time.
* **[DIY Running Gel Recipe & Batch Calculator](https://paceandfuel.com/blog/how-to-make-diy-running-gels/)** – Formulates 90g/hr citrus pectin and sodium alginate hydrogels for reusable soft flasks.
* **[London Marathon Hub](https://paceandfuel.com/london-marathon/)** – Course elevation breakdown, official Maurten/Lucozade station mapping, and split strategies.

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
