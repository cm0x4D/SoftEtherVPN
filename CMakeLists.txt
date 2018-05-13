cmake_minimum_required(VERSION 3.0)

project(SoftEtherVPN)


########################################################################################################################
# Global definitions.
#

add_definitions(
    -DUNIX 
    -DUNIX_LINUX 
    -DCPU_64 
    -D_REENTRANT 
    -DREENTRANT 
    -D_THREAD_SAFE 
    -D_THREADSAFE 
    -DTHREAD_SAFE 
    -DTHREADSAFE 
    -D_FILE_OFFSET_BITS=64 
    -fsigned-char
)

if(CMAKE_BUILD_TYPE MATCHES "DEBUG")
    add_definitions(
        -D_DEBUG 
        -DDEBUG
    )
else()
    add_definitions(
        -DNDEBUG 
        -DVPN_SPEED
    )
endif()


#
# Find required Packages.    
#

find_package(OpenSSL REQUIRED)
find_package(Threads REQUIRED)
find_package(Curses REQUIRED)
find_package(ZLIB REQUIRED)
find_library(READLINE_LIBRARIES readline)


#
# Mayaqua lib.
#

set(HEADERS_MAYAQUA 
    src/Mayaqua/Cfg.h 
    src/Mayaqua/cryptoki.h 
    src/Mayaqua/Encrypt.h 
    src/Mayaqua/FileIO.h 
    src/Mayaqua/intelaes/iaesni.h 
    src/Mayaqua/Internat.h 
    src/Mayaqua/Kernel.h 
    src/Mayaqua/Mayaqua.h 
    src/Mayaqua/MayaType.h 
    src/Mayaqua/Memory.h 
    src/Mayaqua/Microsoft.h 
    src/Mayaqua/Network.h 
    src/Mayaqua/Object.h 
    src/Mayaqua/OS.h 
    src/Mayaqua/Pack.h 
    src/Mayaqua/pkcs11.h 
    src/Mayaqua/pkcs11f.h 
    src/Mayaqua/pkcs11t.h 
    src/Mayaqua/Secure.h 
    src/Mayaqua/Str.h
    src/Mayaqua/Table.h 
    src/Mayaqua/TcpIp.h 
    src/Mayaqua/Tick64.h 
    src/Mayaqua/Tracking.h 
    src/Mayaqua/TunTap.h 
    src/Mayaqua/Unix.h 
    src/Mayaqua/Win32.h 
    src/Mayaqua/zlib/zconf.h 
    src/Mayaqua/zlib/zlib.h
)

set(SOURCES_MAYAQUA
    src/Mayaqua/Cfg.c 
    src/Mayaqua/Encrypt.c 
    src/Mayaqua/FileIO.c 
    src/Mayaqua/Internat.c 
    src/Mayaqua/Kernel.c 
    src/Mayaqua/Mayaqua.c 
    src/Mayaqua/Memory.c 
    src/Mayaqua/Microsoft.c 
    src/Mayaqua/Network.c 
    src/Mayaqua/Object.c 
    src/Mayaqua/OS.c 
    src/Mayaqua/Pack.c 
    src/Mayaqua/Secure.c 
    src/Mayaqua/Str.c 
    src/Mayaqua/Table.c 
    src/Mayaqua/TcpIp.c 
    src/Mayaqua/Tick64.c 
    src/Mayaqua/Tracking.c 
    src/Mayaqua/Unix.c 
    src/Mayaqua/Win32.c
)

add_library(Mayaqua SHARED ${HEADERS_MAYAQUA} ${SOURCES_MAYAQUA})
target_include_directories(Mayaqua PUBLIC src/Mayaqua src)
target_link_libraries(Mayaqua OpenSSL::SSL OpenSSL::Crypto Threads::Threads)


#
# Cedar library.
#

set(HEADERS_CEDAR
    src/Cedar/Account.h src/Cedar/Admin.h 
    src/Cedar/AzureClient.h 
    src/Cedar/AzureServer.h 
    src/Cedar/Bridge.h 
    src/Cedar/BridgeUnix.h 
    src/Cedar/BridgeWin32.h 
    src/Cedar/Cedar.h 
    src/Cedar/CedarPch.h 
    src/Cedar/CedarType.h 
    src/Cedar/Client.h 
    src/Cedar/CM.h 
    src/Cedar/CMInner.h 
    src/Cedar/Command.h 
    src/Cedar/Connection.h 
    src/Cedar/Console.h 
    src/Cedar/Database.h 
    src/Cedar/DDNS.h 
    src/Cedar/EM.h 
    src/Cedar/EMInner.h 
    src/Cedar/EtherLog.h 
    src/Cedar/Hub.h 
    src/Cedar/Interop_OpenVPN.h 
    src/Cedar/Interop_SSTP.h 
    src/Cedar/IPsec.h 
    src/Cedar/IPsec_EtherIP.h 
    src/Cedar/IPsec_IKE.h 
    src/Cedar/IPsec_IkePacket.h 
    src/Cedar/IPsec_IPC.h 
    src/Cedar/IPsec_L2TP.h 
    src/Cedar/IPsec_PPP.h 
    src/Cedar/IPsec_Win7.h 
    src/Cedar/IPsec_Win7Inner.h 
    src/Cedar/Layer3.h 
    src/Cedar/Link.h 
    src/Cedar/Listener.h 
    src/Cedar/Logging.h 
    src/Cedar/Nat.h 
    src/Cedar/NativeStack.h 
    src/Cedar/netcfgn.h 
    src/Cedar/netcfgx.h 
    src/Cedar/NM.h 
    src/Cedar/NMInner.h 
    src/Cedar/NullLan.h 
    src/Cedar/Protocol.h 
    src/Cedar/Radius.h 
    src/Cedar/Remote.h 
    src/Cedar/Sam.h 
    src/Cedar/SecureInfo.h 
    src/Cedar/SecureNAT.h 
    src/Cedar/SeLowUser.h 
    src/Cedar/Server.h 
    src/Cedar/Session.h 
    src/Cedar/SM.h 
    src/Cedar/SMInner.h 
    src/Cedar/SW.h 
    src/Cedar/SWInner.h 
    src/Cedar/UdpAccel.h 
    src/Cedar/UT.h 
    src/Cedar/VG.h 
    src/Cedar/Virtual.h 
    src/Cedar/VLan.h 
    src/Cedar/VLanUnix.h 
    src/Cedar/VLanWin32.h 
    src/Cedar/WaterMark.h 
    src/Cedar/WebUI.h 
    src/Cedar/Win32Com.h 
    src/Cedar/winpcap/bittypes.h 
    src/Cedar/winpcap/bucket_lookup.h 
    src/Cedar/winpcap/count_packets.h 
    src/Cedar/winpcap/Devioctl.h 
    src/Cedar/winpcap/Gnuc.h 
    src/Cedar/winpcap/ip6_misc.h 
    src/Cedar/winpcap/memory_t.h 
    src/Cedar/winpcap/normal_lookup.h 
    src/Cedar/winpcap/Ntddndis.h 
    src/Cedar/winpcap/Ntddpack.h 
    src/Cedar/winpcap/Packet32.h 
    src/Cedar/winpcap/pcap.h 
    src/Cedar/winpcap/pcap-bpf.h 
    src/Cedar/winpcap/pcap-int.h 
    src/Cedar/winpcap/pcap-stdinc.h 
    src/Cedar/winpcap/pthread.h 
    src/Cedar/winpcap/remote-ext.h 
    src/Cedar/winpcap/sched.h 
    src/Cedar/winpcap/semaphore.h 
    src/Cedar/winpcap/tcp_session.h 
    src/Cedar/winpcap/time_calls.h 
    src/Cedar/winpcap/tme.h 
    src/Cedar/winpcap/Win32-Extensions.h 
    src/Cedar/WinUi.h src/Cedar/Wpc.h
)

set(SOURCES_CEDAR
    src/Cedar/Account.c 
    src/Cedar/Admin.c 
    src/Cedar/AzureClient.c 
    src/Cedar/AzureServer.c 
    src/Cedar/Bridge.c 
    src/Cedar/BridgeUnix.c 
    src/Cedar/BridgeWin32.c 
    src/Cedar/Cedar.c 
    src/Cedar/CedarPch.c 
    src/Cedar/Client.c 
    src/Cedar/CM.c 
    src/Cedar/Command.c 
    src/Cedar/Connection.c 
    src/Cedar/Console.c 
    src/Cedar/Database.c 
    src/Cedar/DDNS.c 
    src/Cedar/EM.c 
    src/Cedar/EtherLog.c 
    src/Cedar/Hub.c 
    src/Cedar/Interop_OpenVPN.c 
    src/Cedar/Interop_SSTP.c 
    src/Cedar/IPsec.c 
    src/Cedar/IPsec_EtherIP.c 
    src/Cedar/IPsec_IKE.c 
    src/Cedar/IPsec_IkePacket.c 
    src/Cedar/IPsec_IPC.c 
    src/Cedar/IPsec_L2TP.c 
    src/Cedar/IPsec_PPP.c 
    src/Cedar/IPsec_Win7.c 
    src/Cedar/Layer3.c 
    src/Cedar/Link.c 
    src/Cedar/Listener.c 
    src/Cedar/Logging.c 
    src/Cedar/Nat.c 
    src/Cedar/NativeStack.c 
    src/Cedar/NM.c 
    src/Cedar/NullLan.c 
    src/Cedar/Protocol.c 
    src/Cedar/Radius.c 
    src/Cedar/Remote.c 
    src/Cedar/Sam.c 
    src/Cedar/SecureInfo.c 
    src/Cedar/SecureNAT.c 
    src/Cedar/SeLowUser.c 
    src/Cedar/Server.c 
    src/Cedar/Session.c 
    src/Cedar/SM.c 
    src/Cedar/SW.c 
    src/Cedar/UdpAccel.c 
    src/Cedar/UT.c 
    src/Cedar/VG.c 
    src/Cedar/Virtual.c 
    src/Cedar/VLan.c 
    src/Cedar/VLanUnix.c 
    src/Cedar/VLanWin32.c 
    src/Cedar/WaterMark.c 
    src/Cedar/WebUI.c 
    src/Cedar/WinUi.c 
    src/Cedar/Wpc.c
)

add_library(Cedar SHARED ${HEADERS_CEDAR} ${SOURCES_CEDAR})
target_include_directories(Cedar PUBLIC src/Cedar src/Mayaqua src)
target_link_libraries(Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads)


#
# Hamcore builder & hamcore file
#

add_executable(hamcorebuilder src/hamcorebuilder/hamcorebuilder.c)
target_link_libraries(hamcorebuilder Mayaqua Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads ${READLINE_LIBRARIES} ${CURSES_LIBRARIES} ZLIB::ZLIB)

set(HAMCORE_FILES
    src/bin/hamcore/authors.txt
    src/bin/hamcore/backup_dir_readme.txt 
    src/bin/hamcore/empty.config 
    src/bin/hamcore/empty_sevpnclient.config 
    src/bin/hamcore/eula.txt 
    src/bin/hamcore/install_src.dat 
    src/bin/hamcore/lang.config 
    src/bin/hamcore/languages.txt 
    src/bin/hamcore/languages_wine.txt 
    src/bin/hamcore/legal.txt 
    src/bin/hamcore/openvpn_readme.pdf 
    src/bin/hamcore/openvpn_readme.txt 
    src/bin/hamcore/openvpn_sample.ovpn 
    src/bin/hamcore/root_certs.dat 
    src/bin/hamcore/SOURCES_OF_BINARY_FILES.TXT 
    src/bin/hamcore/strtable_cn.stb 
    src/bin/hamcore/strtable_en.stb 
    src/bin/hamcore/strtable_ja.stb 
    src/bin/hamcore/vpnweb_sample_cn.htm 
    src/bin/hamcore/vpnweb_sample_en.htm 
    src/bin/hamcore/vpnweb_sample_ja.htm 
    src/bin/hamcore/warning_cn.txt 
    src/bin/hamcore/warning_en.txt 
    src/bin/hamcore/warning_ja.txt 
    src/bin/hamcore/webui/cryptcom.cgi 
    src/bin/hamcore/webui/edituser.cgi 
    src/bin/hamcore/webui/error.cgi 
    src/bin/hamcore/webui/hub.cgi 
    src/bin/hamcore/webui/license.cgi 
    src/bin/hamcore/webui/listener.cgi 
    src/bin/hamcore/webui/localbridge.cgi 
    src/bin/hamcore/webui/login.cgi 
    src/bin/hamcore/webui/newhub.cgi 
    src/bin/hamcore/webui/redirect.cgi 
    src/bin/hamcore/webui/securenat.cgi 
    src/bin/hamcore/webui/server.cgi 
    src/bin/hamcore/webui/session.cgi 
    src/bin/hamcore/webui/user.cgi 
    src/bin/hamcore/webui/webui.css
)

add_custom_target(hamcore.se2 
    ${CMAKE_CURRENT_BINARY_DIR}/hamcorebuilder ${CMAKE_CURRENT_SOURCE_DIR}/src/bin/hamcore ${CMAKE_CURRENT_BINARY_DIR}/hamcore.se2
    SOURCES ${HAMCORE_FILES}
)
add_dependencies(hamcore.se2 hamcorebuilder)


#
# VPN Server.
#

add_executable(vpnserver src/vpnserver/vpnserver.c)
add_dependencies(vpnserver hamcore.se2)
target_link_libraries(vpnserver Mayaqua Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads ${READLINE_LIBRARIES} ${CURSES_LIBRARIES} ZLIB::ZLIB)


#
# VPN Client.
#

add_executable(vpnclient src/vpnclient/vpncsvc.c)
add_dependencies(vpnclient hamcore.se2)
target_link_libraries(vpnclient Mayaqua Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads ${READLINE_LIBRARIES} ${CURSES_LIBRARIES} ZLIB::ZLIB)


#
# VPN Bridge.
#

add_executable(vpnbridge src/vpnbridge/vpnbridge.c)
add_dependencies(vpnbridge hamcore.se2)
target_link_libraries(vpnbridge Mayaqua Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads ${READLINE_LIBRARIES} ${CURSES_LIBRARIES} ZLIB::ZLIB)


#
# VPN Command.
#

add_executable(vpncmd src/vpncmd/vpncmd.c)
add_dependencies(vpncmd hamcore.se2)
target_link_libraries(vpncmd Mayaqua Cedar OpenSSL::SSL OpenSSL::Crypto Threads::Threads ${READLINE_LIBRARIES} ${CURSES_LIBRARIES} ZLIB::ZLIB)


#
# Installation.
#

install(
    TARGETS Mayaqua Cedar vpnserver vpnclient vpnbridge vpncmd
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/hamcore.se2
    DESTINATION bin
)