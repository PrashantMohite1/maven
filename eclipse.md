

**mvn clean install in eclipse**
right click on project --> run as --> mvn install


**skip Tests from Eclipse**
right click on project--> run as --> run configuration --> maven build --> right click on it select new configuration -->
- basedirectory : select our project , 
- goals : install , 
- enable skip test 


**where to see jars defined in maven dependency section**

go to eclipse --> in your project --> maven dependencies - here you will see all jars which you defined as dependencies.


basically new jar are first downloaded from maven remote repositories and store in our .m2 folder whenever require that same jar then maven will use that jar stored in m2. 

when we define any dependencies in pom.xml then maven will also download other dependencies required for that project 