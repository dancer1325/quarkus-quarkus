* `List<GeneratedResourceBuildItem> writeReflectionData(
  List<ReflectiveClassBuildItem> classes,
  List<ReflectiveMethodBuildItem> methods,
  List<ReflectiveFieldBuildItem> fields) {}`
  * if `quarkus.debug.reflection` == true -> write ALL reflective classes | "META-INF"

* `quarkus.debug.reflection`
  * == system property
