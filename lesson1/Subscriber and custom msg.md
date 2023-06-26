# ros_noetic_tutorial_c++
this repo contain readme files for ros noetic tutorial lessons.

each lesson folder contains lesson src pkgs and readme file for instruction and commands that used with document them and and any useful resources that used in the lesson 

- lesson0 : intro to ros  and understanat ros architure,pkg system,ros features 
- lesson1: understand publisher , topic and subscriber and implement them with c++
- lesson2: create custom msg and use it in the our pkg with mini project to practics 
- lesson3: create server and client code with c++
- lesson4: create action 
- lesson5: hand on debug tools in ros like rqt_graph ,rviz,terminal commands



lesson 1:

create subscriber

```
#include <ros/ros.h>
#include <std_msgs/Int32.h>

void counterCallback(const std_msgs::Int32::ConstPtr& msg) // Define a function called 'callback' that receives a                                                                // parameter named 'msg' 
{
  ROS_INFO("%d", msg->data); // Print the value 'data' inside the 'msg' parameter
}

int main(int argc, char** argv) {

    ros::init(argc, argv, "topic_subscriber"); // Initiate a Node called 'topic_subscriber'
    ros::NodeHandle nh;
    
    ros::Subscriber sub = nh.subscribe("counter", 1000, counterCallback); // Create a Subscriber object that will                                                                               // listen to the /counter topic and will
                                                                          // call the 'callback' function each time                                                                             // it reads something from the topic
    
    ros::spin(); // Create a loop that will keep the program in execution
    
    return 0;
}
```



add to CmaleList.txt

```
add_executable(simple_topic_subscriber src/simple_topic_subscriber.cpp)
add_dependencies(simple_topic_subscriber ${simple_topic_subscriber_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(simple_topic_subscriber
   ${catkin_LIBRARIES}
 )
```







how to create custom msg in ros

In order to create a new message, you will need to do the following steps:

1. Create a directory named 'msg' inside your package

2. Inside this directory, create a file named Name_of_your_message.msg (more information down)

3. Modify CMakeLists.txt file (more information down)

4. Modify package.xml file (more information down)

5. Compile

6. Use in code

   

3) In CMakeLists.txt

You will have to edit four functions inside CMakeLists.txt:

find_package()
add_message_files()
generate_messages()
catkin_package()

For example, let's create a message that indicates age, with years, months, and days.

1) Create a directory msg in your package.

In [ ]:

```
roscd <package_name>
mkdir msg
```

2) The Age.msg file must contain this:

```
float32 years
float32 months
float32 days
```



3) In CMakeLists.txt

You will have to edit four functions inside CMakeLists.txt:

```
find_package()
add_message_files()
generate_messages()
catkin_package()
```


I. find_package()
This is where all the packages required to COMPILE the messages of the topics, services, and actions go. In package.xml, you have to state them as build_depend.

HINT 1: If you open the CMakeLists.txt file in your IDE, you'll see that almost all of the file is commented. This includes some of the lines you will have to modify. Instead of copying and pasting the lines below, find the equivalents in the file and uncomment them, and then add the parts that are missing.

```
find_package(catkin REQUIRED COMPONENTS
       roscpp
       std_msgs
       message_generation   # Add message_generation here, after the other packages
)
```


II. add_message_files()
This function includes all of the messages of this package (in the msg folder) to be compiled. The file should look like this.

```
find_package(catkin REQUIRED COMPONENTS
       roscpp
       std_msgs
       message_generation   # Add message_generation here, after the other packages
)
```


III. generate_messages()
Here is where the packages needed for the messages compilation are imported.

```
generate_messages(
      DEPENDENCIES
      std_msgs
) # Dont Forget to uncoment here TOO
```


IV. catkin_package()
State here all of the packages that will be needed by someone that executes something from your package. All of the packages stated here must be in the package.xml as exec_depend.

```
catkin_package(
      CATKIN_DEPENDS roscpp std_msgs message_runtime   # This will NOT be the only thing here
)
```


Summarizing, this is the minimum expression of what is needed for the CMakaelist.txt to work:

Note: Keep in mind that the name of the package in the following example is topic_ex, so in your case, the name of the package may be different.

```
cmake_minimum_required(VERSION 2.8.3)
project(topic_ex)


find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
)

add_message_files(
  FILES
  Age.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(
  CATKIN_DEPENDS roscpp std_msgs message_runtime
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)


<!-- for publisher new msg add topic_ex_generate_messages_cpp -->
add_executable(publish_age src/publish_age.cpp)
add_dependencies(publish_age ${publish_age_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(publish_age
   ${catkin_LIBRARIES}
 )
add_dependencies(publish_age topic_ex_generate_messages_cpp)
```


4) Modify package.xml

Just add these 3 lines to the package.xml file.

```
<build_depend>message_generation</build_depend>

<build_export_depend>message_runtime</build_export_depend>
<exec_depend>message_runtime</exec_depend>
```


This is the minimum expression of the package.xml

Note: Keep in mind that the name of the package in the following example is topic_ex, so in your case, the name of the package may be different.

```
<?xml version="1.0"?>
<package format="2">
  <name>topic_ex</name>
  <version>0.0.0</version>
  <description>The topic_ex package</description>


  <maintainer email="user@todo.todo">user</maintainer>

  <license>TODO</license>

  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>message_generation</build_depend>
  <build_export_depend>roscpp</build_export_depend>
  <exec_depend>roscpp</exec_depend>
  <build_export_depend>std_msgs</build_export_depend>
  <exec_depend>std_msgs</exec_depend>
  <build_export_depend>message_runtime</build_export_depend>
  <exec_depend>message_runtime</exec_depend>

  <export>

  </export>
</package>
￼
```


5) Now you have to compile the msgs. To do this, you have to type in a WebShell:

   Execute in Shell #1

```
roscd; cd ..
catkin_make
source devel/setup.bash
```

