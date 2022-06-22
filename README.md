# Replace ROS2 Communication Channels with Lingua Franca

## Prerequisites 

1. install Lingua Franca lfc (command line compiler)
2. install Linuga Franca RTI (runtime infrastructure)
3. install ROS2 foxy

## Build Instructions 

### Clone the repository 

```
git clone [repo link]
cd [repo name]
```
### Build ROS2 Package 

```
source [path/to/ros2/environment]
cd lf_simple_package && colcon build
```

> **INFO** After ROS2 package is built successfully, we need to delete the `int main()` function in both publisher and subscriber files. Having the main functions in `publisher_member_function.cpp` and `subscriber_member_function.cpp` will lead to redefinition errors when we compile Lingua Franca code. 

1. Open the file `lf_simple_package/lf_simple/src/publisher_member_function.cpp`
2. Remove main function shown below:
```
int main(int argc, char * argv[])
{
  rclcpp::init(argc, argv);
  rclcpp::spin(std::make_shared<MinimalPublisher>());
  rclcpp::shutdown();
  return 0;
}
```

3. Open the file `lf_simple_package/lf_simple/src/subscriber_member_function.cpp`
4. Remove main function shown below:
```
int main(int argc, char * argv[])
{
  rclcpp::init(lf_simpleargc, argv);
  rclcpp::spin(std::make_shared<MinimalSubscriber>());
  rclcpp::shutdown();
  return 0;
}
```
### Build Lingua Franca program

```
cd .. #Going back to [repo name] folder
source lf_simple_package/install/setup.bash
lfc lf-project/src/Main.lf
. lf-project/bin/Main
```

Now you should have the program running with Lingua Franca communication channels!