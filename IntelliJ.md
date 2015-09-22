#IntelliJ

##Enable Annotation processing in IntelliJ
* Build, Execution, Deployment > Compiler > Annotation Processors > Enable annotation processing
* Plugins > Browse repositories > Lombok
* Add @ComponentScan annotation to your @SpringBootApplication, otherwise @Autowired can't be found by IntelliJ (?)