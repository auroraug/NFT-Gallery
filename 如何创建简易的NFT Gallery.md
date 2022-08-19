### [4. 如何创建 NFT 画廊](https://docs.alchemy.com/docs/how-to-create-an-nft-gallery)

欢迎来到 Web3 之路系列的第 4 周。在本教程中，您将学习如何开发一个 NFT 库，通过钱包地址和智能合约地址显示 NFT。

[官方文档](https://docs.alchemy.com/edit/how-to-create-an-nft-gallery)

将公共数据存储在区块链上并不容易访问。查询区块链实际上**是开发人员最大的提升之一，**他们需要从链的起源开始收集连接点和回答问题所需的数据。

假设开发人员想要回答以下问题：“钱包拥有哪些 NFT？”。

这听起来像是一个简单的询问，但答案远非易事。

在这种情况下，开发人员需要找到在给定链上铸造的所有 NFT，然后按照所有转移函数来了解当前拥有 NFT 的人。

这项任务可能需要开发人员数周时间。幸运的是，有一个解决方案 - 使用[Free Alchemy account](https://auth.alchemyapi.io/signup/?a=fe330e8900)和[Alchemy NFT API](https://docs.alchemy.com/reference/nft-api)，这允许开发人员在几毫秒内从区块链中获取 NFT，而不是几周。

------

由于 Alchemy 已经查询了完整的区块链并为其数据建立了索引，因此 NFT API 使您能够完全访问信息。

#### 如何创建 NFT 图库教程

在本教程中，您将学习如何使用 Alchemy NFT API 来构建一个能够基于三件事获取 NFT 的 NFT 库：

1. 钱包地址
2. 收货地址
3. 钱包地址+收款地址



![2612](https://files.readme.io/5266633-Blavk.png)![2612](https://files.readme.io/5266633-Blavk.png)

NFT 画廊示例



#### 所需工具

我们将使用 Alchemy NFT API 调用来获取 NFT 及其元数据，并在我们的 NFT 库中显示图像、描述和 ID，使用：

- Next JS
- Tailwind CSS
- Alchemy NFT API
- [Alchemy Account](https://auth.alchemyapi.io/signup/?a=fe330e8900)

#### 一、 创建项目设置

您需要做的第一件事是创建 Next JS 项目样板并安装 TailwindCSS，它将处理您的样式。

打开终端并编写以下代码：

```
npx create-next-app -e with-tailwindcss nameoftheproject
```

![img](https://mirror.xyz/_next/image?url=https%3A%2F%2Fimages.mirror-media.xyz%2Fpublication-images%2FUYMpT1-QymG2UQJBbzddP.png&w=3840&q=90)

导航到您的项目并启动 VSCode：

```
cd nameoftheproject && code .
```

现在您的项目样板已经创建，测试一切正常。

在您的项目目录中运行以下代码：

```
npm run dev
```

浏览到 localhost:3000 应该会呈现以下页面：

![1347](https://files.readme.io/d48c519-nextjs.png)![1347](https://files.readme.io/d48c519-nextjs.png)

浏览到 localhost:3000 后呈现的页面

因为我们不需要它，所以您可以删除 <div> 标记中 的所有代码，文件中的代码现在应该如下所示：`pages>index.tsx.`

```
index.tsx
// Some code
```

因为本教程将使用 Javascript 而不是 Typescript，所以在编写代码之前，请将您的和 _app.tsx 文件从文件转换为文件。`index.jsx``.tsx``.jsx`

- 将两个文件扩展名更改为`.jsx`
- 删除两个文件顶部导入的类型

#### 二、创建主页

创建可搜索 NFT 库的第一步是创建文本输入，您将在其中添加用于搜索 NFT 的钱包地址和收藏地址。

在页面中，添加以下代码：`index.jsx`

```
const Home = () => {
 
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
      </div>
    </div>
  )
}

export default Home
```

如您所见，我们已经在输入的 className 属性中实现了 Tailwind CSS 类。我们不会详细介绍 TailwindCSS 的工作原理，如果您想了解更多信息，可以[参考官方文档](https://tailwindcss.com/docs/installation).

##### 创建两个变量来存储钱包和收款地址

接下来，我们需要创建两个变量来存储我们将在输入字段中插入的钱包地址和收藏地址，使用[useState() React Hook](https://reactjs.org/docs/hooks-state.html).

从“react”导入“useState”hook，并在“Home”组件返回语句的正上方添加以下代码：

```
  import { useState } from 'react'

  const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
      </div>
    </div>
  )
}

export default Home
 
```

要将文本输入的值存储在“wallet”和“collection”变量中，请使用“onChange”事件处理程序。

这[“onChange”事件处理程序](https://reactjs.org/docs/handling-events.html)每次更改输入字段的值时都会触发，使用and函数获取输入并将其存储在相应的变量中。`setWallet``setCollectionAddress`

您还需要反映“wallet”和“collection”变量的变化，分配它们在输入中显示的值。

在文本输入标签中，添加以下代码：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input onChange={(e)=>{setWalletAddress(e.target.value)}} value={wallet} type={"text"} placeholder="Add your wallet address"></input>
        <input onChange={(e)=>{setCollectionAddress(e.target.value)}} value={collection} type={"text"} placeholder="Add the collection address"></input>
      </div>
    </div>
  )
}

export default Home
```

您的输入现在将在它们各自的变量中存储我们将在其中写入的地址。

##### 使用浏览器中的“React 开发人员工具”检查一切是否正常

我们将在 Chrome 上完成安装过程，但所有这些也适用于 Mozilla Firefox。

- 前往浏览器扩展商店
- 搜索[React 开发者工具](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
- 点击“添加到 Chrome”

下载并安装后，您将能够在 Chrome 开发人员工具的“检查”视图中看到 React 开发工具选项卡。

返回终端，在您的应用程序文件夹中，并再次启动您的应用程序：

```
npx run dev
```

- 转到您的应用程序页面（本地主机：3000）
- 右键单击页面上的任意位置
- 点击检查
- 点击“>>”符号
- 转到“组件”


![1312](https://files.readme.io/674bdfd-components.png)
在“组件”视图中，我们将能够看到页面中包含的所有组件的列表，并且在右侧，挂钩中保存的不同状态以及有关应用程序的其他信息：
![1312](https://files.readme.io/0c0dd48-fo.png)

安装 React 开发人员工具后，您可以通过写入输入并检查状态值是否更新来测试hook：`useState()`


![3446](https://files.readme.io/8f86eb9-wallet_col.png)

现在您可以存储钱包和收款地址，让我们完成主页。

##### 完成主页

接下来，您需要添加一个按钮，该按钮将触发从 Alchemy NFT API 获取 NFT 的功能，并添加一个切换按钮来决定您是要按钱包地址、收藏还是两者进行搜索。

- 在集合文本输入下添加一个具有“复选框”类型的新输入
- 将其包裹在 ` ` 函数中以添加一些文本
- 创建按钮

```
 import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"}>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

现在您有了按钮，添加一个处理程序，该处理程序将触发您的函数以根据拥有 NFT 的钱包获取 NFT：`onClick()``fetchNFTs()`

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
          () => {
        
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

现在，创建要在处理程序中添加的函数并获取您的 NFT。`fetchNFTs()``onClick`

但在此之前，您需要创建一个新应用程序来获取 Alchemy API 密钥。

#### 三、 创建一个新的Alchemy app

导航到 alchemy.com 并单击“创建应用程序”以创建一个新应用程序：



![2918](https://files.readme.io/0e7ff54-new_app.png)![2918](https://files.readme.io/0e7ff54-new_app.png)

在 Alchemy 仪表板中创建一个新应用程序。



以下是要添加的详细信息：

- Name
- Description
- Chain（Ethereum）
- Network（mainnet）

选择以太坊主网将允许您仅从以太坊获取 NFT。

如果您想在 Polygon 或其他链上获取 NFT，您需要使用相应的链创建一个新应用程序并更改基本 URL 以反映您要使用的链，例如，Polygon 的 URL 将是：https://polygon-mumbai.g.alchemy.com/v2/YOUR-API-KEY

现在我们的 Alchemy 应用程序已经启动并运行了，让我们创建一个函数来获取地址拥有的所有 NFT。`fetchNFTs`

#### 四、创建 FetchNFTs 函数

要获取钱包地址拥有的 NFT，请使用[获取 NFT](https://docs.alchemy.com/reference/getnfts)Alchemy NFT API 的端点。

在 home 组件中，声明一个新的 useState() 变量，就像我们之前对钱包和收藏地址所做的那样，以存储我们将使用 Alchemy NFT API 获取的 NFT：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])

  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
          () => {
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

现在，总是在主页组件内部，添加该功能。`fetchNFTs()`

```
const fetchNFTs = async() => {
  let nfts; 
  console.log("fetching nfts");
  const api_key = "A8A1Oo_UTB9IN5oNHfAc2tAxdR4UVwfM"
  const baseURL = `https://eth-mainnet.alchemyapi.io/v2/${api_key}/getNFTs/`;
  var requestOptions = {
      method: 'GET'
    };
   
  if (!collection.length) {
  
    const fetchURL = `${baseURL}?owner=${wallet}`;

    nfts = await fetch(fetchURL, requestOptions).then(data => data.json())
  } else {
    console.log("fetching nfts for collection owned by address")
    const fetchURL = `${baseURL}?owner=${wallet}&contractAddresses%5B%5D=${collection}`;
    nfts= await fetch(fetchURL, requestOptions).then(data => data.json())
  }

  if (nfts) {
    console.log("nfts:", nfts)
    setNFTs(nfts.ownedNfts)
  }
}
```

关于您的函数，首先要注意的是 async 关键字，它允许您在不阻塞整个应用程序的情况下获取数据。`fetchNFTs()`

> ### 📘
>
> 阅读更多关于[异步等待 JavaScript 工作流程](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

然后，您声明了一个新变量，用于存储您将获取的 NFT，然后创建您将在您的[fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)检索 nfts。

基本 URL，根据[Alchemy NFT API 文档](https://docs.alchemy.com/reference/nft-api), 由以下组成：

- Application base URL
- Your API key
- Owner address
- Collection address **-** optional

如果提供了收藏地址，NFT API 将按收藏过滤提取的 NFT，如果未提供，API 将检索提供的钱包地址所拥有的所有 NFT。

因此，您需要一种方法来了解是否提供了集合地址，因此：如果您想按集合过滤。

为此，您可以添加一个“if”语句来检查“collection”变量是否为空：

如果为空，则只需提供钱包地址参数：

```
const fetchURL = `${baseURL}?owner=${wallet}`;
```

如果不是，则获取钱包拥有的所有 NFT 并按集合过滤掉它们：

```
const fetchURL = `${baseURL}?owner=${wallet}&contractAddresses%5B%5D=${collection}`;
```

“contractAddresses”参数后面的“5B%5D”字符串指定“contractAddresses”参数是一个数组，而不是一个简单的字符串。这是因为您实际上可以按多个“contractAddresses”过滤，而不仅仅是一个。

在这两种情况下，无论是否按集合过滤，您都希望将“baseURL”输入一个函数，等待结果的到来，并将其转换为 JSON`fetch()`[使用 .json() 函数](https://developer.mozilla.org/en-US/docs/Web/API/Response/json).

打印我们的函数检索到的数据，将记录以下对象：`fetch()`

![1306](https://files.readme.io/f2a9d32-jsx.png)

在获取的对象中，您拥有的信息比您需要的要多，因为您只需要包含我们提供的钱包地址所拥有的 NFT 的**数组**。

这就是为什么在函数中你提供 nfts.ownednfts 而不仅仅是 nfts 的原因，因为你只需要存储数组以便以后使用它来显示你的 NFT。`setNFTs()``ownednfts`

现在您的函数已准备就绪，您需要实现一个新函数以在没有 NFT 所有者的情况下按集合获取 NFT。`fetchNFTs()`

#### 五、通过 Collection 函数创建 FetchNFT

要按集合获取 NFT，您可以使用[getNFTsForCollection](https://docs.alchemy.com/reference/getnftsforcollection)Alchemy端点。

端点将需要两个参数：`getNFTsForCollection`

- `contractAddress`- NFT 集合的合约地址**[字符串]**
- `withMetadata` ***-***（可选） ******如果设置为，则返回 NFT 元数据；否则只会返回 tokenIds。默认为**[boolean]**`true``false`

第一个参数是指定要获取的集合的合约地址。

如果您还想获取集合中包含的 NFT 的元数据（例如，标题、图像、描述、属性）或仅获取它们的 ID，则第二个参数指定 NFT API。

你可以阅读更多关于[Alchemy NFT API](https://docs.alchemy.com/reference/getnftsforcollection).

正如我们之前所做的，让我们先复制代码并了解它的作用：

```
const fetchNFTsForCollection = async () => {
  if (collection.length) {
    var requestOptions = {
      method: 'GET'
    };
    const api_key = "A8A1Oo_UTB9IN5oNHfAc2tAxdR4UVwfM"
    const baseURL = `https://eth-mainnet.alchemyapi.io/v2/${api_key}/getNFTsForCollection/`;
    const fetchURL = `${baseURL}?contractAddress=${collection}&withMetadata=${"true"}`;
    const nfts = await fetch(fetchURL, requestOptions).then(data => data.json())
    if (nfts) {
      console.log("NFTs in collection:", nfts)
      setNFTs(nfts.nfts)
    }
  }
}
```

它与我们之前构建的函数非常相似，主要有两点不同：`fetchNFTs()`

- 使用的端点
- NFTs 状态变量中存储的值

这是正在发生的事情：

1. 您首先验证Collection address不为空
2. 然后你声明告诉 fetch() 你的`requestOptions`[HTTP 请求将是一个“GET”请求](https://reqbin.com/Article/HttpGet)
3. 最后，您构建了传递集合地址值作为参数并将参数设置为 true。`baseURL``contractAddress``withMetadata`

最后，您使用 async await 工作流程 +函数将获取的数据转换为 JSON。`json()`

如果我们 console.log 包含提取的 NFT 的 JSON 对象，我们会注意到它包含 2 个属性：

![1302](https://files.readme.io/9c7e67e-arrayNft.png)

控制台日志示例



- 下一个令牌
- nfts

在这种情况下，您只需要通过将“nfts.nfts”传递给函数来包含 NFTs 数组的“nfts”属性。 此时您的代码应如下所示：`setNFTs()`

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])

  const fetchNFTs = async() => {
    let nfts; 
    console.log("fetching nfts");
    const api_key = "A8A1Oo_UTB9IN5oNHfAc2tAxdR4UVwfM"
    const baseURL = `https://eth-mainnet.alchemyapi.io/v2/${api_key}/getNFTs/`;
    var requestOptions = {
        method: 'GET'
      };
     
    if (!collection.length) {
    
      const fetchURL = `${baseURL}?owner=${wallet}`;
  
      nfts = await fetch(fetchURL, requestOptions).then(data => data.json())
    } else {
      console.log("fetching nfts for collection owned by address")
      const fetchURL = `${baseURL}?owner=${wallet}&contractAddresses%5B%5D=${collection}`;
      nfts= await fetch(fetchURL, requestOptions).then(data => data.json())
    }
  
    if (nfts) {
      console.log("nfts:", nfts)
      setNFTs(nfts.ownedNfts)
    }
  }
  
  const fetchNFTsForCollection = async () => {
    if (collection.length) {
      var requestOptions = {
        method: 'GET'
      };
      const api_key = "A8A1Oo_UTB9IN5oNHfAc2tAxdR4UVwfM"
      const baseURL = `https://eth-mainnet.alchemyapi.io/v2/${api_key}/getNFTsForCollection/`;
      const fetchURL = `${baseURL}?contractAddress=${collection}&withMetadata=${"true"}`;
      const nfts = await fetch(fetchURL, requestOptions).then(data => data.json())
      if (nfts) {
        console.log("NFTs in collection:", nfts)
        setNFTs(nfts.nfts)
      }
    }
  }
  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
          () => {
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

现在您的获取 NFT 的函数正在运行，您需要将它们附加到您之前创建的按钮的“onClick”触发器上。

#### 六、触发 FetchNFTs 和 FetchNFTsByCollection 函数

您需要做的第一件事是添加一个名为“fetchForCollection”的新状态变量，它将检查您是否要按收藏或钱包地址进行搜索：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])
  const [fetchForCollection, setFetchForCollection]=useState(false)


  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
          () => {
         
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

此变量将由我们创建的复选框输入处理，根据复选框是否选中来更新其值：

- **已检查**：我们正在按集合获取 - 状态变量将为 true
- **未选中**：我们通过钱包地址获取 - 状态变量将为 false

为此，您需要向输入添加另一个“onChange”处理程序，但这次[使用“e.target.checked”](https://bobbyhadz.com/blog/react-check-if-checkbox-is-checked), value 作为状态输入的值，而不是 e.target.value：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])
  const [fetchForCollection, setFetchForCollection]=useState(false)


  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input onChange={(e)=>{setFetchForCollection(e.target.checked)}} type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
          () => {
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

我们可以返回并使用 React 开发人员工具检查变量是否正确更新。

既然您知道您是按钱包还是按收藏查找 NFT，请确保您的按钮能够根据我们的“fetchForCollection”变量触发正确的函数：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])
  const [fetchForCollection, setFetchForCollection]=useState(false)


  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input onChange={(e)=>{setFetchForCollection(e.target.checked)}} type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
           () => {
            if (fetchForCollection) {
              fetchNFTsForCollection()
            }else fetchNFTs()
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

在这里，我们基本上是在告诉我们的按钮：

- 如果“fetchForCollection”为真
- 然后运行 fetchNFTsForCollection() 函数，如果没有，只需 fetchNFTs。

为确保您在根据集合查找 NFT 时不会在钱包地址输入中添加钱包地址，您可以将“禁用”属性添加到您的钱包输入中，并在 fetchForCollection 为 true 时禁用它：

```
import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])
  const [fetchForCollection, setFetchForCollection]=useState(false)


  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input disabled={fetchForCollection} type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input onChange={(e)=>{setFetchForCollection(e.target.checked)}} type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
           () => {
            if (fetchForCollection) {
              fetchNFTsForCollection()
            }else fetchNFTs()
          }
        }>Let's go! </button>
      </div>
    </div>
  )
}

export default Home
```

太棒了，下一步是可视化你的 NFT。为此，您需要创建组件。`NFTCard`

#### 七、创建 NFT Card 组件

在项目的根文件夹中，创建一个新文件夹并将其命名为“组件”。

在此文件夹中创建一个新文件并将其命名为“nftCard.jsx”。

您的 NFT 卡将 NFT 作为道具（在 ReactJS 文档中了解有关道具的更多信息），并将获取其元数据以将其显示在卡中，如下所示：


![2282](https://files.readme.io/20eb9a4-bored.png)![2282](https://files.readme.io/20eb9a4-bored.png)

为此，在您刚刚创建的文件中，添加以下代码：

```
export const NFTCard = ({ nft }) => {

    return (
        <div className="w-1/4 flex flex-col ">
        <div className="rounded-md">
            <img className="object-cover h-128 w-full rounded-t-md" src={nft.media[0].gateway} ></img>
        </div>
        <div className="flex flex-col y-gap-2 px-2 py-3 bg-slate-100 rounded-b-md h-110 ">
            <div className="">
                <h2 className="text-xl text-gray-800">{nft.title}</h2>
                <p className="text-gray-600">Id: {nft.id.tokenId}</p>
                <p className="text-gray-600" >{nft.contract.address}</p>
            </div>

            <div className="flex-grow mt-2">
                <p className="text-gray-600">{nft.description}</p>
            </div>
        </div>

    </div>
    )
}
```

如您所见，我们显示了 5 个属性：

- 图片
- 标题
- TokenId
- 合约地址
- 描述

要访问这些属性，我们可以再次查看 NFT 对象：

![2508](https://files.readme.io/05d3fb3-nftObject.png)

以下是获取 NFT 图像的方法：

- 访问媒体对象的第一个索引
- 访问其中的 gateway 属性以获取图像 URL
- 将图像 URL 分配给“media[0].gateway”上方代码中的标记`img`

要访问 NFT 标题，您只需要访问 NFT 对象本身内部的 title 属性。

使用您的 NFT 卡，转到您的文件并将其导入以创建 NFT 图库。`home.jsx`

#### 八、创建 NFT 画廊

在文件中，在您的按钮正下方，导入以下代码：`pages>index.js`

```
 import { NFTCard } from "./components/nftCard"
 import { useState } from 'react'

const Home = () => {
  const [wallet, setWalletAddress] = useState("");
  const [collection, setCollectionAddress] = useState("");
  const [NFTs, setNFTs] = useState([])
  const [fetchForCollection, setFetchForCollection]=useState(false)


  return (
    <div className="flex flex-col items-center justify-center py-8 gap-y-3">
      <div className="flex flex-col w-full justify-center items-center gap-y-2">
        <input disabled={fetchForCollection} type={"text"} placeholder="Add your wallet address"></input>
        <input type={"text"} placeholder="Add the collection address"></input>
        <label className="text-gray-600 "><input onChange={(e)=>{setFetchForCollection(e.target.checked)}} type={"checkbox"} className="mr-2"></input>Fetch for collection</label>
        <button className={"disabled:bg-slate-500 text-white bg-blue-400 px-4 py-2 mt-3 rounded-sm w-1/5"} onClick={
           () => {
            if (fetchForCollection) {
              fetchNFTsForCollection()
            }else fetchNFTs()
          }
        }>Let's go! </button>
      </div>
      <div className='flex flex-wrap gap-y-12 mt-4 w-5/6 gap-x-2 justify-center'>
        {
          NFTs.length && NFTs.map(nft => {
            return (
              <NFTCard nft={nft}></NFTCard>
            )
          })
        }
      </div>
    </div>
  )
}

export default Home
```

这是这段代码中发生的事情：

1. NFTCard 已导入文件顶部。
2. 在主页组件中，创建了一个新的 div，我们打开花括号，并使用条件渲染检查状态变量中是否存在 NFT。
3. 我们使用 map 函数迭代 NFT 数组并为每个 NFT 返回一个 NFTCard，将 NFT 本身作为 NFTCard 的 prop 传递。

太棒了，每次我们都会获取 NFT，并在 NFT 状态变量中存储一个数组

Next 现在将为每个 NFT 返回一张 NFT 卡，显示其信息。

要构建这样的 NFT 画廊或其他有趣的 web3 应用程序，