# ros_noetic_tutorial_c++
this repo contain readme files for ros noetic tutorial lessons.

each lesson folder contains lesson src pkgs and readme file for instruction and commands that used with document them and and any useful resources that used in the lesson 

- lesson0 : intro to ros  and understanat ros architure,pkg system,ros features 
- and create publisher and  topic
- lesson1: understand publisher , topic and subscriber and implement them with c++
- lesson2: create custom msg and use it in the our pkg with mini project to practics 
- lesson3: create server and client code with c++
- lesson4: create action 
- lesson5: hand on debug tools in ros like rqt_graph ,rviz,terminal commands





publicher_topic_cpp





```
#include <ros/ros.h>
#include <std_msgs/Int32.h>

int main(int argc, char** argv) {

    ros::init(argc, argv, "topic_publisher");
    ros::NodeHandle nh;
    
    ros::Publisher pub = nh.advertise<std_msgs::Int32>("counter", 1000);
    ros::Rate loop_rate(2);
    
    std_msgs::Int32 count;
    count.data = 0;
    
    while (ros::ok())
    {
        pub.publish(count);
        ros::spinOnce();
        loop_rate.sleep();
        ++count.data;
    }
    
    return 0;
}



```

CmakeList.txt

```
add_executable(simple_topic_publisher src/simple_topic_publisher.cpp)
add_dependencies(simple_topic_publisher ${simple_topic_publisher_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(simple_topic_publisher
   ${catkin_LIBRARIES}
 )
```

to check or grep the topic 

```
rostopic list | grep '/counter'
```

rostopic echo onece

```
rostopic echo <topic_name> -n1
```

to show msg

```
rosmsg show rosmsg show std_msgs/Int32
roscd std_msgs/msg/
```

