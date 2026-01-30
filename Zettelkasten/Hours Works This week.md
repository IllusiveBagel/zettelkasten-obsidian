26-01-2026 09-48
Status: #idea
Tags: [[Asshole Boss]]

# Hours Works This week

## 26-01-2026
9:30: Start
9:45: Task 8317
9:55: Got a coffee
10:14: Task 8317 to DEV
10:27: Task 8125
11:37: Task 8125 to DEV
11:44: Task 8162
12:10: Fixed Issue with 8317
12:22: Task 8162 and 8317 to DEV
12:33: Task 8158
12:42: Investigated 500 Errors for kih student portal
13:19: Deployed Potential fix for 500 errors
14:45: Past hour and a half spent learning how to get the new kih config working and testing changes
14:54: kih portals back up and running in dev
15:08: Back to 8158
15:35: Task 8158 to DEV
15:51: Mentor Portal to PROD
16:28: Deployed kih portal changes to PROD
16:30: Task 8159
16:32: Looking at URL length issue with KIH changes and rolled back prod
17:44: Trial and Error in dev to use serve for kih issue
18:00: Got build to run but with wrong path deployed and signing off for the day

## 27/01/2026
9:07: Start
09:14: Verified KIH changes are working
09:24: Task 8159
09:43: Finished 9:30 morning meeting
10:00: Investigated Deployment slots for Azure
10:14: Back to Task 8159
10:40: Got a coffee
11:30: Deployed a21 pupil portal with potential fix for 8159
11:48: Prepared KIH changes for PROD release
12:03: Investigated deployment slots for A21 projects but permissions were wrong spoke to Alex
12:30: Deployed KIH portals and A21 pupil portal to PROD
12:35: Configured A21 Pupil Portal pipeline to use staging slot and swap
12:55 Got an error around permissions while setting up the staging
13:39: Got A21 Pupil staging slot working and deployment pipeline deploys then swaps the slots
14:00: Went on lunch
15:04: Back to work
15:10: Started writing some tickets to update A21 projects to align them with KIH and modernise the frameworks used - Task 8333
15:18: Task 8334
16:10: Got mentor portal running without running the dotnet server
16:16: Planning moving to vite and researching what steps need to be taken and what the potential difficulties will be
17:02: Got mentor portal transferred over to using vite
17:14: Testing Mentor Portal locally to check for any issues after the transfer
17:31: Issue with Register component in shared component causing blank page in mentor portal
17:32: Signed off for the day

## 28/01/2026
09:30: Start
09:45: Start 8244 + 8246
10:21: 8244+8246 to DEV
10:26: Back to 8334
11:22: Updated common library to change how SVG's are imported in Register component
11:29: Verified updates work in Mentor Portal
11:35: Verifying no other pages or components are effected
11:48: Verifying Build
12:02: Modifying pipeline file for new setup
13:03: Fixing warnings in mentor portal code
13:36: Removing unused code from mentor portal
14:02: went on lunch
15:00: back from lunch
15:19: Going through all files seeing what is still needed and what can be removed
15:33: Got rid of all the outdated PDF's that arent used to cut some bulk from the build
15:57: Merged 8334 into dev and tested build
16:39: Discovered that the Azure web app being windows is causing issues asked Alex how easy it would be to change it to Linux
17:01: Successfully deployed the refactored Mentor Portal to Dev on the windows instance
17:08: squashed commits where I was figuring out the pipeline issues into one commit
17:20: Started 8335
17:41: Signing off for the day
## 29/01/2026
9:25: Start
9:50: Investigating impersonate not working in dev on mentor portal
10:25: Discovered vite wasn't handling the dots often found in jwt tokens. Deploying fix to DEV
10:33: Impersonate is working locally but doesn't work on DEV
10:43: Added web.config to Mentor Portal to fix Masq in DEV
10:44: Going back to 8335
11:26: Removed dotnet code and got pupil portal running with vite
11:45: got a coffee
12:26: Cleaned up warning and commented out code in pupil portal
12:30: removed unused packages from pupil portal
12:44: Looked at 8360 but has complications because of Mentor Portal refactor asked Alex for advice
13:31: Released 8360 to PROD
13:42: Merged 8335 into DEV and started build/release
13:49: Verified working in DEV
13:51: Started 8336
14:16: went of lunch
15:19: Back from lunch
15:22: Investigating issues getting vite running in teacher portal
15:27: Got teacher portal running with vite
16:26: Fixed warning and removed unused files and code
16:36: Updated pipeline and README and removed unused packages
16:49: Merged 8336 to DEV and started build/deploy
16:55: Started investigating creation of POC react app to connect to AD and get user roles
17:33: Signing off for the day

## 30/01/2026
09:26: Start
09:37: Created POC project for AD
10:11: Built basic implementation and getting Alex to create Azure Registration to test against
10:46: Got POC to authenticate but issues getting roles
10:50: Asked Alex about roles assigned in azure to narrow down if issue is code or config
11:12: App not calling anything after login need to figure out different structure
12:10: structured everything differently and authenticated correctly now to get the roles and display them
12:17: Got roles displayed
12:35: Got a coffee
12:50: Start Mentor Portal OOHN PROD setup. Asked Alex if that change should be isolated or combined with  the refactor
13:09: Mentor portal cherry picked all commits verifying before pushing
13:24: Mentor Portal deployed to PROD
13:30: Meeting
14:00: Gone on lunch
14:54: Back from lunch
15:00: one on one with Alex
15:18: Looking into further improvements for A21 Portals
15:54: Lots more Mentor Portal code clean ups
16:20: Adding aliases to clean up imports and avoid relative paths
16:40: Got Components alias done
17:15: With the split between mentor portal and admin centre the normal find and replace is causing issues with the aliases
17:30: Signing off for the day

---
# References