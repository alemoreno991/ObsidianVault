
# Tech debt

- Define how many waypoints we will receive per time and how we are going to manage that internally. -> Very important for mission 1 already
- Set up the `ref_lla` to be something other than simply the first position that is broadcasted
- Potentially change the namespace we are using from `ARAIC` to araic to respect the code guidelines that we are using.
- Run the `pre-commit` in the whole repo
- Fix `pre-commit` hook to not analyze system dependencies.
- Tag the `demo0` commit for posterity
- Change `ThreadSafeQueues` custom type to `folly` or some other highly optimized thread-safe-queue implementation.

## Commander

- Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- Change the name of the queues:
	- `sys_to_gnc_queue_` -> `from_gateway_queue_`
	- `gnc_to_sys_queue_` -> `to_gateway_queue_`
	- `drone_to_gnc_queue_` -> `from_mavbridge_queue_`
	- `sys_to_drone_queue_` -> `to_mavbridge_queue_`
- Move `private` function (marked with `_foo`) to the appropriate access specifier section without breaking tests.
	- `_Takeoff`
	- `_Land`
	- `_GoToGoal`
	- `_Arm` 
	- `_Disarm` 
	- `_DecodeSystemMessage` (maybe even change name)
	- 
- Maybe change `getIsArmed()` -> `IsArmed()` (the function that indicates if the drone is armed or not)
- Maybe change `setIsArmed(bool is_armed)` -> `IsArmed(bool is_armed)` (probably this should be set by listening to the mavreader)
- Maybe change `getIsAirborne()` -> `IsAirborne()` (the function that indicates if the drone is airborne or not)
- Maybe change `setIsAirborne(bool is_airborne)` -> `IsAirborne(bool is_airborne)` (probably this should be set by listening to the mavreader)
- Maybe change `getDroneState()` -> `getDroneState(enum option)` (return the desired state, i.e pos, vel, accel, yaw, yaw_rate, etc).
- Add the ability to pass (either in the constructor or by the BPRT or mixture) a `configuration` to set the configurations to use by 
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
- Are `_CheckForSystemMessages`, `_CheckForDroneMessages`, `_SendMessageToDrone` and `__SendMessageToSystem`.
- How are we gonna deal with debug logging?
- Move the schema for the `action` messages coming from the gateway to a common file we (GNC and BPRT) can reference to from anywhere. 
- Move the schema for the `command` messages going to the mavbridge to a common file we (GNC) can reference to from anywhere. 

## Mavbridge

Is it being used?

- Create a `MavBridgeWrapper` to handle both the `reader` and `sender`
## MavbridgeReader

- Absolute path (from root of repo) to the header files.
- Maybe change the name of the queue
- Capitalize the following:
```cpp
#ifndef mavbridge_reader_H
#define mavbridge_reader_H
```
- The use of MACROS does not check the type. I think it's better to use `constexpr`
- Move the schema for the `topic` definition going out of the mavbridge to a common file we (GNC and BPRT) can reference to from anywhere. 
- The `print_usage` function is not needed.
- Maybe put the `MavbridgeReader` class inside the `araic` namespace
- Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- I wonder if we want to have an `Initialize()` function that calls the `ConnectToDrone` and `EnableTelemetry` methods (which could be private in that case).
- I wonder if the `EnableTelemetry` method should receive an argument that tells it which date to enable telemetry on.
- I wonder why the `ConvertToString`, `ConvertConnectionResultToString`, `ConvertFlightModeToString` and `ConvertLandedStateToString` are outside of the class. Should they be moved to some `utils` directory?
- The `try-catch` in the `ConnectToDrone` method is wrong (or at least very weird). It should not be split in two pieces.
- Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- Use `inline` for methods with very short implementation.
- The maps should probably live at the top of the file or maybe even in a different file. Is `map` better and `unordered_map`?
- Move the schema for the `info` messages going out from the mavbridge to a common file we (GNC) can reference to from anywhere. 
- Is it necessary to implement the destroyer to "close" the connection?
- **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The reader should only interact with this class(es).
- Potentially implement one queue per topic to communicate the messages from the `mavreader`
## MavbridgeSender

- Maybe change the name of the queue
- Absolute path (from root of repo) to the header files.
- Change `std::shared_ptr<ThreadSafeQueue<std::shared_ptr<nlohmann::json>>>` -> `std::shared_ptr<ThreadSafeQueue<std::unique_ptr<nlohmann::json>>>`.
- Move the schema for the `command` messages comming to the mavbridge to a common file we (GNC) can reference to from anywhere. 
- The maps should probably live at the top of the file or maybe even in a different file. Is `map` better and `unordered_map`?
- Move the schema for the `trajectory` messages comming to the mavbridge to a common file we (GNC) can reference to from anywhere. 
- I wonder if there should exist a `MavlinkSocket` class that takes care only of the connection process. Both the `mavbridge_reader` and `mavbridge_sender` are using the same stuff to connect. So, It makes sense to me to have this class I mention and pass it to the specific instance. Therefore any update or improvement on this class would automatically benefit both the sender and reader. Additionally, it would be simpler to test (avoid duplicating a very similar test).
- Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- Move the `GenerateTrajectoryFromJSON` outside of the mavbridge_sender.
- Modify the way the sampling is conducted. Change the implemention of the `ThreadSafeQueue` to a `wait_and_pop(dt)` (or even a `wait_for_and_pop(dt)`)
- Hardcoded conversion to degrees should be avoided. Create a `deg2rad` and a `rad2deg` function in the `utils` package.
- Is it necessary to implement the destroyer to "close" the connection?
- **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The sender should only interact with this class(es).

## Middleware

- The tolerance MACROS and the `Position3D` struct should be defined in the `utils` package. Also, what is `#pragma pack(push,1)` and `#pragma pack(pop)`?
- `BatteryStatus` struct should be moved to the `utils` package. 
- Maybe change the name of the queue
- Move the schema for the `topic` definition going out of the mavbridge to a common file we (GNC and BPRT) can reference to from anywhere. 
- Move the schema for the `info` messages going out from the mavbridge to a common file we (GNC) can reference to from anywhere. 
- Definition of the constructor should live in the source file.
- The `test_function` should not exist.
- Is it necessary to implement the destroyer to "close" the publishers and session and other stuff?
- Discuss how are we gonna deal with errors:
	- Return `int` or `bool`?
	- Throw exception? we need to define a strategy (our own exception classes to make the handling or those exceptions easier to filter)
- Discuss logging strategy (remember logging takes time):
	- What level to use (e.g debug, info, etc)
	- Should we define the logging messages as a `constexpr` at the top of the file to avoid cluttering the code?
- Avoid using `std::cout`
- `_UnpackBytesviewToPosition` is probably best to refactor. 
```
I assume that bytesview is refering to raw data coming from zenoh, which means that it's created in a different machine (potentially). If this is true and the machine architecture of the sender is different from the machine architecture of the receiver (let's say 64-bits on one end and 32-bits on the other end) then could it be possible that this way of checking the number of elements fail? Same thing for the endianness and encoding.

Additionally, why not parse the elements and instantiate the Position3D? I'm worried that the way in which the compiler handles structs could not be reliable (e.g. I know that sometimes it adds padding in different ways depending on the optimization flag one uses).
```
- **IDEA:** Refactor several parts. It seems to me that a somehow there should exist a class that packs and unpacks the messages that are passed around. The middleware should only interact with this class(es).


## Path Planner

This is the one module that I understand the least.

- The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual).

## Trajectory

- Documentation about the implementation of the module.
- The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual).

### Adimensional polynomial

- Maybe it should disappear in favor of a `dimensional polynomial` implementation of the Taylor Serie. (This would solve the division by 0 problem and simplify the implementation of the `dimensional polynomial`).

### Dimensional polynomial

- Maybe it should implement the Taylor Serie directly (This would solve the division by 0 problem and simplify the implementation).

### TargetAlignedTrajectory

- Moving target
- Considering the height of the target in the solution


## TrajectoryGenerator

- The destructor of the base class should be made virtual [link](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual)
- Documentation of the package
- Fix the `trajectory generator` issue when some entry of the first and final position are the same

# Next steps

- Allow preppending and appending polynomials to the piecewise polynomial that describes the translational trajectory
- Refactor and finish the `trajectory swapper`
- Local Mapper
- CI
	- Build
	- Automated tests (there should probably exist a bazel [sh_rule](https://bazel.build/reference/be/shell#sh_test)for some of these cases) 
		- Unit Tests that thoroughly test the functionality of the module. Try to test edge cases and provide an overall feeling that the module actually works properly. 
		- Integration tests (no-sitl): make sure that the different modules play nicely together. Thoroughly test the test cases that this integration is trying to test. Test edge cases and provide an overall feeling that the integration actually works properly. 
		- E2E tests (sitl): properly test that the solution works in different scenarios. Think about a lot of the edge cases that we want to cover with these tests
	- Coverage test analysis
	- Memory test (i.e. `valgrind`). Very important in languages like C++.
	- Performance test (i.e. speed of certain functions)
	- Tag tests (`unit`, `integration`, `sitl`, `hitl`, etc)
- CD
	- Deploy to the raspberry pi and the flight controller
	- HITL
# Bazel

- Pure bazel build for `zehoh`
- Pure bazel build for `ompl`
- Update bazel version and remove `--non_hermetic_tmp` thingy

# Code of Conduct

- Style convention.

