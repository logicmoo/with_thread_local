# with_thread_local
Call a Goal with local assertions


Installation using SWI-Prolog 7.1 or later:

    `?- pack_install(rtrace).`

  or

    `?- pack_install('https://github.com/logicmoo/rtrace.git'). `

This module uses [semantic versioning](http://semver.org/).

Source code available and pull requests accepted at
http://github.com/logicmoo/rtrace

```prolog

 / *Example usages: */ 
 
 with_prolog_flag(Flag,Value,Goal):- 
       current_prolog_flag(Flag,Was), 
       each_call_cleanup( 
                   set_prolog_flag(Flag,Value), 
                   Goal, 
                   set_prolog_flag(Flag,Was)). 
 
 

 % notrace/1 that is not like once/1 
  no_trace(Goal):- 
     ( 
     notrace((tracing,notrace)) 
      - 
    ('$leash'(OldL, OldL), 
     '$visible'(OldV, OldV), 
     each_call_cleanup( 
         notrace((visible(-all),leash(-all), 
              leash(+exception),visible(+exception))) 
         Goal, 
         notrace(('$leash'(_, OldL),'$visible'(_, OldV),trace)))) 
    ; 
    Goal). 
            
 
 % Trace non interactively 
 with_trace_non_interactive(Goal):- 
    (   tracing- Undo=trace ; Undo = notrace ), 
 
    '$leash'(OldL, OldL), 
    '$visible'(OldV, OldV), 
    each_call_cleanup( 
         notrace((visible(+all),leash(-all),leash(+exception),trace)), 
         Goal, 
         notrace(('$leash'(_, OldL),'$visible'(_, OldV),Undo))) 

```

[BSD 2-Clause License](LICENSE.md)

Copyright (c) 2017, 
Douglas Miles <logicmoo@gmail.com>
All rights reserved.
