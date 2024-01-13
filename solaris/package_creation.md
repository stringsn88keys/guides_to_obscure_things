# Example of package commands

# Details of package creation / manipulation commands
## `pkgsend`


## `pkgfmt`


## `pkgmogrify`
[details from this PR](https://github.com/chef/omnibus/pull/1144)

A depend directive appears as the following:
```
depend fmri=pkg:/shell/bash@5.1.16-11.4.45.0.1.119.0 type=require
^      ^                                             ^
|      |                                             |
|      +-attribute                                   +-attribute
+- action
```

The can be transformed using:
```
<transform depend fmri=pkg:/shell/bash -> edit fmri '@.+' ''>
 ^         ^      ^                    ^       ^     ^    ^
 |         |      |                    |       |     |    +-replacement string
 |         |      |                    |       |     | 
 |         |      |                    |       |     +-match regex
 |         |      |                    |       |     
 |         |      |                    |       +-name of attribute to modify
 |         |      |                    |       
 |         |      |                    +- matching-criteria / operation separator
 |         |      |                   
 |         |      +-"attribute=<value-regexp>" matching criteria
 |         |      
 |         +-"action-type matching criteria
 |         
 +- transform directive
```

Resulting in the following directive, with the version removed.
```
depend facet.version-lock.*>=false fmri=pkg:/shell/bash type=require
```

Since the attribute matching criteria is a regex, the transforms added can be reduced from:
```
<transform depend fmri=pkg:/shell/bash -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/shell/ksh93 -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/core-os -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/library/libc -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/library/math -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/library/security/crypto -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/library -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system/linker -> edit fmri '@.+' ''>
```

to:
```
<transform depend fmri=pkg:/shell -> edit fmri '@.+' ''>
<transform depend fmri=pkg:/system -> edit fmri '@.+' ''>
```