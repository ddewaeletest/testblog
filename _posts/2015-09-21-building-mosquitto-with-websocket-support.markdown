---
title: Building Mosquitto with Websocket support
date: 2015-09-21T17:52:58+02:00
layout: post
categories: [iot, mqtt]
tags: [iot,mqtt,mosquitto]
---
The popular mosquitto webbrowser has been supporting websockets for a while now, but most distributions don't have it enabled by default. At the moment, the only way to have full websocket support in the browser is if you build mosquitto from the sources. This post will cover the steps needed in order to have full websocket in mosquitto.

## Prerequisites

We need to make sure that our environment has the required build tools to do the setup:

### CentOS

{% highlight bash %}
yum install gcc gcc-c++ make cmake openssl-devel libuuid-devel git
{% endhighlight %}

### Raspberry Pi

On the Raspberry Pi we can execute the following command.

{% highlight bash %}
sudo apt-get update
sudo apt-get install build-essential python quilt devscripts python-setuptools python3
sudo apt-get install libssl-dev libwrap0-dev libc-ares-dev
{% endhighlight %}


## Building libwebsockets from source

We'll start by getting the [canonical libwebsockets.org websocket library](https://github.com/warmcat/libwebsockets.git) and building it from source. 

This can be done using the following commands :

{% highlight bash %}
git clone https://github.com/warmcat/libwebsockets.git  
cd libwebsockets
git checkout v1.4-chrome43-firefox-36
mkdir build  
cd build  
cmake .. -DOPENSSL_ROOT_DIR=/usr/bin/openssl  
make  
sudo make install  
{% endhighlight %}


We need to generate the required make files using ```cmake```. Make sure to specify your OpenSSL location using the ```-DOPENSSL_ROOT_DIR``` param.

The output should look like this :

{% highlight bash %}
/Applications/CMake.app/Contents/bin/cmake .. -DOPENSSL_ROOT_DIR=/usr/bin/openssl

-- The C compiler identification is AppleClang 6.0.0.6000057
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- CMAKE_TOOLCHAIN_FILE=''
-- Found Git: /usr/bin/git  
Git commit hash: 3ae1bad
-- Performing Test HAVE_inline
-- Performing Test HAVE_inline - Success
-- Performing Test HAVE___inline__
-- Performing Test HAVE___inline__ - Success
-- Performing Test HAVE___inline
-- Performing Test HAVE___inline - Success
-- Looking for bzero
-- Looking for bzero - found
-- Looking for fork
-- Looking for fork - found
-- Looking for getenv
-- Looking for getenv - found
-- Looking for malloc
-- Looking for malloc - found
-- Looking for memset
-- Looking for memset - found
-- Looking for realloc
-- Looking for realloc - found
-- Looking for socket
-- Looking for socket - found
-- Looking for strerror
-- Looking for strerror - found
-- Looking for vfork
-- Looking for vfork - found
-- Looking for getifaddrs
-- Looking for getifaddrs - found
-- Looking for dlfcn.h
-- Looking for dlfcn.h - found
-- Looking for fcntl.h
-- Looking for fcntl.h - found
-- Looking for in6addr.h
-- Looking for in6addr.h - not found
-- Looking for inttypes.h
-- Looking for inttypes.h - found
-- Looking for memory.h
-- Looking for memory.h - found
-- Looking for netinet/in.h
-- Looking for netinet/in.h - found
-- Looking for stdint.h
-- Looking for stdint.h - found
-- Looking for stdlib.h
-- Looking for stdlib.h - found
-- Looking for strings.h
-- Looking for strings.h - found
-- Looking for string.h
-- Looking for string.h - found
-- Looking for sys/prctl.h
-- Looking for sys/prctl.h - not found
-- Looking for sys/socket.h
-- Looking for sys/socket.h - found
-- Looking for sys/stat.h
-- Looking for sys/stat.h - found
-- Looking for sys/types.h
-- Looking for sys/types.h - found
-- Looking for unistd.h
-- Looking for unistd.h - found
-- Looking for vfork.h
-- Looking for vfork.h - not found
-- Looking for zlib.h
-- Looking for zlib.h - found
-- Looking for 4 include files stdlib.h, ..., float.h
-- Looking for 4 include files stdlib.h, ..., float.h - found
-- Looking for stddef.h
-- Looking for stddef.h - found
-- Check size of pid_t
-- Check size of pid_t - done
-- Check size of size_t
-- Check size of size_t - done
-- Found ZLIB: /usr/lib/libz.dylib (found version "1.2.5") 
zlib include dirs: /usr/include
zlib libraries: /usr/lib/libz.dylib
Compiling with SSL support
-- Found OpenSSL: /usr/lib/libssl.dylib;/usr/lib/libcrypto.dylib (found version "0.9.8z") 
OpenSSL include dir: /usr/include
OpenSSL libraries: /usr/lib/libssl.dylib;/usr/lib/libcrypto.dylib
Searching for OpenSSL executable and dlls
OpenSSL executable: /usr/bin/openssl
Generating SSL Certificates for the test-server...
SUCCSESFULLY generated SSL certificate
Generating API documentation
-- Looking for RPMTools... - rpmbuild NOT FOUND
---------------------------------------------------------------------
  Settings:  (For more help do cmake -LH <srcpath>)
---------------------------------------------------------------------
 LWS_WITH_SSL = ON  (SSL Support)
 LWS_SSL_CLIENT_USE_OS_CA_CERTS = 1
 LWS_USE_CYASSL = OFF (CyaSSL replacement for OpenSSL)
 LWS_WITHOUT_BUILTIN_GETIFADDRS = OFF
 LWS_WITHOUT_CLIENT = OFF
 LWS_WITHOUT_SERVER = OFF
 LWS_LINK_TESTAPPS_DYNAMIC = OFF
 LWS_WITHOUT_TESTAPPS = OFF
 LWS_WITHOUT_TEST_SERVER = OFF
 LWS_WITHOUT_TEST_SERVER_EXTPOLL = OFF
 LWS_WITHOUT_TEST_PING = OFF
 LWS_WITHOUT_TEST_CLIENT = OFF
 LWS_WITHOUT_TEST_FRAGGLE = OFF
 LWS_WITHOUT_EXTENSIONS = OFF
 LWS_WITH_LATENCY = OFF
 LWS_WITHOUT_DAEMONIZE = ON
 LWS_USE_LIBEV = 
 LWS_IPV6 = OFF
 LWS_WITH_HTTP2 = OFF
---------------------------------------------------------------------
-- Configuring done
-- Generating done
-- Build files have been written to: /Users/ddewaele/Projects/MQTT/libwebsockets/build
{% endhighlight %}

Now that the build files have been generated we can start by building the library.
{% highlight bash %}
make

Scanning dependencies of target websockets
[  2%] Building C object CMakeFiles/websockets.dir/lib/base64-decode.c.o
[  4%] Building C object CMakeFiles/websockets.dir/lib/handshake.c.o
[  6%] Building C object CMakeFiles/websockets.dir/lib/libwebsockets.c.o
[  8%] Building C object CMakeFiles/websockets.dir/lib/service.c.o
[ 10%] Building C object CMakeFiles/websockets.dir/lib/pollfd.c.o
[ 12%] Building C object CMakeFiles/websockets.dir/lib/output.c.o
[ 14%] Building C object CMakeFiles/websockets.dir/lib/parsers.c.o
[ 16%] Building C object CMakeFiles/websockets.dir/lib/context.c.o
[ 18%] Building C object CMakeFiles/websockets.dir/lib/sha-1.c.o
[ 20%] Building C object CMakeFiles/websockets.dir/lib/alloc.c.o
[ 22%] Building C object CMakeFiles/websockets.dir/lib/header.c.o
[ 25%] Building C object CMakeFiles/websockets.dir/lib/client.c.o

..... after a lot of output .....

85 warnings generated.
[ 89%] Building C object CMakeFiles/websockets_shared.dir/lib/lws-plat-unix.c.o
[ 91%] Building C object CMakeFiles/websockets_shared.dir/lib/server.c.o
[ 93%] Building C object CMakeFiles/websockets_shared.dir/lib/server-handshake.c.o
[ 95%] Building C object CMakeFiles/websockets_shared.dir/lib/extension.c.o
[ 97%] Building C object CMakeFiles/websockets_shared.dir/lib/extension-deflate-frame.c.o
[100%] Building C object CMakeFiles/websockets_shared.dir/lib/extension-deflate-stream.c.o
Linking C shared library lib/libwebsockets_shared.dylib
[100%] Built target websockets_shared
{% endhighlight %}

{% highlight bash %}
sudo make install

[ 43%] Built target websockets
[ 45%] Built target test-client
[ 47%] Built target test-echo
[ 50%] Built target test-fraggle
[ 52%] Built target test-ping
[ 54%] Built target test-server
[ 56%] Built target test-server-extpoll
[100%] Built target websockets_shared
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/lib/pkgconfig/libwebsockets.pc
-- Installing: /usr/local/lib/libwebsockets.a
-- Installing: /usr/local/include/libwebsockets.h
-- Installing: /usr/local/include/lws_config.h
-- Installing: /usr/local/lib/libwebsockets_shared.dylib
-- Up-to-date: /usr/local/include/libwebsockets.h
-- Up-to-date: /usr/local/include/lws_config.h
-- Installing: /usr/local/bin/libwebsockets-test-client
-- Installing: /usr/local/bin/libwebsockets-test-server
-- Installing: /usr/local/bin/libwebsockets-test-server-extpoll
-- Up-to-date: /usr/local/bin/libwebsockets-test-client
-- Installing: /usr/local/bin/libwebsockets-test-fraggle
-- Installing: /usr/local/bin/libwebsockets-test-ping
-- Installing: /usr/local/bin/libwebsockets-test-echo
-- Installing: /usr/local/share/libwebsockets-test-server/favicon.ico
-- Installing: /usr/local/share/libwebsockets-test-server/leaf.jpg
-- Installing: /usr/local/share/libwebsockets-test-server/libwebsockets.org-logo.png
-- Installing: /usr/local/share/libwebsockets-test-server/test.html
-- Installing: /usr/local/share/libwebsockets-test-server/libwebsockets-test-server.key.pem
-- Installing: /usr/local/share/libwebsockets-test-server/libwebsockets-test-server.pem
{% endhighlight %}

## Building mosquitto from source

Now that we have a ```libwebsockets``` installed on our system, we can start building mosquitto from the source.

We'll start by downloading the sources.

{% highlight bash %}
wget http://mosquitto.org/files/source/mosquitto-1.4.4.tar.gz
{% endhighlight %}

### Enableling websockets

In order to enable websocket, we need to go into the ```config.mk``` and set the following to ```yes```

{% highlight bash %}
1:  WITH_WEBSOCKETS:=yes 
{% endhighlight %}

*Note :* This doesn't work on mac !


{% highlight bash %}
vi /Users/ddewaele/Projects/MQTT/mosquitto-1.4.4/src/mosquitto.c

rm -rf mosquitto-1.4.4
tar -zpxpvf mosquitto-1.4.4.tar.gz 
cd mosquitto-1.4.4
/Applications/CMake.app/Contents/bin/cmake . -DWITH_WEBSOCKETS=ON 
make
make install
{% endhighlight %}

## Starting mosquitto

### Config file

{% highlight bash %}
autosave_interval 1800
persistence true
persistence_file m2.db
persistence_location /var/mosquitto/
connection_messages true
log_timestamp true


listener 1883

listener 9001 127.0.0.1
protocol websockets
{% endhighlight %}

## Starting mosquitto

You can start mosquitto by pointing to the configuration file we just created.
You should see the MQTT broker starting with a standard mqtt protocol listener (port 1883) and a websocket listener running at port 9001.

{% highlight bash %}
./mosquitto -c mosquitto.conf
1442878411: mosquitto version 1.4.4 (build date 2015-09-22 01:32:55+0200) starting
1442878411: Config loaded from mosq.conf.
1442878411: Opening ipv4 listen socket on port 1883.
1442878411: Opening ipv6 listen socket on port 1883.
1442878411: Opening websockets listen socket on port 9001.
{% endhighlight  %}

## Testing it with HiveMQ

The HiveMQ Websocket client allows you to publish MQTT messages as well as subscribing to MQTT topics.
It provides you with a nice web interface. The first thing we need to do is to connect to our broker. We need to connect to the websocket port ```9001```.

![Git gh-pages]({{ site.url }}/assets/hivemq-client-connect.png)

As soon as it connects you'll see it in the mosquitto command outout :

{% highlight bash %}
1442878442: New client connected from 127.0.0.1 as clientId-H35rQAxL2c (c1, k60).
1442878474: New connection from ::1 on port 1883.
1442878474: New client connected from ::1 as mosqsub/39580-MacBook-P (c1, k60).
1442878485: Socket error on client mosqsub/39580-MacBook-P, disconnecting.
1442878488: New connection from ::1 on port 1883.
1442878488: New client connected from ::1 as mosqsub/39591-MacBook-P (c1, k60).
{% endhighlight %}

Once we're connected. we can start publishing messages and subscribe to topcis.

![Git gh-pages]({{ site.url }}/assets/hivemq-client.png)

## Testing command line

{% highlight bash %}
mosquitto_sub -t testtopic/# -d -v
Client mosqsub/39591-MacBook-P sending CONNECT
Client mosqsub/39591-MacBook-P received CONNACK
Client mosqsub/39591-MacBook-P sending SUBSCRIBE (Mid: 1, Topic: testtopic/#, QoS: 0)
Client mosqsub/39591-MacBook-P received SUBACK
Subscribed (mid: 1): 0
Client mosqsub/39591-MacBook-P received PUBLISH (d0, q0, r0, m0, 'testtopic/1', ... (8 bytes))
testtopic/1 testmsg3
{% endhighlight %}


## References

- [Mosquitto download](http://mosquitto.org/download/)
- [Mosquitto WebSocket support](http://jpmens.net/2014/07/03/the-mosquitto-mqtt-broker-gets-websockets-support/)
- [Mosquitto Docker Centos](https://github.com/toulouse/docker-centos7-mosquitto/blob/master/Dockerfile)
http://stackoverflow.com/questions/4743233/is-usr-local-lib-searched-for-shared-librariesx



