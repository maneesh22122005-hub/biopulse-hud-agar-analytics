# biopulse-hud-agar-analytics
An automated digital microbiology and computer vision-assisted culture monitoring platform. It implements interactive image thresholding, real-time colony isolation, population morphological analysis, and standard kinetic growth modeling within a responsive high-tech HUD.
# BioPulse HUD  Automated Agar Analysis Platform v3.0

BioPulse HUD is a high-tech computer vision and data analytics interface designed for rapid, automated colony-forming unit (CFU) counting, artifact identification, and kinetic prediction modeling directly from petri dish photographs. 

Powered by browser-based digital image manipulation frameworks and lightweight machine learning architecture integration, this platform provides real-time quantitative dashboards for micro-biological plates.

## Key Features

### 1. Computer Vision & Dynamic Segmentation Engine
* **Otsu Dynamic Thresholding:** Automatically analyzes the pixel luminance distribution to dynamically extract true colony bounds from background media noise.
* **Invert Luminance Polarity Switch:** Contextually adjust algorithms depending on whether colonies are darker than background agar or lighter.
* **Vector Boundary Controls:** Custom perimeter range settings filter out microscale artifacts or massive un-splittable clusters.
* **Edge Vector Isolation:** Algorithmically discards geometric collisions around the rim/perimeter of the plastic culture dish.

### 2. Multi-Dimensional Plate Health Index
Calculates a real-time health profile using three structural characteristics:
* **Density:** Gauges real-time surface occupancy levels, tracking average radial dimensions to monitor when cultures approach conflence.
* **Diversity:** Uses a localized phenotypical neural tracking sequence to evaluate phenotypic heterogeneity or pure monomicrobial state distributions.
* **Cohesion:** Measures geometric circularity to cleanly segregate biological targets from air bubbles, optical flares, or deep plastic scratches.

### 3. Population Distribution Analytics
* **Morphological Analysis:** Renders interactive multi-variable statistical graphs tracking **Area vs. Cohesion profiles**.
* **Spectrum Density Profiles:** Dynamically updates a live population size distribution histogram to help pinpoint contamination events or poly-microbial growth velocities.

### 4. Time-Lapse Kinetic Growth Predictor
Extrapolates present colony footprints using standard biological models:
* **Exponential:** Early-phase unconstrained growth trajectory.
* **Logistic:** Density-limited growth curve tracking nutrient depletion thresholds.
* **Gompertz:** Asymmetric model capturing initial lag-phase offsets alongside plateau deceleration milestones.
* Includes an interactive timeline scrubber to preview a modeled visual bloom projection on the active viewport canvas.

### 5. Interactive Neural Cleanup Workspace
* Toggle **AI Cluster Cleanup** mode to click directly onto the canvas view, flag false-positive structural noise, and scrub congruent configurations out of the master analytics count instantly.

## Technologies Used
* Vanilla JavaScript (ES6+) Canvas API
* TensorFlow.js & MobileNet Architecture Support
* Chart.js (Data visualization and scattering profiles)
* Custom Glassmorphic Responsive Stylesheets

## Installation & Usage
1. Clone the repository:
   ```bash
   git clone [https://github.com/yourusername/biopulse-hud-agar-analytics.git](https://github.com/yourusername/biopulse-hud-agar-analytics.git)
