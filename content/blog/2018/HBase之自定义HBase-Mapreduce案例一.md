---
title: "HBase之自定义HBase-Mapreduce案例一"
date: 2018-11-04 11:09:30
draft: false
---
**1.需求场景**

将HBase中的ys表中的一部分数据通过Mapreduce迁移到ys_mr表中

**2.代码编写**

1)构建ReadysMapreduce类，用于读取ys表中的数据
package cn.ysjh; import java.io.IOException; import org.apache.hadoop.hbase.Cell; import org.apache.hadoop.hbase.CellUtil; import org.apache.hadoop.hbase.client.Put; import org.apache.hadoop.hbase.client.Result; import org.apache.hadoop.hbase.io.ImmutableBytesWritable; import org.apache.hadoop.hbase.mapreduce.TableMapper; import org.apache.hadoop.hbase.util.Bytes; import org.apache.hadoop.mapreduce.Mapper; public class ReadysMapreduce extends TableMapper<ImmutableBytesWritable,Put>{ @Override protected void map(ImmutableBytesWritable key, Result value, Mapper<ImmutableBytesWritable, Result, ImmutableBytesWritable, Put>.Context context) throws IOException, InterruptedException { //将 fruit 的 name 和 color 提取出来，相当于将每一行数据读取出来放入到 Put 对象中。 Put put = new Put(key.get()); //遍历添加 column 行 for(Cell cell: value.rawCells()){ //添加/克隆列族:info if("info".equals(Bytes.toString(CellUtil.cloneFamily(cell)))){ //添加/克隆列：name if("name".equals(Bytes.toString(CellUtil.cloneQualifier(cell)))){ //将该列 cell 加入到 put 对象中 put.add(cell); //添加/克隆列:color }else if("color".equals(Bytes.toString(CellUtil.cloneQualifier(cell)))){ //向该列 cell 加入到 put 对象中 put.add(cell); } } } //将从 fruit 读取到的每行数据写入到 context 中作为 map 的输出 context.write(key, put); } }

2)构建WriteysReduce类，用于将读取到的fruit表中的数据写入到fruit_mr表中

package cn.ysjh; import java.io.IOException; import org.apache.hadoop.hbase.client.Mutation; import org.apache.hadoop.hbase.client.Put; import org.apache.hadoop.hbase.io.ImmutableBytesWritable; import org.apache.hadoop.hbase.mapreduce.TableReducer; import org.apache.hadoop.io.NullWritable; import org.apache.hadoop.mapreduce.Reducer; public class WriteysReduce extends TableReducer<ImmutableBytesWritable, Put, NullWritable>{ @Override protected void reduce(ImmutableBytesWritable key, Iterable<Put> values, Context context) throws IOException, InterruptedException { //读出来的每一行数据写入到 fruit_mr 表中 for(Put put: values){ context.write(NullWritable.get(), put); } } }

3)构建JobysMapreduce类，用于创建Job任务

package cn.ysjh; import java.io.IOException; import org.apache.hadoop.conf.Configuration; import org.apache.hadoop.conf.Configured; import org.apache.hadoop.hbase.HBaseConfiguration; import org.apache.hadoop.hbase.client.Put; import org.apache.hadoop.hbase.client.Scan; import org.apache.hadoop.hbase.io.ImmutableBytesWritable; import org.apache.hadoop.hbase.mapreduce.TableMapReduceUtil; import org.apache.hadoop.mapreduce.Job; import org.apache.hadoop.util.Tool; import org.apache.hadoop.util.ToolRunner; public class JobysMapreduce extends Configured implements Tool{ public int run(String[] args) throws Exception { //得到 Configuration Configuration conf = this.getConf(); //创建 Job 任务 Job job = Job.getInstance(conf, this.getClass().getSimpleName()); job.setJarByClass(JobysMapreduce.class); //配置 Job Scan scan = new Scan(); scan.setCacheBlocks(false); scan.setCaching(500); //设置 Mapper，注意导入的是 mapreduce 包下的，不是 mapred 包下的，后者是老版本 TableMapReduceUtil.initTableMapperJob( "ys", //数据源的表名 scan, //scan 扫描控制器 ReadysMapreduce.class,//设置 Mapper 类 ImmutableBytesWritable.class,//设置 Mapper 输出 key 类型 Put.class,//设置 Mapper 输出 value 值类型 job//设置给哪个 JOB ); //设置 Reducer TableMapReduceUtil.initTableReducerJob("ys_mr", WriteysReduce.class, job); //设置 Reduce 数量，最少 1 个 job.setNumReduceTasks(1); boolean isSuccess = job.waitForCompletion(true); if(!isSuccess){ throw new IOException("Job running with error"); } return isSuccess ? 0 : 1; } public static void main( String[] args ) throws Exception{ Configuration conf = HBaseConfiguration.create(); int status = ToolRunner.run(conf, new JobysMapreduce(), args); System.exit(status); } }

**3.打包运行**

使用maven 打包命令：-P local clean package，然后将jar包上传到集群上运行测试

注意： 如果待导入数据的表不存在，则需要提前创建