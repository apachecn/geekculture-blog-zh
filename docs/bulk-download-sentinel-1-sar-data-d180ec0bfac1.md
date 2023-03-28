# 批量下载 Sentinel 1 SAR 数据

> 原文：<https://medium.com/geekculture/bulk-download-sentinel-1-sar-data-d180ec0bfac1?source=collection_archive---------3----------------------->

## 使用 Python 自动从 ASF 存档下载

哨兵 1 号的合成孔径雷达数据可以从阿拉斯加卫星设施和哥白尼开放访问中心的数据门户网站上下载。两者都支持强大的 API，允许用户搜索、获取元数据和下载多个图像。与 Open Access Hub 相比，ASF 的搜索 API 提供了更快的下载速度，允许同时下载，最重要的是，它允许您访问档案中的所有图像。这在搜索前一段时间(例如 2015 年)获取的图像时非常有用。

本教程旨在通过演示如何为感兴趣的特定区域下载 2017 年的多个 Sentinel 1 Single Look Complex (SLC)图像，对 ASF 搜索 API 进行简要介绍。

**第一步:安装 *asf_search* 库**

*asf_search* 库可以通过 [PyPI](https://pypi.org/project/asf-search/) 和 [Conda](https://anaconda.org/conda-forge/asf_search) 获得。

```
pip install asf_search
conda install -c conda-forge asf_search
```

**步骤 2:获取感兴趣区域的 WKT 坐标**

如果您计划搜索与预定义的感兴趣区域重叠的 Sentinel 1 图像，您必须以众所周知的文本(WKT)格式获取其地理坐标。Shapely 和 Geopandas 是我们用来简化 shapefile 和定义搜索查询的两个库。

```
import asf_search as asf
import geopandas as gpd
from shapely.geometry import box
from datetime import date 
```

在本教程中，我有一个描述我感兴趣的领域的 shapefile。为了简化 shapefile，我使用 geopandas 的 *total_bounds* 方法来获取边界框坐标。然后我用 shapely 的 *box()* 方法用这些坐标创建一个多边形对象。最后，我使用 geopandas 的 *to_wkt()* 方法将这些坐标保存为 wkt 格式。

```
### 1\. Read Shapefile using Geopandas
gdf **=** gpd**.**read_file(path_to_shapefile)### 2\. Extract the Bounding Box Coordinates
bounds **=** gdf**.**total_bounds### 3\. Create GeoDataFrame of the Bounding Box 
gdf_bounds **=** gpd**.**GeoSeries([box(*****bounds)])### 4\. Get WKT Coordinates
wkt_aoi = gdf_bounds**.**to_wkt()**.**values**.**tolist()[0]
```

**第三步:定义搜索查询**

为了构建我们的查询，我们将使用带有以下选项的 *search()* 方法:

*   *平台*:采集数据的卫星平台名称
*   *处理级别*:数据已经处理到的级别。
*   *开始* & *结束*:根据数据采集的起止日期过滤搜索结果的选项。支持时间戳以及自然语言，如“3 周前”
*   *intersectsWith* :获取已定义多边形、线串或点几何图形的 WKT 坐标。

搜索功能可用输入选项的完整列表及其描述可以通过 asf_search 模块的 [github 库](https://github.com/asfadmin/Discovery-asf_search)访问。

```
results **=** asf**.**search(
    platform**=** asf**.**PLATFORM**.**SENTINEL1A,
    processingLevel**=**[asf**.**PRODUCT_TYPE**.**SLC],
    start **=** date(2017, 1, 1),
    end **=** date(2017, 12, 31),
    intersectsWith **=** wkt_aoi
    )print(f'Total Images Found: {len(results)}')### Save Metadata to a Dictionary
metadata **=** results**.**geojson()
```

搜索查询的输出保存在名为*结果、*的变量中，该变量是一个类对象，包含符合搜索标准的每个图像的元数据。可以使用 *geojson()* 方法将所有输出图像的元数据保存到一个字典对象中。

**步骤 4:使用您的 EarthData 凭据进行身份验证**

任何拥有 EarthData 帐户的人都可以访问 ASF 档案中的数据。为了初始化下载任务，您必须使用 *auth_with_creds()* 方法提供用于认证会话的 EarthData 登录凭证。文档中提供了其他身份验证方法的描述。

```
session = asf**.**ASFSession()**.**auth_with_creds(USERNAME, PASSWORD)
```

**注意**:如果你还没有账户，可以免费[注册](https://urs.earthdata.nasa.gov/users/new)。当从 NASA 的几个不同的地球观测数据门户网站(包括 USGS EarthExplorer)下载数据时，EarthData 帐户非常方便。

**第五步:开始下载**

结果对象(在步骤 3 中创建)有一个 *download()* 方法，它接受三个选项:

*   *路径*:保存下载文件的位置
*   *会话*:下载时使用的认证会话
*   *进程*:同时下载的数量。默认为 1。

```
results**.**download(
     path **=** path_to_download_directory,
     session **=** session, 
     processes **=** 2 
  )
```

在本文中，我演示了如何使用基于 python 的脚本下载特定感兴趣区域的多个 Sentinel 1 SLC 图像。我们的目标是使这项任务自动化，这样就可以很容易地在其他领域复制这项任务。

**有用的资源**

1.  [ASF admin/Discovery-ASF _ search(github.com)](https://github.com/asfadmin/Discovery-asf_search)
2.  [基础知识——ASF SAR 数据搜索手册(alaska.edu)](https://docs.asf.alaska.edu/asf_search/basics/)
3.  [Earthdata 登录用户注册(nasa.gov)](https://urs.earthdata.nasa.gov/users/new)