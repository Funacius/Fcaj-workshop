---
title: "Cleanup"
weight: 9
chapter: false
pre: "<b>5.9.</b>"
---

# Cleanup

Delete resources only after the instructor has reviewed the live project and all
required screenshots have been saved.

Recommended order:

1. Disable or delete the Amplify app if it is no longer required.
2. Disable the CloudFront distribution, wait for deployment, then delete it.
3. Remove the S3 OAC bucket policy and empty the upload bucket.
4. Delete the upload bucket.
5. Terminate the Elastic Beanstalk environment, then delete old application
   versions and deployment bundles.
6. Delete the Cognito User Pool and pre-sign-up Lambda if no longer needed.
7. Delete the two Parameter Store values.
8. Delete custom IAM roles and inline policies after confirming they are unused.
9. Pause or delete the Supabase project after exporting any required data.
10. Review AWS Billing and Cost Explorer for remaining resources.

> Note: Deleting Cognito, S3, Parameter Store, or the database can permanently
> remove accounts, uploads, secrets, and project data. Export what you need first
> and verify every resource name and Region before deletion.
