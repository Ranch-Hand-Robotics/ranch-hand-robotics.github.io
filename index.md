---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
software_features:
  - url: https://github.com/ranchhandrobotics/rde-urdf
    image_path: /assets/images/urdf_editor.png
    alt: "URDF Editor"
    caption: "A Visual Studio Code extension for editing and previewing URDF files."

  - url: https://github.com/ranchhandrobotics/rde-ros-1
    image_path: /assets/images/urdf_editor.png
    alt: "Debugging Tools for ROS 1"
    caption: "A Visual Studio Code extension for Debugging ROS 1 based Robots."
  
  - url: https://github.com/ranchhandrobotics/rde-ros-2
    image_path: /assets/images/urdf_editor.png
    alt: "Debugging  Tools for ROS 2"
    caption: "A Visual Studio Code extension for Debugging ROS 2 based Robots."

---

<div class="feature_row">
  <h2>Robotics Development Environment</h2>
  {% include wall id="software_features" layout="wall_half" %}
</div>


<div class="feature_row">
  <h2>Robotics Hardware</h2>
  {% include wall id="hardware_features" layout="wall_half" %}
</div>

<div class="feature_row">
  <h2>Robotics Services</h2>
  {% include wall id="services_features" layout="wall_half" %}
</div>

