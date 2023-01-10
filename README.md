# intro
example jvm plugin for es 2.2

# references
```
https://www.luminis.eu/blog/search-en/creating-an-elasticsearch-plugin-the-basics/
https://github.com/elastic/elasticsearch/tree/2.2/plugins/jvm-example
https://github.com/codelibs/elasticsearch-plugin-example
```

# build
```
$ export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_74/
$ mvn clean package
$ ls target/ | grep -E "jar|zip"
jvm-example-1.0-SNAPSHOT.jar
jvm-example-1.0-SNAPSHOT-plugin.zip
```

# inspect content of the zip file
```
$ unzip -l target/jvm-example-1.0-SNAPSHOT-plugin.zip  | sort
---------                     -------
---------  ---------- -----   ----
        0  2023-01-10 11:14   bin/
        0  2023-01-10 13:23   config/
       10  2023-01-10 11:14   bin/test.bat
       10  2023-01-10 11:14   config/example.yaml
    10804                     7 files
       21  2023-01-10 11:14   bin/test
     2803  2023-01-10 13:38   plugin-descriptor.properties
     7960  2023-01-10 14:02   jvm-example-1.0-SNAPSHOT.jar
Archive:  target/jvm-example-1.0-SNAPSHOT-plugin.zip
  Length      Date    Time    Name
```


# install zip file to the es instance
```
$ ./plugin install file:./jvm-example-1.0-SNAPSHOT-plugin.zip 
-> Installing from file:./jvm-example-1.0-SNAPSHOT-plugin.zip...
Trying file:./jvm-example-1.0-SNAPSHOT-plugin.zip ...
Downloading .DONE
Verifying file:./jvm-example-1.0-SNAPSHOT-plugin.zip checksums if available ...
NOTE: Unable to verify checksum for downloaded plugin (unable to find .sha1 or .md5 file to verify)
Installed jvm-example into /elasticsearch-2.2.1/plugins/jvm-example


and list plugin installed
$ ./plugin list
Installed plugins in /elasticsearch-2.2.1/plugins:
    - jvm-example

log output
$ ./elasticsearch
[2023-01-10 14:16:40,283][INFO ][node                     ] [Nightshade] version[2.2.1], pid[68790], build[d045fc2/2016-03-09T09:38:54Z]
[2023-01-10 14:16:40,284][INFO ][node                     ] [Nightshade] initializing ...
[2023-01-10 14:16:40,622][INFO ][plugins                  ] [Nightshade] modules [lang-expression, lang-groovy], plugins [], sites []
[2023-01-10 14:16:40,638][INFO ][env                      ] [Nightshade] using [1] data paths, mounts [[/ (/dev/nvme0n1p2)]], net usable_space [220.2gb], net total_space [914.3gb], spins? [no], types [ext4]
[2023-01-10 14:16:40,638][INFO ][env                      ] [Nightshade] heap size [989.8mb], compressed ordinary object pointers [true]
[2023-01-10 14:16:41,921][INFO ][node                     ] [Nightshade] initialized
[2023-01-10 14:16:41,922][INFO ][node                     ] [Nightshade] starting ...
[2023-01-10 14:16:42,045][INFO ][transport                ] [Nightshade] publish_address {127.0.0.1:9300}, bound_addresses {[::1]:9300}, {127.0.0.1:9300}
[2023-01-10 14:16:42,052][INFO ][discovery                ] [Nightshade] elasticsearch/nxrWzceIQri8_BJtB4XemA
[2023-01-10 14:16:45,099][INFO ][cluster.service          ] [Nightshade] new_master {Nightshade}{nxrWzceIQri8_BJtB4XemA}{127.0.0.1}{127.0.0.1:9300}, reason: zen-disco-join(elected_as_master, [0] joins received)
[2023-01-10 14:16:45,137][INFO ][http                     ] [Nightshade] publish_address {127.0.0.1:9200}, bound_addresses {[::1]:9200}, {127.0.0.1:9200}
[2023-01-10 14:16:45,138][INFO ][node                     ] [Nightshade] started
[2023-01-10 14:16:45,152][INFO ][gateway                  ] [Nightshade] recovered [0] indices into cluster_state    


restart es

$ ./elasticsearch
[2023-01-10 14:22:46,781][INFO ][node                     ] [Bevatron] version[2.2.1], pid[69834], build[d045fc2/2016-03-09T09:38:54Z]
[2023-01-10 14:22:46,781][INFO ][node                     ] [Bevatron] initializing ...
[2023-01-10 14:22:47,176][INFO ][plugins                  ] [Bevatron] modules [lang-expression, lang-groovy], plugins [jvm-example], sites []
[2023-01-10 14:22:47,193][INFO ][env                      ] [Bevatron] using [1] data paths, mounts [[/ (/dev/nvme0n1p2)]], net usable_space [220.2gb], net total_space [914.3gb], spins? [no], types [ext4]
[2023-01-10 14:22:47,193][INFO ][env                      ] [Bevatron] heap size [989.8mb], compressed ordinary object pointers [true]
[2023-01-10 14:22:48,645][INFO ][node                     ] [Bevatron] initialized
[2023-01-10 14:22:48,645][INFO ][node                     ] [Bevatron] starting ...
[2023-01-10 14:22:48,736][INFO ][transport                ] [Bevatron] publish_address {127.0.0.1:9300}, bound_addresses {[::1]:9300}, {127.0.0.1:9300}
[2023-01-10 14:22:48,743][INFO ][discovery                ] [Bevatron] elasticsearch/N03VDZEBQBetjBcoZqpHvg
[2023-01-10 14:22:51,796][INFO ][cluster.service          ] [Bevatron] new_master {Bevatron}{N03VDZEBQBetjBcoZqpHvg}{127.0.0.1}{127.0.0.1:9300}, reason: zen-disco-join(elected_as_master, [0] joins received)
[2023-01-10 14:22:51,836][INFO ][http                     ] [Bevatron] publish_address {127.0.0.1:9200}, bound_addresses {[::1]:9200}, {127.0.0.1:9200}
[2023-01-10 14:22:51,836][INFO ][node                     ] [Bevatron] started
[2023-01-10 14:22:51,850][INFO ][gateway                  ] [Bevatron] recovered [0] indices into cluster_state
```

# test
```
$ curl 'localhost:9200/_cat/health'
1673331817 14:23:37 elasticsearch green 1 1 0 0 0 0 0 0 - 100.0% 
$ curl 'localhost:9200/_cat/configured_example'
foo 
$ 
```



