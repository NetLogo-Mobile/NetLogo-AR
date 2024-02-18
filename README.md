# NetLogo AR
## What Is It
NetLogo AR is a spatial Augmented Reality (AR) authoring toolkit that combines room-scale AR technology with NetLogo. It is freely distributed as a part of [Turtle Universe](https://turtlesim.com/products/turtle-universe/), the mobile version of NetLogo. [NetLogo](https://ccl.northwestern.edu/netlogo/) is an agent-based programming language widely used in both scientific research and education. It enables the creation of room-scale multi-agent models, simulations, artworks, or games with existing NetLogo grammar. It also enables the seamless blending of existing NetLogo models into the physical world around you. 

The following figure demonstrates a sample transition included in this repository:
![NetLogo AR Video](https://github.com/NetLogo-Mobile/NetLogo-AR/assets/12299703/a81825d8-165f-426e-8445-df0b270044da)

The following video provides a quick overview of the technical system:

If you have any questions or need technical support, please feel free to send your thoughts to [NetLogo's Official Forum](https://community.netlogo.org/). If you find any bugs when using the system, please [raise an issue here](https://github.com/NetLogo-Mobile/NetLogo-AR/issues).

## Hardware Requirement
NetLogo AR is supported on all devices that Turtle Universe supports, which includes iOS, Android, Windows, macOS, and Chromebook. All devices can be used to create a room-scale AR experience that works on all devices. However, only some devices support the visualization to the fullest extent. Here is a simplified matrix of NetLogo AR's device-feature compatibility:

| Device / Features           | Room-scale AR | Plane-based AR | Non-AR (Topdown View) |
|-----------------------------|---------------|----------------|-----------------------|
| iPad & iPhone with LIDAR    | Yes           | Yes            | Yes                   |
| iPad & iPhone without LIDAR | No            | Yes            | Yes                   |
| Android with ARCore         | No            | Yes            | Yes                   |
| Windows, macOS, Chromebooks | No            | No             | Yes                   |

### Frequently Asked Questions
* How can I know if my iOS device has LIDAR?
Apple provides a list of LIDAR-enabled devices [here](https://support.apple.com/en-us/102468#ipad).
* Does NetLogo AR support VisionOS?
We do have plans for VisionOS, but do not have a device at this moment. If you happen to have one, please let us know. 

## How To Use It
### Try It Yourself
Before starting to make a NetLogo model with NetLogo AR, consider playing with it first. We have made a few examples that you can play in Turtle Universe. If you make or see an interesting AR model, please let us know. We are happy to include your model!

### Children's Projects
During our recent study in 2023, 7 children aged 11-13 in an after-school program created 7 projects. 6 of them are NetLogo AR projects. You might want to try them out as well:

### Adapt an Existing Model

### Create a new AR Model

## License
The documentation and official samples in this repository adopt an MIT license. Individual authors, including children, own their respective models. 

The AR and Physics extensions of NetLogo Web, which serves as the infrastructure of NetLogo AR, adopt an MIT license. Commercial licenses are also available. To inquire about commercial licenses, please contact Uri Wilensky at uri@northwestern.edu.

## Citation Format
If you use NetLogo AR in your academic work, please cite in the following format:
* Chen, J. & Wilensky, U. (2023). NetLogo AR. Center for Connected Learning and Computer-Based Modeling, Northwestern University, Evanston, IL.

You may also be interested in the papers we published, which describe the way we use NetLogo AR with children:
* Chen, J., Horn, M., & Wilensky, U. (2023, June). NetLogo AR: Bringing Room-Scale Real-World Environments Into Computational Modeling for Children. In Proceedings of the 22nd Annual ACM Interaction Design and Children Conference (pp. 736-739).
* Chen, J., Zhao, L., Li, Y., Xie, Z., Wilensky, U., & Horn, M. S. (2024, May). “Oh My God! It’s Recreating Our Room!” Understanding Children’s Experiences with A Room-Scale Augmented Reality Authoring Toolkit. Proceedings of the 2024 CHI Conference on Human Factors in Computing Systems.
