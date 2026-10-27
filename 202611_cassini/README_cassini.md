# cassini
Contains materials relevant to the 12th CASSINI Hackathon (27-29 November 2026)

The 12th edition asks for space-enabled responses to instability in Ukraine and
other conflict-affected regions, built on Galileo and Copernicus. The notebooks
here are ready-made Copernicus demos: each runs an
[ESA APEx](https://github.com/ESA-APEx/apex_algorithms) openEO algorithm (UDP)
on the Copernicus Data Space Ecosystem (CDSE) over a real event in Ukraine and
saves a figure you can show straight away. 

## Notebooks

| Notebook | Event | Hackathon topic | Sensors | Output figure |
|---|---|---|---|---|
| [flood_detection_kherson](notebooks/flood_detection_kherson.ipynb) | Kakhovka dam breach, June 2023 | Crisis response and civil resilience | Sentinel-1, Sentinel-2 | [flood_detection_kherson_comparison.png](notebooks/flood_detection_kherson_comparison.png) |
| [ship_detection_mykolaiv](notebooks/ship_detection_mykolaiv.ipynb) | Port of Mykolaiv, September 2023 | Countering transnational crime (maritime monitoring) | Sentinel-1, Sentinel-2 | [ship_detection_mykolaiv.png](notebooks/ship_detection_mykolaiv.png) |
| [burnt_scar_luhansk](notebooks/burnt_scar_luhansk.ipynb) | Siverskyi Donets wildfires, Sept–Oct 2020 | Crisis response and civil resilience | Sentinel-2 | [wildfire_visualization_luhansk.png](notebooks/wildfire_visualization_luhansk.png) |

### Flood detection: Kakhovka dam breach, Kherson

Figure: <https://github.com/ESA-APEx/apex_events/blob/main/202611_cassini/notebooks/flood_detection_kherson_comparison.png>

The dam on the Dnipro was destroyed on 6 June 2023 and flooded the river
downstream. The notebook runs the `flood_detection` algorithm twice, once before
the breach and once during the flood. It marks water using Sentinel-1 radar,
which sees through cloud, and lays it over a Sentinel-2 background.

**Figure:** two maps side by side, before and during the flood. Water is blue
and land is grey. In the "during" panel, the islands and floodplain of the
Dnipro delta are underwater. This shows how radar can map damage to critical
infrastructure within days, whatever the weather. Note that the "during" image
is made from two Sentinel-1 passes, so you can see a seam near the western
edge.

### Ship detection: Dnipro-Bug estuary, Mykolaiv

Figure: <https://github.com/ESA-APEx/apex_events/blob/main/202611_cassini/notebooks/ship_detection_mykolaiv.png>

This notebook runs the `ship_detection` algorithm over the port of Mykolaiv and
the Southern Bug river. It finds water using Sentinel-2 and flags bright
Sentinel-1 radar returns on that water as vessels.

**Figure:** a true-colour map of the estuary with each detection circled in red
(293 ship pixels on 4–5 September 2023). It is a base for vessel monitoring:
port activity, sanctions or grain-corridor checks, or spotting ships that switch
off their AIS transponder. The detector is simple, so it also flags bright
objects that aren't ships, such as harbour structures and some spots over the
city and small inland ponds. Treat the circles as candidates to check, not
confirmed vessels.

### Burnt scar visualization: Siverskyi Donets, east of Luhansk

Figure: <https://github.com/ESA-APEx/apex_events/blob/main/202611_cassini/notebooks/wildfire_visualization_luhansk.png>

This notebook runs the `wildfire_visualization` algorithm on a cloud-free
Sentinel-2 scene from 5 October 2020. The image is true colour, with burnt
ground (low Normalised Burn Ratio) highlighted in yellow-orange.

**Figure:** large burn scars along the river, with smoke plumes from fires that
were still active. It shows how much land burned near the front line, which is
useful for damage assessment and planning humanitarian help. Some harvested
fields in the corners are also highlighted, so don't count every highlighted
patch as burnt.

## Running the notebooks

The Python environment is managed with [uv](https://docs.astral.sh/uv/).

```bash
cd 202611_cassini
uv sync
uv run jupyter lab
```

To use the environment as a VS Code Jupyter kernel, register it once:

```bash
uv run python -m ipykernel install --user --name cassini-notebooks --display-name "Python (cassini-notebooks)"
```

You need a free [CDSE account](https://dataspace.copernicus.eu/). The first cell
of each notebook opens a browser so you can log in. Processing runs as batch
jobs on CDSE and can take several minutes. The resulting GeoTIFFs are
downloaded next to the notebook. Each flood GeoTIFF is about 360 MB. They are
ignored by git.
