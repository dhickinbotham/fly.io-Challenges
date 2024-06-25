<picture>
 <source media="(prefers-color-scheme: dark)" srcset="[YOUR-DARKMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <source media="(prefers-color-scheme: light)" srcset="[YOUR-LIGHTMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <img alt="YOUR-ALT-TEXT" src="https://fly.io/static/images/docs-machines-fast.webp">
</picture>

## Move Fly App to another Region

One of the features of Fly.io is the ability to run your app on a server in a [region](https://fly.io/docs/reference/regions/) close to your userbase. You even have the ability to completely move a Fly App from one region to a new region.

### Determine which region(s) your app is currently in
To display what region(s) your app has Machines and Volumnes in, you can check using `fly machines list`. You will see the app with each Machine and Volume listed and additional information as well as which region(s) they are in. Here is an example of the outputs shown: 

```
1 machine has been retrieved from app AppName.
View it in the UI here (​https://fly.io/apps/AppName/machines/)

AppName
ID              NAME                    STATE   CHECKS  REGION  ROLE    IMAGE                   IP ADDRESS
        VOLUME                  CREATED                 LAST UPDATED            APP PLATFORM    PROCESS GROUP   SIZE

908016eec66d##  morning-mountain-####   stopped         syd             flyio/hellofly:latest   fdaa:9:6dfe:a7b:2d8:d930:##a8:2 vol_49kwg##gk8dwg39v    2024-06-25T14:20:36Z    2024-06-25T14:35:20Z    v2              app             shared-cpu-1x:1024MB
```

### Create a copy of a volume (Fork a Volume)
Create an exact copy of a volume, including its data, in the region you want by forking the current volume.

<div class="alert alert-info important icon" role="alert"><p><strong class="font-[550] text-navy-950">Important:</strong> After you fork a volume, the new volume is independent of the source volume. The new volume and the source volume do not continue to sync.</p>
</div>

#### Find the Volume ID

`fly volumes list`
```
ID                      STATE   NAME            SIZE    REGION  ZONE    ENCRYPTED       ATTACHED VM     CREATED AT
vol_49kwg##gk8dwg39v    created myapp_data      1GB     syd     8f69    true            908016eec66d##  6 minutes ago
```

#### Fork the Volume

`fly volumes fork vol_49kwg##gk8dwg39v --region ord`
```
                  ID: vol_49kwg##ww5xl07ov
                Name: myapp_data
                 App: AppName
              Region: ord
                Zone: 7074
             Size GB: 1
           Encrypted: true
          Created at: 25 Jun 24 14:27 UTC
  Snapshot retention: 5
 Scheduled snapshots: true
```

### Clone Machine and attach Forked Volume
Once you have forked your Machine's volume, you can now clone the machine and attach the newly forked volume with the following command: `fly machine clone <machine id> --region <region code> --attach-volume <volume id>:<destination mount path>`

`fly machine clone 90801690c606## --region ord --attach-volume vol_49kwg##ww5xl07ov:/data`
```
Cloning Machine 90801690c606## into region ord
Attaching existing volume vol_49kwg##ww5xl07ov
Provisioning a new Machine with image docker-hub-mirror.fly.io/flyio/hellofly:latest...
  Machine 857100a4e917## has been created...
  Waiting for Machine 857100a4e917## to start...
No health checks found
Machine has been successfully cloned!
```
### Destroy old Volume
Once you have successfully forked and cloned the Volume and Machine into the new region, you can now destroy your old volume.
<div class="warning icon"><p><b>Warning:</b> When you destroy a volume, you permanently delete all its data.</p>
</div>

Run `fly volumes list` to get the Volume ID
```
ID                      STATE   NAME            SIZE    REGION  ZONE    ENCRYPTED       ATTACHED VM     CREATED AT
vol_49kwg##ww5xl07ov    created myapp_data      1GB     ord     7074    true            908016eec66d28  6 minutes ago
vol_49kwg##gk8dwg39v    created myapp_data      1GB     syd     8f69    true                            1 year ago
```

Destroy the Volume:
`fly volumes destroy <volume id>`

```
fly volumes delete vol_49kwg##gk8dwg39v
Warning! Every volume is pinned to a specific physical host. You should create two or more volumes per application. Deleting this volume will leave you with 1 volume(s) for this application, and it is not reversible.  Learn more at https://? Are you sure you want to destroy this volume? Yes
Destroyed volume ID: vol_49kwg##gk8dwg39v name: myapp_data
```

Once you have moved your app to it's new region, you can run `fly regions list` again to check which region(s) your app has machines/volumes in:

```
Regions [app]: ord
```

That's it! Your app is now running it's machine(s) with attached volume(s) in region(s) close to your userbase.

### Related Topics
- [Scale the Number of Machines](https://fly.io/docs/apps/scale-count/)
- [Fly scale count](https://fly.io/docs/flyctl/scale-count/)
- [Create and manage volumes](https://fly.io/docs/volumes/volume-manage/)
- [Scale an app with volumes](https://fly.io/docs/apps/scale-count/#scale-an-app-with-volumes)
- [Primary Region](https://fly.io/docs/reference/configuration/#primary-region)
- [Restore a Deleted Volume](https://fly.io/docs/volumes/volume-manage/#restore-a-deleted-volume)
