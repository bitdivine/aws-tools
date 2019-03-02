Basic idea:

* Creating and maintaining tight IAM policies is very time consuming.  How to make this fast?

* Create a role with super-liberal permissions - think admin - in a developer account.  This will be popular with developers because they hate running into IAM permissions.  Errors in code often appear as IAM error messages so by granting liberal permissions you give the developers all they need to debug problems themselves.

* Use cloudwatch to log which permissions the role actually uses

* Generate a policy that allows just those actions and use that tight role in production.

Implementation:

*  Collecting cloudwatch logs is easy with the aws cli.

* The logs are not all that useful as given, so extract just the role, action and resource with `cloudtrail-log-roletree`.

* At this point I use grep and uniq to eyeball the records and filter out the ones I don't want and to make some records more general.

* Convert the logs into a policy statement with `cloudtrail-log-roletree-policy`.

* Again, eyeball this and where it is too specific, modify.

* In production, use no shared policies.  Just one inline policy as generated above.
