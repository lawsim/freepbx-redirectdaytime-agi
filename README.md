# freepbx-redirectdaytime-agi
Redirect calls during the daytime to voicemail script

To use:
Add the string "daytimesendoutsidetovm=yes" to the description of users this should apply to.

Modified extensions_override_freepbx.conf:

```
[from-did-direct]
include => from-did-direct-custom
include => ext-findmefollow
include => ext-local
```
Added to extensions_custom.conf:

```
[from-did-direct-custom]
exten => _XXXX,1,AGI(daytimeredirect.agi)
exten => _XXXX,n,NOOP(Forward state is ${FORWARDTOVM})
exten => _XXXX,n,GotoIf($["${FORWARDTOVM}" != "1"]?SkipForwardVM)
exten => _XXXX,n(ForwardVM),Voicemail(${EXTEN},u)
exten => _XXXX,n(SkipForwardVM),Goto(from-did-direct-continue,${EXTEN},1)

[from-did-direct-continue]
include => ext-findmefollow
include => ext-local
```

Wrote/placed this agi script into the AGI bin dir (/var/lib/asterisk/agi-bin).  Make sure you have unix line endings.