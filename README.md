Geoserver
=========

The geoserver sub-module for geonetwork.  It builds a custom geoserver.

Upgrade of GeoServer
--------------------

* Choose a version http://geoserver.org/

* Then add this version to the core-maven-repo:
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
mvn install:install-file -DgroupId=org.geoserver \
  -DartifactId=geoserver -Dversion=$GS_VERSION \
  -Dpackaging=war -DcreateChecksum=true \
  -Dfile=geoserver.war 
cd ..
rm -fr geoserver-$GS_VERSION-war

# Copy the artifact from your local maven repository to ``maven_repo`` module.
cp -R $HOME/.m2/repository/org/geoserver/geoserver/$GS_VERSION \
   $GN_SOURCE/core-maven-repo/org/geoserver/geoserver

# Publish to github maven repo
cd core-maven-repo
git add .
git commit -m "Add GeoServer version $GS_VERSION."
git push origin master

```


* Then update GeoNetwork to use this new version:
```

cd ../develop/geoserver

git checkout -b $GS_VERSION 

# Update pom.xml to point to the new version
vi pom.xml
  <version>2.9.0</version>
  <properties>
    <geoserver.version>2.9.0</geoserver.version>
    
# Configure GeoServer data dir
vi src/main/webapp/WEB-INF/web.xml
# Update the web.xml with the one from the new version and update the GEOSERVER_DATA_DIR
   <context-param>
      <param-name>GEOSERVER_DATA_DIR</param-name>
      <param-value>../geonetwork/data/geoserver_data</param-value>
   </context-param>

# (Optional but recommended ?)
# Update the data dir with the default one


git add .
git commit -m "Update GeoServer to version $GS_VERSION."
cd ..
git add geoserver
git commit -m "Update GeoServer submodule."

``` 
