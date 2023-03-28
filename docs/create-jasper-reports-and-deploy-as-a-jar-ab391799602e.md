# 创建 Jasper 报告并作为 JAR 部署

> 原文：<https://medium.com/geekculture/create-jasper-reports-and-deploy-as-a-jar-ab391799602e?source=collection_archive---------5----------------------->

![](img/314a7ffa5243c55f12bed5b321ac95d5.png)

Jasper reports 是一个开源的报告框架，帮助构建从简单到复杂的报告，提供各种格式，如 PDF、Excel、CVS 和 TXT 文件。这是一个 Java 库，可以在任何类型的 Java 应用程序中使用，您可以使用 Jasper Studio 或支持 Jasper reports 构建的工作室来构建它们。通过使用这些工具，构建报告变得很容易，因为它提供了拖放支持。在本文中，我们将探讨如何在报表和子报表中传递参数、字段，以及如何在使用 jasper 报表时创建可部署的 JAR 文件。

奇异值的行为类似于键值对，可以在报表和子报表中作为参数传递。另一方面，字段的行为类似于也可以传递给子报表的元素列表。

现在让我们进入代码，为了确保 Jasper 文件在 JAR 上工作，它必须被作为一个资源作为流，它被设置为输入流。流形式的资源获取文件位置的类路径，并在创建 JAR 后将信息保存在文件所在的位置。

```
ClassLoader classLoader = Constants.class.getClassLoader();InputStream in = classLoader.getResourceAsStream
               ("Folder" + File.*separator* + filename + ".jrxml");

JasperReport jasperReport = JasperCompileManager.*compileReport*(in);
```

正如你在代码*文件中看到的，使用了分隔符*。如果部署在 Windows 或 Linux 上，它有助于根据环境设置反斜杠和斜杠，确保文件可以在任一环境中找到。

为了传递参数，使用映射对象，如下所述:

```
Map<String, Object> parameters = new HashMap<>();
JasperReport jasperSubReport = *loadReport*("fileName.jasper");
parameters.put("key", "value");
parameters.put("key", "value");
parameters.put("SUBREPORT_INPUT_STREAM", jasperSubReport);
```

在 jasper 文件中，需要进行以下更改:

```
<parameter name="SUBREPORT_INPUT_STREAM" class="java.lang.Object" isForPrompting="false"/><subreport>
  <reportElement x="25" y="0" width="170" height="30" uuid="a0373767-848a-44ea-b6eb-80128fa60453"/>
   <dataSourceExpression>
<![CDATA
[new net.sf.jasperreports.engine.data.JRBeanCollectionDataSource
($F{outputs})]]>
</dataSourceExpression>
   <subreportExpression class="java.io.InputStream"><![CDATA[$P{SUBREPORT_INPUT_STREAM}]]></subreportExpression>
</subreport>
```

要传递 Jasper 列表对象中的字段，按如下方式传递:

```
JRBeanCollectionDataSource dataSource = 
new JRBeanCollectionDataSource(list);
```

现在设置 jasper 文件中的所有参数，如下所示:

```
JasperPrint jasperPrint = JasperFillManager.*fillReport* (jasperReport, parameters, dataSource);
```

现在，为了导出文件，使用了以下代码:

```
JRXlsExporter jrXlsExporter = new JRXlsExporter();
jrXlsExporter.setExporterInput(new SimpleExporterInput(jasperPrint));
jrXlsExporter.setExporterOutput(new SimpleOutputStreamExporterOutput(file+ {{format}})); // e.g# .xls 
jrXlsExporter.exportReport();
```

上面的指南总结了如何设置 JAR 上可部署的 Jasper 报告的导出。使用 Java 通过 Jasper 中的参数和字段传递数据，并通过主报表将数据传递给子报表。