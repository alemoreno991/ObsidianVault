### Existing issues

- [ ] [Github Issue]: Write lla2ned coordinate converter

- [ ] Investigate how to modules should communicate in the companion computer
- [ ] Investigate ModelAI Modal Pipe Architecture ModalAI prio/medium PX4

- [ ] HW Lab Design Definition HW Lab prio/critical
### New issues

- [ ] Set up the `ref_lla` to be something other than simply the first position that is broadcasted
- [ ] Define how many waypoints we will receive per time and how we are going to manage that internally. -> Very important for mission 1 already
## CICD

### Existing issues

- [ ] [CICD] Investigate how we can deploy GNC as a bazel Module in a Bazel Registry Bazel cicd
- [ ] [CICD] Investigate how to spin up a Gazebo docker image to execute tests that requires an active simulated drone. cicd
- [ ] [CICD] Add a Tekton step in the CICD to run the tests and shows the checks in github actions Bazel cicd

- [ ] [Github Issue]: Create a Clean Bazel Build for Path Planner Bazel
- [ ] [BUG] CI is failing when cloning the submodules is enabled. kind/defect
- [ ] [Work Item]: Use bazel px4 microcontroller-specific toolchain(s) to build px4 firmware images
- [ ] Perform the cross-compilation needed to deploy PX4 on the QBR5165 Platform ModalAI PX4
- [ ] Explicitly define the toolchain used by Bazel
- [ ] [Github Issue]: Adjust CMake files for OMPL OMPL prio/low proof-of-concept
- [ ] git clone public repositories is randomly failing when using Araic WIFI prio/high
- [ ] [Github Issue]: Migrate "Large" Repo files to Github Large File Storage (GLFS)
- [ ] Use our internal forked MAVSDK instead of the public one as Third-party dependency.

### New issues

- [x] Pure bazel build for `zenoh`
- [x]  Update bazel version and remove `--non_hermetic_tmp` thingy

## Style and documentation 

### Existing issues

- [ ] [Github Issue]: Write a doxygen config file kind/debt prio/low
- [ ] [Github Issue]: Discuss error handling, conversion, constant expressions etc.
- [ ] How to manage errors?
- [ ] Enforce consistent style across the repository kind/debt prio/medium
- [ ] Document (high-level) the trajectory package with README
- [ ] [Github Issue]: Migrate from `#ifndef` to `#pragma` once
- [ ] [Chore] Apply pre-commits on all folders.
- [ ] [Github Issue]: Adjust packages visibility prio/low
- [ ] Wrap custom types into the ARAIC namespace
- [ ] [Doc] Add documentation for the setting up the ModalAI Sentinel drone ModalAI

### New issues

- [x] Discuss about naming convention.
- [x] Discuss about what style-guide to enforce with the `pre-commit` hooks (e.g. cppcoreguidelines).
- [x] Tag `demo 0` commit for posterity.
- [x] Fix the `pre-commit` hook to not analyze system dependencies.
## Commander 

### Existing issues 

- [ ] Move private functions to the corresponding access specifier commander

### New issues

- [x] Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- [x] Change the name of the queues:
	- `sys_to_gnc_queue_` -> `from_gateway_queue_`
	- `gnc_to_sys_queue_` -> `to_gateway_queue_`
	- `drone_to_gnc_queue_` -> `from_mavbridge_queue_`
	- `sys_to_drone_queue_` -> `to_mavbridge_queue_` 
- [x] Maybe change `getIsArmed()` -> `IsArmed()` (the function that indicates if the drone is armed or not)
- [x] Maybe change `setIsArmed(bool is_armed)` -> `IsArmed(bool is_armed)` (probably this should be set by listening to the mavreader)
- [x] Maybe change `getIsAirborne()` -> `IsAirborne()` (the function that indicates if the drone is airborne or not)
- [x] Maybe change `setIsAirborne(bool is_airborne)` -> `IsAirborne(bool is_airborne)` (probably this should be set by listening to the mavreader)
- [x] Maybe change `getDroneState()` -> `getDroneState(enum option)` (return the desired state, i.e pos, vel, accel, yaw, yaw_rate, etc).
- [x] Add the ability to pass (either in the constructor or by the BPRT or mixture) a `configuration` to set the configurations to use by 
	- `path planner`: 
	```cpp
typedef struct {
  double time_constraint;
  std::string env_file_path;
  std::string robot_file_path;
  std::string planner_name;
  Eigen::Vector3d lower_bounds;
  Eigen::Vector3d upper_bounds;
  double resolution;
} PathPlannerOMPLConfiguration_t;
```
- `trajectory generator`:
```cpp
typedef struct {
  // Currently it is more focused on the parameters that must be passed to the trajectory planner
  // but this is still a WIP
  std::vector<TrajectoryConstraint_t> trajectory_constraints;
  double max_velocity;
  double max_acceleration;
  double max_jerk;
  double max_snap;
  size_t problem_dimensions;
  size_t polynomial_order;
  size_t continuity_order;
  size_t derivative_order;
  bool polish_flag;
  bool verbose_flag;
  double avg_velocity;
  Yaw_t yaw;
} CommanderConfig_t;
```
- [x] Are `_CheckForSystemMessages`, `_CheckForDroneMessages`, `_SendMessageToDrone` and `__SendMessageToSystem` needed?
- [x] How are we gonna deal with debug logging?
- [x] Move the schema for the `action` messages coming from the gateway to a common file we (GNC and BPRT) can reference to from anywhere. 
- [x] Move the schema for the `command` messages going to the mavbridge to a common file we (GNC) can reference to from anywhere. 

# Mavbridge Reader

### Existing issues 

- [ ] Unify MavbridgeReader and MavbridgeSender base implementation kind/defect

### New issues 

- [x] Create a MavbridgeManager
- [x] - Absolute path (from root of repo) to the header files.
- [x] Maybe change the name of the queue
- [x] Capitalize the following:
```cpp
#ifndef mavbridge_reader_H
#define mavbridge_reader_H
```
- [x] The use of MACROS does not check the type. I think it's better to use `constexpr`
- [x] Move the schema for the `topic` definition going out of the mavbridge to a common file we (GNC and BPRT) can reference to from anywhere. 
- [x] The `print_usage` function is not needed.
- [x] Maybe put the `MavbridgeReader` class inside the `araic` namespace
- [x] Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- [x] I wonder if we want to have an `Initialize()` function that calls the `ConnectToDrone` and `EnableTelemetry` methods (which could be private in that case).
- [x] I wonder if the `EnableTelemetry` method should receive an argument that tells it which date to enable telemetry on.
- [x] I wonder why the `ConvertToString`, `ConvertConnectionResultToString`, `ConvertFlightModeToString` and `ConvertLandedStateToString` are outside of the class. Should they be moved to some `utils` directory?
- [x] The `try-catch` in the `ConnectToDrone` method is wrong (or at least very weird). It should not be split in two pieces.
- [x] Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- [x] Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- [x] Use `inline` for methods with very short implementation.
- [x] The maps should probably live at the top of the file or maybe even in a different file. Is `map` better and `unordered_map`?
- [x] Move the schema for the `info` messages going out from the mavbridge to a common file we (GNC) can reference to from anywhere. 
- [x] Is it necessary to implement the destroyer to "close" the connection?
- [x] **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The reader should only interact with this class(es).
- [x] Potentially implement one queue per topic to communicate the messages from the `mavreader`

# Mavbridge Sender

- [ ]  Maybe change the name of the queue
- [x] Absolute path (from root of repo) to the header files.
- [x]  Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- [x] Move the schema for the `command` messages comming to the mavbridge to a common file we (GNC) can reference to from anywhere. 
- [x] The maps should probably live at the top of the file or maybe even in a different file. Is `map` better and `unordered_map`?
- [x] Move the schema for the `trajectory` messages comming to the mavbridge to a common file we (GNC) can reference to from anywhere. 
- [x] I wonder if there should exist a `MavlinkSocket` class that takes care only of the connection process. Both the `mavbridge_reader` and `mavbridge_sender` are using the same stuff to connect. So, It makes sense to me to have this class I mention and pass it to the specific instance. Therefore any update or improvement on this class would automatically benefit both the sender and reader. Additionally, it would be simpler to test (avoid duplicating a very similar test).
- [x] Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- [x] Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- [x] Move the `GenerateTrajectoryFromJSON` outside of the mavbridge_sender.
- [x] Modify the way the sampling is conducted. Change the implemention of the `ThreadSafeQueue` to a `wait_and_pop(dt)` (or even a `wait_for_and_pop(dt)`)
- [x] Hardcoded conversion to degrees should be avoided. Create a `deg2rad` and a `rad2deg` function in the `utils` package.
- [x] Is it necessary to implement the destroyer to "close" the connection?
- [x] **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The sender should only interact with this class(es).

# Middleware

### Existing issues

- [ ] [Github Issue]: Refactor Middleware test kind/debt

### New issues

- [x]  Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- [x] The tolerance MACROS and the `Position3D` struct should be defined in the `utils` package. Also, what is `#pragma pack(push,1)` and `#pragma pack(pop)`?
- [ ] `BatteryStatus` struct should be moved to the `utils` package. 
- [ ] Maybe change the name of the queue
- [x] Move the schema for the `topic` definition going out of the mavbridge to a common file we (GNC and BPRT) can reference to from anywhere. 
- [x] Move the schema for the `info` messages going out from the mavbridge to a common file we (GNC) can reference to from anywhere. 
- [x] Definition of the constructor should live in the source file.
- [x] The `test_function` should not exist.
- [x] Is it necessary to implement the destroyer to "close" the publishers and session and other stuff?
- [x] Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- [x] Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- [x] Avoid using `std::cout`
- `_UnpackBytesviewToPosition` is probably best to refactor. 
```
I assume that bytesview is refering to raw data coming from zenoh, which means that it's created in a different machine (potentially). If this is true and the machine architecture of the sender is different from the machine architecture of the receiver (let's say 64-bits on one end and 32-bits on the other end) then could it be possible that this way of checking the number of elements fail? Same thing for the endianness and encoding.

Additionally, why not parse the elements and instantiate the Position3D? I'm worried that the way in which the compiler handles structs could not be reliable (e.g. I know that sometimes it adds padding in different ways depending on the optimization flag one uses).
```
- [x]  **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The middleware should only interact with this class(es).


# Path planner

### Existing issues

- [ ] [Github Issue]: Fix the Path Planner Origin Issue
- [ ] [Github Issue]: Investigate Collision Checkers in OMPL OMPL prio/medium

### New issues

- [x] The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual).


# Trajectory
### Existing issues

- [ ] [Github Issue]: Include timestamp in the JSON representation of generated trajectories

### New issues

- [x] Documentation about the implementation of the module.
- [x] The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual).

## Adimensional polynomial

### New issues 

- [x] Maybe it should disappear in favor of a `dimensional polynomial` implementation of the Taylor Serie. (This would solve the division by 0 problem and simplify the implementation of the `dimensional polynomial`).

## Dimensional polynomial

### New issues 

- [x] Maybe it should implement the Taylor Serie directly (This would solve the division by 0 problem and simplify the implementation).

## TargetAlignedTrajectory

### Existing issues 

- [ ] [Github Issue]: Adjust PointFixedYaw class in the trajectory generator

### New issues

- [ ] Moving target
- [ ] Considering the height of the target in the solution


# TrajectoryGenerator

### Existing issues

- [ ] [Github Issue]: Move the GenerateTrajectoryFromJSON function from the Mavbridge Sender module to TrajectoryGeneration module trajectory
### New issues 
- [x] The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual)
- [x] Documentation of the package
- [x] Fix the `trajectory generator` issue when some entry of the first and final position are the same

# ThreadSafeQueue

- [x] Add an argument to `wait_and_pop` to allow the user to tell how long to wait for.
- [x] Think about Folly instead of this custom implementation.



