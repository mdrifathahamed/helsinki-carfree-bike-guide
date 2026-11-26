# Car-free guide for bike owners in Helsinki 🚲🚇

**[Open the live map](https://mdrifathahamed.github.io/helsinki-carfree-bike-guide/)**

An interactive web map that answers a question I face every day as a bike commuter in Helsinki:
*what is the cheapest sensible way to get to the city centre right now?* For every 250 m grid cell in
the Helsinki region, it recommends **bike**, **bike + metro/train** or **public transport**, and it
changes the recommendation with the live weather.

<!-- Add a screenshot: docs/screenshot.png -->

## Features
- **Automatic weather**: reads the current Helsinki weather (Open-Meteo) when the map opens and
  shows the matching layer: normal, rain, or snow / extreme cold. A small chip shows the temperature
  and a one-word status, e.g. "Normal", "Raining" or "Rain in ~20 min".
- **Bike + metro/train**: in the HSL area, bikes can be taken on the metro and commuter trains, so the map
  also models riding to the nearest station and continuing by rail.
- **Cost rule**: cycling is free and a trip costs €3.50, so the bike is recommended unless a paid
  option saves more than 5 minutes.
- **Search, pins and location**: search a place (OpenStreetMap Nominatim), right-click / long-press to
  drop a pin, or allow location access. The map centres there and shows the suggestion.
- **Minimal design**: hover for the suggestion, click for all travel times, collapsible legend.

## How it works
| Option | How the travel time is estimated |
|---|---|
| Bike only | Walking route distance; 15 km/h for the first 2.5 km (city traffic), 20 km/h after that |
| Public transport | Rush-hour travel time from the Helsinki Region Travel Time Matrix |
| Bike + metro/train | Nearest station (`sjoin_nearest`), straight-line distance × 1.3 at 15 km/h, plus the rush-hour public transport time from the station's grid cell |

| Weather | Rule |
|---|---|
| Normal | Bike, unless a paid option saves more than 5 min |
| Rain | Bike only if the ride takes 15 min or less |
| Snowing, ≥5 cm snow in the last 8 h, or below −15 °C | No cycling |

## Run it yourself
```bash
conda env create -f environment.yml
conda activate carfree-bike-guide
jupyter lab carfree_bike_guide.ipynb
```
Running all cells creates `docs/index.html`, which is published with GitHub Pages.

## Data and credits
- **Helsinki Region Travel Time Matrix**, Digital Geography Lab, University of Helsinki (CC BY 4.0)
- **Metro and train stations and lines**: City of Helsinki open data WFS (CC BY 4.0)
- **Weather**: [Open-Meteo.com](https://open-meteo.com/) (CC BY 4.0)
- **Place search**: OpenStreetMap Nominatim; **base map**: Esri World Gray Canvas

This project started as my final map for **Exercise 5 of Automating GIS Processes 2026**
([AutoGIS](https://autogis-site.readthedocs.io/), University of Helsinki).

## Limitations
- All suggestions are for trips **to Helsinki Central Railway Station**, because the travel time data
  only covers that destination.
- Bike times are estimates with fixed speeds; electric bikes are usually faster.
- Bikes fit on trains and metro at rush hour only if there is space.
- Weather is measured at one point in central Helsinki.

## Roadmap
- Trips between any two places with HSL's Digitransit routing API
- Personal speed settings (e.g. e-bike)
- Season ticket mode (no per-trip fare)

## AI use
Built with help from Claude (Anthropic), which wrote much of the Python and JavaScript code. The
topic, rules, design decisions and testing are mine.

## License
Code: MIT. Data belongs to its providers (see above).
