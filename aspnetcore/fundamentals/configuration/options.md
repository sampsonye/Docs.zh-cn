---
title: "在 ASP.NET Core 选项模式"
author: guardrex
description: "了解如何使用选项模式来表示 ASP.NET Core 应用中的相关设置的组。"
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="ea0a6-103">在 ASP.NET Core 选项模式</span><span class="sxs-lookup"><span data-stu-id="ea0a6-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="ea0a6-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ea0a6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ea0a6-105">选项模式使用选项类代表一组相关的设置。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="ea0a6-106">当配置设置按功能隔离到单独的选项类别时，应用程序遵循两个重要软件工程原则：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="ea0a6-107">[接口分隔原则 (ISP)](http://deviq.com/interface-segregation-principle/)： 依赖于配置设置的功能 （类） 仅取决于它们使用的配置设置。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="ea0a6-108">[关注点分离](http://deviq.com/separation-of-concerns/)： 应用程序的不同部分的设置不依赖或耦合到另一个。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="ea0a6-109">[查看或下载的示例代码](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)([如何下载](xref:tutorials/index#how-to-download-a-sample)) 此文章是更轻松地遵循与示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="ea0a6-110">基本选项配置</span><span class="sxs-lookup"><span data-stu-id="ea0a6-110">Basic options configuration</span></span>

<span data-ttu-id="ea0a6-111">作为示例演示了基本选项配置&num;1[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-112">非抽象类必须是选项类具有一个公共的无参数构造函数。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="ea0a6-113">下面的类， `MyOptions`，具有两个属性，`Option1`和`Option2`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="ea0a6-114">设置默认值是可选的但在以下示例中的类构造函数设置的默认值`Option1`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="ea0a6-115">`Option2`已通过直接初始化的属性设置了默认值 (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="ea0a6-116">`MyOptions`类添加到的服务容器[IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)并将其绑定到配置：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="ea0a6-117">以下页上的模型使用[构造函数依赖关系注入](xref:fundamentals/dependency-injection#what-is-dependency-injection)与[IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1)来访问这些设置 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="ea0a6-118">示例的*appsettings.json*文件指定的值`option1`和`option2`:</span><span class="sxs-lookup"><span data-stu-id="ea0a6-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="ea0a6-119">运行该应用程序，页模型`OnGet`方法返回显示选项类值的字符串：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="ea0a6-120">使用委托配置简单的选项</span><span class="sxs-lookup"><span data-stu-id="ea0a6-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="ea0a6-121">使用委托配置简单的选项的示例演示&num;2 英寸[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-122">使用委托来设置选项值。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-122">Use a delegate to set options values.</span></span> <span data-ttu-id="ea0a6-123">此示例应用程序使用`MyOptionsWithDelegateConfig`类 (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="ea0a6-124">在下面的代码中，第二个`IConfigureOptions<TOptions>`服务添加到服务容器。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ea0a6-125">它使用委托来配置的绑定`MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="ea0a6-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="ea0a6-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea0a6-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="ea0a6-127">你可以添加多个配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="ea0a6-128">配置提供程序 NuGet 包中。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="ea0a6-129">以便将它们注册要应用这些策略。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="ea0a6-130">每次调用[配置&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure)添加`IConfigureOptions<TOptions>`服务到服务容器。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="ea0a6-131">在前面的示例中，值`Option1`和`Option2`同时中指定*appsettings.json*，但值`Option1`和`Option2`重写了配置的委托。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="ea0a6-132">当启用多个配置服务时，指定上次的配置源*wins*和设置配置值。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="ea0a6-133">运行该应用程序，页模型`OnGet`方法返回显示选项类值的字符串：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="ea0a6-134">Suboptions 配置</span><span class="sxs-lookup"><span data-stu-id="ea0a6-134">Suboptions configuration</span></span>

<span data-ttu-id="ea0a6-135">作为示例演示了 suboptions 配置&num;中的 3[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-136">应用程序应创建适用于应用中的特定功能组 （类） 的选项类别。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="ea0a6-137">部分应用程序需要配置值仅应有权访问他们使用的配置值。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="ea0a6-138">当绑定到配置选项，在选项类型的每个属性绑定到窗体的配置键`property[:sub-property:]`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="ea0a6-139">例如，`MyOptions.Option1`属性绑定到键`Option1`，这从读取`option1`中的属性*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="ea0a6-140">在下面的代码中，第三个`IConfigureOptions<TOptions>`服务添加到服务容器。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="ea0a6-141">它将绑定`MySubOptions`的部分`subsection`的*appsettings.json*文件：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="ea0a6-142">`GetSection`扩展方法要求[Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="ea0a6-143">如果应用使用[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage，包将自动包含。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="ea0a6-144">示例的*appsettings.json*文件定义`subsection`具有键成员`suboption1`和`suboption2`:</span><span class="sxs-lookup"><span data-stu-id="ea0a6-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="ea0a6-145">`MySubOptions`类定义属性，属性`SubOption1`和`SubOption2`，以保留子选项值 (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="ea0a6-146">页模型`OnGet`方法返回与子选项值的字符串 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="ea0a6-147">当应用运行时，`OnGet`方法返回字符串，它显示类值的子选项：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="ea0a6-148">通过视图模型或直接视图注入与提供的选项</span><span class="sxs-lookup"><span data-stu-id="ea0a6-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="ea0a6-149">通过视图模型或直接视图注入与提供的选项的示例演示&num;4 英寸[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-150">可以提供选项，在视图模型或通过将注入`IOptions<TOptions>`直接到视图 (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="ea0a6-151">为直接注入注入`IOptions<MyOptions>`与`@inject`指令：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="ea0a6-152">当应用运行时，选项值显示在呈现的页面：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-152">When the app is run, the option values are shown in the rendered page:</span></span>

![选项值选项 1: value1_from_json 和选项 2： 从模型而注入视图加载为-1。](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="ea0a6-154">重新加载具有 IOptionsSnapshot 配置数据</span><span class="sxs-lookup"><span data-stu-id="ea0a6-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="ea0a6-155">重新加载配置数据与`IOptionsSnapshot`示例所示&num;5 中的[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-156">*需要 ASP.NET Core 1.1 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="ea0a6-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="ea0a6-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1)重新加载与最小的处理开销的选项的支持。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="ea0a6-158">ASP.NET 核心 1.1 中`IOptionsSnapshot`是快照[IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)和更新自动每当监视器触发的更改基于数据源更改。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="ea0a6-159">ASP.NET 核心 2.0 及更高版本，选项会计算一次每个请求时访问和缓存请求的生存期。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="ea0a6-160">下面的示例演示如何新`IOptionsSnapshot`后创建*appsettings.json*更改 (*Pages/Index.cshtml.cs*)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="ea0a6-161">对服务器的多个请求返回常量的值由提供*appsettings.json*文件之前文件更改和配置重新加载。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="ea0a6-162">下图显示了初始`option1`和`option2`值加载从*appsettings.json*文件：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="ea0a6-163">更改中的值*appsettings.json*文件为`value1_from_json UPDATED`和`200`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="ea0a6-164">保存*appsettings.json*文件。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="ea0a6-165">刷新浏览器以查看更新的选项值：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="ea0a6-166">名为 IConfigureNamedOptions 选项支持</span><span class="sxs-lookup"><span data-stu-id="ea0a6-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="ea0a6-167">名为选项支持[IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1)作为示例演示了&num;中的第 6[示例应用程序](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="ea0a6-168">*需要 ASP.NET Core 2.0 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="ea0a6-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="ea0a6-169">*名为选项*支持允许区分命名的选项配置此应用。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="ea0a6-170">在示例应用中，命名的选项使用声明[ConfigureNamedOptions&lt;TOptions&gt;。配置](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure)方法：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="ea0a6-171">示例应用程序访问的命名的选项[IOptionsSnapshot&lt;TOptions&gt;。获取](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get)(*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="ea0a6-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="ea0a6-172">运行示例应用程序，将返回的已命名的选项：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="ea0a6-173">`named_options_1`值提供从配置，其中加载从*appsettings.json*文件。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="ea0a6-174">`named_options_2`通过提供值：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="ea0a6-175">`named_options_2`委托中`ConfigureServices`为`Option1`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="ea0a6-176">默认值为`Option2`由`MyOptions`类。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="ea0a6-177">配置具有所有命名的选项实例[OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)方法。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="ea0a6-178">下面的代码将配置`Option1`所有名为具有公共值配置实例。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="ea0a6-179">手动添加以下代码`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="ea0a6-180">添加代码后运行示例应用程序产生以下结果：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="ea0a6-181">在 ASP.NET 核心 2.0 和更高版本，所有选项的都命名实例。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="ea0a6-182">现有`IConfigureOption`实例将被视为面向`Options.DefaultName`实例，即`string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="ea0a6-183">`IConfigureNamedOptions`此外实现`IConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="ea0a6-184">默认实现[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([引用源](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) 使用每个相应的逻辑。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="ea0a6-185">`null`命名的选项用于目标的所有命名实例而不是特定的命名实例 ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)和[PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)使用此约定)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="ea0a6-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="ea0a6-186">IPostConfigureOptions</span></span>

<span data-ttu-id="ea0a6-187">*需要 ASP.NET Core 2.0 或更高版本。*</span><span class="sxs-lookup"><span data-stu-id="ea0a6-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="ea0a6-188">设置与 postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="ea0a6-189">在所有运行 postconfiguration [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)进行配置：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ea0a6-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) ，可后配置名称的选项：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="ea0a6-191">使用[PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall)后配置所有名为配置实例：</span><span class="sxs-lookup"><span data-stu-id="ea0a6-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="ea0a6-192">选项工厂、 监视和缓存</span><span class="sxs-lookup"><span data-stu-id="ea0a6-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="ea0a6-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1)用于通知时`TOptions`实例更改。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="ea0a6-194">`IOptionsMonitor`支持 reloadable 选项更改通知，和`IPostConfigureOptions`。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="ea0a6-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 或更高版本) 负责，创建新选项实例。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="ea0a6-196">它具有单个[创建](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)方法。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="ea0a6-197">默认实现将所有已注册`IConfigureOptions`和`IPostConfigureOptions`并运行所有配置第一次后, 跟后配置。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="ea0a6-198">它区分`IConfigureNamedOptions`和`IConfigureOptions`且仅调用适当的接口。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="ea0a6-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 或更高版本) 由`IOptionsMonitor`缓存`TOptions`实例。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="ea0a6-200">`IOptionsMonitorCache`失效监视器中的选项实例，以便重新计算值 ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove))。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="ea0a6-201">值可以手动引入以及[TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd)。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="ea0a6-202">[清除](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)时应该按需重新创建所有的命名的实例使用方法。</span><span class="sxs-lookup"><span data-stu-id="ea0a6-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="ea0a6-203">请参阅</span><span class="sxs-lookup"><span data-stu-id="ea0a6-203">See also</span></span>

* [<span data-ttu-id="ea0a6-204">配置</span><span class="sxs-lookup"><span data-stu-id="ea0a6-204">Configuration</span></span>](xref:fundamentals/configuration/index)