# C Documentation Checklist

- [ ] Did you cross-check the arguments and their meanings in the most recent version of the SOILWAT2 description paper?
- [ ] Are the arguments (@param) in order (as they are called in the function)?
- [ ] Is there one @param for each argument call in the function?
- [ ] Is there a grammatically correct sentence describing each argument/return/side effect?
- [ ] Are @return and @sideeffects called correctly?
  * A variable should be both input (@param) and a sideeffect (@sideeffect) if it is in the function arguments and then updated within the function.
  * A return is used when a new data object is created by the function.
- [ ] Does each @param/return/sideeffect have units?
  * Units should be written in scientific format (cm/day)
- [ ] Are citations properly documented in the SOILWAT2.bib file and referenced with @cite?
- [ ] Did you check that documentation formatting appears correct in the index.html copy?
- [ ] Are the reference links working in the index.html?
