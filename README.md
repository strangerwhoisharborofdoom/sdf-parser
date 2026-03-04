# SDF Parser

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Gazebo](https://img.shields.io/badge/Gazebo-SDF-orange.svg)

A powerful Python package for parsing, validating, and analyzing Gazebo SDF (Simulation Description Format) files. Extract robot properties, visualize URDF/SDF structures, and automate simulation setup for robotics projects.

---

## 🚀 Features

- **Parse SDF/URDF files** - Load and parse simulation description files
- **Extract robot properties** - Get links, joints, sensors, and actuators
- **Validate SDF syntax** - Check for common errors and warnings
- **Visualize robot structure** - Generate tree diagrams of robot hierarchy
- **Export to various formats** - Convert between SDF, URDF, and JSON
- **Batch processing** - Process multiple robot models efficiently
- **Physics parameters extraction** - Get mass, inertia, friction, and collision data

---

## 📦 Installation

```bash
# Install from PyPI (coming soon)
pip install sdf-parser

# Or install from source
git clone https://github.com/strangerwhoisharborofdoom/sdf-parser.git
cd sdf-parser
pip install -e .
```

---

## 🔧 Quick Start

### Parse an SDF file

```python
from sdf_parser import SDFParser

# Load an SDF file
parser = SDFParser('robot_model.sdf')

# Parse the file
robot = parser.parse()

# Print robot information
print(f"Robot Name: {robot.name}")
print(f"Number of Links: {len(robot.links)}")
print(f"Number of Joints: {len(robot.joints)}")
```

### Extract link properties

```python
# Get all links
for link in robot.links:
    print(f"\nLink: {link.name}")
    print(f"  Mass: {link.mass} kg")
    print(f"  Inertia: {link.inertia}")
    
    # Get collision geometries
    for collision in link.collisions:
        print(f"  Collision: {collision.geometry_type}")
    
    # Get visual geometries
    for visual in link.visuals:
        print(f"  Visual: {visual.geometry_type}")
```

### Extract joint information

```python
# Get all joints
for joint in robot.joints:
    print(f"\nJoint: {joint.name}")
    print(f"  Type: {joint.joint_type}")
    print(f"  Parent: {joint.parent}")
    print(f"  Child: {joint.child}")
    print(f"  Axis: {joint.axis}")
    print(f"  Limits: {joint.limit_lower} to {joint.limit_upper}")
```

### Validate SDF file

```python
from sdf_parser import SDFValidator

validator = SDFValidator('robot_model.sdf')
errors, warnings = validator.validate()

if errors:
    print("Errors found:")
    for error in errors:
        print(f"  - {error}")

if warnings:
    print("Warnings:")
    for warning in warnings:
        print(f"  - {warning}")
```

### Visualize robot structure

```python
from sdf_parser import SDFVisualizer

visualizer = SDFVisualizer(robot)

# Generate tree diagram
visualizer.plot_tree('robot_structure.png')

# Export to graphviz DOT format
dot_content = visualizer.to_dot()
with open('robot_structure.dot', 'w') as f:
    f.write(dot_content)
```

### Convert between formats

```python
from sdf_parser import SDFConverter

# SDF to URDF
converter = SDFConverter()
converter.sdf_to_urdf('robot.sdf', 'robot.urdf')

# URDF to SDF
converter.urdf_to_sdf('robot.urdf', 'robot.sdf')

# SDF to JSON
converter.to_json('robot.sdf', 'robot.json')
```

---

## 📚 API Reference

### SDFParser Class

```python
SDFParser(file_path: str)
```

**Methods:**
- `parse() -> Robot`: Parse the SDF file and return a Robot object
- `get_model_names() -> List[str]`: Get all model names in the SDF
- `get_world_info() -> WorldInfo`: Get world information (gravity, etc.)

### Robot Class

```python
Robot(name: str, links: List[Link], joints: List[Joint], models: List[Model])
```

**Attributes:**
- `name`: Robot/model name
- `links`: List of Link objects
- `joints`: List of Joint objects
- `models`: List of nested models (for hierarchical robots)

### Link Class

```python
Link(name: str, mass: float, inertia: Inertia, collisions: List[Collision], visuals: List[Visual])
```

**Attributes:**
- `name`: Link name
- `mass`: Mass in kg
- `inertia`: 3x3 inertia matrix
- `collisions`: Collision geometries
- `visuals`: Visual geometries

### Joint Class

```python
Joint(name: str, joint_type: str, parent: str, child: str, axis: Vector3, limits: JointLimits)
```

**Attributes:**
- `name`: Joint name
- `joint_type`: revolute, prismatic, fixed, continuous, etc.
- `parent`: Parent link name
- `child`: Child link name
- `axis`: Joint axis vector
- `limits`: Joint position/velocity/effort limits

---

## 🛠️ Command Line Interface

```bash
# Parse and display robot info
sdf-parser info robot.sdf

# Validate SDF file
sdf-parser validate robot.sdf

# Extract link properties
sdf-parser links robot.sdf

# Extract joint properties
sdf-parser joints robot.sdf

# Convert to URDF
sdf-parser convert robot.sdf --format urdf --output robot.urdf

# Visualize structure
sdf-parser visualize robot.sdf --output robot_tree.png

# Batch process multiple files
sdf-parser batch process/*.sdf --output results.json
```

---

## 📋 Supported SDF Elements

### Geometries
- ✅ Box
- ✅ Cylinder
- ✅ Sphere
- ✅ Plane
- ✅ Mesh (COLLADA, OBJ)
- ✅ Heightmap

### Joint Types
- ✅ Fixed
- ✅ Revolute
- ✅ Continuous
- ✅ Prismatic
- ✅ Ball
- ✅ Screw

### Sensors (Partial)
- ✅ IMU
- ✅ Contact
- ✅ Ray (LIDAR)
- ✅ Camera
- ⏳ Depth Camera
- ⏳ RGB-D Camera

---

## 🧪 Testing

```bash
# Install test dependencies
pip install -e .[test]

# Run all tests
pytest

# Run with coverage
pytest --cov=sdf_parser --cov-report=html

# Run specific test file
pytest tests/test_parser.py
```

---

## 📁 Project Structure

```
sdf-parser/
├── sdf_parser/
│   ├── __init__.py
│   ├── parser.py          # SDF/URDF parsing logic
│   ├── validator.py       # Validation rules
│   ├── visualizer.py      # Tree visualization
│   ├── converter.py       # Format conversion
│   ├── models.py          # Data classes (Robot, Link, Joint)
│   └── cli.py             # Command-line interface
├── tests/
│   ├── test_parser.py
│   ├── test_validator.py
│   └── fixtures/
│       └── *.sdf
├── examples/
│   ├── basic_usage.py
│   ├── batch_processing.py
│   └── robot_analysis.ipynb
├── docs/
│   ├── api.md
│   └── tutorials.md
├── setup.py
├── pyproject.toml
└── README.md
```

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Open Robotics](https://www.openrobotics.org/) for Gazebo and SDF specification
- [ROS](https://www.ros.org/) community for URDF tools and inspiration
- [sdformat](https://github.com/gazebosim/sdformat) - Official SDF parsing library

---

## 📞 Contact & Support

**Author:** Pavan C N  
**GitHub:** [@strangerwhoisharborofdoom](https://github.com/strangerwhoisharborofdoom)  
**Issues:** [Report bugs or request features](https://github.com/strangerwhoisharborofdoom/sdf-parser/issues)  

---

## 🚀 Roadmap

### Version 0.1.0 (Current)
- ✅ Basic SDF parsing
- ✅ Link and joint extraction
- ✅ SDF validation
- ✅ CLI tool

### Version 0.2.0 (Planned)
- ⏳ URDF support
- ⏳ Format conversion
- ⏳ Tree visualization
- ⏳ Sensor parsing

### Version 0.3.0 (Future)
- ⏳ Trajectory analysis
- ⏳ Collision checking
- ⏳ Kinematics/dynamics computation
- ⏳ Integration with Gazebo simulator

---

*Built with ❤️ for the robotics community*
