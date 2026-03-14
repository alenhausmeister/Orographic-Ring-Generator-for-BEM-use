Orthographic Ring Generator
A Grasshopper tool for generating a 3D horizon envelope and optional Sky View Factor (SVF) from PVGIS horizon data.

Overview
The Orthographic Ring Generator is a computational tool designed to evaluate horizon obstruction at micro locations by using elevation data sourced from the PVGIS platform, derived from Shuttle Radar Topography Mission (SRTM) datasets at a spatial resolution of 3 arc seconds (~90 m). Horizon angles for each site are calculated using the GRASS GIS Horizon module, which determines the elevation of surrounding terrain relative to a defined observation point.

A circular terrain influence zone with a 100 m radius is generated around the analysis point. This zone is subdivided into 48 evenly spaced azimuthal directions (7.5° increments), consistent with common solar analysis frameworks. For each direction, the vertical elevation angle is computed by comparing the height difference between the observer and surrounding terrain with the corresponding horizontal distance. The maximum elevation angle along each direction is then extracted to form the effective horizon profile.

These directional horizon angles are translated into three dimensional geometry within Grasshopper. Vertically offset points and directional vectors are used to construct polylines representing each of the 48 horizon segments. The segments are organized by azimuth and lofted into a continuous virtual horizon surface that encloses the analysis point.

This workflow enables precise modelling of terrain induced solar obstruction and provides a structured geometric representation suitable for environmental simulation, daylighting studies, and parametric design applications.

Features
Extracts horizon angles
Reads directional horizon obstruction from PVGIS horizon profiles.

Samples 48 azimuth direction
Automatically divides the analysis perimeter into 48 azimuths (7.5° increments) for consistent angular elevation.

Automated maximum elevation detection
Identifies the highest obstruction angle for each direction to generate an accurate horizon profile.

Grasshopper geometry generation
Converts a continuous 3D virtual horizon surface around selected analysis point.

Custom radius support
Allows manual adjustment of the analysis radius for alternative terrain assessments.

SVF (Sky View Factor)
Includes optional generation of SVF projections, available in orthographic or stereographic form.

Integration with environmental analysis tools
Design for use inside Rhino/Grasshopper workflows, supporting solar design applications.

Requirements
To successfully run the Orthographic Ring Generator, you need the following software installed:

Software:
Rhino 3D (version 7 or later),
Grasshopper (included with Rhino)
Ladybug Tools – version 1.9.0 (used for environmental analysis components inside Grasshopper)

External data source:
PVGIS Horizon data (.csv)
Downloadable from the Photovoltaic geographical information system platform: https://re.jrc.ec.europa.eu/pvg_tools/en/

*Rhino and Grasshopper also run on macOS, but Ladybug Tools support may vary.

HOW TO USE

I.	Download Horizon Data from PVGIS
1.	Visit the PVGIS Solar Tools platform
https://re.jrc.ec.europa.eu/pvg_tools/en/
2.	Use the map or enter coordinates to select your location.
3.	Download the horizon profile as a .csv file.
4.	Save the file into your project directory – this will be the data source for the Grasshopper workflow.

II.	Load the CSV File into Grasshopper
1.	Open Rhino and launch Grasshopper.
2.	Locate the Path component in the Orthographic Ring Generator script.
3.	Right-click the component > Select one existing file > navigate to your downloaded .csv file > Open.
4.	The script will automatically read the horizon height values for your chosen location.

III.	Terrain parametrization
Once the CSV is loaded, proceed to the terrain-processing group:
-	The script generates a circular analysis zone around the observation point.
-	The circle radius is defined by the Radius component – you can increase it if you wish to analyse a broader terrain area.
-	The algorithm divides the circle into 48 segments, each representing 7.5° increments.
-	Horizon heights from the CSV file are mapped onto these segments to create angular obstruction geometry.

IV.	Geometry Construction
-	A base circle is created using the Cir component.
-	Direction vectors and height values are combined to translate the raw horizon data into 3D points.
-	All elevated points are interpolated into a single 3D horizon curve.
-	Between the base circle and the elevated horizon curve, the script can generate ruled surfaces (RuleSfr).
-	The geometry is automatically rotated by 97.5 ° to align North with the green axis in Rhino’s gumball orientation.
-	A final Brep component constructs a closed 3D terrain-based horizon form.

V.	Sky View Factor (SVF) Calculation (Optional)
The workflow enables SVF calculation directly in Rhino/Grasshopper. You can toggle between two projection modes:
-	Stereographic
-	Orthographic

The selected projection will influence how the SVF is visualized in Rhino.

VI.	Adjust parameters as needed
You may customize:
-	Analysis radius
-	Output geometry type
-	SVF projection mode

All results update parametrically when any input value changes.

Methodology/How it works
The Orthographic Ring Generator converts horizon-obstruction data into a structured 3D geometric representation using PVGIS horizon datasets and Grasshopper’s parametric modelling capabilities. The workflow is composed of three main computational stages:

1.	Data Acquisition and Pre-processing
Horizon elevation data are downloaded as a .csv file from the PVGIS platform. In Grasshopper, the file path is supplied through the Path component, which imports the numerical values into the algorithm.

2.	Azimuth Subdivision and Angle mapping
A circular influence zone is generated around the selected observation point. The circle is subdivided into 48 equal angular segments, each representing 7.5° increments. This azimuth resolution aligns with the structure of the PVGIS horizon dataset.

For each azimuth direction:
-	The algorithm reads the corresponding horizon elevation angle from the imported .csv.
-	These values are mapped onto the circular geometry to create a directional obstruction profile.
-	The resulting set of 48 angles forms a full 360° horizon ring.

3.	3D horizon geometry construction
The numeric azimuth-angle pairs are transformed into continuous 3D geometry as follows:
-	Base circle setup: A planar base circle is generated around the analysis point to define the 48 sampling directions (7.5° increments) and serve as the spatial scaffold for point placement.
-	Angle-to-point translation: For each azimuth, the horizon elevation angle is converted into a vertical offset at the corresponding perimeter point on the base circle, producing 48 elevated 3D points.
-	Continuous horizon curve: The elevated points are interpolated into a single continuous 3D horizon curve.
-	Surface generation: The base circle and the elevated horizon curve are lofted to form a continuous ring-shaped horizon surface representing the obstructed sky envelope.
-	Orientation alignment: Rotate the resulting geometry by the 97.5° so that North aligns with Rhino’s green axis.
-	Closed brep output: From the lofted surface, construct a closed Brep to obtain a solid, unified geometric object suitable for visualization or further simulation (e.g. SVF).

Outputs

A structured list of 48 horizon elevation angles at 7.5° azimuth increments describing the effective 360° horizon profile around the observer.

A smooth, ring shaped surface created by lofting the base circle with the 3D horizon curve forming a continuous horizon envelope enclosing the analysis point.

The horizon geometry is rotated by 97.5° so that North aligns with Rhino’s green axis, ensuring consistent orientation for visualization and analysis.

A closed Brep constructed from the lofted surface, providing a solid, unified geometric entity that represents the obstructed sky dome and is ready for simulation or use as a reference model in Rhino.

SVF visualization computed directly in Rhino/Grasshopper with selectable Orthographic or Stereographic projection, enabling environmental analysis based on the generated horizon geometry.
