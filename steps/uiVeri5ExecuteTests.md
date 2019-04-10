# uiVeri5ExecuteTests

## Description

With this step [UIVeri5](https://github.com/SAP/ui5-uiveri5) tests can be executed.

UIVeri5 describes following benefits on its GitHub page:

* Automatic synchronization with UI5 app rendering so there is no need to add waits and sleeps to your test. Tests are reliable by design.
* Tests are written in synchronous manner, no callbacks, no promise chaining so are really simple to write and maintain.
* Full power of webdriverjs, protractor and jasmine - deferred selectors, custom matchers, custom locators.
* Control locators (OPA5 declarative matchers) allow locating and interacting with UI5 controls.
* Does not depend on testability support in applications - works with autorefreshing views, resizing elements, animated transitions.
* Declarative authentications - authentication flow over OAuth2 providers, etc.
* Console operation, CI ready, fully configurable, no need for java (comming soon) or IDE.
* Covers full ui5 browser matrix - Chrome,Firefox,IE,Edge,Safari,iOS,Android.
* Open-source, modify to suite your specific neeeds.

!!! note "Browser Matrix"
    With this step and the underlying Docker image ([selenium/standalone-chrome](https://github.com/SeleniumHQ/docker-selenium/tree/master/StandaloneChrome)) only Chrome tests are possible.

    Testing of further browsers can be done with using a custom Docker image.

## Prerequisites

## Parameters

| name | mandatory | default | possible values |
|------|-----------|---------|-----------------|
| `dockerEnvVars` | no |  |  |
| `dockerImage` | no |  |  |
| `dockerWorkspace` | no |  |  |
| `failOnError` | no |  | `true`, `false` |
| `gitBranch` | no |  |  |
| `gitSshKeyCredentialsId` | no |  |  |
| `installCommand` | no | `npm install @ui5/uiveri5 --global --quiet` |  |
| `runCommand` | no | `uiveri5 --seleniumAddress='http://${config.seleniumHost}:${config.seleniumPort}/wd/hub'` |  |
| `script` | yes |  |  |
| `seleniumHost` | no |  |  |
| `seleniumPort` | no | `4444` |  |
| `sidecarEnvVars` | no |  |  |
| `sidecarImage` | no |  |  |
| `stashContent` | no | `[buildDescriptor, tests]` |  |
| `testOptions` | no |  |  |
| `testRepository` | no |  |  |
| `testServerUrl` | no |  |  |

* `dockerEnvVars` - Environment variables to set in the container, e.g. [http_proxy: 'proxy:8080'].
* `dockerImage` - Name of the docker image that should be used. If empty, Docker is not used and the command is executed directly on the Jenkins system.
* `dockerWorkspace` - Kubernetes only: Specifies a dedicated user home directory for the container which will be passed as value for environment variable `HOME`.
* `failOnError` - With `failOnError` the behavior in case tests fail can be defined.
* `gitBranch` - Only if `testRepository` is provided: Branch of testRepository, defaults to master.
* `gitSshKeyCredentialsId` - Only if `testRepository` is provided: Credentials for a protected testRepository
* `installCommand` - The command that is executed to install the test tool.
* `runCommand` - The command that is executed to start the tests.
* `script` - The common script environment of the Jenkinsfile running. Typically the reference to the script calling the pipeline step is provided with the this parameter, as in `script: this`. This allows the function to access the commonPipelineEnvironment for retrieving, for example, configuration parameters.
* `seleniumHost` - The host of the selenium hub, this is set automatically to `localhost` in a Kubernetes environment (determined by the `ON_K8S` environment variable) of to `selenium` in any other case. The value is only needed for the `runCommand`.
* `seleniumPort` - The port of the selenium hub. The value is only needed for the `runCommand`.
* `sidecarEnvVars` - as `dockerEnvVars` for the sidecar container
* `sidecarImage` - as `dockerImage` for the sidecar container
* `stashContent` - Specific stashes that should be considered for the step execution.
* `testOptions` - This allows to set specific options for the UIVeri5 execution. Details can be found [in the UIVeri5 documentation](https://github.com/SAP/ui5-uiveri5/blob/master/docs/config/config.md#configuration).
* `testRepository` - Define an additional repository where the test implementation is located. For protected repositories the `testRepository` needs to contain the ssh git url.
* `testServerUrl` - The `testServerUrl` is passed as environment variable `TARGET_SERVER_URL` to the test execution. The tests should read the host information from this environment variable in order to be infrastructure agnostic.

## Step configuration

We recommend to define values of step parameters via [config.yml file](../configuration.md).

In following sections of the config.yml the configuration is possible:

| parameter | general | step/stage |
|-----------|---------|------------|
| `dockerEnvVars` |  | X |
| `dockerImage` |  | X |
| `dockerWorkspace` |  | X |
| `failOnError` |  | X |
| `gitBranch` |  | X |
| `gitSshKeyCredentialsId` | X | X |
| `installCommand` |  | X |
| `runCommand` |  | X |
| `script` |  |  |
| `seleniumHost` |  | X |
| `seleniumPort` |  | X |
| `sidecarEnvVars` |  | X |
| `sidecarImage` |  | X |
| `stashContent` |  | X |
| `testOptions` |  | X |
| `testRepository` |  | X |
| `testServerUrl` |  | X |

## Exceptions

If you see an error like `fatal: Not a git repository (or any parent up to mount point /home/jenkins)` it is likely that your test description cannot be found.<br />
Please make sure to point parameter `testOptions` to your `conf.js` file like `testOptions: './path/to/my/tests/conf.js'`

## Examples

### Passing credentials from Jenkins

When running acceptance tests in a real environment, authentication will be enabled in most cases. UIVeri5 includes [features to automatically perform the login](https://github.com/SAP/ui5-uiveri5/blob/master/docs/config/authentication.md) with credentials in the `conf.js`. However, having credentials to the acceptance system stored in plain text is not an optimal solution.

Therefore, UIVeri5 allows templating to set parameters at runtime, as shown in the following example `conf.js`:

```js
// Read environment variables
const defaultParams = {
    url: process.env.TARGET_SERVER_URL,
    user: process.env.TEST_USER,
    pass: process.env.TEST_PASS
};

// Resolve path to specs relative to the working directory
const path = require('path');
const specs = path.relative(process.cwd(), path.join(__dirname, '*.spec.js'));

// export UIVeri5 config
exports.config = {
    profile: 'integration',
    baseUrl: '${params.url}',
    specs: specs,
    params: defaultParams, // can be overridden via cli `--params.<key>=<value>`
    auth: {
        // set up authorization for CF XSUAA
        'sapcloud-form': {
            user: '${params.user}',
            pass: '${params.pass}',
            userFieldSelector: 'input[name="username"]',
            passFieldSelector: 'input[name="password"]',
            logonButtonSelector: 'input[type="submit"]',
            redirectUrl: /cp.portal\/site/
        }
    }
};
```

While default values for `baseUrl`, `user` and `pass` are read from the environment, they can also be overridden when calling the CLI.

In a custom Pipeline, this is very simple: Just wrap the call to `uiVeri5ExecuteTests` in `withCredentials` (`TARGET_SERVER_URL` is read from `config.yml`):

```groovy
withCredentials([usernamePassword(
    credentialsId: 'MY_ACCEPTANCE_CREDENTIALS',
    passwordVariable: 'password',
    usernameVariable: 'username'
)]) {
    uiVeri5ExecuteTests script: this, testOptions: "./uiveri5/conf.js --params.user=${username} --params.pass=${password}"
}
```

In a Pipeline Template, a [Stage Exit](#) can be used to fetch the credentials and store them in the environment. As the environment is passed down to uiVeri5ExecuteTests, the variables will be present there. This is an example for the stage exit `.pipeline/extensions/Acceptance.groovy` where the `credentialsId` is read from the `config.yml`:

```groovy
void call(Map params) {
    // read username and password from the credential store
    withCredentials([usernamePassword(
        credentialsId: params.config.acceptanceCredentialsId,
        passwordVariable: 'password',
        usernameVariable: 'username'
    )]) {
        // store the result in the environment variables for executeUIVeri5Test
        withEnv(["TEST_USER=${username}", "TEST_PASS=${password}"]) {
            //execute original stage as defined in the template
            params.originalStage()
        }
    }
}
return this
```
