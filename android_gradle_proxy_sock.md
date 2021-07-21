android gradle set proxy

Method 1:
org.gradle.jvmargs=-Xmx2048m -DsocksProxyHost=127.0.0.1 -DsocksProxyPort=19181

Method 2:

System.setProperty(“socksProxyHost”,“127.0.0.1”)
System.setProperty(“socksProxyPort”,“19181”)
