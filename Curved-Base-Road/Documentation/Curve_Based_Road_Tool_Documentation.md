**Curve-Based Road Tool:**

Quick Start

1.  Create or import a voxel-based terrain, such as a Houdini
    HeightField.

2.  Draw or provide a curve defining the desired road path across the
    terrain.

3.  Connect the terrain and curve to the corresponding inputs of the
    Curve-Based Road Tool.

- Input 1: Curve defining the road path

- Input 2: Voxel-based terrain

4.  Use the exposed Road parameters to control the generated road,
    including:

- Road Width

- Edge Width

- Road Width Ramp

- Terrain Offset Ramp

5.  Use the Terrain → Carve parameters to control the road imprint on
    the terrain:

- Road Imprint Width

- Smoothness

6.  Use Mask Visualization and Voxel Layer Visualization to inspect the
    generated masks and terrain layers in the Houdini viewport.

7.  The tool provides separate outputs for the two data types:

- Road: Procedural road mesh

- Terrain: Modified voxel-based terrain with the generated road imprint

**Introduction**

The Curve-Based Road Tool is a procedural Houdini tool designed to
generate roads from artist-defined input curves across voxel-based
terrain, such as Houdini HeightFields.

The tool creates a procedural road mesh and applies a corresponding road
imprint to the input terrain. Artist-controlled parameters provide
control over key aspects of road generation, terrain modification, and
procedural variation.

**Design Goals**

- Generate a procedural road mesh and corresponding terrain imprint.

- Provide artist control through exposed parameters.

- Support road generation from artist-defined curve inputs.

**Problem Statement**

Manually creating roads across terrain requires repeated modeling and
adjustment to maintain the intended path, conform to the terrain
surface, and create the required terrain imprint. This process becomes
inefficient when generating multiple road variations or adapting roads
across different terrain conditions.

**Solution**

The Curve-Based Road Tool generates a procedural road from an
artist-defined input curve and applies a corresponding road imprint to
the input voxel-based terrain. The tool provides parameter-driven
controls for adjusting road generation and terrain modification.

**Features**

- **Procedural Curve-Based Road Generation —** Generates a road
  procedurally from an artist-defined input curve.

- **Procedural Road Imprint Generation —** Modifies the input
  voxel-based terrain to create a corresponding road imprint.

- **Road Falloff Mask —** Generates a mask representing the falloff from
  the road center toward its outer edges, which can be used by other
  tools such as Scatter.

**See**: *Outputs → Output Attribute*

- **Road Width Control —** Provides control over the width of the
  generated road.

- **Adjustable Road Imprint —** Provides adjustable parameters for
  controlling the road imprint on the terrain.

- **Edge Zone Mask —** Generates an edge zone mask around the modified
  road area on the terrain.

**See**: *Outputs → Output Voxel Layer*

**Workflow**

The Curve-Based Road Tool follows a procedural pipeline for generating a
road from an input curve and modifying the input voxel-based terrain to
create a road imprint.

Curve INPUT Terrain INPUT

↓ ↓

Curve Processing Terrain Processing

\\ /

\\ /

**Ray 1**

↓

┌─────────┴─────────┐

↓ ↓

Primary

Road Generation Terrain Modification

↓ ↓

└─────────┬─────────┘

Ray 2

↓

Secondary Terrain

Modification

↓

Ray 3

↓

┌────────┴────────┐

↓ ↓

Road OUTPUT Terrain OUTPUT

**Curve / Road Generation**

**Curve Input → Smooth → Attribute Cleanup → Resample → Ray 1 —
Bidirectional Ray Projection → Curve Frame Generation → Road Edge
Generation → Edge Construction → Road Surface Construction → Road
Falloff Mask → Ray 2 → Ray 3 → Final Road Cleanup → Terrain Offset →
Road Output**

**Curve Input**\
Defines the road path used for procedural road generation.

**Smooth**\
Smooths the input curve before subsequent processing.

**Attribute Cleanup**\
Removes unnecessary curve attributes before further processing.

**Resample**\
Resamples the curve to provide consistent point spacing for subsequent
operations.

**Ray 1 — Bidirectional Ray Projection**\
Projects the input curve onto the terrain using downward and upward ray
projections to conform curve points located above or below the terrain
surface.

**Curve Frame Generation**\
Generates the tangent, normal, and bitangent vectors used for
directional road edge construction.

**Road Edge Generation**\
Generates the inner and outer road boundary curves from the projected
center curve.

**Edge Construction**\
Constructs the road edge geometry from the generated inner and outer
boundary curves.

**Road Surface Construction**\
Constructs the road surface from the generated road boundary, then
merges it with the constructed edges.

**Road Falloff Mask**\
Generates a falloff mask based on the distance from the road surface.

**See:** *Generation Process → Road Falloff Mask*

**Ray 2 and Ray 3**

The road is reprojected onto the terrain after each terrain modification
stage to conform it to the updated terrain surface.

**Final Road Cleanup**\
Cleans and prepares the generated road geometry for the final output
stage.

**Terrain Offset**\
Applies a user-controlled offset to the final cleaned road along its
surface normal.

**Road Output**\
Outputs the final generated road mesh.

**Terrain Modification**

**Terrain Input → Normal → Ray 1 — Bidirectional Ray Projection →
Primary Road Mask → Primary Blur & Carve → Ray 2 → Secondary Blur &
Carve → Ray 3 — Final Road Reprojection → Edge Zone Mask → Terrain
Output**

**Terrain Input**\
Provides the voxel-based terrain used for road projection and terrain
modification.

**Normal**\
Generates terrain normals used during terrain processing and road
interaction.

**Primary Road Mask**\
Generates the primary mask defining the main area affected by the road
imprint.

**Primary Blur & Carve**\
Uses the primary road mask to modify the terrain and create the main
road imprint.

**See:** *Generation Process → Primary Blur & Carve*

**Ray 2 — Primary Terrain Reprojection**\
Projects the generated road onto the terrain after the primary terrain
modification.

**Secondary Blur & Carve**\
Applies a subtle secondary blur and terrain modification around the
primary road imprint using a secondary generated mask.

**Ray 3 — Final Road Reprojection**\
Projects the final generated road onto the final modified terrain.

**Edge Zone Mask**\
Generates an edge zone mask on the modified terrain.

**Terrain Output**\
Outputs the modified voxel-based terrain with the generated road
imprint.

**Installation**

1.  In Houdini, go to Assets → Install Asset Library....

2.  Select the downloaded .hdanc file.

3.  Choose the installation scope:

- Current HIP File Only – Available only in the current project.

- Scanned Asset Library Directories – Available throughout the current
  Houdini session.

4.  Click Install (or Install and Create to immediately create an
    instance of the HDA).

**Inputs**

The Curve-Based Road Tool requires two inputs:

- **Input Curve:** Defines the road path used for procedural road
  generation.

- **Voxel-Based Terrain:** Provides the terrain used for road projection
  and terrain modification.

**Outputs**

The tool provides two outputs: a generated road mesh and modified
voxel-based terrain.

- **Road Output**

> Outputs the procedurally generated road mesh based on the input curve
> and conformed to the modified terrain.
>
> **Output Attribute**
>
> \`road_falloff\` — The road falloff mask.

- **Terrain Output**

> Outputs the modified voxel-based terrain with the generated road
> imprint.
>
> **Output Voxel Layer**
>
> \`edge_zone\` — The edge zone mask on the terrain.

**Parameters**

The Curve-Based Road Tool exposes parameters for controlling road width,
terrain projection, road falloff, terrain imprinting, edge zone
generation, and mask visualization.

**Road**

**Basic**

**Edge Width**\
Controls the width of the road edge area generated between the inner and
outer road boundaries. This width also contributes to the resulting road
imprint.

**Road Width**\
Controls the overall width of the generated road and defines the minimum
width of the resulting road imprint.

**See:** *Generation Process → Road Edge Generation*

**Road Width Ramp**\
Controls the variation of road width along the input curve. Changes to
road width also affect the resulting road imprint.

**Terrain Offset Ramp**\
Controls the distribution of the vertical offset along the road curve.
The ramp determines how the offset is applied along the curve from its
beginning to its end.\
**See**: *Generation Process → Terrain Offset.*

**Radius**\
Controls the maximum magnitude of the vertical offset applied to the
road curve.\
**See**: *Generation Process → Terrain Offset.*

**Mask**

**Falloff Mask Ramp**\
Controls the remapping curve applied to the generated road falloff
mask.\
**See**: *Generation Process → Road Falloff Mask.*

**Mask Visualization**\
Selects the mask to visualize in the Houdini viewport
<span dir="rtl"></span>and unreal engine.

**Terrain**

**Carve**

**Road Imprint Width**\
Controls how far the road imprint extends outward from the road edges,
with the generated road width defining the minimum imprint width.\
**See**: *Generation Process → Primary Blur & Carve and Secondary Blur &
Carve.*

**Smoothness**\
Controls the smoothing applied during road imprint generation.\
**See:** *Generation Process → Primary Blur & Carve and Secondary Blur &
Carve.*

**Edge Zone Mask**

**Outer Edge Expansion**\
Controls how far the edge zone mask expands outward from the outer road
edge.\
**See:** *Generation Process → Edge Zone Mask.*

**Inner Edge Expansion**\
Controls how far the edge zone mask expands inward from the inner road
edge.\
**See**: *Generation Process → Edge Zone Mask.*

**Blur**\
Controls the amount of blur applied to the edge zone mask.\
**See**: *Generation Process → Edge Zone Mask.*

**Voxel Layer Visualization**\
Selects the voxel layer to visualize in the Houdini viewport.

**Generation Process**

**Curve Preprocessing**

The input curve is processed before road generation. The curve is
smoothed, unnecessary attributes are removed, and the curve is resampled
to provide consistent point spacing for subsequent road-generation
stages.

**Terrain Projection**

The processed input curve is projected onto the input terrain using
bidirectional ray projection. A downward and an upward ray are used to
project curve points located above or below the terrain surface onto the
terrain.

**Curve Frame Generation**

A PolyFrame operation generates the tangent, normal, and bitangent
vectors used for directional road boundary construction.

**Road Edge Generation**

The projected center curve is offset using the bitangent direction to
generate the inner and outer road boundary curves.

**Edge Construction**

The inner and outer road boundaries are separated into edge groups and
used to construct the road edge geometry.

**Road Surface Construction**

The generated road boundary curves are used to construct the road
surface. The resulting surface is remeshed for subsequent terrain
interaction and mask generation, then merged with the constructed edges.

**Road Falloff Mask**

A road falloff mask is generated from the distance between the road
surface and the projected input curve. The mask is blurred and
normalized to define the falloff from the road center toward its outer
edges.

**Primary Road Mask**

A primary road mask is generated on the terrain from the generated road
mesh to define the main area affected by the road imprint.

**Primary Blur & Carve**

The primary road mask is strongly blurred to define the main area of
terrain modification, which is then used to create the primary road
imprint.

**Ray 2 — Primary Terrain Reprojection**

After the primary terrain modification, the generated road is projected
onto the modified terrain before the secondary terrain modification
stage.

**Secondary Blur & Carve**

After the road is projected onto the terrain modified by the primary
carve, a secondary mask is generated and subtly blurred before the
secondary terrain modification is applied.

**Ray 3 — Final Road Reprojection**

After the secondary terrain modification, the generated road is
projected onto the final modified terrain.

**Terrain Offset**

A user-controlled offset is applied to the final reprojected road along
its surface normal. The offset magnitude is controlled by the Radius
parameter, while the Terrain Offset Ramp controls how the offset varies
along the road curve.

**Edge Zone Mask**

The outer and inner extents and the blur of the mask are controlled by
the corresponding Edge Zone Mask parameters<span dir="rtl">.</span>

**Final Road Cleanup**

The generated road geometry is cleaned and prepared for the final road
output.

**Outputs**

The final generated road mesh and modified voxel-based terrain are
provided as the tool outputs.

**Examples**

**Performance**

Performance profiling and optimization are pending.

The tool includes terrain projection, terrain modification, mask
generation, and procedural road geometry processing. Further profiling
and optimization may be performed based on larger terrain inputs and
production-oriented use cases.

**Unreal Integration**

The Curve-Based Road Tool has been tested with Unreal Engine through the
Houdini Engine workflow.

**Tested Versions**

- Houdini 20.0.547

- Unreal Engine 5.2.1

The tool generates the procedural road and terrain-related outputs for
use within the tested Houdini–Unreal workflow.

**Requirements**

• Houdini Apprentice or higher license.

• Houdini version: Houdini Version 20.0.547

Unreal Engine integration requires Houdini Engine compatibility and an
appropriate Houdini Engine license.

Houdini Apprentice is intended for learning and non-commercial workflows
and does not support a full Houdini Engine production pipeline.

**Limitations**

- Requires a voxel-based terrain input for terrain projection and road
  imprint generation.

- Unreal Engine compatibility has been tested with the versions listed;
  compatibility with other versions has not been fully evaluated.

- Performance characteristics have not yet been comprehensively
  profiled.

**Known Issues (Pending)**

No known issues.

**Future Improvements / Roadmap**

- Performance profiling and optimization.

- Further testing across different terrain configurations and
  production-scale inputs.

- Additional improvements based on production testing and workflow
  requirements.

- Compatibility testing with additional Houdini and Unreal Engine
  versions.

**Changelog**

**Version 1.0.0**

— Initial release

**License**

This tool is provided for non-commercial, educational, and personal use.

The Curve-Based Road Tool was developed using Houdini Apprentice and is
distributed as a non-commercial Houdini Digital Asset (.hdanc).

**Credits**

**Tool Development**

Mina D.

**Documentation**

Mina D.

**Software**

- Houdini 20.0.547

- Unreal Engine 5.2.1

**Development Assistance**

AI-assisted learning and documentation workflow support provided through
OpenAI ChatGPT.

Document Information

Author: Mina D.

Version: 1.0.0

Last Updated: August 2026

License: Non-commercial (.hdanc)
