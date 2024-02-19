# NetLogo AR
## What Is It
NetLogo AR is a spatial Augmented Reality (AR) authoring toolkit that combines room-scale AR technology with NetLogo. It is freely distributed as a part of [Turtle Universe](https://turtlesim.com/products/turtle-universe/), the mobile version of NetLogo. [NetLogo](https://ccl.northwestern.edu/netlogo/) is an agent-based programming language widely used in scientific research and education. It enables the creation of room-scale multi-agent models, simulations, artworks, or games with existing NetLogo grammar. It also enables the seamless blending of existing NetLogo models into the physical world around you. 

The following figure demonstrates a sample transition included in this repository:

<img src="https://github.com/NetLogo-Mobile/NetLogo-AR/assets/12299703/a81825d8-165f-426e-8445-df0b270044da" alt="NetLogo AR Preview" width="480"/>

The following video provides a quick overview of the technical system:

[![NetLogo AR Video](https://img.youtube.com/vi/xJcEGpp6rCE/0.jpg)](https://www.youtube.com/watch?v=xJcEGpp6rCE)

If you have any questions or need technical support, please send your thoughts to [NetLogo's Official Forum](https://community.netlogo.org/). If you find any bugs when using the system, please [raise an issue here](https://github.com/NetLogo-Mobile/NetLogo-AR/issues).

### Supported Modalities
NetLogo AR currently supports three modalities:
![Examples of the three modalities](https://github.com/NetLogo-Mobile/NetLogo-AR/assets/12299703/6b7a71a5-3220-42c0-9841-3305d669fca8)
* **Room-scale AR**: In this modality, NetLogo AR will attempt to recognize your physical surroundings (e.g., walls, doors, tables, chairs) at a room-scale. By default, it will attempt to visualize the physical items as semi-translucent boxes. They will also be mapped to the model as polygons or lines that can interact with NetLogo agents.
* **Plane-based AR**: In this modality, NetLogo AR will attempt to recognize planes in your physical surroundings (e.g., walls, floors, tables) as polygons. The outline of the polygon will also be mapped to the model as lines that can interact with NetLogo agents.
* **Non-AR**: In this modality, since the device cannot acquire information directly from the physical surroundings, NetLogo AR supports loading from an existing save. A save can be exported from a supported device (e.g. a LIDAR-equipped iPad or iPhone). Turtle Universe also embeds a scan that will be loaded by default.

Users can switch between (room-scale AR or plane-based AR) and non-AR at will. Switching between Room-scale AR or Plane-based AR requires a re-scan. Models created in any modality can be loaded from other modalities as needed. 

### Hardware Requirement
NetLogo AR is supported on all devices that Turtle Universe supports, which includes iOS, Android, Windows, macOS, and Chromebook. All devices can create a room-scale AR experience that works on all devices. However, only some devices support the visualization to the fullest extent. Here is a simplified matrix of NetLogo AR's device-modality compatibility:

| Device / Modalities         | Room-scale AR | Plane-based AR | Non-AR (Topdown View) |
|-----------------------------|---------------|----------------|-----------------------|
| iPad & iPhone with LIDAR    | Yes           | Yes            | Yes                   |
| iPad & iPhone without LIDAR | No            | Yes            | Yes                   |
| Android with ARCore         | No            | Yes            | Yes                   |
| Windows, macOS, Chromebooks | No            | No             | Yes                   |

### Frequently Asked Questions
**Q:** How can I know if my iOS device has LIDAR?
**A:** Apple provides a list of LIDAR-enabled devices [here](https://support.apple.com/en-us/102468#ipad).
**Q:** Does NetLogo AR support VisionOS?
**A:** We do have plans for VisionOS, but do not have a device at this moment. If you have one, please let us know. 
**Q:** How can I know if my Android device has ARCore?
**A:** If you see an "AR" button on the top-left in any models opened with Turtle Universe, your Android device likely has ARCore support.
**Q:** How do you install Turtle Universe on Chromebook?
**A:** You need to enable Google Play support on your Chromebook.

## How To Use It
### Try It Yourself
Before making a NetLogo model with NetLogo AR, consider playing with it first. We have made a few examples that you can play in Turtle Universe. Both models are open-sourced that could help you understand how it works. If you make or see an interesting AR model, please let us know. We are happy to include your model!
* [**Ants (AR)**](https://turtlesim.com/tuc/?global-model-63ffd904c53d30d62d4d58c7?): The AR version of the classical [Ants](https://www.netlogoweb.org/launch#https://www.netlogoweb.org/assets/modelslib/Sample%20Models/Biology/Ants.nlogo) model. The main difference is that ants interact with obstacles in the physical world, and you can play as an ant with infinite or limited vision. 
* [**Balls AR**](https://turtlesim.com/tug/?global-model-643ecde0c84b870b4099ce58?): An AR model that contains multiple balls (circles) doing random movement in the physical space. The balls will collide into real-world elements and each other. You can play as one ball in the world, with an option to attract other balls.

### Children's Projects
During our recent study in 2023, 7 children aged 11-13 in an after-school program created seven projects. Six of them are NetLogo AR projects. You might want to try them out as well:

### Create an AR model
Essentially, a NetLogo AR model is a NetLogo model using [XR](https://github.com/NetLogo-Mobile/Tortoise/blob/wip-tortuga/engine/src/main/coffee/extensions/xr.json) and [phys](https://github.com/NetLogo-Mobile/Tortoise/blob/wip-tortuga/engine/src/main/coffee/extensions/phys.json) extensions.

```
extensions [ xr phys ]
```

The `XR` extension provides primitives that allow you to convert a real-world scan into your NetLogo world. Specifically:
* **xr:room-scan** initiates the scan and calls back when the scan is complete. The user might load an existing scan, which also counts. This allows the model to run on non-AR devices.
* **xr:resize-world** automatically resizes the world based on the real-world dimensions. You can specify a minimum world size as an optional parameter.
* **xr:iterate-as-patches** allows you to iterate through scanned items (walls, tables, etc) as patches, so you can mark each patch as “impassable” or with a special flag.
* **xr:iterate-as-turtles** does the same, but could be used to create physical turtles.

For example, the following code performs a `setup`-like action when the user finishes a scan:
```
to setup
  xr:room-scan [ [ method id ] ->
    ; Resizing the world with a minimum size of 20
    xr:resize-world 20
    ; Clearing everything from the environment
    clear-all
    ; Initializing the obstacles
    xr:iterate-as-turtles "init-an-obstacle"
    ; Making the patches transparent
    ask patches [ set pcolor [ 0 0 0 0 ] ]
    ;;; Your real setup code below, for example, we create 50 balls below:
    create-turtles 50 [
      setxy random-xcor random-ycor
      set shape "circle"
    ]
  ]
end
```

Then, use the `phys` extension to create physical turtles with polygon shapes. For example, the following code converts scanned walls, windows, and doors into edges and other items as polygons: 
```
; Initialize a single obstacle
to init-an-obstacle [ class object points zmin zmax ]
  create-turtles 1 [
    set color xr:color-of class object ; Matching the color with the type of obstacle
    ifelse length points = 4 [ ; If the obstacle is a wall
      phys:make-edges true points ; Create the edges of the wall
    ] [
      set heading 0 ; Re-orient the heading to avoid bugs
      phys:set-type 1 ; Make the obstacles stay there (not pushed away by balls)
      phys:make-polygon true points ; Create the polygon of the obstacle
      phys:set-origin 0 0 ; Setting the origin of the polygon
    ]
  ]
end
```

Finally, use `xr:xcor` and `xr:ycor` to trace the world coordinates of the user. For example, the following code makes the red turtle follow the user's position:
```
; Makes the turtle 0 follow you
to go
  ask turtle 0 [
    setxy xr:xcor xr:ycor
    set heading xr:heading
  ]
end
```

Until here, you will have a skeleton AR model that spawns 50 balls, with one of them constantly follows the user's location. Note that the behavior of `xr:xcor` and `xr:ycor` differs in different AR modalities:
* In **room-scale AR**, it points to the user's real-world location mapped in the model space.
* In **plane-based AR**, it points to where the device's camera center is pointed, mapped in the model space.
* In **non-AR**, it points to where the user's viewport center is, mapped in the model space.

## License
The documentation and official samples in this repository adopt an MIT license. Individual authors, including children, own their respective models. 

The AR and Physics extensions of NetLogo Web, which serves as the infrastructure of NetLogo AR, adopt an MIT license. Commercial licenses are also available. To inquire about commercial licenses, please contact Uri Wilensky at uri@northwestern.edu.

## Citation Format
If you use NetLogo AR in your academic work, please cite in the following format:
* Chen, J. & Wilensky, U. (2023). NetLogo AR. Center for Connected Learning and Computer-Based Modeling, Northwestern University, Evanston, IL.

You may also be interested in the papers we published, which describe the way we use NetLogo AR with children:
* Chen, J., Horn, M., & Wilensky, U. (2023, June). NetLogo AR: Bringing Room-Scale Real-World Environments Into Computational Modeling for Children. In Proceedings of the 22nd Annual ACM Interaction Design and Children Conference (pp. 736-739).
* Chen, J., Zhao, L., Li, Y., Xie, Z., Wilensky, U., & Horn, M. S. (2024, May). “Oh My God! It’s Recreating Our Room!” Understanding Children’s Experiences with A Room-Scale Augmented Reality Authoring Toolkit. Proceedings of the 2024 CHI Conference on Human Factors in Computing Systems.
