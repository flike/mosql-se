cmake_minimum_required(VERSION 2.6)
project(mosql-se)
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(LIBTAPIOCA REQUIRED)
find_package(MYSQLSRC REQUIRED)
find_package(MYSQL REQUIRED)
find_package(LIBEVENT REQUIRED)
find_package(MSGPACK REQUIRED)
find_package(LIBUUID REQUIRED)
find_package(PCRE REQUIRED)

SET (MYSQLCOMMONFLAGS "-mtune=native -DUNIV_MUST_NOT_INLINE -static-libgcc -fno-exceptions -fno-omit-frame-pointer -fno-strict-aliasing   -DMYSQL_DYNAMIC_PLUGIN -rdynamic ")

SET (MYSQLCXXFLAGS " -fno-rtti ")
SET (MYSQLCFLAGS " ")
SET (MYDEBUGFLAGS " -O0 -g3 -DEXTRA_DEBUG -DSAFEMALLOC -DPEDANTIC_SAFEMALLOC -DFORCE_INIT_OF_VARS  -DSAFE_MUTEX ")
SET (MYOPTFLAGS " -g -O0 -DDBUG_OFF -DFORCE_DBUG_OFF ")

IF (CMAKE_BUILD_TYPE STREQUAL Debug)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MYDEBUGFLAGS} ${MYSQLCOMMONFLAGS} ${MYSQLCXXFLAGS} ")
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${MYDEBUGFLAGS} ${MYSQLCOMMONFLAGS} ${MYSQLCFLAGS} ")
ELSE()
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MYOPTFLAGS} ${MYSQLCOMMONFLAGS} ${MYSQLCXXFLAGS} ")
	SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${MYOPTFLAGS} ${MYSQLCOMMONFLAGS} ${MYSQLCFLAGS} ")
ENDIF()


add_subdirectory(handler)
add_subdirectory(tests)

INSTALL(FILES ../scripts/launch_db.sh DESTINATION scripts)
INSTALL(FILES ../config/my.cnf DESTINATION config)
