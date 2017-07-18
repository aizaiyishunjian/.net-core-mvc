> The route by which API controllers are reached can be defined only using the Route attribute and cannot be defined in the application configuration in the Startup class.
>
> The content format selected by MVC depends on four factors: the formats that the client will accept, the formats that MVC can produce, the content policy specified by the action, and the type returned by the action method.

# 1. Rest API

* GET     /api/reseravtions          获取所有资源
* GET     /api/reseravtions/1       获取指定Id资源
* POST   /api/reservation             创建资源
* PUT      /api/reservation             更新资源
* DELETE  /api/reservation/1       删除指定资源

# 2. 复杂查询

```
// GET api/v1/[controller]/items[?pageSize=3&pageIndex=10]
        [HttpGet]
        [Route("[action]")]
        public async Task<IActionResult> Items([FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)
```

```
// GET api/v1/[controller]/items/withname/samplename[?pageSize=3&pageIndex=10]
        [HttpGet]
        [Route("[action]/withname/{name:minlength(1)}")]
        public async Task<IActionResult> Items(string name, [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)
```

```
// GET api/v1/[controller]/items/type/1/brand/null[?pageSize=3&pageIndex=10]
        [HttpGet]
        [Route("[action]/type/{catalogTypeId}/brand/{catalogBrandId}")]
        public async Task<IActionResult> Items(int? catalogTypeId, int? catalogBrandId, [FromQuery]int pageSize = 10, [FromQuery]int pageIndex = 0)
```



