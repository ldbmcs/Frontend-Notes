# Difference between npm and yarn

https://www.geeksforgeeks.org/difference-between-npm-and-yarn/

![2021-05-28-gglG1J](https://image.ldbmcs.com/2021-05-28-gglG1J.jpg)

**NPM** and **Yarn** are package managers that help to manage a project’s dependencies. A dependency is, as it sounds, something that a project depends on, a piece of code that is required to make the project work properly. We need them because managing the project’s dependencies is a difficult task and it quickly becomes tedious, and out of hand when the project grows. By managing the dependencies, we mean to include, un-include, and update them.

**[npm:](https://www.geeksforgeeks.org/node-js-npm-node-package-manager/)** It is a package manager for the JavaScript programming language. It is the default package manager for the JavaScript runtime environment Node.js. It consists of a command-line client, also called npm, and an online database of public and paid-for private packages called the npm registry.

**yarn:** It stands for **Yet Another Resource Negotiator** and it is a package manager just like npm. It was developed by Facebook and is now open-source. The intention behind developing yarn(at that time) was to **fix performance and security concerns with npm.**

> npm 是按照队列执行每个 package，也就是说必须要等到当前 package 安装完成之后，才能继续后面的安装。而 Yarn 是同步执行所有任务，提高了性能。

**The differences between npm and yarn are explained below:**

**Installation procedure**

- **npm:** npm is installed with Node automatically.

- yarn:

  To install yarn npm have to be installed.

  ```bash
  npm install yarn --global
  ```

**The lock file**

- **npm:** NPM generates a ‘package-lock.json’ file. The package-lock.json file is a little more complex due to a trade-off between determinism and simplicity. Due to this complexity, the package-lock will generate the same node_modules folder for different npm versions. Every dependency will have an exact version number associated with it in the package-lock file.
- **yarn:** Yarn generates a ‘yarn.lock’ file. Yarn lock files help in easy merge. The merges are predictable as well, because of the design of the lock file.

**Output log**

- **install:** The npm creates massive output logs of npm commands. It is essentially a dump of stack trace of what npm is doing.

  ![2021-05-28-ITfYJU](https://image.ldbmcs.com/2021-05-28-ITfYJU.jpg)

- **add:** The yarn output logs are clean, visually distinguishable and brief. They are also ordered in a tree form for understandability.

  ![](https://media.geeksforgeeks.org/wp-content/uploads/20200224130227/20200224-130033.gif)

**Installing global dependencies**

- npm:

  To install a global package, the command template for npm is:

  ```bash
  npm install -g package_name@version_number
  ```

- yarn:

  To install a global package, the command template for yarn is:

  ```bash
  yarn global add package_name@version_number
  ```

**The ‘why’ command:**

- **npm:** npm yet doesn’t has a ‘why’ functionality built in.
- **yarn:** Yarn comes with a ‘why’ command that tells why a dependency is present in the project. For example, it is a dependency, a native module, or a project dependency.

**License Checker**

- **npm:** npm doesn’t has a license checker that can give a handy description of all the licenses that a project is bound with, due to installed dependencies.

- yarn:

  Yarn has a neat license checker. To see them, run

  ```bash
  yarn licenses list
  ```

  ![](https://media.geeksforgeeks.org/wp-content/uploads/20200224132022/20200224-131920.gif)

**Fetching packages**

- **npm:** npm fetches dependencies from the npm registry during every ‘npm install‘ command.
- **Yarn:** yarn stores dependencies locally, and fetches from the disk during a ‘yarn add‘ command (assuming the dependency(with the specific version) is present locally).

**Commands changed in yarn after npm**

| command                | npm                                                          | yarn                                                         |
| :--------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Install dependencies   | npm install                                                  | yarn                                                         |
| Install package        | npm install package_name npm install package_name@version_number | yarn add package_name yarn add package_name@version_number   |
| Uninstall package      | npm uninstall package_name                                   | yarn remove package_name                                     |
| Install dev package    | npm install package_name –save-dev                           | yarn add package_name –dev                                   |
| Update dev package     | npm update package_name npm update package_name@version_number | yarn upgrade package_name yarn upgrade package_name@version_number |
| View package           | npm view package_name                                        | yarn info package_name                                       |
| Global install package | npm install -g package_name                                  | yarn global add package_name                                 |

**Commands same for npm and yarn:**

| npm                 | yarn                 |
| :------------------ | :------------------- |
| npm init            | yarn init            |
| npm run [script]    | yarn run [script]    |
| npm list            | yarn list            |
| npm test            | yarn test            |
| npm link            | yarn link            |
| npm login or logout | yarn login or logout |
| npm publish         | yarn publish         |

