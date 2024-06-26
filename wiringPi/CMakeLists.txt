# CMakeLists.txt for wiringPi

cmake_minimum_required(VERSION 3.10)
project(WiringPi)

# Source files
set(SRC
    wiringPi.c
    wiringSerial.c wiringShift.c
    piHiPri.c piThread.c
    wiringPiSPI.c wiringPiI2C.c
    softPwm.c softTone.c
    mcp23008.c mcp23016.c mcp23017.c
    mcp23s08.c mcp23s17.c
    sr595.c
    pcf8574.c pcf8591.c
    mcp3002.c mcp3004.c mcp4802.c mcp3422.c
    max31855.c max5322.c ads1115.c
    sn3218.c
    bmp180.c htu21d.c ds18b20.c rht03.c
    drcSerial.c drcNet.c
    pseudoPins.c
    wpiExtensions.c
    wiringPiLegacy.c
)

# Headers
set(HEADERS
    wiringPi.h
    wiringSerial.h wiringShift.h
    wiringPiSPI.h wiringPiI2C.h
    softPwm.h softTone.h
    mcp23008.h mcp23016.h mcp23017.h
    mcp23s08.h mcp23s17.h
    sr595.h
    pcf8574.h pcf8591.h
    mcp3002.h mcp3004.h mcp4802.h mcp3422.h
    max31855.h max5322.h ads1115.h
    sn3218.h
    bmp180.h htu21d.h ds18b20.h rht03.h
    drcSerial.h drcNet.h
    pseudoPins.h
    wpiExtensions.h
)
# Compiler flags
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Wextra -Wformat=2 -Winline -pipe -fPIC -Wformat-security")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,-soname,libwiringPi.so${WIRINGPI_SONAME_SUFFIX}")

# Read version from version.h
file(READ "${CMAKE_CURRENT_SOURCE_DIR}/../version.h" VERSION)
string(REGEX REPLACE ".*VERSION \"([0-9]+.[0-9]+)\".*" "\\1" VERSION "${VERSION}")

# Add the library target
add_library(wiringPi SHARED ${SRC})

# Set the library version
set(WIRINGPI_SONAME_SUFFIX "1")
set_target_properties(wiringPi PROPERTIES VERSION ${VERSION} SOVERSION ${WIRINGPI_SONAME_SUFFIX})
target_include_directories(wiringPi PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# Add -lm to link with the math library
target_link_libraries(wiringPi PUBLIC m crypt)
# Install headers
install(FILES ${HEADERS} DESTINATION include)
# Install the library
install(TARGETS wiringPi LIBRARY DESTINATION lib)
