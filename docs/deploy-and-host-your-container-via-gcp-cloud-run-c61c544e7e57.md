# 通过 GCP 云运行部署和托管您的容器

> 原文：<https://medium.com/geekculture/deploy-and-host-your-container-via-gcp-cloud-run-c61c544e7e57?source=collection_archive---------24----------------------->

![](img/3e255f3161dbddfd173f07f962d500ff.png)

Photo by [william william](https://unsplash.com/@william07?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> TL；博士；医生

```
docker push <gcr-hostname>/<project-name>/<image-name>:<tag>gcloud run deploy <service-name> --image gcr.io/<project-name>/<image-name>:<tag>
```

您正在本地机器上构建一个漂亮的 web 应用程序或 API。一切进展顺利，您已经准备好将应用程序部署到生产环境中了。你的想法比…超前一步