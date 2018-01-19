# MIMTeam.Housekeeping
Designing and scheduling Housekeeping policy entirely within the FIM Portal

# Author
Bob Bradley

Selected FIM policy which drives workflow (such as email notifications or data integrity checks and corrections) can be scheduled to be selectively re-applied using nothing more than some simple but imaginative FIM schema and policy extensions, and leveraging a simple workflow activity to set the current datetime.

> (from the TEC 2012 Presentation Topic Summary)

> Give FIM the gift of Self-Healing
> Have you written FIM policy to cover all the use cases you can think of, and yet you’re still worried you’ve missed something?  Perhaps you feel like the boy with his finger in the dam … not really certain when things might just come crashing down around you?  Wish you had the assurance that the “FIM Fairy” to run along behind you and make sure every event was captured, every workflow run without error, and every policy change retrospectively applied?  Well such things are now possible, and you don’t even have to leave the FIM Portal to do it!
> 
> Introducing Self-Healing Policy for FIM.
> 
> In this session, learn how a simple design principle centred on a single custom class can be used to deliver self-healing policy entirely within the FIM Portal.  Using the same workflows you have constructed to implement you regular FIM policy, a consistent yet imaginative approach is used to retrospectively apply policy on regular cycles to suit the occasion.  Missed an event?  No problem!  The FIM housekeeping fairy will save you with her email notifications and data integrity checks and corrections, thereby ensuring the ongoing integrity of your FIM Portal.

# Background
Often FIM data is updated at a point in time as a result of a workflow action.  Sometimes the circumstances which normally lead to an expected update of a FIM object can be such that a trigger/event is missed, resulting in that object being in an unexpected state, and while the circumstances might be able to be recreated, remedial action is at best an ad-hoc and admin-intensive activity.  In such circumstances a PowerShell script might normally be created to periodically check for inconsistencies and either alert to the need for remediation, or even attempt automatic remediation.  However, the latter is not so easy when the key to remediation is encapsulated in FIM Portal policy.  Also, when notifications are involved, the same FIM email templates that form part of standard FIM policy should be leveraged.

# Concept
By adopting a prescribed approach to repeatable “FIM Housekeeping” policy design, a generic extensible model can be applied to reinforce FIM policy, incorporating:

* A single “housekeeping” resource type, incorporating bindings for
** a single target (set transition only) MPR reference
** a last run time, and
** a run frequency interval (days)
* Temporal sets to identify housekeeping jobs falling due at varying intervals
* A single workflow to activate (and immediately deactivate) the target MPR for a housekeeping instance, and to set the last run time to the current datetime
* Instances of the housekeeping resource to target set transition MPRs which can be rerun over an arbitrary set of FIM objects to execute an arbitrary workflow (these can be existing MPRs, or new ones specifically designed to run in this fashion).

# More Details Attached
See the files in the repository for full details, including schema and policy definitions.

Note: the default Function Evaluator would be the only workflow activity that you would need if only it was possible to set a DateTime property to the current date time ... perhaps somehow using the xpath function "fn:current-dateTime()".  However I haven't managed to achieve this myself, so I used my own custom workflow activity here.  You may need to do the same, but other than this, all you need to implement this policy is explained herein.