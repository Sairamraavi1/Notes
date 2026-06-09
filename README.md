Sure Mark — what I meant is:

* SVC_VPR_ELEVATEDBLINK (or whichever service account gets whitelisted) would be used only for the DB link connectivity activities.
* Your individual account MTAMELI2 can be used for schema/work table validation activities during the snapshot read-write window.
* Since the testing happens only while VPRS is in snapshot mode, if any additional temporary users or objects are required, I can create them during the approved change window itself.
* Once VPRS is converted back to physical standby/read-only mode and resynchronized, all temporary DB links, users, and objects created during the snapshot window will automatically roll back and no longer exist.

So the goal is to minimize permanent provisioning while still supporting the validation activities needed during the testing window.