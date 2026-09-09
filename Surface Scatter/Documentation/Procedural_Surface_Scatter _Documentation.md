**Procedural Surface Scatter Documentation**

Quick Start

1.  Install the HDA.

2.  Place the Procedural Surface Scatter node in a Geometry network.

3.  Assign the input mesh to the HDA's physical input.

4.  Assign one or more Instance Geometry inputs through the parameter
    interface.

5.  Adjust Density Scale, Density Falloff Ramp, and scale parameters to
    control the scatter.

6.  Use Global Seed to generate different procedural variations.

7.  Adjust the procedural mask, cluster, neighbor filtering, and Surface
    Offset parameters as needed.

8.  Use Enable Input Mesh Output if the original input mesh should be
    included **in the final output.**

**Introduction**

**Procedural Surface Scatter** is a Houdini Digital Asset (HDA) that
procedurally distributes instance geometry across polygonal mesh
surfaces. It uses configurable procedural masks, controlled
randomization, and artist-driven parameters to generate natural-looking,
repeatable scatter distributions.

The tool is designed to reduce the manual effort of placing large
numbers of instances while providing flexible control over distribution,
density, scaling, and variation. It supports reusable surface-scattering
workflows for a wide range of environments.

**Design Goals**

- Produce natural-looking procedural distributions across polygonal mesh
  surfaces.

- Maintain artist control through exposed parameters and procedural
  masks.

- Support flexible mesh-based inputs for reusable surface scattering
  workflows.

- Generate deterministic, repeatable variations through seed-driven
  procedural controls.

**Problem Statement**

Manually placing large numbers of instances across polygonal mesh
surfaces is repetitive, time-consuming, and difficult to maintain
consistently. Achieving natural-looking distributions while preserving
artistic control often requires significant manual effort, making the
process inefficient and difficult to reproduce.

**Solution**

The Procedural Surface Scatter tool automates instance placement across
polygonal mesh surfaces using procedural masks, controlled
randomization, and configurable parameters. It combines deterministic
seed-driven variation with configurable distribution controls to
generate consistent procedural scatter distributions while providing
user control over instance placement.

**Features**

- **Procedural Scatter** – Procedurally distributes instances across
  polygonal mesh surfaces.

- **Multiple Instance Geometry – Supports multiple instance geometry
  references for scattering.**

- **Density Control** – Controls scatter density using the Density Scale
  and Density Falloff Ramp parameters.

- **Seed-Driven Variation** – Generates repeatable procedural variations
  through seed-controlled randomization.

- **Procedural Masks** – Computes height-, slope-, and edge-based masks
  with curve-remapped adjustment to control instance distribution.

- **Custom Mask Support** – Supports user-defined custom mask and custom
  exclusion mask point attributes to control instance distribution.

- **Global Scale Control** – Controls global instance scale with curve
  remapping.

- **Slope-Driven Scale Control** – Modulates instance scale based on
  surface slope using a curve-remapped scale ramp.

- **Edge Exclusion** – Excludes instance generation within a specified
  distance from the input mesh bounding box.

- **Cluster Distribution** – Optionally generates clustered
  distributions using Perlin noise with an adjustable cluster size.

- **Neighbor-Based Point Filtering** – Removes nearby scatter points
  based on a configurable neighbor search radius and Maximum Neighbors.

**Workflow**

The Procedural Surface Scatter tool follows a sequential procedural
workflow:

**Input Mesh → Procedural Mask Computation → Distribution Control →
Point Processing → Instance Generation → Output**

**See:** *Generation Process*

**1. Input Mesh**

Receives the polygonal mesh surface used for scatter generation.

**2. Procedural Mask Computation**

Computes height-, slope-, and edge-based masks from the input mesh and
combines them with user-supplied custom inclusion and exclusion masks to
generate the distribution mask.

**3. Distribution Control**

Applies optional cluster distribution and density remapping to the
distribution mask.

**4. Point Processing**

Generates scatter points across the input mesh, filters nearby points
using the neighbor-based filtering system, applies scale variation,
randomizes instance orientation, and offsets points along the surface
normal.

**5. Instance Generation**

Copies the selected instance geometry onto the generated scatter points
using the computed transformation attributes.

**6. Output**

Outputs the generated packed primitive instances and, optionally, the
original input mesh.

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

The Procedural Surface Scatter tool accepts the following inputs:

- Input Mesh – The polygonal mesh surface connected to the HDA's
  physical input connector and used for scatter generation.

- Instance Geometry – Geometry supplied through the HDA's instance
  parameters and copied onto the generated scatter points. Instance
  geometry is referenced through the parameter interface, not through a
  physical SOP input connection.

**See:** *Parameters → Instance Parameters → Instance Geometry*

**Outputs**

The Procedural Surface Scatter tool generates the following outputs:

- Generated Instances – Packed primitive instances distributed across
  the input mesh.

- Input Mesh (Optional) – The original input mesh, when the Enable Input
  Mesh Output option is enabled.

> **See:** *Parameters → Output Parameters*

**Parameters**

The Procedural Surface Scatter tool provides user-controlled parameters
for configuring instance inputs, distribution, procedural variation,
mask computation, clustering, and output generation.

**See:** *Generation Process*

**Instance Parameters**

**Number of Instances**

Specifies the number of instance geometry parameter slots available for
scatter generation. Empty slots are ignored.

**Instance Geometry**

Specifies the instance geometry referenced through each instance
parameter slot. Geometry can be assigned through the parameter interface
using a geometry or object path or by dragging and dropping the geometry
into the parameter.

When multiple instance geometries are supplied, the tool randomly
selects among them. Empty instance slots are ignored.

**Distribution Parameters**

**Density Scale**

Controls the density value used by the Houdini Scatter node. The
remapped distribution mask is stored as the
<span class="mark">density_mask</span> attribute and is used by the
Scatter node together with the Density Scale value to determine the
generated point density.

The HDA parameter has a displayed range of **0.1–10000**; the underlying
parameter has no enforced maximum.

**Density Falloff Ramp**

Remaps the distribution mask to control the final scatter density. The
remapped result is stored as the **density_mask** attribute used by the
Scatter node for point generation. The ramp uses a **0–1** range.

**Variation Parameters**

**Global Seed**

Controls the seed used for procedural randomization, including instance
selection, scale variation, and cluster generation. Changing the seed
produces different procedural variations while preserving the same
parameter settings.

**Global Instance Scale**

Controls the overall scale applied to generated instances.

**Scale Ramp**

Remaps the generated pscale values to control the final instance scale
distribution.

**Slope-Driven Scale Ramp**

Modulates instance scale based on the local surface slope using a
user-defined ramp.

**Procedural Mask Parameters**

**Visualization Mode**

Specifies which procedural mask is displayed for visualization.
Available modes include **No Color, Edge, Height Mask, Slope Mask,
Custom Mask, Custom Exclusion Mask, and Density Mask**.

**Edge Exclusion Distance**

Controls the distance from the input mesh bounding box within which
scatter generation is excluded. The edge mask evaluates the distance of
each point from the four X/Z boundaries of the input mesh bounding box.

**Custom Mask Attribute**

Specifies the name or names of point attributes containing normalized
values in the range **0–1**. Custom mask attributes are optional;
missing attributes are ignored.

**Custom Exclusion Mask Attribute**

Specifies the name or names of point attributes containing normalized
values in the range **0–1**. Custom exclusion mask attributes are
optional; missing attributes are ignored.

**Height Ramp Mask**

Remaps the height-based distribution mask.

**Slope Ramp Mask**

Remaps the slope-based distribution mask.

**Cluster Parameters**

**Enable Cluster Distribution**

Enables clustered scatter generation using Perlin noise.

**Cluster Size**

Controls the Perlin noise element size used to generate the cluster
distribution. The parameter has a UI range of **0–10**.

**Advanced Parameters**

**Maximum Neighbors**

Specifies the maximum number of neighboring scatter points considered
during neighbor-based point filtering.

**Neighbor Search Radius**

Defines the radius used to search for neighboring scatter points. The
filtering proximity threshold is derived from this value.

**Surface Offset**

Offsets generated scatter points along the surface normal to control
their placement relative to the input mesh. The parameter has a UI range
of **-5–5**.

**Output Parameters**

**Enable Input Mesh Output**

When enabled, merges the original input mesh with the generated
instances in the final output. When disabled, the output contains only
the generated instances.

**Generation Process**

The Procedural Surface Scatter tool generates instances through a
sequential procedural workflow that combines surface analysis,
procedural masks, density modulation, procedural variation, and
post-processing to produce controllable, repeatable scatter
distributions.

**See:** *Parameters*

**Surface Analysis**

The input mesh is processed to compute surface normals. These normals
are used for slope evaluation, slope-driven scale modulation, and
surface offset throughout the generation process.

**Procedural Mask Computation**

Height-, slope-, and edge-based masks are computed from the input mesh.
The height mask is derived from the normalized vertical position of each
point and remapped using the \*\*Height Ramp Mask\*\*. The slope mask is
derived from the surface normal relative to the world up direction and
remapped using the \*\*Slope Ramp Mask\*\*. The edge mask is evaluated
relative to the input mesh bounding box.

These height-, slope-, and edge-based masks are combined to produce the
base distribution mask.

**Custom Mask Processing**

User-defined custom mask and custom exclusion mask point attributes are
applied to the distribution mask. The attributes are expected to contain
normalized float values in the range **0–1**, and their names are
provided as string parameters. Custom mask attributes modulate the base
distribution mask, while custom exclusion mask attributes remove areas
from the distribution.

**Cluster Distribution**

When enabled, an Attribute Noise node configured with Perlin noise
generates the cluster distribution. The resulting cluster data is
multiplied with the distribution mask before density remapping. Cluster
size is controlled through the **Cluster Size** parameter.

**Density Remapping**

The distribution mask is remapped using the user-defined Density Falloff
Ramp. The remapped result is stored as the **density_mask** attribute
and used by the Scatter node to generate points.

**Point Distribution**

Scatter points are generated across the input mesh using the computed
density distribution.

**Neighbor-Based Point Filtering**

Generated scatter points are queried for neighboring points within the
specified Neighbor Search Radius, up to the Maximum Neighbors limit.
Points within the resulting proximity threshold are added to a removal
group and subsequently deleted to reduce excessive clustering.

**Scale Variation**

Random per-instance scale values are generated using the pscale
attribute. The scale is then processed in the following order:

**Global Instance Scale → Scale Ramp → Slope-Driven Scale Ramp**

The **Scale Ramp** remaps the current scale values. The **Slope-Driven
Scale Ramp** uses the local surface slope to generate a scale
multiplier, which is applied to the current pscale.

The **Scale Ramp** and **Slope-Driven Scale Ramp** use a **0–1** range.

**Orientation Randomization**

Randomizes the orientation of generated instances after scale variation
and before surface offset. This changes instance orientation without
modifying the scatter point normals, allowing instances such as trees to
maintain correct placement relative to the surface.

**Surface Offset**

Scatter points are offset along the surface normal to control the
placement of generated instances relative to the input mesh.

**Instance Generation**

The selected instance geometry is copied onto the generated scatter
points using the computed transformation attributes.

**Output Generation**

The generated instances are output as packed primitives. When **Enable
Input Mesh Output** is enabled, the original input mesh is merged with
the generated instances. When disabled, only the generated instances are
output. Temporary attributes are removed before the final output is
generated.

**Mask Visualization**

The tool provides a visualization mode for inspecting procedural masks
directly in Houdini. The selected mask can also be visualized in Unreal
Engine using the Mask Visualization Material, providing a consistent way
to inspect and validate mask output during engine integration.

**Examples**

**Example 1 – Basic Surface Scatter**

Scatter instances across a polygonal mesh using **Density Scale**,
**Density Falloff Ramp**, and **Global Instance Scale**.

**See:** *Parameters → Distribution Parameters*

**See:** *Parameters → Variation Parameters*

**Example 2 – Mask-Driven Scatter**

Control instance distribution using **Height Ramp Mask**, **Slope Ramp
Mask**, **Edge Exclusion Distance**, **Custom Mask Attribute**, and
**Custom Exclusion Mask**.

**See:** *Parameters → Procedural Mask Parameters*

**Example 3 – Clustered Scatter**

Generate clustered distributions using **Enable Cluster Distribution**
and **Cluster Size**.

**See:** *Parameters → Cluster Parameters*

**Example 4 – Neighbor-Based Spacing**

Control the spacing between generated instances using **Maximum
Neighbors** and **Neighbor Search Radius.**

**See:** *Parameters → Advanced Parameters*

**Performance (Pending)**

Performance testing has not yet been conducted.

Future performance evaluation will include:

- Measuring cook time at different scatter point counts.

- Comparing performance with neighbor-based point filtering enabled and
  disabled.

- Evaluating the effect of Neighbor Search Radius and Maximum Neighbors
  on cook time.

- Testing performance with different input mesh complexities.

- Recording representative results to identify potential performance
  bottlenecks.

**Unreal Integration**

The Scatter tool has been tested in Unreal Engine through Houdini
Engine.

The following features have been successfully validated:

- Height, slope, and edge masks.

- Combined distribution masks.

- Custom inclusion and exclusion masks.

- Density control and density remapping.

- Cluster-based distribution.

- Minimum-distance filtering.

- Instance scale variation.

- Slope-driven scale control.

- Surface offset.

- Scatter instance generation.

- Optional input mesh output.

Mask attributes were also visualized in Unreal using the provided Mask
Visualization Material to verify the generated mask values after Houdini
Engine cooking.

**Mask Visualization Material**\
Specifies the material used to visualize the selected mask in Unreal
Engine. The material is intended for mask inspection and debugging
rather than final asset shading.

The Mask Visualization Material is provided as an optional download.

**Requirements**

• Houdini Apprentice or higher license.

• Houdini version: Houdini Version 20.0.547

Unreal Engine integration requires Houdini Engine compatibility and an
appropriate Houdini Engine license.

Houdini Apprentice is intended for learning and non-commercial workflows
and does not support a full Houdini Engine production pipeline.

**Testing Status**

Unreal Engine integration has been validated for the tested workflow and
geometry used during development.

Behavior may vary depending on how the input geometry is generated,
transformed, or evaluated during Houdini Engine cooking. Broader
compatibility and production-level performance testing have not yet been
conducted.

**Limitations**

• The tool operates on polygonal mesh surfaces and does not directly
accept HeightField inputs.

**Known Issues (Pending)**

No known issues.

**Future Improvements / Roadmap**

Potential areas for future development include:

- Extended procedural distribution and masking controls.

- Further performance profiling and optimization.

- Expanded Unreal Engine integration and validation.

- Additional workflow controls based on production use and user
  feedback.

**Changelog**

**Version 1.0.0**

- Added procedural height, slope, and edge masks.

- Added custom inclusion and exclusion masks.

- Added optional clustered distribution.

- Added density remapping.

- Added neighbor-based point filtering.

- Added procedural scale variation and slope-driven scale control.

- Added surface offset control.

**License**

This tool is provided for non-commercial, educational, and personal use.

The Procedural Surface Scatter was developed using Houdini Apprentice
and is distributed as a non-commercial Houdini Digital Asset (.hdanc).

**Credits**

**Tool Development**

Mina D.

**Documentation**

Mina D.

**Software**

- Houdini FX 20.0.547

- Unreal Engine 5.2.1

**Development Assistance**

AI-assisted learning and documentation workflow support provided through
OpenAI ChatGPT.

Document Information

Author: Mina D.

Version: 1.0.0

Last Updated: August 2026

License: Non-commercial (.hdanc)

**Integration Testing**

- Houdini FX 20.0.547

- Unreal Engine 5.2.1
