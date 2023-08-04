# tiago_navigation

This repo is forked from the PAL Robotics `tiago_navigation` repo, in order to collect the maps built during the use of TIAGo at WE-COBOT.

>📝 Any other change to the software in this repo should be done in a separated branch.

## How to
The following instructions are mainly a summary of the instructions in the official handbook, plus few instructions on how to name, update and backup maps.

### 1. Build a map
On the development computer, start the mapping:
```
export ROS_MASTER_URI=http://tiago-0c:11311
export ROS_IP=10.68.0.128
rosservice call /pal_navigation_sm "input: 'MAP'"
```
then open Rviz to check the map being built
```
export ROS_MASTER_URI=http://tiago-0c:11311
export ROS_IP=10.68.0.128
rosrun rviz rviz -d `rospack find tiago_2dnav`/config/rviz/navigation.rviz
```
Now, navigate the robot around the room using the joystick, until you are satisfied with the result of the mapping.

### 2. Save a map
In order to automatically save the map, finish mapping and start using the map for localization and path
planning, using the following command:
```
export ROS_MASTER_URI=http://tiago-0c:11311
export ROS_IP=10.68.0.128
rosservice call /pal_navigation_sm "input: 'LOC'"
```
The map will be stored with other map files in the path `$HOME/.pal/tiago_maps/configurations` in the robot’s file system. The current map in use is the one
pointed to by the symbolic link `$HOME/.pal/tiago_maps/config`.

The map you have just created will be named by default with the current date and time.

### 3. Rename a map
The map you have just created will be named by default with the current date and time. As this might no be very meaningful to future readers, it strongly suggested to rename the map.
To rename, connect to the robot's PC via (`ssh pal@tiago-0c`), then 
```
cd $HOME/.pal/tiago_maps/configurations
mv <current_map_name> <new_map_name>
```
Good examples for `<new_map_name>` could be, for instance, `wecobot_lab` or `wecobot_corridor`.
Changing the name of the map will breake the `$HOME/.pal/tiago_maps/config` simlink. Update it with:
```
cd `$HOME/.pal/tiago_maps
ln -sfn <map_to_use>
```

### 4. Copy a map to the development PC
To backup the map, and push it to the present repo, it could be useful to copy it from the robot's PC to the development PC.
To do so, connect the development PC to TIAGo via Ethernet or WiFi. Then, from the **development** PC run
```
scp -r pal@tiago:/home/pal/.pal/tiago_maps/configurations/<map_to_copy> $HOME/tiago_public_ws/tiago_navigation/tiago_maps/configurations
```

Do not forget to push the updated map to GitHub! It is important both to backup it, and to keep a log of map changes.
