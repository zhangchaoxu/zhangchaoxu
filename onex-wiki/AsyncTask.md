# AsyncTask
对于耗时的任务处理,比如excel导入,可以使用异步任务来处理。

注意点
1. 默认线程数8,超过数量后可以通过spring.task.execution.pool.core-size来设置,建议不要超过CPU核心数
2. 超过最大核心数后,发起的任务会进入等待

### 第一步: 配置最大核心数
```
spring:
  ## 异步任务
  task:
    execution:
      pool:
        # 核心线程数，默认为8
        core-size: 16
```

### 第二步: 添加@EnableAsync注解
```
@EnableAsync
public class ApiApplication
```

### 第三步: 定义AsyncTask
```
@Component
@Slf4j
@Async
public class AsyncTask {
     /**
     * 导入数据
     */
    public Future<Dict> importFormExcel(PushToNbgjwlRequest request) {
        // todo 处理耗时任务
        return new AsyncResult<>(dict);
        
    }
}
```

### 第四步: 在需要的地方调用
```
@Slf4j
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class DataTest {

    @Autowired
    private AsyncTask asyncTask;

    /**
     * 导入数据
     */
    @Test
    public void importFormExcel() throws Exception {
        // todo 导入参数
        Future<Dict> future = asyncTask.importFormExcel(request);
        log.info("处理结果-完成" + future.get());
    }

}
```
