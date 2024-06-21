<picture>
 <source media="(prefers-color-scheme: dark)" srcset="[YOUR-DARKMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <source media="(prefers-color-scheme: light)" srcset="[YOUR-LIGHTMODE-IMAGE](https://fly.io/static/images/docs-machines-fast.webp)">
 <img alt="YOUR-ALT-TEXT" src="https://fly.io/static/images/docs-machines-fast.webp">
</picture>

## Move Fly App to another Region


Scale up machine(s) in another region  fly scale count 2 --region syd
scale down machine(s) in a region  fly scale count 0 --region ord
Show VM Scale fly scale show
 VM Resources for app: winde

Groups
NAME    COUNT   KIND    CPUS    MEMORY  REGIONS
app     4       shared  1       1024 MB ord(2),syd(2)

List regions your app has Machine(s) in fly regions list

Run fly platform regions to get a list of regions. (More about Regions here: https://fly.io/docs/reference/regions/)
