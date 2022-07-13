## 延时队列

!> forked from hibiken/asynq

基于 [asynq](https://github.com/hibiken/asynq) 进行改进, 支持多项目部署, 支持统一的 Redis 前缀

### 使用

```bash
go mod edit -replace github.com/hibiken/asynq=github.com/nfangxu/asynq

go get -d github.com/hibiken/asynq
```

更详细的使用方式, 请访问 [原始文档](https://github.com/hibiken/asynq)
