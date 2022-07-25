# cloudtrail2sightings

## How to use

1. Download Cloudtrail data into this directory. This project assumes any Cloudtrail data it processes to be from an incident, or from known attacks on your environment. For generating Cloudtrail data attached to known attack techniques, you can use [stratus-red-team](http://stratus-red-team.cloud/) or from flaws.cloud [public dataset](https://summitroute.com/blog/2020/10/09/public_dataset_of_cloudtrail_logs_from_flaws_cloud/) of logs from attacks in this environment. 

I added `cloudtrail.zip` to the top level directory here if you want to play with a lot of Cloudtrail logs!

2. Ensure the data is in the correct Cloudtrail format. `jq -r 'has("Records")' < ./path/to/datafile.json` should return `true`. Data downloaded from Cloudtrail _should_ be in this format.

```bash
└> jq -r 'has("Records")' < event_history.json
true
```
3. Add newlines to the datafile (1 to end of line plus 1 more empty). This ensures vector knows when to stop processing. `echo "\n" >> ./path/to/datafile.json`

4. This will writeout a directory called `local_cloudtrail_logs` to keep track of where it processed files. If this exists, go ahead and delete it `rm -rf local_cloudtrail_logs`. It will also writeout all processed cloudtrail logs to `sightings.json`, you can delete this too via `rm -rf sightings.json`.

5. Run vector `vector --config vector.toml`. It will start to write data out to `sightings.json`

6. If you want to run it again and combine steps 4 & 5: `rm -rf sightings.json local_cloudtrail_logs/ && vector --config vector.toml`


## Processing sightings data, useful queries