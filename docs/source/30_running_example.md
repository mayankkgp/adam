## flagstat

Once you have data converted to ADAM, you can gather statistics from the ADAM file using `flagstat`.
This command will output stats identically to the samtools `flagstat` command, e.g.

```bash
$ adam-submit flagstat NA12878_chr20.adam
```
Outputs:
```
51554029 + 0 in total (QC-passed reads + QC-failed reads)
0 + 0 duplicates
50849935 + 0 mapped (98.63%:0.00%)
51554029 + 0 paired in sequencing
25778679 + 0 read1
25775350 + 0 read2
49874394 + 0 properly paired (96.74%:0.00%)
50145841 + 0 with itself and mate mapped
704094 + 0 singletons (1.37%:0.00%)
158721 + 0 with mate mapped to a different chr
105812 + 0 with mate mapped to a different chr (mapQ>=5)
```

In practice, you'll find that the ADAM `flagstat` command takes orders of magnitude less
time than samtools to compute these statistics. For example, on a MacBook Pro the command 
above took 17 seconds to run while `samtools flagstat NA12878_chr20.bam` took 55 secs.
On larger files, the difference in speed is even more dramatic. ADAM is faster because
it's multi-threaded and distributed and uses a columnar storage format (with a projected
schema that only materializes the read flags instead of the whole read). 

## Running on a cluster

We provide the `adam-submit` and `adam-shell` commands under the `bin` directory. These can
be used to submit ADAM jobs to a spark cluster, or to run ADAM interactively.

## Running Plugins

ADAM allows users to create plugins via the [ADAMPlugin](https://github.com/bigdatagenomics/adam/blob/master/adam-core/src/main/scala/org/bdgenomics/adam/plugins/ADAMPlugin.scala)
trait. These plugins are then imported using the Java classpath at runtime. To add to the classpath when
using appassembler, use the `$CLASSPATH_PREFIX` environment variable. For an example of how to use
the plugin interface, please see the [adam-plugins repo](https://github.com/heuermh/adam-plugins).
