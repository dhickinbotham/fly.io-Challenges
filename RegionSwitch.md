<picture>
 <source media="(prefers-color-scheme: dark)" srcset="[YOUR-DARKMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <source media="(prefers-color-scheme: light)" srcset="[YOUR-LIGHTMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <img alt="YOUR-ALT-TEXT" src="https://fly.io/static/images/docs-machines-fast.webp">
</picture>

## Move Fly App to another Region

One of the features of Fly.io is the ability to run your app on a server in a [region](https://fly.io/docs/reference/regions/) close to your userbase. You even have the ability to completely move a Fly App from one region to a new region.

### Determine which region(s) your app is currently in
To list regions your app has Machine(s) in, you can check using `fly regions list`. You will see the app with each region listed. Here is an example of the outputs shown: 

```
Regions [app]: ord
```

### Scale an App's region
Moving an app with attached Fly Machine(s) and Fly Volume(s) can be done by [scaling your app](https://fly.io/docs/apps/scale-count/#scale-an-apps-regions) up in the region(s) you need them to be in and scaling down in the region(s) you no longer want the app to run in using `fly scale count` which creates and/or destroys Machines to reach the specified target count across the regions you list in this option.

To get a list of regions available use `fly platform regions`.

#### Scaling up in the region(s) you want to run in
To scale up a machine in a region, utilize the `fly scale count --region <region abbreviation>`. For example, if you wanted to scale up in the Sydney region, you would use the following: `fly scale count 1 --region syd`

```
App 'AppName' is going to be scaled according to this plan:
? Scale app AppName? (y/N) y
Executing scale plan
  Created 1781994c3d0### group:app region:ord size:shared-cpu-1x
```
#### Scaling down in the region(s) you no longer want to run in
To scale down a machine in a region, utilize the `fly scale count --region <region abbreviation>`. For example, if you had a machine in the Chicago region, you would use the following
scale down machine(s) in a region  `fly scale count 0 --region ord`

```
App 'AppName' is going to be scaled according to this plan:
? Scale app winde? (y/N) y
Executing scale plan
  Destroyed 080560f6620### group:app region:ord size:shared-cpu-1x
```
Once you have moved your app to it's new region, you can run `fly regions list` again to check which region(s) your app has machines in:

```
Regions [app]: syd
```

That's it! Your app is now running it's machines in region(s) close to your userbase.

### Related Topics
- [Scale the Number of Machines](https://fly.io/docs/apps/scale-count/)
- [Fly scale count](https://fly.io/docs/flyctl/scale-count/)
- [Create and manage volumes](https://fly.io/docs/volumes/volume-manage/)
- [Scale an app with volumes](https://fly.io/docs/apps/scale-count/#scale-an-app-with-volumes)
- [Primary Region](https://fly.io/docs/reference/configuration/#primary-region)
