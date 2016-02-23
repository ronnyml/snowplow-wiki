The below link references the document reflecting internal component dependencies for each of the Snowplow releases. You can use it as a guidance when either building or upgrading Snowplow pipeline.

- [Snowplow Version Matrix](https://docs.google.com/spreadsheets/d/1oI3n7nUzNfCwuNJk-LNdMdkAwUt6kI1irPg1FCFyxKc/pubhtml?gid=1937493131&single=true)


<!---
Release version | SCE | RAE | PAE | AMI | CC | SHE | SCE in SHE | SHS | EER | SL | SSC | SKE | SCE in SKE | KES
---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
[R76 Changeable Hawk-Eagle](https://github.com/snowplow/snowplow/releases/tag/r76-changeable-hawk-eagle) | 0.20.1 | 0.8.0 | 0.7.0 | 3.7.0 | 1.1.0 | 1.5.1 | 0.20.1 | 0.7.0 | 0.20.0 | 0.6.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R75 Long-Legged Buzzard](https://github.com/snowplow/snowplow/releases/tag/r75-long-legged-buzzard) | 0.20.0 | 0.8.0 | 0.7.0 | 3.7.0 | 1.1.0 | 1.5.0 | 0.20.0 | 0.6.0 | 0.20.0 | 0.6.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R74 European Honey Buzzard](https://github.com/snowplow/snowplow/releases/tag/r74-european-honey-buzzard) | 0.19.0 | 0.8.0 | 0.7.0 | 3.7.0 | 1.1.0 | 1.4.0 | 0.19.0 | 0.6.0 | 0.19.0 | 0.6.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R73 Cuban Macaw](https://github.com/snowplow/snowplow/releases/tag/r73-cuban-macaw) | 0.18.0 | 0.8.0 | 0.7.0 | 3.7.0 | 1.1.0 | 1.3.0 | 0.18.0 | 0.6.0 | 0.19.0 | 0.6.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R72 Great Spotted Kiwi](https://github.com/snowplow/snowplow/releases/tag/r72-great-spotted-kiwi) | 0.17.0 | 0.7.0 | 0.6.0 | 3.7.0 | 1.1.0 | 1.2.0 | 0.17.0 | 0.5.0 | 0.18.0 | 0.4.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R71 Stork-Billed Kingfisher](https://github.com/snowplow/snowplow/releases/tag/r71-stork-billed-kingfisher) | 0.16.0 | 0.7.0 | 0.6.0 | 3.7.0 | 1.0.0 | 1.1.0 | 0.16.0 | 0.5.0 | 0.18.0 | 0.4.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R70 Bornean Green Magpie](https://github.com/snowplow/snowplow/releases/tag/r70-bornean-green-magpie) | 0.15.0 | 0.6.0 | 0.5.0 | 3.7.0 | 1.0.0 | 1.0.0 | 0.14.0 | 0.4.0 | 0.17.0 | 0.4.0 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R69 Blue-Bellied Roller](https://github.com/snowplow/snowplow/releases/tag/r69-blue-bellied-roller) | 0.15.0 | 0.6.0 | 0.5.0 | 3.6.0 | 1.0.0 | 1.0.0 | 0.14.0 | 0.4.0 | 0.16.0 | 0.3.3 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R68 Turquoise Jay](https://github.com/snowplow/snowplow/releases/tag/r68-turquoise-jay) | 0.15.0 | 0.6.0 | 0.5.0 | 3.6.0 | 1.0.0 | 1.0.0 | 0.14.0 | 0.4.0 | 0.16.0 | 0.3.3 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R67 Bohemian Waxwing](https://github.com/snowplow/snowplow/releases/tag/r67-bohemian-waxwing) | 0.15.0 | 0.6.0 | 0.5.0 | 3.6.0 | 1.0.0 | 1.0.0 | 0.14.0 | 0.4.0 | 0.15.0 | 0.3.3 | 0.5.0 | 0.6.0 | 0.15.0 | 0.4.0
[R66 Oriental Skylark](https://github.com/snowplow/snowplow/releases/tag/r66-oriental-skylark) | 0.14.0 | 0.6.0 | 0.5.0 | 3.6.0 | 1.0.0 | 1.0.0 | 0.14.0 | 0.4.0 | 0.15.0 | 0.3.3 | 0.4.0 | 0.5.0 | 0.13.1 | 0.3.0
[R65 Scarlet Rosefinch](https://github.com/snowplow/snowplow/releases/tag/r65-scarlet-rosefinch) | 0.13.1 | 0.6.0 | 0.5.0 | 2.4.2 | 1.0.0 | 0.14.1 | 0.13.1 | 0.4.0 | 0.14.0 | 0.3.3 | 0.4.0 | 0.5.0 | 0.13.1 | 0.3.0
[R64 Palila](https://github.com/snowplow/snowplow/releases/tag/r64-palila) | 0.13.1 | 0.6.0 | 0.5.0 | 2.4.2 | 1.0.0 | 0.14.1 | 0.13.1 | 0.4.0 | 0.14.0 | 0.3.3 | 0.3.0 | 0.4.0 | 0.13.0 | 0.2.0
[R63 Red-Cheeked Cordon Bleu](https://github.com/snowplow/snowplow/releases/tag/r63-red-cheeked-cordon-bleu) | 0.13.0 | 0.5.0 | 0.4.0 | 2.4.2 | 1.0.0 | 0.14.0 | 0.13.0 | 0.4.0 | 0.13.0 | 0.3.3 | 0.3.0 | 0.4.0 | 0.13.0 | 0.2.0
[R62 Tropical Parula](https://github.com/snowplow/snowplow/releases/tag/r62-tropical-parula) | 0.12.0 | 0.4.0 | 0.3.0 | 2.4.2 | 1.0.0 | 0.13.0 | 0.12.0 | 0.3.0 | 0.13.0 | 0.3.3 | 0.3.0 | 0.3.0 | 0.11.0 | 0.1.0
[R61 Pygmy Parrot](https://github.com/snowplow/snowplow/releases/tag/r61-pygmy-parrot) | 0.12.0 | 0.4.0 | 0.3.0 | 2.4.2 | 1.0.0 | 0.13.0 | 0.12.0 | 0.3.0 | 0.12.0 | 0.3.3 | 0.3.0 | 0.3.0 | 0.11.0 | 0.1.0
[R60 Bee Hummingbird](https://github.com/snowplow/snowplow/releases/tag/r60-bee-hummingbird) | 0.11.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.9.1 | 0.12.0 | 0.11.0 | 0.3.0 | 0.11.0 | 0.3.3 | 0.3.0 | 0.3.0 | 0.11.0 | 0.1.0
[v0.9.14](https://github.com/snowplow/snowplow/releases/tag/0.9.14) | 0.10.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.9.1 | 0.11.0 | 0.10.0 | 0.3.0 | 0.10.0 | 0.3.3 | 0.2.0 | 0.2.1 | 0.9.1 | -
[v0.9.13](https://github.com/snowplow/snowplow/releases/tag/0.9.13) | 0.9.1 | 0.4.0 | 0.3.0 | 2.4.2 | 0.9.0 | 0.10.0 | 0.9.0 | 0.2.1 | 0.9.2 | 0.3.3 | 0.2.0 | 0.2.1 | 0.9.1 | -
[v0.9.12](https://github.com/snowplow/snowplow/releases/tag/0.9.12) | 0.9.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.9.0 | 0.10.0 | 0.9.0 | 0.2.1 | 0.9.2 | 0.3.3 | 0.2.0 | 0.2.0 | 0.9.0 | -
[v0.9.11](https://github.com/snowplow/snowplow/releases/tag/0.9.11) | 0.8.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.9.0 | 0.9.0 | 0.8.0 | 0.2.1 | 0.9.2 | 0.3.3 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.10](https://github.com/snowplow/snowplow/releases/tag/0.9.10) | 0.7.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.8.0 | 0.8.0 | 0.7.0 | 0.2.1 | 0.9.2 | 0.3.3 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.9](https://github.com/snowplow/snowplow/releases/tag/0.9.9) | 0.7.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.8.0 | 0.8.0 | 0.7.0 | 0.2.1 | 0.9.2 | 0.3.3 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.8](https://github.com/snowplow/snowplow/releases/tag/0.9.8) | 0.6.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.7.0 | 0.7.0 | 0.6.0 | 0.2.1 | 0.9.1 | 0.3.2 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.7](https://github.com/snowplow/snowplow/releases/tag/0.9.7) | 0.5.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.6.0 | 0.7.0 | 0.5.0 | 0.2.1 | 0.9.1 | 0.3.2 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.6](https://github.com/snowplow/snowplow/releases/tag/0.9.6) | 0.5.0 | 0.4.0 | 0.3.0 | 2.4.2 | 0.6.0 | 0.6.0 | 0.5.0 | 0.2.0 | 0.9.0 | 0.3.1 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.5](https://github.com/snowplow/snowplow/releases/tag/0.9.5) | 0.4.0 | 0.3.0 | 0.2.0 | 2.4.2 | 0.6.0 | 0.5.0 | 0.4.0 | 0.1.0 | 0.8.0 | 0.3.0 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.4](https://github.com/snowplow/snowplow/releases/tag/0.9.4) | 0.4.0 | 0.3.0 | 0.2.0 | 2.4.2 | 0.6.0 | 0.5.0 | 0.4.0 | 0.1.0 | 0.7.0 | 0.2.0 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.3](https://github.com/snowplow/snowplow/releases/tag/0.9.3) | 0.4.0 | 0.3.0 | 0.2.0 | 2.4.2 | 0.6.0 | 0.5.0 | 0.4.0 | 0.1.0 | 0.7.0 | 0.2.0 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.2](https://github.com/snowplow/snowplow/releases/tag/0.9.2) | 0.4.0 | 0.3.0 | 0.2.0 | - | 0.5.0 | 0.5.0 | 0.4.0 | 0.1.0 | 0.6.0 | 0.2.0 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.1](https://github.com/snowplow/snowplow/releases/tag/0.9.1) | 0.3.0 | 0.3.0 | 0.2.0 | - | 0.5.0 | 0.4.0 | 0.3.0 | 0.1.0 | 0.6.0 | 0.2.0 | 0.1.0 | 0.1.0 | 0.2.0 | -
[v0.9.0](https://github.com/snowplow/snowplow/releases/tag/0.9.0) | 0.2.0 |  |  | - |  |  |  |  |  |  | 0.1.0 | 0.1.0 | 0.2.0 | -


**Legend**	

- AMI   - Recommended EMR AMI
- CC    - Clojure Collector
- EER   - EmrEtlRunner
- KES   - Kinesis Elasticsearch Sink
- KLSS  - Kinesis LZO S3 Sink
- PAE   - Postgres atomic.events
- RAE   - Redshift atomic.events
- SCE   - Scala Common Enrich
- SHE   - Scala Hadoop Enrich
- SHS   - Scala Hadoop Shred
- SKE   - Scala Kinesis Enrich
- SL    - StorageLoader
- SSC   - Scala Stream Collector
--->
