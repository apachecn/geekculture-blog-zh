# 反冲:React 的高效状态管理库

> 原文：<https://medium.com/geekculture/recoil-efficient-state-management-library-for-react-b9e158b74460?source=collection_archive---------18----------------------->

`selector`关于测验的初始提示页面也执行这些。它有`get` QueryData `atom`和异步 API 请求服务器获取测验数据。

```
export default selector<TResponseData>({
  key: 'initilaOrderState',
  get: async ({ get }) => {
    const queryData = get(QueryDataState);

    if (queryData == undefined || window.location.pathname != `/${QUIZ_PAGENAME}`) return undefined;

    const { amount, difficulty } = queryData;

    const axios = customAxios();
    const response = await axios({
      method: 'GET',
      params: {
        amount,
        difficulty,
        type: 'multiple',
      },
    });

    const decodedResponseData = {
      ...response.data,
      ...
    };

   return decodedResponseData;
  },
});
```

执行异步请求后，`selector`返回响应数据，即全局状态值。有一个组件需要获取该值(例如，测验组件)。然后，怎么渲染呢？

# 用`React.suspense`用异步数据渲染组件

`Quiz component`在 QuizPage 中，获取异步全局状态(initialProps)以呈现来自服务器的测验数据。所以，它用`InitialPropsState`执行`useRecoilValue`。

```
import { useEffect, useRef, useState } from 'react';
import { useRecoilState, useRecoilValue, useSetRecoilState } from 'recoil';
import {
  InitialPropsState,
  CurrentQuizIndexState,
  SelectedAnswerState,
  QuizResultsState,
} from 'src/state';

const Quiz = () => {
  const initialProps = useRecoilValue(InitialPropsState);
  const currentQuizIndex = useRecoilValue(CurrentQuizIndexState);
  const currentQuiz = (initialProps as TResponseData).results[currentQuizIndex];

  ...

  return (
    <Content
      header={`QUIZ ${currentQuizIndex + 1} / ${(initialProps as TResponseData).results.length}`}
      headerRight={currentQuiz.difficulty}>
        <Atoms.Div ... >
          <Atoms.Div>
            <Atoms.Div fontSize="24px">{currentQuiz.question</Atoms.Div>
            <Atoms.Ul>
              {currentQuiz.examples.map((answer: string, i: number) => (<Atoms.Li key={answer} value={answer}>
      <Atoms.Checkbox
        id={answer}
        name={answer}
        disabled={selectedAnswer != undefined}
        onChange={handleChange}
      />
        <Atoms.Label
          isSelected={selectedAnswer == answer}
          isCorrect={selectedAnswer == currentQuiz.correct_answer}>{answer}</Atoms.Label>
      </Atoms.Li>))}
            </Atoms.Ul>
        </Atoms.Div>
      </Atoms.Div>
    </Content>
  );
};

export default Quiz;
```

然后，`QuizPage component`渲染`Quiz component`

```
import { Quiz, QuizFooter, QuizResult } from 'components/Organisms';

const QuizPage = () => {
  return (
    <>
      <Quiz />
      <QuizResult />
      <QuizFooter />
    </>
  );
};

export default QuizPage;
```

最后用`Suspense`在`Router.tsx`中渲染`QuizPage component`。

```
import { Suspense } from 'react';
import { Helmet } from 'react-helmet';
import { Route, Switch } from 'react-router';
import { BrowserRouter } from 'react-router-dom';
import { QUIZ_PAGENAME, RESULT_PAGENAME } from 'src/constant';
import { ErrorBoundary, LandingPage, QuizPage, ResultsPage, ShimmerPage} from 'src/components/Pages';

const Router = () => {
  return (
    <BrowserRouter>
      <ErrorBoundary>
        <Suspense fallback={<ShimmerPage />}>
          <Switch>
            <Route path={`/${QUIZ_PAGENAME}`}>
              <Helmet title="Quiz page" />
              <QuizPage />
            </Route>
            ...
          </Switch>
        </Suspense>
      </ErrorBoundary>
    </BrowserRouter>
  );
};

export default Router;
```

`Suspense`是一个拥有子组件的组件，该子组件从服务器获取带有异步数据的全局状态(例如，`Quiz component`带有`initialPropsState`)。它还有名为`fallback`的道具，必须分配一个组件。当异步请求在子组件的全局状态下开始时，它被呈现，其状态将是`loading`。执行异步请求后，状态为`hasValue`,`Suspense`的子组件被渲染。

如果异步请求状态是`hasError`，`ErrorBoundary`被渲染。因为它可以用`getDerivedStateFromError`方法处理错误。

仅此而已。

# 结论

我们可以从这篇文章中学到一些东西，比如

*   原子
*   选择器
*   如何通过 React .悬念呈现带有异步数据的组件

这些不仅容易学习，而且使你的代码简洁。使用完这些，你会觉得反冲是一个强大的状态管理库。所以，试试吧，快乐编码。