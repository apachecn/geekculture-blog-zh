# 节点中的简单队列系统🚀

> 原文：<https://medium.com/geekculture/simple-queues-system-in-nodejs-e3a2e23b5ebf?source=collection_archive---------14----------------------->

![](img/f029183dbe09bd7fa1f0af19f217a03e.png)

现代应用程序越来越需要昂贵的处理、异步操作、完成前必要的任务延迟、日志记录以及从启动的应用程序/用户事件触发过程。

# 为什么要排队系统？

因此，队列系统将有效地满足由于动作/事件的增加而增加的异步操作。从而减少错误，改善用户体验，在后台处理操作，并在准备就绪时执行功能或触发应用服务。

作业队列可以处理的一些任务:

*   发送系统创作的推送通知
*   发送系统创作的电子邮件
*   与第三方应用程序和 webhooks 的通信
*   事件驱动编程
*   记录事件，等等

队列上下文中的必要术语；

**Job** —这是一个可以推入队列系统的任务，将在应用程序流的稍后阶段进行处理。作业是包含消费者处理所需数据的对象。

**消费者** —这是队列流程中要执行的任务。例如发送电子邮件、推送通知等

**生产者** —生产者将代表实例化的队列构造器。许多作业可以添加到指定的命名队列对象中；以便进行处理。例如推送通知、通知、电子邮件。options 参数将包含延迟(从创建时开始执行作业的时间)、尝试(作业失败时的重试次数)、作业速率限制等选项

# 让我们从用 [Bull](https://www.npmjs.com/package/bull#documentation) 实现一个简单的队列系统开始

![](img/a513f517fcea3438da4939593bb38e6a.png)

Bull 将其队列系统实现作为

> **最快、最可靠、基于 Redis 的节点队列。为坚如磐石的稳定性和原子性而精心编写。**

Bull 与 typescript 完全兼容，经过良好的测试，队列系统 UI，并建立在 Redis 缓存之上，具有许多有用的抽象方法和功能。

当然，社区可以使用更抽象的系统来简化解决方案，当使用具有激增复杂性的过多工具时😅。

# 履行

*   在本地安装 [Redis](https://redis.io/topics/quickstart) 或如下设置 Redis docker 容器；让我们添加一个`Dockerfile`和一个`docker-compose.yml`:

```
# docker-compose.yaml# pull a light-weight redis image from docker
# open a reverse port 
services:
 simple_queue_redis:
    image: redis:6.2-alpine
    container_name: simple_queue_redis
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
    ports:
      - 6379:6379
    volumes:
      - ./redis:/db# docker-compose.yamlFROM node:14-alpine# Create app directory
WORKDIR /app# Install dependencies
RUN yarn install
RUN yarn# Copy source files from the host computer to the container
COPY . .# Run the app
CMD ["yarn", "start:dev"]
```

*   使用以下命令安装 NPM 的 Bull 包。

`npm install bull --save`

*   创建队列和流程。我们假设我们在一个类型安全的环境中工作，因此使用了 typescript。创建一个名为`notificationQueue.ts`的新文件，并粘贴以下代码。这里我们假设我们正在处理一个通知任务。

在这种情况下，`NotificationQueue`实例代表生产者，通知作业被添加到消费者,`notificationProcess`。

```
// ./queues/notificationQueue.tsimport Queue from 'bull';import { notificationProcess } from '../processes';// producer
const NotificationQueue = new Queue('Notification', process.env.REDIS_URL);// consumer
NotificationQueue.process(notificationProcess);export const addNewNotification = (data: {
title: string,
body: string,
url: string,
}
) => {
  NotificationQueue.add(data);
};export default NotificationQueue;
```

以下代码块定义了服务器推送通知作业的实现。

我们正在使用[推送光束](https://pusher.com/docs/beams/getting-started/web/sdk-integration/)从服务器发送推送消息。它易于使用，有很好的文档，并有助于抽象插入服务器以允许服务器和客户机之间的消息传递的复杂性😀。

使用以下命令安装 NPM 的 pusher Nodejs SDK。

`npm i @pusher/push-notifications-server`

在本教程中，我们不会关注 pusher 是如何工作的，官方的[文档](https://github.com/pusher/push-notifications-node#readme)是一个很好的资源。

```
// ./processes/notification.process.tsimport { Job } from 'bull';
import PushNotifications from '[@pusher/push-notifications-server](http://twitter.com/pusher/push-notifications-server)';const beamsClient = new PushNotifications({
  instanceId: process.env.PUSHER_BEAMS_INSTANCE,
  secretKey: process.env.PUSHER_BEAMS_SECRET,
});const NotificationProcess = async (job: Job) => {
  const {
    title,
    body,
    url,} = job.data as {
title: string,
body: string,
url: string,
};await beamsClient.publishToInterests([userId], {
    web: {
      notification: {
        title,
        body,
        deep_link: `${process.env.BASE_URL}${url}`,
      },
    },});
 return true;
}export default NotificationProcess;
```

保存文件并使用节点`./index.ts`运行它。将向订阅的客户端发送通知。(本教程不涉及)。

```
// ./index.ts
import { NotificationQueueHelpers, } from '.../queues';...NotificationQueueHelpers.addNewNotification({
    title: "simple queue system",
    body: "push notification sent...",
    url: `/notification/${user.id}`,
  });
```

# 总结

这是一个简单的队列系统示例，使用 Bull 创建队列和调度作业。如上所述，bull 提供了更多的选项来管理 Redis 缓存系统，例如事件监听器、速率限制器、作业类型、延迟作业、重试次数、使用 Cron 表达式等。

[**官方文档**](https://optimalbits.github.io/bull/) 仍然是一个非常值得探索的资源，一定要去看看。

感谢观众，希望这篇文章对你有所帮助🤗。请随时在 [Github](https://github.com/nextwebb) 、 [Twitter](https://twitter.com/i_am_nextwebb) 和 [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) 上联系我。一定要点赞、评论和分享😌。

快乐编码💙！

*原载于*[*https://blog . next Webb . tech*](http://blog.nextwebb.tech/simple-queues-system-in-nodejs)*。*