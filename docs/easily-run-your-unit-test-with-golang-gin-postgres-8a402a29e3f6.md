# 使用 GoLang + Gin + Postgres 轻松运行单元测试

> 原文：<https://medium.com/geekculture/easily-run-your-unit-test-with-golang-gin-postgres-8a402a29e3f6?source=collection_archive---------1----------------------->

## GoLang pointers 让我们很容易模仿对象！

![](img/770ddf6723786034b62bd46427385b30.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编写单元测试总是令人困惑，尤其是当我们在应用程序中集成了多个库和框架时。请跟随我们编写我们的初始项目和测试模板，这样您就可以轻松地开始您的项目了！

# 该结构

任何项目中最重要的一个方面是我们的文件夹结构。GoLang 可以将一个目录下的多个文件编译成一个包，当我们导入一个包时，它会导入整个目录。因为我们要构建一个 REST API，所以我们可以遵循一个基本的结构模型——控制器，并像这样构造我们的文件夹:

```
|__main.go            # initialize our app
|__internals
|  |__api
|     |__controllers  # all our controllers
|        |__sample-controller.go
|     |__router       # our routers
|        |__router.go
|     |__api.go       # initialize gin
|  |__pkg
|     |__database.go  # configuration to our DB
|     |__models.go    # DB models
|__test               # all our test
```

对于这个项目，我们将使用`go-pg/pg/v10` ORM 库，我们可以在这里找到。

# 设置

对于这个单元测试，我们所做的就是看看我们是否可以创建一个在测试环境中运行的模拟`gin`服务器，并根据它的实际意图将其路由器映射到控制器。因为我们的控制器可以访问内置的数据库，所以我们必须将指针改为指向一个模拟数据库，这样它就可以创建/删除值。

首先，让我们确保我们的数据库连接被导出为一个指针，以便它可以被其他包引用:

**数据库. go**

```
package dbimport (
  "context"
  "github.com/go-pg/pg/v10"
) var DB *pg.DB//SetupDB opens a database and saves the reference to `Database` pointer. This allows us to have access to the initialized DB from other packagesfunc SetupDB() {
  DB = pg.Connect(&pg.Options{
    Addr:     "some_host" + ":" + "some_port",
    User:     "username",
    Password: "password",
    Database: "database",
  })

  // Check if database is connected
  ctx := context.Background() if err := DB.Ping(ctx); err != nil {
    panic(err)
  }
}
```

初始化应用程序时，我们可以运行`setupDB()`。

为了引用数据库，当我们将包`import`到我们的控制器中时，我们可以简单地获得指针，如下所示:

**样本控制器. go**

```
package controllers import (
  "net/http"
  "github.com/gin-gonic/gin" // rename packe for easier read  
  pg "sample/internal/pkg/db"
  models "sample/internal/pkg/models"
)// Get all Sample object from DB
func GetSamples(c *gin.Context) {
  var samples = []models.Sample{} // We access the DB pointer from the package 
  err := pg.DB.Model(&samples).Select()
  if err != nil {
    c.JSON(http.StatusBadRequest, {})
    return
  } c.JSON(http.StatusOK, actions)
}
```

就是这样！接下来让我们进入测试文件夹来构建我们的测试用例。

这里我们至少需要一个文件，`init_test.go`，它将负责设置我们的测试环境。

**init_test.go**

```
package testimport (
  "context"
  "fmt"
  "log"
  "os"
  "testing" "sample/internal/api/controllers"
  "sample/internal/pkg/models" // Grabs the same DB pointer
  pointer_db "sample/internal/pkg/db" "github.com/gin-gonic/gin"
  "github.com/go-pg/pg/v10"
  "github.com/go-pg/pg/v10/orm" unitTest "github.com/Valiben/gin_unit_test"
)func init() {
  // initialize the router
  router := gin.Default() // Handlers for testing
  router.GET("/sample", controllers.GetSamples) // Setup the router
  unitTest.SetRouter(router)
  newLog := log.New(os.Stdout, "",log.Llongfile|log.Ldate|log.Ltime)
  unitTest.SetLog(newLog)
}...
```

但是我们还没有完成，上面的代码仅仅创建了`gin`测试环境，它还没有为测试准备好我们的数据库。

我们需要做的是`BeforeAll`创建一个数据库和`AfterAll`删除数据库进行清理。使用`TestMain()`可以做到这一点

为此，我们可以将它添加到我们的`init_test.go`

**init_test.go(续)**

```
....// TestMain will automatically run before all test and hook back in // after all test is done
func TestMain(m *testing.M) { // create a local connection
  pgdb := pg.Connect(&pg.Options{
    Addr:     os.Getenv("DATABASE_URL") + ":5432",
    User:     "username",
    Password: "password",
  }) defer pgdb.Close() // trigger function to create schema
  createSchema(pgdb) // THIS IS KEY, we replace the DB pointer to the mock one we    
  // recently build
  pointer_db.DB = pgdb log.Println("Everything above here run before ALL test") // Run test suites
  exitVal := m.Run() log.Println("Everything below run after ALL test") // we can do clean up code here os.Exit(exitVal)
}// Schema in the mock DB still needs to be created
func createSchema(db *pg.DB) error {
  models := []interface{}{
    (*models.Action)(nil),
  } for _, model := range models {
    err := db.Model(model).CreateTable(&orm.CreateTableOptions{
      // set Temp=True so no tables/data are actually created
      Temp:        true,
    })
  } return nil
}
```

注意，在`TestMain`中，我们用函数中最近创建的值覆盖了`pointer_db.DB`值。因为它是一个指针，所以在测试期间，甚至控制器内部的`pointer_db.DB`都指向模拟数据库。这意味着我们需要做的最后一件事是编写一个测试！

注意:我不得不将我们的数据库库重命名为`pointer_db`，因为默认的`pg`在这个实例中被 postgres 库使用，而不是在`sample-controller.py`中

给`sample-controller.go`写一个吧

**sample _ controller _ test . go**

```
package testimport (
  "testing"
  unitTest "github.com/Valiben/gin_unit_test"
  "github.com/Valiben/gin_unit_test/utils" pg "sample/internal/pkg/db"
  "sample/internal/pkg/models"
)// Our test will see if we receive an array of samples
func TestGetSample(t *testing.T) {
  var resp []models.Sample // Our db starts empty, so we will need to populate it
  populateDatabase(test_models) err := unitTest.TestHandlerUnMarshalResp(
            utils.GET, "/sample","json", nil, &resp) if err != nil {
    t.Errorf("TestGetSample: %v\n", err)
    return
  } // Validate there is at least one
  if len(resp) != 1 {
    t.Errorf("Expected %v but got %v", 1, len(resp))
    return
  } // Emptying the DB for the next test
  clearDatabase(test_models) t.Log("Success")
} // Takes in array of interface to create object in DB
func populateDatabase(test_models []interface{}) error {
  for _, model := range test_models {
    _, err := pg.DB.Model(model).Insert()
    if err != nil {
      fmt.Print(err.Error())
      return err
    }
  }
  return nil
}func clearDatabase(test_models []interface{}) error {
  for _, model := range test_models {
    _, err := pg.DB.Model(model).Delete()
    if err != nil {
      fmt.Print(err.Error())
      return err
    }
  }
  return nil
}
```

**就是它**！接下来你可以写更多的例子来看看当它通过 API 被触发时你的控制器应该如何表现。

这种方法的关键是:

*   postgres 数据库需要运行，因为它不在真空中工作
*   在每个测试中，我们需要填充并创建我们的表

**如果您对完整的项目感兴趣，请查看我的**[**Github**](https://github.com/sutjin/go-rest-template)**中的示例。**

如果您正在通过 GitLab 运行这个项目，并且想要为它构建一个测试工作，那么您很幸运！我写过一篇关于如何将 postgres 作为 Gitlab 构建的一部分的文章

[在 GitLab Pipeline 中使用 Docker 进行单元测试的数据库|作者 Nabil Sutjipto | Geek Culture | 2021 年 5 月| Medium](/geekculture/using-a-database-for-unit-testing-with-docker-in-gitlab-pipeline-c919121e25ef)

祝你下一个项目好运！