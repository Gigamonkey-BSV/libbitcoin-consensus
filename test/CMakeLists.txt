cmake_minimum_required(VERSION 3.1...3.14)

# Back compatibility for VERSION range
if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION})
endif()

ADD_DEFINITIONS(-DBOOST_TEST_DYN_LINK)
ADD_EXECUTABLE(libbitcoin-consensus-test consensus__script_error_to_verify_result.cpp consensus__script_verify.cpp consensus__verify_flags_to_script_flags.cpp main.cpp script.hpp)
        link_directories(${Boost_LIBRARY_DIR} )
        target_link_libraries(libbitcoin-consensus-test libbitcoin-consensus  ${Boost_LIBRARIES} ${Secp256k1_LIBRARY} ${GMP_LIBRARY} ${GMPXX_LIBRARY} )