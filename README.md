Geoserver
=========

The geoserver sub-module for geonetwork.  It builds a custom geoserver.

Upgrade of GeoServer
--------------------

* Choose a version

* Update the version of GeoServer in the ``pom.xml``.
```
    <geoserver.version>2.9.0</geoserver.version>
```

* Then:
```
export GS_VERSION=2.9.0
export GN_SOURCE=/data/dev/gn

cd $GN_SOURCE
git clone git@github.com:geonetwork/core-maven-repo.git

# Download the latest stable version of GeoServer war package
mkdir geoserver-$GS_VERSION-war
cd geoserver-$GS_VERSION-war
wget http://pilotfiber.dl.sourceforge.net/project/geoserver/GeoServer/$GS_VERSION/geoserver-$GS_VERSION-war.zip

#  Unzip the file 
unzip geoserver-$GS_VERSION-war.zip 

# Deploy the war package in the local maven repository
mvn install:install-file -DgroupId=org.geoserver -DartifactId=geoserver -Dversion=$GS_VERSION -Dpackaging=war -DcreateChecksum=true -Dfile=geoserver.war 
cd ..
rm -fr geoserver-$GS_VERSION-war

# Copy the artifact from your local maven repository to ``maven_repo`` module.

cp -R $HOME/.m2/repository/org/geoserver/geoserver/$GS_VERSION $GN_SOURCE/core-maven-repo/org/geoserver/geoserver

# Configure GeoServer with custom data dir
cd  $GN_SOURCE/core-maven-repo/org/geoserver/geoserver/$GS_VERSION
unzip geoserver-$GS_VERSION.war -d war

```


* Unzip ``geoserver.war`` and copy the file ``WEB-INF\web.xml`` to ``GN_SOURCES_PATH/geoserver/src/main/webapp/WEB-INF/web.xml``, adding the following section for the data directory.
```
   <context-param>
      <param-name>GEOSERVER_DATA_DIR</param-name>
      <param-value>../geonetwork/data/geoserver_data</param-value>
   </context-param>
``` 
