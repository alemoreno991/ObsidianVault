Tags: #simulation

---
## Download Worlds
It is possible to download *worlds* from [gazebosim](https://app.gazebosim.org/fuel/worlds). 

For example, the [tugbot_depot.sdf](https://fuel.gazebosim.org/1.0/sharukh/worlds/tugbot_depot) (see image below) can be downloaded into `px4-firmware/Tools/simulation/gz/worlds`
![[Pasted image 20240815163606.png]]

Then to launch the gazebo simulation with this world one needs to run the following command: 
```shell
PX4_GZ_WORLD=tugbot_depot make px4_sitl gz_x500
```

## Customization of the downloaded world

Based off of the `default` world one can modify the `tugbot_depot.sdf` as follows:
```xml
<?xml version='1.0' encoding='ASCII'?>
<sdf version='1.7'>
  <world name='depot'>
    <physics type="ode">
      <max_step_size>0.004</max_step_size>
      <real_time_factor>1.0</real_time_factor>
      <real_time_update_rate>250</real_time_update_rate>
    </physics>

    <gravity>0 0 -9.8</gravity>
    <magnetic_field>6e-06 2.3e-05 -4.2e-05</magnetic_field>
    <atmosphere type='adiabatic'/>

    <plugin name='gz::sim::systems::Physics' filename='gz-sim-physics-system'/>
    <plugin name='gz::sim::systems::UserCommands' filename='gz-sim-user-commands-system'/>
    <plugin name='gz::sim::systems::SceneBroadcaster' filename='gz-sim-scene-broadcaster-system'/>
    <plugin name='gz::sim::systems::Contact' filename='gz-sim-contact-system'/>
    <plugin name='gz::sim::systems::Imu' filename='gz-sim-imu-system'/>
    <plugin name='gz::sim::systems::AirPressure' filename='gz-sim-air-pressure-system'/>
    <plugin name='gz::sim::systems::Sensors' filename='gz-sim-sensors-system'>
      <render_engine>ogre2</render_engine>
    </plugin>

    <gui fullscreen='false'>
      <plugin name='3D View' filename='GzScene3D'>
        <gz-gui>
          <title>3D View</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='string' key='state'>docked</property>
        </gz-gui>
        <engine>ogre2</engine>
        <scene>scene</scene>
        <ambient_light>0.5984631152222222 0.5984631152222222 0.5984631152222222</ambient_light>
        <background_color>0.8984631152222222 0.8984631152222222 0.8984631152222222</background_color>
        <camera_pose>-6 0 6 0 0.5 0</camera_pose>
      </plugin>
      <plugin name='World control' filename='WorldControl'>
        <gz-gui>
          <title>World control</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='bool' key='resizable'>0</property>
          <property type='double' key='height'>72</property>
          <property type='double' key='width'>121</property>
          <property type='double' key='z'>1</property>
          <property type='string' key='state'>floating</property>
          <anchors target='3D View'>
            <line own='left' target='left'/>
            <line own='bottom' target='bottom'/>
          </anchors>
        </gz-gui>
        <play_pause>1</play_pause>
        <step>1</step>
        <start_paused>1</start_paused>
      </plugin>
      <plugin name='World stats' filename='WorldStats'>
        <gz-gui>
          <title>World stats</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='bool' key='resizable'>0</property>
          <property type='double' key='height'>110</property>
          <property type='double' key='width'>290</property>
          <property type='double' key='z'>1</property>
          <property type='string' key='state'>floating</property>
          <anchors target='3D View'>
            <line own='right' target='right'/>
            <line own='bottom' target='bottom'/>
          </anchors>
        </gz-gui>
        <sim_time>1</sim_time>
        <real_time>1</real_time>
        <real_time_factor>1</real_time_factor>
        <iterations>1</iterations>
      </plugin>
      <plugin name='Entity tree' filename='EntityTree'/>
    </gui>

    <scene>
      <ambient>1 1 1 1</ambient>
      <background>0.3 0.7 0.9 1</background>
      <shadows>0</shadows>
      <grid>1</grid>
    </scene>

    <light name='sunUTC' type='directional'>
      <pose>0 0 500 0 -0 0</pose>
      <cast_shadows>true</cast_shadows>
      <intensity>1</intensity>
      <direction>0.001 0.625 -0.78</direction>
      <diffuse>0.904 0.904 0.904 1</diffuse>
      <specular>0.271 0.271 0.271 1</specular>
      <attenuation>
        <range>2000</range>
        <linear>0</linear>
        <constant>1</constant>
        <quadratic>0</quadratic>
      </attenuation>
      <spot>
        <inner_angle>0</inner_angle>
        <outer_angle>0</outer_angle>
        <falloff>0</falloff>
      </spot>
    </light>

    <include>
      <uri>https://fuel.ignitionrobotics.org/1.0/OpenRobotics/models/Depot</uri>
      <pose>7.19009 1.09982 0 0 0 0</pose>
    </include>

    <model name='depot_collision'>
      <static>1</static>
      <pose>7.19009 1.09982 0 0 0 0</pose>
      <link name='collision_link'>
        <pose>0 0 0 0 0 0</pose>
        <collision name='wall1'>
          <pose>0 -7.6129 4.5 0 0 0</pose>
          <geometry>
            <box>
              <size>30.167 0.08 9</size>
            </box>
          </geometry>
        </collision>
        <collision name='wall2'>
          <pose>0 7.2875 4.5 0 0 0</pose>
          <geometry>
            <box>
              <size>30.167 0.08 9</size>
            </box>
          </geometry>
        </collision>
        <collision name='wall3'>
          <pose>-15 0 4.5 0 0 0</pose>
          <geometry>
            <box>
              <size>0.08 15.360 9</size>
            </box>
          </geometry>
        </collision>
        <collision name='wall4'>
          <pose>15 0 4.5 0 0 0</pose>
          <geometry>
            <box>
              <size>0.08 15.360 9</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes1'>
          <pose>0.22268 -4.7268 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes2'>
          <pose>3.1727 -4.7268 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes3'>
          <pose>5.95268 -4.7268 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes4'>
          <pose>8.55887 -4.7268 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes5'>
          <pose>11.326 -4.7268 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes6'>
          <pose>0.22268 -2.37448 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes7'>
          <pose>3.1727 -2.37448 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes8'>
          <pose>5.95268 -2.37448 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes9'>
          <pose>8.55887 -2.37448 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes10'>
          <pose>11.326 -2.37448 0.68506 0 0 0</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='boxes11'>
          <pose>-1.2268 4.1557 0.68506 0 0 -1.02799893</pose>
          <geometry>
            <box>
              <size>1.288 1.422 1.288</size>
            </box>
          </geometry>
        </collision>
        <collision name='pilar1'>
          <pose>-7.5402 3.6151 1 0 0 0</pose>
          <geometry>
            <box>
              <size>0.465 0.465 2</size>
            </box>
          </geometry>
        </collision>
        <collision name='pilar2'>
          <pose>7.4575 3.6151 1 0 0 0</pose>
          <geometry>
            <box>
              <size>0.465 0.465 2</size>
            </box>
          </geometry>
        </collision>
        <collision name='pilar3'>
          <pose>-7.5402 -3.8857 1 0 0 0</pose>
          <geometry>
            <box>
              <size>0.465 0.465 2</size>
            </box>
          </geometry>
        </collision>
        <collision name='pilar4'>
          <pose>7.4575 -3.8857 1 0 0 0</pose>
          <geometry>
            <box>
              <size>0.465 0.465 2</size>
            </box>
          </geometry>
        </collision>
        <collision name='pallet_mover1'>
          <pose>-0.6144 -2.389 0.41838 0 0 0</pose>
          <geometry>
            <box>
              <size>0.363 0.440 0.719</size>
            </box>
          </geometry>
        </collision>
        <collision name='pallet_mover2'>
          <pose>-1.6004 4.8225 0.41838 0 0 0</pose>
          <geometry>
            <box>
              <size>0.363 0.244 0.719</size>
            </box>
          </geometry>
        </collision>
        <collision name='stairs'>
          <pose>13.018 3.1652 0.25 0 0 0</pose>
          <geometry>
            <box>
              <size>1.299 0.6 0.5</size>
            </box>
          </geometry>
        </collision>
        <collision name='shelfs1'>
          <pose>1.4662 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs2'>
          <pose>2.6483 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs3'>
          <pose>5.3247 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs4'>
          <pose>6.5063 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs5'>
          <pose>9.0758 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs6'>
          <pose>10.258 -0.017559 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs7'>
          <pose>1.4662 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs8'>
          <pose>2.6483 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs9'>
          <pose>5.3247 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs10'>
          <pose>6.5063 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs11'>
          <pose>9.0758 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs12'>
          <pose>10.258 2.5664 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs13'>
          <pose>1.4662 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs14'>
          <pose>2.6483 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs15'>
          <pose>5.3247 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs16'>
          <pose>6.5063 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs17'>
          <pose>9.0758 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
        <collision name='shelfs18'>
          <pose>10.258 5.1497 0.5 0 0 0</pose>
          <geometry>
            <cylinder>
              <radius>0.03</radius>
              <length>1</length>
            </cylinder>
          </geometry>
        </collision>
      </link>
    </model>

  </world>
</sdf>
```

## Export world from Blender

### Install blender
```shell
sudo apt install blender
```
### Convert .dae file to .sdf file

Install the [blender_sdf_exporter](https://gazebosim.org/api/gazebo/6/blender_sdf_exporter.html) plug-in.

1. Open Blender
2. File->Import file
3. Select the `.dae` file that describes the world. (e.g `gnc/test/data/path_planner/mesh_files/cubicles.dae)
4. Export the environment in `.sdf` format to `px4-firmware/Tools/simulation/gz/worlds/`. This will generate the following output:
	1. `model.sdf`
	2. `model.config`
	3. `meshes/model.dae`

### Hacky way to create the .sdf file

Once the `model.sdf` file is obtained it is manually copied into the `cubicle.sdf` as follows.
```xml
<?xml version='1.0' encoding='ASCII'?>
<sdf version='1.7'>
  <world name='cubicle'>
    <physics type="ode">
      <max_step_size>0.004</max_step_size>
      <real_time_factor>1.0</real_time_factor>
      <real_time_update_rate>250</real_time_update_rate>
    </physics>

    <gravity>0 0 -9.81</gravity>
    <magnetic_field>6e-06 2.3e-05 -4.2e-05</magnetic_field>
    <atmosphere type='adiabatic'/>

    <plugin name='gz::sim::systems::Physics' filename='gz-sim-physics-system'/>
    <plugin name='gz::sim::systems::UserCommands' filename='gz-sim-user-commands-system'/>
    <plugin name='gz::sim::systems::SceneBroadcaster' filename='gz-sim-scene-broadcaster-system'/>
    <plugin name='gz::sim::systems::Contact' filename='gz-sim-contact-system'/>
    <plugin name='gz::sim::systems::Imu' filename='gz-sim-imu-system'/>
    <plugin name='gz::sim::systems::AirPressure' filename='gz-sim-air-pressure-system'/>
    <plugin name='gz::sim::systems::Sensors' filename='gz-sim-sensors-system'>
      <render_engine>ogre2</render_engine>
    </plugin>

    <gui fullscreen='false'>
      <plugin name='3D View' filename='GzScene3D'>
        <gz-gui>
          <title>3D View</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='string' key='state'>docked</property>
        </gz-gui>
        <engine>ogre2</engine>
        <scene>scene</scene>
        <ambient_light>0.5984631152222222 0.5984631152222222 0.5984631152222222</ambient_light>
        <background_color>0.8984631152222222 0.8984631152222222 0.8984631152222222</background_color>
        <camera_pose>-6 0 6 0 0.5 0</camera_pose>
      </plugin>
      <plugin name='World control' filename='WorldControl'>
        <gz-gui>
          <title>World control</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='bool' key='resizable'>0</property>
          <property type='double' key='height'>72</property>
          <property type='double' key='width'>121</property>
          <property type='double' key='z'>1</property>
          <property type='string' key='state'>floating</property>
          <anchors target='3D View'>
            <line own='left' target='left'/>
            <line own='bottom' target='bottom'/>
          </anchors>
        </gz-gui>
        <play_pause>1</play_pause>
        <step>1</step>
        <start_paused>1</start_paused>
      </plugin>
      <plugin name='World stats' filename='WorldStats'>
        <gz-gui>
          <title>World stats</title>
          <property type='bool' key='showTitleBar'>0</property>
          <property type='bool' key='resizable'>0</property>
          <property type='double' key='height'>110</property>
          <property type='double' key='width'>290</property>
          <property type='double' key='z'>1</property>
          <property type='string' key='state'>floating</property>
          <anchors target='3D View'>
            <line own='right' target='right'/>
            <line own='bottom' target='bottom'/>
          </anchors>
        </gz-gui>
        <sim_time>1</sim_time>
        <real_time>1</real_time>
        <real_time_factor>1</real_time_factor>
        <iterations>1</iterations>
      </plugin>
      <plugin name='Entity tree' filename='EntityTree'/>
    </gui>

    <scene>
      <ambient>1 1 1 1</ambient>
      <background>0.3 0.7 0.9 1</background>
      <shadows>0</shadows>
      <grid>1</grid>
    </scene>

    <light name='sunUTC' type='directional'>
      <pose>0 0 500 0 -0 0</pose>
      <cast_shadows>true</cast_shadows>
      <intensity>1</intensity>
      <direction>0.001 0.625 -0.78</direction>
      <diffuse>0.904 0.904 0.904 1</diffuse>
      <specular>0.271 0.271 0.271 1</specular>
      <attenuation>
        <range>2000</range>
        <linear>0</linear>
        <constant>1</constant>
        <quadratic>0</quadratic>
      </attenuation>
      <spot>
        <inner_angle>0</inner_angle>
        <outer_angle>0</outer_angle>
        <falloff>0</falloff>
      </spot>
    </light>

    <!-- This model was obtained with blender_sdf_exporter -->
    <model name="test">
      <static>true</static>
      <link name="testlink">
        <visual name="instance_0">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.001">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.001</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.002">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.002</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.003">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.003</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.004">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.004</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.005">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.005</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.006">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.006</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.007">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.007</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.008">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.008</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.009">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.009</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.010">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.010</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.011">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.011</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.012">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.012</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.013">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.013</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.014">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.014</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.015">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.015</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.016">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.016</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.017">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.017</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <visual name="instance_0.018">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
              <submesh>
                <name>instance_0.018</name>
              </submesh>
            </mesh>
          </geometry>
          <material>
            <diffuse>1.0 1.0 1.0 1.0</diffuse>
            <specular>0.0 0.0 0.0 1.0</specular>
            <pbr>
              <metal/>
            </pbr>
          </material>
        </visual>
        <collision name="collision">
          <geometry>
            <mesh>
              <uri>meshes/cubicle.dae</uri>
            </mesh>
          </geometry>
          <surface/>
          <contact/>
          <collide_bitmask>0x01</collide_bitmask>
        </collision>
      </link>
    </model>

  </world>
</sdf>
```

> NOTE: the `px4-firmware/Tools/simulation/gz/worlds/meshes/model.dae` was renamed to `px4-firmware/Tools/simulation/gz/worlds/meshes/cubicle.dae`. Additionally, the `uri` used in the `.sdf` files to refer to this file was changed from `<uri>model.dae<uri>` to `<uri>meshes/cubicle.dae<uri>`.

As before, launch the gazebo simulation with this world by running the following command: 
```shell
PX4_GZ_WORLD=cubicle make px4_sitl gz_x500
```
