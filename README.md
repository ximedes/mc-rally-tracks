# Usage

Detailed usage on Rally can be found at the [Rally docs](https://esrally.readthedocs.io/en/stable/quickstart.html),
but to quickly get started, here's the command to run Rally:

```
$ esrally
  --target-hosts=<hostname>:<port>
  --track=mc
  --client-options="use_ssl:true,verify_certs:true,timeout:60"
  --pipeline=benchmark-only
  --telemetry=node-stats
  --track-params="params/<file>.json" 
```

Be sure to replace `<hostname>` and `<port>` with the proper values for the ES cluster
you're running Rally against.

See below under **Available load** what to use for `<file>` for the `--track-params`. Note
that this argument is required for our tracks.

You can specify the additional option `--user-tag="some-key:some-value"` if you need it,
this will be added to all metrics as `meta.key_some-key = some-value`.

Here's an example for quick copy/paste-ing that only needs the hostname to be replaced and
runs against the _biggest_ division on ACC:

```
esrally --target-hosts=HOSTNAME:443 --track=mc --client-options="use_ssl:true,verify_certs:true,timeout:60" --pipeline=benchmark-only --telemetry=node-stats --track-params="params/ultra.json" 
```

# Running specific tasks

You can specify specific tasks to run, if you don't want to run the entire track. Use
the `--include-tasks` argument to do so. All of our tasks are named after our operations,
which can all be found under the `./operations/` directory; their filename is the same
as their operation name (and thus their task name).

For example, to only run the decline query used by the statistics KPI, use:

```
$ esrally --include-tasks="kpi-statistics-declines" ... (other args omitted for brevity)
```

See [Rally docs on `--include-tasks`](https://esrally.readthedocs.io/en/stable/command_line_reference.html#clr-include-tasks)
for details.



# Available load

The following divisions exist in ACC with (roughly) these number of locations
and transactions per division (as of 2018-08-16). Note that the `mc-rally-trx`
alias must be added manually to any index that needs to be included in the
rally tests, which (as of 2018-08-16) only contains the indexes for 2017-10 up
to and including 2018-08, so these numbers should not change too much as long
as the divisions and locations are kept intact.

To be able to quickly switch between different loads, the divison ID used by
the search queries is parameterized. You can either specify this directly
when running Rally, like so:

```
$ esrally --track-params="division_id:'<uuid>'"
```

or you can specify a file that contains this parameter, some of which we have
predefined as shown in the table below:

```
$ esrally --track-params="params/large.json"
```

| Division ID                          | # of Locs | # of Trx    | Params |
| ------------------------------------ | --------: | ----------: | ------ |
| 01618fdb-9606-40d3-91b6-9b5d24f1005f |         1 |      51.555 | tiny   |
| 016412b0-cb01-4947-8fb1-dcd5bc7b8695 |         1 |      51.555 |        |
| 0164d670-aea8-4438-8647-d9455f7d1b4c |         1 |          58 |        |
| 01626c49-9a0f-4fb2-b540-f3b6d70f9884 |         2 |      10.406 |        |
| 0164ffb5-121e-4821-9d8c-48754c0d308e |         3 |      19.937 |        |
| 01639172-dfc5-43c2-a241-7005796ad263 |         6 |      23.297 |        |
| 01626c49-9a04-4855-abe6-2044cb204c5e |        25 |     298.211 | small  |
| 01626c49-99f7-42ce-a699-fdf37680e8cc |       109 |     394.621 |        |
| 0164696f-c618-46e8-ba30-b9d1d6098426 |       127 |   2.480.003 | medium |
| 0162e47f-405e-4c45-aa96-0577438a904e |       151 |     149.937 |        |
| 016219ac-f7e0-4d79-85cf-e82cfa1f9b3a |       262 |  10.688.447 |        |
| 016200fb-b4e6-4d15-8a8d-ac0fbb663125 |       382 |  21.650.951 | large  |
| 016199f3-224d-4bb4-9f02-8f1517bed30f |       608 |  22.435.060 |        |
| 01626c43-fe89-483e-9265-2a4989895437 |       676 |  10.987.043 |        |
| 01626c49-9992-4e01-87bb-7272cf33013e |       676 |  10.987.043 |        |
| 0162f216-bf88-47fc-81ee-3320c40c0837 |       756 |     546.327 |        |
| 016193a0-12e0-429b-b316-9b6d77a939d0 |       954 |     931.980 |        |
| 01626c49-954a-4463-9ef1-507c8d44697c |      1572 |  67.054.980 | huge   |
| 0162cee8-8d3f-4342-9954-db6ed747aea7 |      1572 |  67.054.980 |        |
| 01626bb7-51ca-40f4-aa18-4620f0d8de59 |      8967 |   9.812.063 |        |
| 01626c49-965f-46ef-b9b2-8e881b0979b1 |      8980 |   9.824.436 |        |
| 0162e414-73d1-4359-911a-73ce9576edcd |     18566 |  79.168.491 | mega   |
| 01626c5b-bb28-4c33-af95-4586ca001c0f |     19823 | 336.019.506 | turbo  |
| 01618ff2-1ad4-417d-94bf-d98816cec345 |     31191 | 424.608.947 | ultra  |



