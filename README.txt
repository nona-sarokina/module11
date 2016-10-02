Apache configuration.

Please copy mod_jk.so file to %apache_folder%/modules folder

Create in the %apache_folder%/conf folder worker.properties file with content
    worker.list=module11
    worker.module11.type=ajp13
    worker.module11.host=localhost
    worker.module11.port=8009

or copy sample from the project root.

Add to the following lines into %apache_folder%/conf/httpd.conf

    LoadModule jk_module modules/mod_jk.so
    JkWorkersFile "conf/worker.properties"
    JkUnMount /module11/resources/* module11
    JkMount  /module11/* module11

for apache 2.4
    Alias /module11/resources "%folder_which_contains_published_files%"
    <Directory "folder_which_contains_published_files%">
        Options Indexes FollowSymLinks MultiViews
        AllowOverride All
        Require all granted
    </Directory>

for apache 2.2
    <Directory "folder_which_contains_published_files%">
        Options Indexes MultiViews
        AllowOverride None
        Order allow,deny
        Allow from all
    </Directory>
=========================================================================================
Tomcat (was tested on the 7.0.72 version).
In the tomcat-users.xml file specify roles and define user with password and roles like

  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <user username="manager" password="manager" roles="manager-gui,manager-script"/>
=========================================================================================
Maven.
Add server into settings.xml maven file
    <server>
        <id>module11Server</id>
        <username>manager</username>
        <password>manager</password>
    </server>

Specify the same folder as "%folder_which_contains_published_files%" in the pom.xml for
the static module for static.output.directory
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <static.output.directory>"%folder_which_contains_published_files%"</static.output.directory>
    </properties>
=========================================================================================
Running.
clean install tomcat7:deploy