
set(THIS daemon-etl)

include_directories(include)

set(SOURCE_FILES
	source/daemon.cpp
	source/main.cpp)

set (CMAKE_CXX_STANDARD 17)

# Make build flags compiler specific
if (CMAKE_COMPILER_IS_GNUCC)
	if (CMAKE_BUILD_TYPE STREQUAL "Debug")
		set (CMAKE_C_FLAGS "-D_REETRANT -ggdb -Wall -Wextra -pedantic -O0")
	elseif( CMAKE_BUILD_TYPE STREQUAL "Release" )
		set (CMAKE_C_FLAGS "-D_REETRANT -DNDEBUG -Wall -Wextra -pedantic -O3")
	endif ()
endif (CMAKE_COMPILER_IS_GNUCC)


add_executable(${THIS} ${SOURCE_FILES})

target_link_libraries(${THIS} libconfig++.a)

# Install the config file
install(FILES resource/config/daemon-etl.conf DESTINATION /etc/daemon-etl)

# Intall the systemd file
install(FILES resource/systemd/daemon-etl.service DESTINATION /etc/systemd/system)

# Install the binary program
install(TARGETS ${THIS} DESTINATION /usr/bin)
