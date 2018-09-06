aux_source_directory(. SRC_LIST)

file(GLOB HEADERS "*.h")

include(EthDependencies)

add_executable(fisco-bcos ${SRC_LIST} ${HEADERS})

find_package(Eth)
find_package(Web3)

target_include_directories(fisco-bcos PRIVATE ..)
					
target_link_libraries(fisco-bcos ${Eth_EVM_LIBRARIES})
target_link_libraries(fisco-bcos ${Web3_WEB3JSONRPC_LIBRARIES})
target_link_libraries(fisco-bcos ${Web3_WEBTHREE_LIBRARIES})
target_link_libraries(fisco-bcos ${Eth_DISKENCRYPTION_LIBRARIES})
target_link_libraries(fisco-bcos JsonRpcCpp::Client)
target_link_libraries(fisco-bcos pbftseal)
target_link_libraries(fisco-bcos contract)
target_link_libraries(fisco-bcos channelserver)
target_link_libraries(fisco-bcos console)
target_link_libraries(fisco-bcos web3jsonrpc)
target_link_libraries(fisco-bcos precompiled)
target_link_libraries(fisco-bcos storage)
target_link_libraries(fisco-bcos initializer)
target_link_libraries(fisco-bcos abi)

if(BUILD_BOOST)
target_include_directories(fisco-bcos PRIVATE ${BOOST_INCLUDE_DIR})
else()
find_package(Boost COMPONENTS system filesystem random thread regex REQUIRED)
target_link_libraries(fisco-bcos ${Boost_LIBRARIES})
endif()

if (UNIX AND NOT APPLE)
	target_link_libraries(fisco-bcos pthread)
endif()

install(TARGETS fisco-bcos DESTINATION bin)