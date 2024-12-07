### **Hi.**
I hope we all help each other to improve knowledge by sharing experiences.
I have a project with C++ and need to use the mailio library to make an OTP login with mail for specific users. First, I tried on Windows 10, and after that, I tried on Windows 11. Here is some information to share:

Windows 10 or 11
CLion IDE (My Original IDE) & MinGW, MSYS, Visual Studio Pro 2022
What needs to be installed and can work with vcpkg:
*You need Build Tools & SDKs (This is really important!)
-> For this option, you can download it from Microsoft or just install it using the Visual Studio installer

*If your system username has a space, you might encounter issues
For example: c:/user/M M/vcpkg..... This can cause many problems in the system environment or in the CMake files for target libraries by vcpkg and more... But I found a way to fix this issue :) First, in the system environment, you should delete the vcpkg root (This is important for CLion IDE users) Second, you can use this code in cmd or terminal to get the binary form for the path with spaces:
for %I in ("C:/Users/your user name/....") do @echo %~sI

*GPT or other AIs do not work and can't return you the correct answer for these, really! It just wastes your time!!!
The best way to fix your issues with vcpkg is to read the documentation from Microsoft. Here are the most important links:

https://vcpkg.io/en/package
https://learn.microsoft.com/en-us/vcpkg/commands/install
https://learn.microsoft.com/en-us/vcpkg/troubleshoot/build-failures
https://learn.microsoft.com/en-us/vcpkg/users/triplets#vcpkg_visual_studio_path
*CMakeLists.txt has problems finding installed libraries with vcpkg!!
Maybe you finally install your libraries, but when you use them in the main code, you see an error saying it can't find the libraries!... WTH?!
For this issue, you should have good knowledge about CMake and vcpkg (This is really important)
After you install libraries with vcpkg, the last line returns some lines to be added in the CMakeLists.txt
But after you add them, it still can't find the libraries!

So what's the answer?!
Some libraries have dependencies, and you should first call them and then call your libraries in CMakeLists.txt You can see the dependencies for all packages at https://vcpkg.io/en/package For example, after many tries for mailio, I found the following solution to make CMake find it:

             cmake_minimum_required(VERSION 3.28)
             set(CMAKE_TOOLCHAIN_FILE "C:/vcpkg/scripts/buildsystems/vcpkg.cmake"
                           CACHE STRING "Vcpkg toolchain file")
             project(project)

             set(CMAKE_CXX_STANDARD 17)

             add_executable(project main.cpp)

             find_package(OpenSSL REQUIRED)
             find_package(mailio CONFIG REQUIRED)
             find_package(Boost REQUIRED)
             find_package(Boost REQUIRED COMPONENTS system asio)

             target_link_libraries(project PRIVATE mailio)
             target_link_libraries(project PRIVATE Boost::boost)
Common bugs for these parts:
              set(CMAKE_TOOLCHAIN_FILE "C:/vcpkg/scripts/buildsystems/vcpkg.cmake"
                            CACHE STRING "Vcpkg toolchain file")
project(project)

              find_package(mailio CONFIG REQUIRED)
              find_package(Boost REQUIRED)
              find_package(Boost REQUIRED COMPONENTS system asio)  //for libraries in the Boost you should call them like this
And an important part is the order; if you try to change this order, you might see some problems. For this, getting help from GPT can be useful.

For CLion users....
In the "Settings" under the "Build, Execution, Deployment" section & in the CMake options, you should check it with GPT to understand and fix some problems from there :)

So this is my experience. I hope it is helpful, and if you know more, please share it with us.
Thanks for reading :)
