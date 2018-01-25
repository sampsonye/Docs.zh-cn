---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "添加的视图 (VB) |Microsoft 文档"
author: Rick-Anderson
description: "本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是一个 ASP.NET MVC Web 应用程序的基础知识..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a><span data-ttu-id="df8aa-103">添加的视图 (VB)</span><span class="sxs-lookup"><span data-stu-id="df8aa-103">Adding a View (VB)</span></span>
====================
<span data-ttu-id="df8aa-104">通过[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="df8aa-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="df8aa-105">本教程将教您构建使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，这是 Microsoft Visual Studio 的免费版本的 ASP.NET MVC Web 应用程序的基础知识。</span><span class="sxs-lookup"><span data-stu-id="df8aa-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="df8aa-106">在开始之前，请确保已安装下面列出的先决条件。</span><span class="sxs-lookup"><span data-stu-id="df8aa-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="df8aa-107">你可以通过单击以下链接安装所有这些： [Web 平台安装程序](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="df8aa-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="df8aa-108">或者，你可以单独安装系统必备组件，使用以下链接：</span><span class="sxs-lookup"><span data-stu-id="df8aa-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="df8aa-109">Visual Studio Web Developer Express SP1 系统必备</span><span class="sxs-lookup"><span data-stu-id="df8aa-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="df8aa-110">ASP.NET MVC 3 Tools 更新</span><span class="sxs-lookup"><span data-stu-id="df8aa-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="df8aa-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（运行时 + 工具支持）</span><span class="sxs-lookup"><span data-stu-id="df8aa-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="df8aa-112">如果你正在使用 Visual Studio 2010，而不 Visual Web Developer 2010，通过单击以下链接安装必备组件： [Visual Studio 2010 系统必备组件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="df8aa-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="df8aa-113">Visual Web Developer 项目与 VB.NET 的源代码是可以附带本主题。</span><span class="sxs-lookup"><span data-stu-id="df8aa-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="df8aa-114">[下载 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="df8aa-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="df8aa-115">如果你愿意 C#，切换到[C# 版本](../cs/adding-a-view.md)本教程。</span><span class="sxs-lookup"><span data-stu-id="df8aa-115">If you prefer C#, switch to the [C# version](../cs/adding-a-view.md) of this tutorial.</span></span>


<span data-ttu-id="df8aa-116">在本部分中我们将修改`HelloWorldController`类使用的视图模板文件，以便完全封装生成 HTML 响应客户端的过程。</span><span class="sxs-lookup"><span data-stu-id="df8aa-116">In this section we're going to modify the `HelloWorldController` class to use a view template file to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="df8aa-117">让我们开始使用具有的视图模板`Index`中的方法`HelloWorldController`类。</span><span class="sxs-lookup"><span data-stu-id="df8aa-117">Let's start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="df8aa-118">当前`Index`方法返回使用的是硬编码在控制器类中的消息的字符串。</span><span class="sxs-lookup"><span data-stu-id="df8aa-118">Currently the `Index` method returns a string with a message that is hard-coded within the controller class.</span></span> <span data-ttu-id="df8aa-119">更改`Index`方法以返回`View`对象，如在下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="df8aa-119">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

<span data-ttu-id="df8aa-120">让我们现在将视图模板添加到我们可以调用与我们项目`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="df8aa-120">Let's now add a view template to our project that we can invoke with the `Index` method.</span></span> <span data-ttu-id="df8aa-121">若要执行此操作，右键单击内`Index`方法，并单击**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="df8aa-121">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

<span data-ttu-id="df8aa-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-122">[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)</span></span>

<span data-ttu-id="df8aa-123">**添加视图**对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="df8aa-123">The **Add View** dialog box appears.</span></span> <span data-ttu-id="df8aa-124">保留默认条目，然后单击**添加**按钮。</span><span class="sxs-lookup"><span data-stu-id="df8aa-124">Leave the default entries and click the **Add** button.</span></span>

<span data-ttu-id="df8aa-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-125">[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)</span></span>

<span data-ttu-id="df8aa-126">*MvcMovie\Views\HelloWorld*文件夹和*MvcMovie\Views\HelloWorld\Index.vbhtml*创建文件。</span><span class="sxs-lookup"><span data-stu-id="df8aa-126">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.vbhtml* file are created.</span></span> <span data-ttu-id="df8aa-127">你可以看到在**解决方案资源管理器**:</span><span class="sxs-lookup"><span data-stu-id="df8aa-127">You can see them in **Solution Explorer**:</span></span>

<span data-ttu-id="df8aa-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-128">[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)</span></span>

<span data-ttu-id="df8aa-129">添加一些 HTML 下的`<h2>`标记。</span><span class="sxs-lookup"><span data-stu-id="df8aa-129">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="df8aa-130">已修改*MvcMovie\Views\HelloWorld\Index.vbhtml*文件如下所示。</span><span class="sxs-lookup"><span data-stu-id="df8aa-130">The modified *MvcMovie\Views\HelloWorld\Index.vbhtml* file is shown below.</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

<span data-ttu-id="df8aa-131">运行应用程序，并浏览到&quot;你好 world&quot;控制器 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="df8aa-131">Run the application and browse to the &quot;hello world&quot; controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="df8aa-132">`Index`方法在你的控制器中未做大量的工作; 它只需运行该语句`return View()`，这指示我们想要使用的视图模板文件来呈现对客户端的响应。</span><span class="sxs-lookup"><span data-stu-id="df8aa-132">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which indicated that we wanted to use a view template file to render a response to the client.</span></span> <span data-ttu-id="df8aa-133">ASP.NET MVC 因为我们未显式指定要使用的视图模板文件的名称，默认为使用*Index.vbhtml*视图文件中的*\Views\HelloWorld*文件夹。</span><span class="sxs-lookup"><span data-stu-id="df8aa-133">Because we did not explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.vbhtml* view file within the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="df8aa-134">下图显示了硬编码在视图中的字符串。</span><span class="sxs-lookup"><span data-stu-id="df8aa-134">The image below shows the string hard-coded in the view.</span></span>

<span data-ttu-id="df8aa-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-135">[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="df8aa-136">看起来一切都非常好。</span><span class="sxs-lookup"><span data-stu-id="df8aa-136">Looks pretty good.</span></span> <span data-ttu-id="df8aa-137">但请注意浏览器的标题栏将显示&quot;索引&quot;，并在页上的大标题显示&quot;我的 MVC 应用程序。&quot;让我们更改那些。</span><span class="sxs-lookup"><span data-stu-id="df8aa-137">However, notice that the browser's title bar says &quot;Index&quot; and the big title on the page says &quot;My MVC Application.&quot; Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="df8aa-138">更改视图和布局页面</span><span class="sxs-lookup"><span data-stu-id="df8aa-138">Changing views and layout pages</span></span>

<span data-ttu-id="df8aa-139">首先，让我们更改文本&quot;我的 MVC 应用程序。&quot;该文本共享，并显示在每一页。</span><span class="sxs-lookup"><span data-stu-id="df8aa-139">First, let's change the text &quot;My MVC Application.&quot; That text is shared and appears on every page.</span></span> <span data-ttu-id="df8aa-140">它实际出现在只有一个位置中我们项目中，即使它位于我们的应用程序中每页上。</span><span class="sxs-lookup"><span data-stu-id="df8aa-140">It actually appears in only one place in our project, even though it's on every page in our application.</span></span> <span data-ttu-id="df8aa-141">转到*/视图/共享*文件夹中的**解决方案资源管理器**并打开 *\_Layout.vbhtml*文件。</span><span class="sxs-lookup"><span data-stu-id="df8aa-141">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.vbhtml* file.</span></span> <span data-ttu-id="df8aa-142">此文件称为布局页，它是共享&quot;shell&quot;其他所有页使用。</span><span class="sxs-lookup"><span data-stu-id="df8aa-142">This file is called a layout page and it's the shared &quot;shell&quot; that all other pages use.</span></span>

<span data-ttu-id="df8aa-143">请注意`@RenderBody()`的文件的底部附近的代码行。</span><span class="sxs-lookup"><span data-stu-id="df8aa-143">Note the `@RenderBody()` line of code near the bottom of the file.</span></span> <span data-ttu-id="df8aa-144">`RenderBody`是你创建的所有页面，都会都显示其中一个占位符&quot;包装&quot;布局页中。</span><span class="sxs-lookup"><span data-stu-id="df8aa-144">`RenderBody` is a placeholder where all the pages you create show up, &quot;wrapped&quot; in the layout page.</span></span> <span data-ttu-id="df8aa-145">更改`<h1>`标题从 **&quot;**  My MVC Application&quot;到&quot;MVC 影片应用程序&quot;。</span><span class="sxs-lookup"><span data-stu-id="df8aa-145">Change the `<h1>` heading from **&quot;** My MVC Application&quot; to &quot;MVC Movie App&quot;.</span></span>

[!code-html[Main](adding-a-view/samples/sample3.html)]

<span data-ttu-id="df8aa-146">运行应用程序，并记下它现在显示&quot;MVC 影片应用程序&quot;。</span><span class="sxs-lookup"><span data-stu-id="df8aa-146">Run the application and note it now says &quot;MVC Movie App&quot;.</span></span> <span data-ttu-id="df8aa-147">单击**有关**链接，并且页面显示了&quot;MVC 影片应用程序&quot;也。</span><span class="sxs-lookup"><span data-stu-id="df8aa-147">Click the **About** link, and that page shows &quot;MVC Movie App&quot;, too.</span></span>

<span data-ttu-id="df8aa-148">完整 *\_Layout.vbhtml*文件如下所示：</span><span class="sxs-lookup"><span data-stu-id="df8aa-148">The complete *\_Layout.vbhtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="df8aa-149">现在，让我们更改索引页 （视图） 的标题。</span><span class="sxs-lookup"><span data-stu-id="df8aa-149">Now, let's change the title of the Index page (view).</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

<span data-ttu-id="df8aa-150">打开*MvcMovie\Views\HelloWorld\Index.vbhtml*。</span><span class="sxs-lookup"><span data-stu-id="df8aa-150">Open *MvcMovie\Views\HelloWorld\Index.vbhtml*.</span></span> <span data-ttu-id="df8aa-151">有两个位置进行更改： 首先，文本显示的标题中的浏览器中，然后在辅助标头 (`<h2>`元素)。</span><span class="sxs-lookup"><span data-stu-id="df8aa-151">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="df8aa-152">我们将使它们略有不同以便你可以查看代码的哪些位更改应用程序的哪个部分。</span><span class="sxs-lookup"><span data-stu-id="df8aa-152">We'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

<span data-ttu-id="df8aa-153">运行应用程序，并浏览到`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="df8aa-153">Run the application and browse to`http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="df8aa-154">请注意，浏览器标题、主标题和辅助标题已更改。</span><span class="sxs-lookup"><span data-stu-id="df8aa-154">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="df8aa-155">很容易地对视图进行少量更改将应用程序中的大更改。</span><span class="sxs-lookup"><span data-stu-id="df8aa-155">It's easy to make big changes in your application with small changes to a view.</span></span> <span data-ttu-id="df8aa-156">（如果没有在浏览器中看到更改，则可能正在查看缓存的内容。</span><span class="sxs-lookup"><span data-stu-id="df8aa-156">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="df8aa-157">在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。）</span><span class="sxs-lookup"><span data-stu-id="df8aa-157">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="df8aa-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-158">[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)</span></span>

<span data-ttu-id="df8aa-159">我们一小段&quot;数据&quot;(在这种情况下&quot;Hello World ！&quot;消息) 但是硬编码。</span><span class="sxs-lookup"><span data-stu-id="df8aa-159">Our little bit of &quot;data&quot; (in this case the &quot;Hello World!&quot; message) is hard-coded, though.</span></span> <span data-ttu-id="df8aa-160">我们 MVC 应用程序具有 V (views)，我们已经有了 C （控制器），但尚没有的 M （模型）。</span><span class="sxs-lookup"><span data-stu-id="df8aa-160">Our MVC application has V (views) and we've got C (controllers), but no M (model) yet.</span></span> <span data-ttu-id="df8aa-161">很快，我们将演练如何创建数据库并从它检索模型数据。</span><span class="sxs-lookup"><span data-stu-id="df8aa-161">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="df8aa-162">将数据从控制器传递给视图</span><span class="sxs-lookup"><span data-stu-id="df8aa-162">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="df8aa-163">我们转到数据库，并讨论模型之前，不过，让我们先介绍一下将从控制器的信息传递给视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-163">Before we go to a database and talk about models, though, let's first talk about passing information from the Controller to a View.</span></span> <span data-ttu-id="df8aa-164">我们要将传递以呈现给客户端 HTML 响应所需的视图模板。</span><span class="sxs-lookup"><span data-stu-id="df8aa-164">We want to pass what a view template requires in order to render an HTML response to a client.</span></span> <span data-ttu-id="df8aa-165">这些对象是通常创建了控制器类由传递给视图模板和它们应包含视图模板所需的数据-不多。</span><span class="sxs-lookup"><span data-stu-id="df8aa-165">These objects are typically created and passed by a controller class to a view template, and they should contain only the data that the view template requires — and no more.</span></span>

<span data-ttu-id="df8aa-166">以前与`HelloWorldController`类，`Welcome`操作方法所花`name`和`numTimes`参数，然后使用参数值，向浏览器的输出。</span><span class="sxs-lookup"><span data-stu-id="df8aa-166">Previously with the `HelloWorldController` class, the `Welcome` action method took a `name` and a `numTimes` parameter and then output the parameter values to the browser.</span></span> <span data-ttu-id="df8aa-167">而是必须继续直接呈现此响应的控制器，让我们改为我们将将放该数据在一个包视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-167">Rather than have the controller continue to render this response directly, let's instead we'll put that data in a bag for the View.</span></span> <span data-ttu-id="df8aa-168">可以使用控制器和视图`ViewBag`对象来存放该数据。</span><span class="sxs-lookup"><span data-stu-id="df8aa-168">Controllers and Views can use a `ViewBag` object to hold that data.</span></span> <span data-ttu-id="df8aa-169">将传递到一个视图模板自动，并用于呈现 HTML 响应作为数据使用的包内容。</span><span class="sxs-lookup"><span data-stu-id="df8aa-169">That will be passed over to a view template automatically, and used to render the HTML response using the contents of the bag as data.</span></span> <span data-ttu-id="df8aa-170">控制器致力于一件事和与另一个的视图模板通过这种方式 — 使我们能够维护干净&quot;关注点分离&quot;应用程序中。</span><span class="sxs-lookup"><span data-stu-id="df8aa-170">That way the controller is concerned with one thing and the view template with another — enabling us to maintain clean &quot;separation of concerns&quot; within the application.</span></span>

<span data-ttu-id="df8aa-171">或者，我们无法定义自定义的类、 然后我们自己在创建该对象的实例、 填充数据和将其传递给该视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-171">Alternatively, we could define a custom class, then create an instance of that object on our own, fill it with data and pass it to the View.</span></span> <span data-ttu-id="df8aa-172">情况通常称作视图模型，因为它是自定义模型的视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-172">That is often called a ViewModel, because it's a custom Model for the View.</span></span> <span data-ttu-id="df8aa-173">对于少量的数据，但是，ViewBag 非常适用。</span><span class="sxs-lookup"><span data-stu-id="df8aa-173">For small amounts of data, however, the ViewBag works great.</span></span>

<span data-ttu-id="df8aa-174">返回到*HelloWorldController.vb*文件更改`Welcome`中将该消息和 NumTimes 放入 ViewBag 控制器方法。</span><span class="sxs-lookup"><span data-stu-id="df8aa-174">Return to the *HelloWorldController.vb* file change the `Welcome` method inside the controller to put the Message and NumTimes into the ViewBag.</span></span> <span data-ttu-id="df8aa-175">ViewBag 是动态对象。</span><span class="sxs-lookup"><span data-stu-id="df8aa-175">The ViewBag is a dynamic object.</span></span> <span data-ttu-id="df8aa-176">这意味着你可以任何你想要将放置到它。</span><span class="sxs-lookup"><span data-stu-id="df8aa-176">That means you can put whatever you want in to it.</span></span> <span data-ttu-id="df8aa-177">ViewBag 一直输入其内部的一些内容，将任何定义的属性。</span><span class="sxs-lookup"><span data-stu-id="df8aa-177">The ViewBag has no defined properties until you put something inside it.</span></span>

<span data-ttu-id="df8aa-178">完整`HelloWorldController.vb`与相同的文件中的新类。</span><span class="sxs-lookup"><span data-stu-id="df8aa-178">The complete `HelloWorldController.vb` with the new class in the same file.</span></span>

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

<span data-ttu-id="df8aa-179">现在我们 ViewBag 包含将通过传递到视图自动的数据。</span><span class="sxs-lookup"><span data-stu-id="df8aa-179">Now our ViewBag contains data that will be passed over to the View automatically.</span></span> <span data-ttu-id="df8aa-180">同样，或者我们无法具有传递如下我们自己对象中如果我们喜欢：</span><span class="sxs-lookup"><span data-stu-id="df8aa-180">Again, alternatively we could have passed in our own object like this if we liked:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

<span data-ttu-id="df8aa-181">现在我们需要`WelcomeView`模板 ！</span><span class="sxs-lookup"><span data-stu-id="df8aa-181">Now we need a `WelcomeView` template!</span></span> <span data-ttu-id="df8aa-182">运行应用程序，因此编译新代码。</span><span class="sxs-lookup"><span data-stu-id="df8aa-182">Run the application so the new code is compiled.</span></span> <span data-ttu-id="df8aa-183">关闭浏览器中，右键单击内`Welcome`方法，，然后单击**添加视图**。</span><span class="sxs-lookup"><span data-stu-id="df8aa-183">Close the browser, right-click inside the `Welcome` method, and then click **Add View**.</span></span>

<span data-ttu-id="df8aa-184">下面是你**添加视图**对话框如下所示。</span><span class="sxs-lookup"><span data-stu-id="df8aa-184">Here's what your **Add View** dialog box looks like.</span></span>

<span data-ttu-id="df8aa-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-185">[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="df8aa-186">添加以下代码在`<h2>`在新的元素*欢迎。*vbhtml 文件。</span><span class="sxs-lookup"><span data-stu-id="df8aa-186">Add the following code under the `<h2>` element in the new *Welcome.*vbhtml file.</span></span> <span data-ttu-id="df8aa-187">我们将进行循环，说&quot;Hello&quot;让用户说出我们应多次 ！</span><span class="sxs-lookup"><span data-stu-id="df8aa-187">We'll make a loop and say &quot;Hello&quot; as many times as the user says we should!</span></span>

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

<span data-ttu-id="df8aa-188">运行应用程序，并浏览到`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span><span class="sxs-lookup"><span data-stu-id="df8aa-188">Run the application and browse to `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`</span></span>

<span data-ttu-id="df8aa-189">现在数据是来自 URL，并自动传递到的控制器。</span><span class="sxs-lookup"><span data-stu-id="df8aa-189">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="df8aa-190">控制器包将数据分成`Model`对象并将该对象传递到视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-190">The controller packages up the data into a `Model` object and passes that object to the view.</span></span> <span data-ttu-id="df8aa-191">不是向用户显示以 html 格式的数据视图。</span><span class="sxs-lookup"><span data-stu-id="df8aa-191">The view than displays the data as HTML to the user.</span></span>

<span data-ttu-id="df8aa-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="df8aa-192">[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)</span></span>

<span data-ttu-id="df8aa-193">当然，这是一种类型的&quot;M&quot;模型，但不数据库类型。</span><span class="sxs-lookup"><span data-stu-id="df8aa-193">Well, that was a kind of an &quot;M&quot; for model, but not the database kind.</span></span> <span data-ttu-id="df8aa-194">让我们用学到的内容来创建一个电影数据库。</span><span class="sxs-lookup"><span data-stu-id="df8aa-194">Let's take what we've learned and create a database of movies.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="df8aa-195">[上一页](adding-a-controller.md)
[下一页](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="df8aa-195">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>