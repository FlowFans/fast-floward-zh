# 极速Flow学习之旅(Fast Floward) | 第 1 周 | 第 2 天

欢迎回来！我希望第一天的学习充满乐趣。今天我们将了解另一个 Flow 开发者工具——**代码演练场(Playground)**。此外，我们将编写我们的第一个 *智能合约*，并使用 *交易* 和 *脚本* 与之交互。应该很刺激！但首先，让我们回顾一下我们在第一天学到的东西。

# 第 1 天的学习回顾

- Flow 是一个很酷的区块链。
- Cadence 是用于在 Flow 上编写 DApp 的编程语言。
- 我们可以使用 `flow cadence` 作为 REPL shell，或者 使用 `flow cadence file.cdc` 来执行脚本。
- Cadence 是严格类型化的，我们探索了以下内置类型。
  - `Int`
  - `Fix64`
  - `Address`
  - `String`
  - `Array`
  - `Dictionary`
- Cadence 可选项 （optionals）, 比如 `let optional: String?`, 当值可以是 `nil` 时使用。
- Cadence 函数是带有命名标签的值类型。
- Cadence 有两种复合类型:
  - 结构 `struct`: 值类型（可复制的）,
  - 资源 `resource`: 线性类型（可移动的，只能存在一次）。
- Cadence 的资源类型`resource` 使用`<-` 表示移动，特殊关键字`create` 和`destroy`，以及`@` 表示资源类型，例如`let canvas: @Canvas`。

# 视频

- [第 2 天 – 回顾、Flow Playground、智能合约、交易、脚本 + 作业任务](https://youtu.be/4wpoqDKzw8Y)

# 作业答疑时间

- [作业任务的实现和一般性问答](https://www.youtube.com/watch?v=DwLdLnx8jKE)

# 代码演练场(Playground)

我们使用 `flow cadence` 执行了第一行 Cadence 代码。这是一个很好的入门方式，因为我们只需要一个编程语言解释器。然而，去中心化应用程序不仅仅是解释执行代码，它们还需要与区块链的全局状态交互。

Flow 为我们提供了许多入门选项。

- 一个公共测试网
- 一个独立的本地 Flow 模拟器
- 代码演练场(Playground)

今天，我们将使用 **Playground**，但我们将在本周晚些时候尝试另外两个解决方案。

## 环境

启动浏览器并打开 [play.onflow.org][1] 以启动 代码演练场(Playground)。

![代码演练场截图](images/playground.jpg)

代码演练场(Playground) 界面有5个关键部分，让我们来逐一看看每一个。

## Cadence 编辑器

这是您存储 Cadence 代码的地方。由于 Playground 模拟了 Flow 区块链，因此 Playground 有一些 Cadence REPL shell 中不存在的特殊限制。

- 您只能在 **合约** 编辑器中定义 `contract`、`struct` 和 `resource` 类型，您可以通过从左侧窗格中选择任何 **account** 来打开该编辑器。
- 同样的限制适用于 Cadence `事件 event`类型。

准备好部署合约后，点击绿色的 **Deploy** 按钮。重新点击该按钮会再一次部署合约。

Flow Playground 允许您更新现有合约，但是，众所周知，有时更新可能会失败，如果您遇到不应该出现的问题，请尝试打开一个新的 Playground 并在那里部署您的合约。

![Cadence 编辑器](images/editor.jpg)

## 账号

在 Flow 中，一切都与帐户一起存储，包括智能合约。因此，无论做什么，您都需要访问一个或多个帐户，幸好 Flow Playground 为我们提供了 5 个自动生成的帐户。这将为我们极大的节省了时间。

Playground 的一个限制是每个账户只能部署一个合约。

![账户](images/accounts.jpg)


## 交易

这是您定义 Flow 交易的地方。交易通常用于改变区块链的状态，因此需要由涉及的每一个参与方签名。与其他区块链一样，Flow 交易必须使用私钥进行加密签名，以对交易数据进行编码。值得庆幸的是，Playground 对此进行了抽象，签署交易只需一键即可完成。

![交易](images/transactions.jpg)


## 脚本

**脚本** 面板是您定义 Flow 脚本的地方，这些脚本是不需要任何区块链改变状态的只读程序。因此，与交易不同（即使 Playground 没有任何费用），它们不会产生 gas 费用，并且它们不需要任何帐户的签名授权。

![脚本](图片/脚本.jpg)

## 日志和存储

Cadence 为开发人员提供了非常方便的日志功能 - `log()`。您可以记录变量，查看程序执行时状态如何变化，我们已经通过 `flow cadence` 体验到了这一点。 Playground 是另一个让你看到你的 `log()` 输出的地方。

一旦您开始使用帐户存储数据，您还会在底部窗格中找到帐户的存储信息。

![日志和存储](images/logAndStorage.jpg)


# 智能合约 #1

让我们创建我们的第一个 Cadence 智能合约！

1. 从 **账号(Accounts)** 窗格中选择 `0x01`。
2. 输入以下代码

```cadence
pub contract Hello {
  pub fun sayHi(to name: String) {
    log("Hi, ".concat(name))
  }
}
```

3. 点击 **Deploy**
4. 从日志中获取确认信息。

```
11:11:11 Deployment > [1] > Deployed Contract To: 0x01
```

此时，您将在“帐户”窗格中的“0x01”帐户下看到您部署的合同的名称。 在这种情况下，名称直接取自源代码，但在 Playground 之外，您可以使用 `name: String` 和合约源代码部署合约。 因此，每个帐户可以在不同名称下拥有同一合约的多个实例。 如前所述，Playground 帐户每个仅支持一份合约。


好的，现在我们已经部署了一个区块链程序，让我们与它进行交互吧！ Flow 提供了两种不同的方法来做到这一点。

- 脚本：匿名和只读。
- 交易：经过身份验证，可以改变区块链状态，并且经过加密签名。

我们将分别举一个例子，来在实践中使用这两种方式。 点击**Script**，让我们编写一些代码。


# 脚本 #1


这对您来说应该很熟悉。 我们在第 1 天使用相同的入口点 `main()` 来运行我们的 Cadence 代码，但现在我们可以访问智能合约了！

我们首先从托管合约的账户地址“导入”我们想要与之交互的合约。 您可以将此模式视为很像从包管理器中的库中导入类时的情况。 考虑到 Flow 上的情况，所有内容都与帐户一起存储，自然而然地它们存储所有现有的合约。


```cadence
import Hello from 0x01

pub fun main() {
  let name = "FastFloward"
  Hello.sayHi(to: name)
}
```

单击 **Execute**，您将在 **Log** 窗格中看到如下的两行。

```
11:11:11 Script > [1] > "Hi, FastFloward"
11:11:11 Script > [2] Result > {"type":"Void"}
```

您通常使用脚本获取有关公共状态的信息，因此预计它们会“返回”某种值。 在我们的例子中，我们没有显式返回任何东西，类似于 JavaScript中，当函数返回 undefined 时，在没有显式 return 语句的 Cadence 函数中返回 Void 类型。

现在，继续看看交易的例子部分......

# 交易 #1


作为参考，请使用[文档][2]。 我们的脚本能够与“Hello”合约的公共函数进行交互，通过交易，我们可以将授权帐户添加到组合中。 让我们编写我们的第一笔交易。


```cadence
import Hello from 0x01

transaction {

  let name: String

  prepare(account: AuthAccount) {
    self.name = account.address.toString()
  }

  execute {
    log(Hello.sayHi(to: self.name))
  }
}
```


与 **Scripts** 一样，我们首先导入我们将与之交互的所有合约。

然后我们声明 `transaction` 主体及其内容。 每个交易有4个顺序执行的步骤，但它们都是可选的。


```cadence
transaction(randomParameter: String) {
  let localVariable: Int
  prepare(signer: AuthAccount) {}
  pre {}
  execute {}
  post {}
}
```

如果我们想在 4 个阶段之间共享数据，我们可以在 `transaction` 主体中声明局部变量，不需要访问修饰符。

## 准备阶段（Prepare phase）

这是您可以直接访问由“AuthAccount”实例提供的帐户存储和其他私有功能的唯一阶段。 目前，我们只是使用 `address` 字段，但您可以在 [docs][3] 中了解更多信息。


## 执行阶段（Execute phase）


在这个阶段，您应该存储交易的主要的业务逻辑。 您 ** 不得** 访问此处 `AuthAccount` 的私有对象。

现在，我们只需要 `prepare` 和 `execute` 阶段。

在我们的 `prepare` 阶段，我们获取 `account.address` 的值并存储下来，以供以后在 `execute` 阶段访问。

在 `execute` 阶段，我们调用 `Hello` 合约的 `sayHi` 函数，使用签名帐户地址作为 `name` 参数的值。


## 执行一个交易



让我们一起去执行这个交易吧！ 在 **Transaction Signers** 窗格中，选择一个帐户，可以是任何帐户，然后单击 **Send**。

![交易签名者](images/transaction.signers.jpg)

您应该会看到 **Log** 窗格有不同帐户地址的信息在更新。


```
11:11:11 Transaction > [1] > "Hi, 0x1"
11:11:11 Transaction > [2] > ()
11:11:12 Transaction > [3] > "Hi, 0x2"
11:11:12 Transaction > [4] > ()
```

# Contract #2

现在我们已经熟悉了  **代码演练场 (Flow Playground)** 的功能，我们可以开始将我们的像素艺术的程序逻辑转换为一个智能合约。

```cadence
pub contract Artist {

  pub struct Canvas {

    pub let width: UInt8
    pub let height: UInt8
    pub let pixels: String

    init(width: UInt8, height: UInt8, pixels: String) {
      self.width = width
      self.height = height
      // The following pixels
      // 123
      // 456
      // 789
      // should be serialized as
      // 123456789
      self.pixels = pixels
    }
  }

  pub resource Picture {

    pub let canvas: Canvas

    init(canvas: Canvas) {
      self.canvas = canvas
    }
  }

  pub resource Printer {

    pub let width: UInt8
    pub let height: UInt8
    pub let prints: {String: Canvas}

    init(width: UInt8, height: UInt8) {
      self.width = width;
      self.height = height;
      self.prints = {}
    }

    pub fun print(canvas: Canvas): @Picture? {
      // Canvas needs to fit Printer's dimensions.
      if canvas.pixels.length != Int(self.width * self.height) {
        return nil
      }

      // Canvas can only use visible ASCII characters.
      for symbol in canvas.pixels.utf8 {
        if symbol < 32 || symbol > 126 {
          return nil
        }
      }

      // Printer is only allowed to print unique canvases.
      if self.prints.containsKey(canvas.pixels) == false {
        let picture <- create Picture(canvas: canvas)
        self.prints[canvas.pixels] = canvas

        return <- picture
      } else {
        return nil
      }
    }
  }

  init() {
    self.account.save(
      <- create Printer(width: 5, height: 5),
      to: /storage/ArtistPicturePrinter
    )
    self.account.link<&Printer>(
      /public/ArtistPicturePrinter,
      target: /storage/ArtistPicturePrinter
    )
  }
}
```

大部分代码你应该很熟悉，并添加了一些新内容。

所有的代码都包含在“contract" 的关键字中，在Flow里写智能合约时，您不能在“contract”关键字之外声明任何内容。

然后你可以看到我对 `Printer` 资源的实现，它将打印独特的 `Canvas` 结构。 我使用字典的数据结构来执行此操作，但您也可以使用数组。

最后，还有 `init()` 初始化程序。 这是我们为合约设置一些初始化操作的地方。 在初始化程序中，我们可以访问“self.account”，它属于我们熟悉的“AuthAccount”类型，并为我们提供了对帐户的完全私有的访问权限。


## 存储


在这里您可以看到正在使用的两个 **Account Storage API** 方法：`save` 和 `link`。 有了它们，我们就能够持久化一个 `Printer` 实例并提供给其他人访问它。

```cadence
init() {
  self.account.save(
    <- create Printer(width: 5, height: 5),
    to: /storage/ArtistPicturePrinter
  )
  self.account.link<&Printer>(
    /public/ArtistPicturePrinter,
    target: /storage/ArtistPicturePrinter
  )
}
```

让我们探索每一个方法的细节以及如何使用它们。


### `save`

我们想要在 Flow 区块链上持久化的所有内容，我们都必须用一个帐户来存储。 `save` 函数正是这样做的，我们给出了一个值和一个唯一的存储位置。


```cadence
fun save<T>(_ value: T, to: StoragePath)
```

您可以在两部分中定义存储值的路径：`/domain/uniqueIdentifier`。 存在三个可能的域。

1. `storage`：值的实际位置，只使用 `save()` 和 `storage`。
2. `public`：可以通过`PublicAccount`，在未经授权的情况下访问。
3. `private`：必须通过`AuthAccount`授权访问。

作为参考，请使用 [docs][5]。


### `link`


如果我们只是用合约账户存储一个 `Printer` 的实例，那么每个想要将 `Canvas` 打印为 `Picture` 的人都需要来自合约账户的授权和签名。 这很麻烦，我们希望让每个人都可以自由打印“图片”。

Cadence 采用**基于能力的访问控制**，允许智能合约将其部分存储公开给其他账户。

通过调用`link`，我们创建了一个`Capability`。

```cadence
fun link<T: &Any>(_ newCapabilityPath: CapabilityPath, target: Path): Capability<T>?
```

你一定会问自己这个`&`符号的目的是什么？



### 引用


我们可以创建对资源和结构的引用。 引用使我们能够访问所引用对象的字段和函数。

您可以创建引用。如下面的代码例子。

```cadence
let name = "Morgan"
let nameRef: &String = &name as &String
```

Or you can borrow them from capabilities.

或者您可以从功能中借用它们。

```cadence
let printerRef = getAccount(0x01)
  .getCapability<&Artist.Printer>(/public/ArtistPicturePrinter)
  .borrow()
  ?? panic("Couldn't borrow reference to Printer")

// 目前 printerRef 可以访问底层打印机资源的每个字段和功能。
printerRef.print(...)
```

作为参考 🙂，请使用 [docs][6]。



# 交易 #2


好吧！ 假设我们有一个新的智能合约，让我们测试一下。


```cadence
import Artist from 0x02

transaction() {

  let pixels: String
  let picture: @Artist.Picture?

  prepare(account: AuthAccount) {
    let printerRef = getAccount(0x02)
      .getCapability<&Artist.Printer>(/public/ArtistPicturePrinter)
      .borrow()
      ?? panic("Couldn't borrow printer reference.")

    // Replace with your own drawings.
    self.pixels = "*   * * *   *   * * *   *"
    let canvas = Artist.Canvas(
      width: printerRef.width,
      height: printerRef.height,
      pixels: self.pixels
    )

    self.picture <- printerRef.print(canvas: canvas)
  }

  execute {
    if (self.picture == nil) {
      log("Picture with ".concat(self.pixels).concat(" already exists!"))
    } else {
      log("Picture printed!")
    }

    destroy self.picture
  }
}
```


此交易旨在打印独特的图片并在之后销毁它们。 艺术的极致体现！ 顺便说一下，`getAccount` 函数使用帐户地址来获取`PublicAccount` 的一个实例。

如果我们运行这个交易两次，我们应该得到以下日志。


```
11:11:11 New Transaction > [1] > "Picture printed!"
11:11:12 New Transaction > [2] > "Picture with *   * * *   *   * * *   * already exists!"
```


最后，我们还可以在托管我们的“艺术家”合约的帐户的 **Storage** 窗格中看到更改。

![存储面板](images/storage.jpg)

# 课后作业


通过第二天的学习，我们已经能够在模拟的 Flow 区块链上编写智能合约并执行交易。

让我们深入学习，看看我们是否可以完成以下的课后作业的任务！


- `W1Q3` – 我的珍宝!


为我们的“艺术家”合约创建一个“Collection 收藏”资源，这将允许帐户在打印后存储他们的“图片”资源。 请注意，您只能在合约中创建资源。


```cadence
pub resource Collection {
  pub fun deposit(picture: @Picture)
}
pub fun createCollection(): Collection
```

测试你的 `Collection  收藏`。


- `W1Q4` – 行家

编写一个脚本，打印所有五个 Playground 帐户（`0x01`、`0x02` 等）的收藏的内容。 请使用您的带框画布打印机功能，以清晰易读的方式在日志中记录每个“图片”的画布。 为没有`Collection收藏` 的帐户提供日志。

请使用此文件夹中提供的“.cdc”存根，来提交您对这些任务的解决方案。

```
- artist.contract.cdc
- getCollections.script.cdc
```

祝你完成课后作业时好运！

[1]: https://play.onflow.org/
[2]: https://docs.onflow.org/cadence/language/transactions/
[3]: https://docs.onflow.org/cadence/language/accounts/
[4]: https://docs.onflow.org/cadence/language/accounts/#account-storage
[5]: https://docs.onflow.org/cadence/language/capability-based-access-control/
[6]: https://docs.onflow.org/cadence/language/references/