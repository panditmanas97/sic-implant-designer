# SiC Implant Designer

A single-file, interactive web app for designing aluminum (p-type) and nitrogen (n-type) ion-implant schedules in 4H-SiC. Drag implants on a live doping-vs-depth plot and see the net profile update immediately.

Open `implant_designer.html` in any modern browser. There is no build step and no server. Everything runs locally in the page.

## Features

- **Live plot.** Net doping (Al minus N plus background) against depth, drawn on a signed log scale (p up, n down) or as |net| on a log axis. Each implant is a draggable dot at its projected range and peak concentration.
- **Dragging.** Drag a dot sideways to change energy. Hold Shift (or tick "Drag changes dose") and drag to change dose. Select a dot and use the arrow keys: left and right change energy, up and down change dose.
- **Energy range.** 10 to 2000 keV for both species.
- **Screen oxide.** Any thickness in nm. The plot starts at -0.1 um (or further left for a thick oxide). Inside the oxide, each implant is drawn as a dashed curve and the net implanted profile as a dashed purple line.
- **Target layers.** Add and remove layers (depth, n or p, net concentration) out to 3 um. A "typical miss vs target" figure is shown under the plot.
- **Background doping.** Set different background ranges at different depths, each n (N) or p (Al).
- **Optimize schedule.** Searches energies and doses for the current target with an exact non-negative least-squares fit, then prunes to a maximum number of implants. "Re-fit doses only" keeps your energies and refits the doses.
- **Import and export.** CSV (`species, energy, dose`) and Sentaurus Process (Sprocess) `implant` commands, both copyable and pasteable back in. The Sprocess panel has editable tilt, rotation and temperature.
- **Depth range** of the plot is user-editable (0.2 to 3 um).

## Sprocess output

Each implant becomes one line:

```
implant Nitrogen	energy= 40 	dose= 3.524e12 	tilt= 0 	rotation= 0 	temp= 25. 
```

Energies are in keV and doses in cm^-2. The screen oxide is not part of the commands, so deposit it in your process flow. Pasting lines of this form (or CSV) into the box and pressing "Apply pasted commands" loads them into the app.

## Model

Implant profiles use the Janson range statistics for Al and N in 4H-SiC (Rp, dRp, skewness and kurtosis as functions of energy) with a Pearson IV distribution, ported from a Python `implant.py` and checked against it. Depth is evaluated at `x + t_screen`, so the oxide shifts the profile as in the original code. Net doping is Al - N + background, with p positive and n negative.

## Notes and limits

- Results are an analytic model, not TCAD. Expect differences from Monte Carlo or calibrated process simulation, especially near the surface, with channeling, or with a thick oxide.
- Optimize schedule fits the profile shape. It does not limit total dose or compensation, so check the Al and N doses shown in the status bar.
- Your last schedule, targets and settings are saved in this browser's local storage. Clear site data to reset.
