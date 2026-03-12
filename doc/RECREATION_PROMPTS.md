# MASTER PROMPT FOR UNITY TERRAIN ROADS & BUILDINGS SYSTEM

**Project Name:** Unity Terrain Roads & Buildings System  
**Repository:** `gadget114514/unity-terrain-roads-buildings`  
**Engine:** Unity 6  
**Target Platform:** PC/Editor

## Core Requirements:

### 1. Road Painting System
- Create `RoadPainter.cs` script that:
  - Accepts spline points (Vector3 array) as input
  - Paints roads on Unity Terrain using terrain painting API
  - Configurable road width (default 5-10 units)
  - Supports height offset adjustment for road elevation
  - Includes material blending with surrounding terrain
  - Has methods: `PaintRoad(Vector3[] splinePoints)`, `ClearRoad(Vector3[] splinePoints)`

- Create `RoadConfig.cs` Scriptable Object with:
  - Road width (float)
  - Height offset (float)
  - Blend radius (float)
  - Blend falloff curve (AnimationCurve)
  - Road material reference

- Create `RoadSegment.cs` data structure with:
  - Start point (Vector3)
  - End point (Vector3)
  - Width (float)
  - Metadata (name, type)

### 2. Terrain Management System
- Create `TerrainManager.cs` script that:
  - References Unity Terrain component
  - Manages multiple terrain layers
  - Handles splat map manipulation
  - Supports layer blending with smooth transitions
  - Methods: `BlendLayers(int layer1, int layer2, float blendAmount)`, `GetTerrainHeight(Vector3 position)`

- Create `TerrainLayerConfig.cs` Scriptable Object with:
  - Layer name (string)
  - Diffuse texture reference
  - Normal map reference
  - Smoothness value (float 0-1)
  - Metallic value (float 0-1)

- Create `MaterialBlender.cs` script that:
  - Handles smooth transitions between terrain layers
  - Uses blend radius and falloff curves for natural blending
  - Modifies splat maps dynamically
  - Ensures no hard edges between terrain types

### 3. Building Data Extraction System
- Create `BuildingData.cs` with serializable structure containing:
  - Building ID (unique identifier)
  - Building name (string)
  - Building type (enum: Residential, Commercial, Industrial, Mixed, Other)
  - Position (Vector3 - Unity world coordinates)
  - Position in OSM (latitude, longitude)
  - Dimensions (length, width, height)
  - Footprint area (float)
  - Roof type (string)
  - Building levels (int)
  - Additional tags (Dictionary<string, string>)
  - Hash for uniqueness checking (int)

- Create `BuildingCollection.cs` script that:
  - Stores list of distinct BuildingData objects
  - Implements deduplication logic (hash-based)
  - Methods: `AddBuilding(BuildingData)`, `GetDistinctBuildings()`, `RemoveDuplicates()`
  - Prevents duplicate entries based on position and name hashing

- Create `JSONExporter.cs` script that:
  - Serializes BuildingCollection to JSON format
  - Exports to `Assets/Data/buildings.json`
  - Methods: `ExportToJSON(BuildingCollection)`, `ImportFromJSON(string filePath)`

### 4. OpenStreetMap Integration
- Create `OSMParser.cs` script that:
  - Reads OpenStreetMap XML files (.osm format)
  - Parses ways (for roads)
  - Parses buildings (nodes and ways)
  - Extracts relevant tags (name, building type, levels, etc.)
  - Methods: `ParseOSMFile(string filePath)`, `GetBuildings()`, `GetRoads()`

- Create `CoordinateConverter.cs` utility script that:
  - Converts OSM coordinates (latitude/longitude) to Unity world coordinates
  - Configurable map center point
  - Configurable scale factor (meters per unit)
  - Methods: `OSMToUnity(double lat, double lon)`, `UnityToOSM(Vector3 unityPos)`

### 5. Project Structure
```
Assets/
├── Scripts/
│   ├── Road/
│   │   ├── RoadPainter.cs
│   │   ├── RoadConfig.cs
│   │   └── RoadSegment.cs
│   ├── Terrain/
│   │   ├── TerrainManager.cs
│   │   ├── TerrainLayerConfig.cs
│   │   └── MaterialBlender.cs
│   ├── Building/
│   │   ├── BuildingData.cs
│   │   ├── BuildingCollection.cs
│   │   └── BuildingRenderer.cs
│   ├── OSM/
│   │   ├── OSMParser.cs
│   │   └── CoordinateConverter.cs
│   ├── Utilities/
│   │   ├── JSONExporter.cs
│   │   └── EditorTools.cs
│   └── Manager/
│       └── SystemManager.cs (Main coordinator)
├── Prefabs/
│   ├── Road.prefab
│   └── Building.prefab
├── Materials/
│   ├── Road.mat
│   ├── Terrain_Grass.mat
│   └── Terrain_Road.mat
├── Data/
│   ├── OSM_Data/ (input .osm files)
│   ├── buildings.json (output)
│   └── roads.json (optional output)
└── Scenes/
    └── Main.unity
```

### 6. Editor Features
- Create `EditorTools.cs` with menu items:
  - "Tools > Terrain Roads > Paint Road From Spline"
  - "Tools > Terrain Roads > Clear Road"
  - "Tools > OSM > Parse OSM File"
  - "Tools > OSM > Export Buildings to JSON"
  - "Tools > OSM > Visualize Buildings"

- Create `SystemManager.cs` that:
  - Coordinates all systems
  - Provides public methods for painting roads and extracting buildings
  - Handles data flow between components

### 7. JSON Output Format
Building data exported as:
```json
{
  "buildings": [
    {
      "id": "building_001",
      "name": "Main Street Building",
      "type": "Commercial",
      "position": {"x": 100, "y": 50, "z": 200},
      "osm_coordinates": {"latitude": 40.7128, "longitude": -74.0060},
      "dimensions": {"length": 25.5, "width": 18.3, "height": 12.0},
      "footprint_area": 466.65,
      "roof_type": "flat",
      "levels": 3,
      "tags": {"shop": "supermarket", "operator": "ABC Corp"}
    }
  ],
  "total_distinct_buildings": 1,
  "export_date": "2026-03-12"
}
```

### 8. Additional Requirements
- All scripts include XML documentation comments
- Serialize all Scriptable Objects and prefab references in Inspector
- Include error handling and validation
- Support undo/redo in Editor
- Performance optimized for large terrain areas
- Compatible with Unity 6 with no deprecated APIs

## Usage Example Prompt:
"I want to load an OpenStreetMap file, parse the building data, paint roads on the terrain, extract distinct buildings to JSON, and visualize them in the scene with proper material blending."