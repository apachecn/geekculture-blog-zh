# Expo 和 Firebase 基于电子邮件和密码的身份验证第 3 部分:登录、忘记密码和更新密码

> 原文：<https://medium.com/geekculture/email-and-password-based-authentication-with-expo-and-firebase-part-3-sign-in-forgot-password-93e9958792f3?source=collection_archive---------17----------------------->

![](img/ee954e9a9b1d6f73f97a86a7a2e1c177.png)

> 这是使用 Expo 和 Firebase 展示基于电子邮件和密码的认证的系列博客文章的第 3/3 部分。
> 
> [第 1 部分:项目设置](/geekculture/email-and-password-based-authentication-with-expo-and-firebase-part-1-project-setup-fef4c6766cd5)
> 
> [第 2 部分:注册、电子邮件验证和退出](/geekculture/email-and-password-based-authentication-with-expo-and-firebase-part-2-sign-up-email-verification-60cc7d1f3ba6)
> 
> 第 3 部分:登录，忘记密码，更新密码(你在这里)

这是一系列博客文章的最后一部分，涵盖了如何使用 Expo 和 Firebase 设置基于电子邮件和密码的身份验证。在这篇博文中，我们将通过实现登录、忘记密码和更新密码功能来结束。我们开始吧！

# 签到

您现在应该非常熟悉每个特性是如何实现的。让我们首先在用户 API 中添加`signIn()`方法，它使用 Firebase 的`signInWithEmailAndPassword()`方法。

```
export const signIn = ({ email = '', password = '' }) =>
  firebase.auth().signInWithEmailAndPassword(email, password)
```

接下来，创建“登录”功能文件夹和一个`/hooks`目录。创建`useSignIn()`钩子`touch src/features/sign-in/hooks/use-sign-in.js`并添加它的实现。`useSignIn()`挂钩的工作原理和其他的一样；它允许访问 Firebase 的“登录”API，并公开一个`isLoading`和`error`状态。

```
export const useSignIn = () => {
  const [state, setState] = useState({
    isLoading: false,
    error: null,
  }) const handleSignIn = async (values) => {
    setState({ isLoading: true, error: null }) try {
      await signIn(values)
      setState({ isLoading: false, error: null })
    } catch (error) {
      setState({ isLoading: false, error })
    }
  } return [handleSignIn, { ...state }]
}
```

接下来，创建`/screens`目录`mkdir src/features/sign-in/screens`和“登录”屏幕`touch src/features/sign-in/screens/SignInScreen.js`。“登录”屏幕重用了`<EmailAndPasswordForm/>`，将其扩展为可选地支持自定义提交按钮文本。请注意，如果出现任何错误，将使用`<ErrorMessage/>`组件([完整源代码](https://github.com/diegocasmo/expo-firebase-authentication/blob/main/src/components/ErrorMessage.js))来呈现，该组件仅使用来自 [native-base](https://docs.nativebase.io/alert) 的`<Alert/>`组件。

```
export const SignInScreen = () => {
  const [signIn, { isLoading, error }] = useSignIn() return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        <ErrorMessage error={error} />
        <EmailAndPasswordForm
          onSubmit={signIn}
          isLoading={isLoading}
          buttonText="Sign in"
        />
      </VStack>
    </Center>
  )
}
```

最后，在访客导航器中定义“登录”屏幕。

```
export const GuestAppNavigator = () => (
  <Stack.Navigator>
    {/* guest welcome and sign up screens omitted for brevity */}
    <Stack.Screen
      name="SignIn"
      component={SignInScreen}
      options={{ title: 'Sign In' }}
    />
  </Stack.Navigator>
)
```

启动应用程序，并验证您能够使用现有用户帐户登录。如果用户的电子邮件未经验证，他们应该显示“验证电子邮件”屏幕，否则，应显示主屏幕。

# 忘记密码

忘记密码功能将帮助用户在忘记密码时找回他们的帐户。为此，用户将指定他们的电子邮件，以便 Firebase 可以发送一个“重置密码”链接。

让我们从创建发送密码重置电子邮件的 API 开始。在用户 API 文件中创建`sendPasswordReset()`方法。

```
export const sendPasswordReset = ({ email = '' }) =>
  firebase.auth().sendPasswordResetEmail(email)
```

接下来，创建忘记密码特征目录`mkdir -p src/features/forgot-password/hooks`和`usePasswordReset()`钩子文件`touch src/features/forgot-password/hooks/use-password-reset.js`。与本系列中创建的所有其他钩子相似，`usePasswordReset()`钩子调用用户 API 方法并跟踪其`isLoading` / `error`状态。

```
export const usePasswordReset = () => {
  const [state, setState] = useState({
    isLoading: false,
    error: null,
  }) const handlePasswordReset = async (values) => {
    setState({ isLoading: true, error: null }) try {
      await sendPasswordReset(values)
      setState({ isLoading: false, error: null })
    } catch (error) {
      setState({ isLoading: false, error })
      throw error
    }
  } return [handlePasswordReset, { ...state }]
}
```

接下来，使用一个按钮扩展登录屏幕，该按钮会将用户重定向到尚未创建的忘记密码屏幕。

```
export const SignInScreen = ({ navigation }) => {
  const handlePressOnForgotPassword = () => {
    navigation.navigate('ForgotPassword')
  } return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        {/* Existing code omitted for brevity */}
        <Button onPress={handlePressOnForgotPassword}>Forgot password</Button>
      </VStack>
    </Center>
  )
}
```

确保在访客导航器堆栈中定义了忘记密码屏幕。

```
export const GuestAppNavigator = () => (
  <Stack.Navigator>
    {/* Existing screens omitted for brevity */}
    <Stack.Screen
      name="ForgotPassword"
      component={ForgotPasswordScreen}
      options={{ title: 'Forgot Password' }}
    />
  </Stack.Navigator>
)
```

忘记密码屏幕使用了一个`<EmailForm/>`组件([完整源代码，此处为](https://github.com/diegocasmo/expo-firebase-authentication/blob/main/src/features/forgot-password/components/EmailForm.js))，它定义了一个电子邮件输入和一个需要与`usePasswordReset()`挂钩连接的提交按钮。创建忘记密码屏幕目录`mkdir src/features/forgot-password/screens`及其文件`touch src/features/forgot-password/screens/ForgotPasswordScreen.js`并填写。

```
export const ForgotPasswordScreen = ({ navigation }) => {
  const [sendPasswordReset, { isLoading, error }] = usePasswordReset() const handlePasswordReset = async (values) => {
    try {
      await sendPasswordReset(values)
      navigation.navigate('SignIn')
    } catch (err) {
      console.error(err.message)
    }
  } return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        <Text>
          Please, enter your email address. You will receive link to create a new
          password via email.
        </Text>
        <ErrorMessage error={error} />
        <EmailForm onSubmit={handlePasswordReset} isLoading={isLoading} />
      </VStack>
    </Center>
  )
}
```

用户现在应该能够通过在表单中输入他们的电子邮件来重置他们的密码，并使用通过电子邮件发送给他们的重置密码链接。

# 更新密码

更新密码功能允许用户更改他们的密码。为此，用户将首先需要重新认证，以便当前登录的用户可以验证他们确实是想要执行更改的人，只有这样，他们才被允许更新他们的密码。

首先在主屏幕中添加一个按钮，该按钮重定向到尚未定义的重新认证屏幕。

```
export const HomeScreen = ({ navigation }) => {
  const handlePressOnUpdatePassword = () => {
    navigation.navigate('Reauthenticate')
  } return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        <Button onPress={handlePressOnUpdatePassword}>Update password</Button>
        {/* Existing code omitted for brevitiy */}
      </VStack>
    </Center>
  )
}
```

接下来，将重新认证屏幕添加到已验证的应用导航器。

```
export const VerifiedAppNavigator = () => (
  <Stack.Navigator>
    <Stack.Screen
      name="Reauthenticate"
      component={ReauthenticateScreen}
      options={{ title: 'Sign in' }}
    />
    {/* Existing code omitted for brevitiy */}
  </Stack.Navigator>
)
```

接下来，通过运行`mkdir -p src/features/update-password/screens`和使用`touch src/features/update-password/screens/ReauthenticateScreen.js`的重新认证屏幕来创建更新密码特性目录。为了重新认证用户，在用户 API 文件中创建`reauthenticate`方法，该方法使用 Firebase 的`reauthenticateWithCredential`方法。

```
export const reauthenticate = ({ email = '', password = '' }) =>
  getUser().reauthenticateWithCredential(
    firebase.auth.EmailAuthProvider.credential(email, password)
  )
```

一旦定义了 API 方法，就可以通过运行`mkdir -p src/features/update-password/hooks`和`touch src/features/update-password/hooks/use-reauthenticate.js`来创建`useReauthenticate()`钩子以方便地访问它。请注意，如果在执行操作时出现异常，它是如何再次抛出错误的。

```
export const useReauthenticate = () => {
  const [state, setState] = useState({
    isLoading: false,
    error: null,
  }) const handleReauthenticate = async (values) => {
    setState({ isLoading: true, error: null }) try {
      await reauthenticate(values)
      setState({ isLoading: false, error: null })
    } catch (error) {
      setState({ isLoading: false, error })
      throw error
    }
  } return [handleReauthenticate, { ...state }]
}
```

最后，在重新认证屏幕中使用`useReauthenticate()`钩子，它的工作方式与登录屏幕非常相似，但是它会重新认证用户。如果用户重新验证成功，他们将被重定向到“更新密码”屏幕。

```
export const ReauthenticateScreen = ({ navigation }) => {
  const [reauthenticate, { isLoading, error }] = useReauthenticate() const handleReauthenticate = async (values) => {
    try {
      await reauthenticate(values)
      navigation.navigate('UpdatePassword')
    } catch (err) {
      console.error(err.message)
    }
  } return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        <ErrorMessage error={error} />
        <EmailAndPasswordForm
          onSubmit={handleReauthenticate}
          isLoading={isLoading}
          buttonText="Re-authenticate"
        />
      </VStack>
    </Center>
  )
}
```

到目前为止，用户应该能够在主屏幕中单击“更新密码”，在重新验证屏幕中重新验证他们的帐户，如果成功，将被重定向到更新密码屏幕。

为了允许用户在成功地重新认证后更新他们的密码，让我们来定义将用于更新密码的 API 方法。在用户 API 文件中，定义`updatePassword()`方法。

```
export const updatePassword = ({ password = '' }) =>
  getUser().updatePassword(password)
```

接下来，为它创建一个钩子`touch src/features/update-password/hooks/use-update-password.js`

```
export const useUpdatePassword = () => {
  const [state, setState] = useState({
    isLoading: false,
    error: null,
  }) const handleUpdatePassword = async (values) => {
    setState({ isLoading: true, error: null }) try {
      await updatePassword(values)
      setState({ isLoading: false, error: null })
    } catch (error) {
      setState({ isLoading: false, error })
      throw error
    }
  } return [handleUpdatePassword, { ...state }]
}
```

随后，在更新密码功能目录`touch src/features/update-password/screens/UpdatePasswordScreen.js`中创建更新密码屏幕。更新密码屏幕将呈现`<UpdatePasswordForm/>` ( [完整源代码](https://github.com/diegocasmo/expo-firebase-authentication/blob/main/src/features/reset-password/components/UpdatePasswordForm.js))并使用`useUpdatePassword()`挂钩设置新的用户密码，如果成功，重定向到导航堆栈的顶部(主屏幕)。

```
export const UpdatePasswordScreen = ({ navigation }) => {
  const [updatePassword, { isLoading, error }] = useUpdatePassword() const handleUpdatePassword = async (values) => {
    try {
      await updatePassword(values)
      navigation.popToTop()
    } catch (err) {
      console.error(err.message)
    }
  } return (
    <Center flex={1}>
      <VStack space={4} alignItems="center" w="90%">
        <ErrorMessage error={error} />
        <UpdatePasswordForm
          onSubmit={handleUpdatePassword}
          isLoading={isLoading}
        />
      </VStack>
    </Center>
  )
}
```

最后，将更新密码屏幕添加到已验证的应用导航器中。

```
export const VerifiedAppNavigator = () => (
  <Stack.Navigator>
    {/* Existing code omitted for brevitiy */}
    <Stack.Screen
      name="UpdatePassword"
      component={UpdatePasswordScreen}
      options={{ title: 'Update Password' }}
    />
  </Stack.Navigator>
)
```

就是这样！一旦重新认证，应用程序的用户现在可以更新他们的密码。

# 结论

这是一系列博客文章的最后一部分，涵盖了如何使用 Expo 和 Firebase 设置基于电子邮件和密码的身份验证。开发的应用程序支持注册、登录、注销、电子邮件验证、忘记密码和更新密码。如果你有任何问题，欢迎在下面发表问题或评论，或者在项目的 [GitHub 库](https://github.com/diegocasmo/expo-firebase-authentication)中提出问题；我很乐意帮助解决任何问题🙂。