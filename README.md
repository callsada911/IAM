# IAM 

This was done using GCp 

On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > IAM.
Click Add and explore the roles in the drop-down menu. Note the various roles associated with each resource by navigating the Roles menu.
Click Cancel.
Switch to the Username 2 Cloud Console tab.
On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > IAM. Browse the list for the lines with the names associated with Username 1 and Username 2 in the Qwiklabs Connection Details dialog.
Username 2 currently has access to the project, but does not have the Project Owner role, so it cannot edit any of the roles. Hover over the pencil icon for Username 2 to verify this.

Switch back to the Username 1 Cloud Console tab.

In the IAM console, for Username 2, click on the pencil icon. Username 2 currently has the Project Viewer role. Do not change the Project Role.

Click Cancel.

Task 3: Prepare a resource for access testing
Create a bucket and upload a sample file
Switch to the Username 1 Cloud Console tab if you aren't already there.
On the Navigation menu (7a91d354499ac9f1.png), click Storage > Browser.
Click Create bucket.
Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Name	Enter a globally unique name
Location type	Multi-Regional
Note the bucket name: it will be used in a later step and referred to as [YOUR_BUCKET_NAME]

Click Create.
Click Upload files.
Upload any sample file from your local machine.
After the upload completes, click Close on the upload window.
When the file has been uploaded, click on the three dots at the end of the line containing the file, and click Rename.
Rename the file to sample.txt, and click Rename.
Click Check my progress to verify the objective.
Create a bucket and upload a sample file

Verify project viewer access
Switch to the Username 2 Cloud Console tab.

In the Console, navigate to Navigation menu > Storage > Browser.

Verify that Username 2 can see the bucket.

Task 4: Remove project access
Remove Project Viewer role for Username 2
Switch to the Username 1 Cloud Console tab.
On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > IAM.
Select Username 2 and click the Remove icon.
Verify that you're removing access for Username 2. If you accidentally remove access for Username 1 you will have to restart this lab!
Confirm by clicking Confirm button.
Notice that the user has disappeared from the list! The user has no access now.

Click Check my progress to verify the objective.
Remove project access

Verify that Username 2 has lost access
Switch to the Username 2 Cloud Console tab.

On the Navigation menu (7a91d354499ac9f1.png), click Home.

On the Navigation menu (7a91d354499ac9f1.png), click Storage > Browser. An error will be displayed. If not, refresh the page. Username 2 still has a Google Cloud account, but has no access to the project.

Task 5: Add storage access
Add storage permissions
Copy the value of Username 2 from the Qwiklabs Connection Details dialog.
Switch to the Username 1 Cloud Console tab.
On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > IAM.
Click Add to add the user.
For New members, paste the Username 2 value you copied from the Qwiklabs Connection Details dialog.
For Select a role, select Cloud Storage > Storage Object Viewer.
Click Save.
Click Check my progress to verify the objective.
Add storage permissions

Verify that Username 2 has storage access
Switch to the Username 2 Cloud Console tab.
Username 2 doesn't have Project Viewer roles, so that user can't see the project or any of its resources in the Console. However, the user has specific access to Cloud Storage.

To start Cloud Shell, click Activate Cloud Shell (857dc9d7dd799cb2.png). If prompted, click Continue.

To view the contents of the bucket you created earlier, run the following command, replacing [YOUR_BUCKET_NAME] with the unique name of the Cloud Storage bucket you created:

gsutil ls gs://[YOUR_BUCKET_NAME]
As you can see, Username 2 has limited access to Cloud Storage.

Close the Username 2 Cloud Console tab. The rest of the lab is performed on the Username 1 Cloud Console tab.

Switch to the Username 1 Cloud Console tab.

Task 6: Set up the Service Account User
In this part of the lab, you assign narrow permissions to service accounts and learn how to use the Service Account User role.

Create a service account
On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > Service accounts.

Click Create service account.

Specify the Service account name as read-bucket-objects .

Click Create.

Specify the Role as Cloud Storage > Storage Object Viewer .

Click Continue.

Click Done.

Add the user to the service account
Select the read-bucket-objects service account.
Click Add member in the Permissions panel. If you do not see the Permission panel, click on Show Info panel.
You will grant the user the role of Service Account User, which allows that person to use a service account on a VM, if they have access to the VM.

You could perform this activity for a specific user, group, or domain.

For training purposes, you will grant the Service Account User role to everyone at a company called Altostrat.com. Altostrat.com is a fake company used for demonstration and training.

Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
New members	altostrat.com
Select a role	Service Accounts > Service Account User
Click Save.

Grant Compute Engine access
You now give the entire organization at Altostrat the Compute Engine Admin role.

On the Navigation menu (7a91d354499ac9f1.png), click IAM & admin > IAM.
Click Add.
Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
New members	altostrat.com
Select a role	Compute Engine > Compute Instance Admin (v1)
Click Save.
This step is a rehearsal of the activity you would perform for a specific user.

This action gives the user limited abilities with a VM instance. The user will be able to connect via SSH to a VM and perform some administration tasks.

Create a VM with the Service Account User
On the Navigation menu (7a91d354499ac9f1.png), click Compute Engine > VM instances.
Click Create.
Specify the following, and leave the remaining settings as their defaults:
Property	Value (type value or select option as specified)
Name	demoiam
Region	us-central1
Zone	us-centra1-c
Series	N1
Machine Type	f1-micro
Boot disk	Debian GNU/Linux 10 (buster)
Service account	read-bucket-objects
Click Create.
Click Check my progress to verify the objective.
Set up the Service Account User and create a VM

Task 7: Explore the Service Account User role
At this point, you might have the user test access by connecting via SSH to the VM and performing the next actions. As the owner of the project, you already possess the Service Account User role. So you can simulate what the user would experience by just using SSH to access the VM from the Cloud Console.

The actions you perform and results will be the same as if you were the target user.

Use the Service Account User
For demoiam, click SSH to launch a terminal and connect.

Run the following command:

gcloud compute instances list
Result (do not copy; this is example output):

ERROR: (gcloud.compute.instances.list) Some requests did not succeed:
 - Required 'compute.zones.list' permission for 'projects/qwiklabs-gcp'
What happened? Why?

Copy the sample.txt file from the bucket you created earlier. Note that the trailing period is part of the command below. It means copy to "this location":

gsutil cp gs://[YOUR_BUCKET_NAME]/sample.txt .
Result (do not copy; this is example output):

Copying gs://train-test-iam/sample.txt...
/ [1 files][   28.0 B/   28.0 B]
Operation completed over 1 objects/28.0 B.
To rename the file you copied, run the following command:

mv sample.txt sample2.txt
To copy the renamed file back to the bucket, run the following command:

gsutil cp sample2.txt gs://[YOUR_BUCKET_NAME]
Result (do not copy; this is example output):

AccessDeniedException: 403 Caller does not have storage.objects.create access to bucket train-test-iam.
What happened?

Because you connected via SSH to the instance, you can "act as the service account," essentially assuming the same permissions.The service account the instance was started with had the Storage Viewer role, which permits downloading objects from GCS buckets in the project.To list instances in a project, you need to grant the compute.instance.list permission. Because the service account did not have this permission, you could not list instances running in the project. Because the service account did have permission to download objects, it could download an object from the bucket. It did not have permission to write objects, so you got a "403 access denied" message.

Task 8: Review
In this lab you exercised granting and revoking Cloud IAM roles, first to a user, Username 2, and then to a Service Account User. You could allocate Service Account User credentials and "bake" them into a VM to create specific-purpose authorized bastion hosts.
