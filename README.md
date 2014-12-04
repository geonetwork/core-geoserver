Geoserver
=========

The geoserver sub-module for geonetwork.  It builds a custom geoserver.

Upgrade of GeoServer
--------------------

1. Download the latest stable version of GeoServer war package (ex. ``geoserver-2.4.2-war.zip``).

2. Unzip the file and deploy the war package in the local maven repository (set the value of ``-Dversion`` to the GeoServer version).
```
    $ unzip geoserver-2.4.2-war.zip 
    $ mvn install:install-file -DgroupId=org.geoserver -DartifactId=geoserver -Dversion=2.4.2 -Dpackaging=war -DcreateChecksum=true -Dfile=geoserver.war 
```
3. Copy the artifact from your local maven repository to ``maven_repo`` module.
```
    $ cp -R $HOME/.m2/repository/org/geoserver/geoserver/2.4.2 GN_SOURCES_PATH/maven_repo/org/geoserver/geoserver
```

4. Download the latest stable version of GeoServer chart plugin (ex. ``geoserver-2.4.2-charts-plugin.zip``).

5. Deploy the chart plugin in the local maven repository (set the value of ``-Dversion`` to the GeoServer version):
```
    $ mvn install:install-file -DgroupId=org.geoserver -DartifactId=charts-plugin -Dversion=2.4.2 -Dpackaging=zip -DcreateChecksum=true -Dfile=geoserver-2.4.2-charts-plugin.zip
```

6. Copy the artifact from your local maven repository to ``maven_repo`` module.
```
    $ cp -R $HOME/.m2/repository/org/geoserver/charts-plugin/2.4.2 GN_SOURCES_PATH/maven_repo/org/geoserver/charts-plugin
```

7. Unzip ``geoserver.war`` and copy the file ``WEB-INF\web.xml`` to ``GN_SOURCES_PATH/geoserver/src/main/webapp/WEB-INF/web.xml``, adding the following section for the data directory.
```
   <context-param>
      <param-name>GEOSERVER_DATA_DIR</param-name>
      <param-value>../geonetwork/data/geoserver_data</param-value>
   </context-param>
``` 

8. Update the version of GeoServer in the ``pom.xml``.
```
    <geoserver.version>2.4.2</geoserver.version>
```
