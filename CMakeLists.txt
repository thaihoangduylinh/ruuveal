cmake_minimum_required(VERSION 3.10)
project(ruuveal VERSION 0.2.0)
set(CMAKE_C_STANDARD 99)

if (WIN32)
	set(OPENSSL_ROOT_DIR "C:/Program Files/OpenSSL-Win64" )
    set(OPENSSL_INCLUDE_DIR "${OPENSSL_ROOT_DIR}/include" )

    # Phát hiện kiến trúc (32-bit hoặc 64-bit)
    set(OPENSSL_ARCH_DIR "VC/x64/MD")

	#set(OPENSSL_ARCH_DIR "C:/Program Files/OpenSSL-Win64/VC/x64/MD" CACHE PATH "Path to the OpenSSL arch lib")
	#message(STATUS "Detected OPENSSL_ARCH_DIR = ${OPENSSL_ARCH_DIR}")
    # Thư mục chứa lib cụ thể
    set(OPENSSL_LIB_DIR "${OPENSSL_ROOT_DIR}/lib/${OPENSSL_ARCH_DIR}")
	message(STATUS "Detected OPENSSL_LIB_DIR = ${OPENSSL_LIB_DIR}")

    # Đặt đường dẫn đến file .lib cụ thể
    set(OPENSSL_CRYPTO_LIBRARY "${OPENSSL_LIB_DIR}/libcrypto.lib" )
    set(OPENSSL_SSL_LIBRARY "${OPENSSL_LIB_DIR}/libssl.lib" )
	
	message(STATUS "Detected OPENSSL_CRYPTO_LIBRARY = ${OPENSSL_CRYPTO_LIBRARY}")
	message(STATUS "Detected OPENSSL_SSL_LIBRARY = ${OPENSSL_SSL_LIBRARY}")

endif()


set(HEADER_FILES 	htcaes.h 
					htczip.h 
					htclargezip.h 
					htckey.h 
					getopt.h
					htc/devices.h
					htc/keydata.h
					htc/keymap.h
					)

# File chung cho cả 2 tool
set(COMMON_SRC htcaes.c 
				htczip.c 
				htclargezip.c 
				htckey.c 
				getopt.c 
				htc/devices.c
				)


add_executable(ruuveal main.c ${COMMON_SRC} ${HEADER_FILES})
target_include_directories(ruuveal PRIVATE ${OPENSSL_INCLUDE_DIR})
target_link_libraries(ruuveal ${OPENSSL_SSL_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY})
add_custom_command(TARGET ruuveal POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libcrypto-3-x64.dll"
		"$<TARGET_FILE_DIR:ruuveal>"
)

add_custom_command(TARGET ruuveal POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libssl-3-x64.dll"
		"$<TARGET_FILE_DIR:ruuveal>"
)

add_executable(ruuvealwp mainwp.c ${COMMON_SRC} ${HEADER_FILES})
target_include_directories(ruuvealwp PRIVATE ${OPENSSL_INCLUDE_DIR})
target_link_libraries(ruuvealwp ${OPENSSL_SSL_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY})
add_custom_command(TARGET ruuvealwp POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libcrypto-3-x64.dll"
		"$<TARGET_FILE_DIR:ruuvealwp>"
)

add_custom_command(TARGET ruuvealwp POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libssl-3-x64.dll"
		"$<TARGET_FILE_DIR:ruuvealwp>"
)

add_executable(bruutveal bruutveal.c ${COMMON_SRC} ${HEADER_FILES})
target_include_directories(bruutveal PRIVATE ${OPENSSL_INCLUDE_DIR})
target_link_libraries(bruutveal ${OPENSSL_SSL_LIBRARY} ${OPENSSL_CRYPTO_LIBRARY})
add_custom_command(TARGET bruutveal POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libcrypto-3-x64.dll"
		"$<TARGET_FILE_DIR:bruutveal>"
)

add_custom_command(TARGET bruutveal POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${OPENSSL_ROOT_DIR}/bin/libssl-3-x64.dll"
		"$<TARGET_FILE_DIR:bruutveal>"
)
