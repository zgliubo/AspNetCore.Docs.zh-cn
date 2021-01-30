---
title: 为 ASP.NET Core Kestrel Web 服务器配置终结点
author: rick-anderson
description: 了解如何为 Kestrel（跨平台 ASP.NET Core Web 服务器）配置终结点。
monikerRange: '>= aspnetcore-5.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/04/2020
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/servers/kestrel/endpoints
ms.openlocfilehash: 780250feab456fa3eedee4e023c9bc774e748291
ms.sourcegitcommit: 063a06b644d3ade3c15ce00e72a758ec1187dd06
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253832"
---
# <a name="configure-endpoints-for-the-aspnet-core-kestrel-web-server"></a><span data-ttu-id="7ed42-103">为 ASP.NET Core Kestrel Web 服务器配置终结点</span><span class="sxs-lookup"><span data-stu-id="7ed42-103">Configure endpoints for the ASP.NET Core Kestrel web server</span></span>

<span data-ttu-id="7ed42-104">默认情况下，ASP.NET Core 绑定到：</span><span class="sxs-lookup"><span data-stu-id="7ed42-104">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="7ed42-105">`https://localhost:5001`（存在本地开发证书时）</span><span class="sxs-lookup"><span data-stu-id="7ed42-105">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="7ed42-106">使用以下内容指定 URL：</span><span class="sxs-lookup"><span data-stu-id="7ed42-106">Specify URLs using the:</span></span>

* <span data-ttu-id="7ed42-107">`ASPNETCORE_URLS` 环境变量。</span><span class="sxs-lookup"><span data-stu-id="7ed42-107">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="7ed42-108">`--urls` 命令行参数。</span><span class="sxs-lookup"><span data-stu-id="7ed42-108">`--urls` command-line argument.</span></span>
* <span data-ttu-id="7ed42-109">`urls` 主机配置键。</span><span class="sxs-lookup"><span data-stu-id="7ed42-109">`urls` host configuration key.</span></span>
* <span data-ttu-id="7ed42-110">`UseUrls` 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="7ed42-110">`UseUrls` extension method.</span></span>

<span data-ttu-id="7ed42-111">采用这些方法提供的值可以是一个或多个 HTTP 和 HTTPS 终结点（如果默认证书可用，则为 HTTPS）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-111">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="7ed42-112">将值配置为以分号分隔的列表（例如 `"Urls": "http://localhost:8000;http://localhost:8001"`）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-112">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="7ed42-113">有关这些方法的详细信息，请参阅[服务器 URL](xref:fundamentals/host/web-host#server-urls) 和[重写配置](xref:fundamentals/host/web-host#override-configuration)。</span><span class="sxs-lookup"><span data-stu-id="7ed42-113">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="7ed42-114">关于开发证书的创建：</span><span class="sxs-lookup"><span data-stu-id="7ed42-114">A development certificate is created:</span></span>

* <span data-ttu-id="7ed42-115">安装 [.NET SDK](/dotnet/core/sdk) 时。</span><span class="sxs-lookup"><span data-stu-id="7ed42-115">When the [.NET SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="7ed42-116">[dev-certs tool](xref:aspnetcore-2.1#https) 用于创建证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-116">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="7ed42-117">某些浏览器需要授予显式权限才能信任本地开发证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-117">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="7ed42-118">项目模板将应用配置为默认情况下在 HTTPS 上运行，并包括 [HTTPS 重定向和 HSTS 支持](xref:security/enforcing-ssl)。</span><span class="sxs-lookup"><span data-stu-id="7ed42-118">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="7ed42-119">调用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> 上的 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> 或 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> 方法以配置 URL 前缀和 Kestrel 的端口。</span><span class="sxs-lookup"><span data-stu-id="7ed42-119">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="7ed42-120">`UseUrls`、`--urls` 命令行参数、`urls` 主机配置键以及 `ASPNETCORE_URLS` 环境变量也有用，但具有本节后面注明的限制（必须要有可用于 HTTPS 终结点配置的默认证书）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-120">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="7ed42-121">`KestrelServerOptions` 配置：</span><span class="sxs-lookup"><span data-stu-id="7ed42-121">`KestrelServerOptions` configuration:</span></span>

## <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="7ed42-122">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="7ed42-122">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="7ed42-123">指定一个为每个指定的终结点运行的配置 `Action`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-123">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="7ed42-124">多次调用 `ConfigureEndpointDefaults`，用最新指定的 `Action` 替换之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-124">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

> [!NOTE]
> <span data-ttu-id="7ed42-125">通过在调用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults%2A> 之前调用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> 创建的终结点将不会应用默认值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-125">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureEndpointDefaults%2A> won't have the defaults applied.</span></span>

## <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="7ed42-126">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="7ed42-126">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="7ed42-127">指定一个为每个 HTTPS 终结点运行的配置 `Action`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-127">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="7ed42-128">多次调用 `ConfigureHttpsDefaults`，用最新指定的 `Action` 替换之前的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-128">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

> [!NOTE]
> <span data-ttu-id="7ed42-129">通过在调用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults%2A> 之前调用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> 创建的终结点将不会应用默认值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-129">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults%2A> won't have the defaults applied.</span></span>

## <a name="configureiconfiguration"></a><span data-ttu-id="7ed42-130">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="7ed42-130">Configure(IConfiguration)</span></span>

<span data-ttu-id="7ed42-131">创建配置加载程序，用于设置将 <xref:Microsoft.Extensions.Configuration.IConfiguration> 作为输入的 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="7ed42-131">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="7ed42-132">配置必须针对 Kestrel 的配置节。</span><span class="sxs-lookup"><span data-stu-id="7ed42-132">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="7ed42-133">`CreateDefaultBuilder` 在默认情况下调用 `Configure(context.Configuration.GetSection("Kestrel"))` 来加载 Kestrel 配置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-133">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },
      "Https": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    }
  }
}
```

## <a name="listenoptionsusehttps"></a><span data-ttu-id="7ed42-134">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="7ed42-134">ListenOptions.UseHttps</span></span>

<span data-ttu-id="7ed42-135">将 Kestrel 配置为使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7ed42-135">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="7ed42-136">`ListenOptions.UseHttps` 扩展：</span><span class="sxs-lookup"><span data-stu-id="7ed42-136">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="7ed42-137">`UseHttps`：将 Kestrel 配置为使用 HTTPS，采用默认证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-137">`UseHttps`: Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="7ed42-138">如果没有配置默认证书，则会引发异常。</span><span class="sxs-lookup"><span data-stu-id="7ed42-138">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="7ed42-139">`ListenOptions.UseHttps` 参数：</span><span class="sxs-lookup"><span data-stu-id="7ed42-139">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="7ed42-140">`filename` 是证书文件的路径和文件名，关联包含应用内容文件的目录。</span><span class="sxs-lookup"><span data-stu-id="7ed42-140">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="7ed42-141">`password` 是访问 X.509 证书数据所需的密码。</span><span class="sxs-lookup"><span data-stu-id="7ed42-141">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="7ed42-142">`configureOptions` 是配置 `HttpsConnectionAdapterOptions` 的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-142">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="7ed42-143">返回 `ListenOptions`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-143">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="7ed42-144">`storeName` 是从中加载证书的证书存储。</span><span class="sxs-lookup"><span data-stu-id="7ed42-144">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="7ed42-145">`subject` 是证书的主题名称。</span><span class="sxs-lookup"><span data-stu-id="7ed42-145">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="7ed42-146">`allowInvalid` 指示是否存在需要留意的无效证书，例如自签名证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-146">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="7ed42-147">`location` 是从中加载证书的存储位置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-147">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="7ed42-148">`serverCertificate` 是 X.509 证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-148">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="7ed42-149">在生产中，必须显式配置 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7ed42-149">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="7ed42-150">至少必须提供默认证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-150">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="7ed42-151">下面要描述的支持的配置：</span><span class="sxs-lookup"><span data-stu-id="7ed42-151">Supported configurations described next:</span></span>

* <span data-ttu-id="7ed42-152">无配置</span><span class="sxs-lookup"><span data-stu-id="7ed42-152">No configuration</span></span>
* <span data-ttu-id="7ed42-153">从配置中替换默认证书</span><span class="sxs-lookup"><span data-stu-id="7ed42-153">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="7ed42-154">更改代码中的默认值</span><span class="sxs-lookup"><span data-stu-id="7ed42-154">Change the defaults in code</span></span>

### <a name="no-configuration"></a><span data-ttu-id="7ed42-155">无配置</span><span class="sxs-lookup"><span data-stu-id="7ed42-155">No configuration</span></span>

<span data-ttu-id="7ed42-156">Kestrel 在 `http://localhost:5000` 和 `https://localhost:5001` 上进行侦听（如果默认证书可用）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-156">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

### <a name="replace-the-default-certificate-from-configuration"></a><span data-ttu-id="7ed42-157">从配置中替换默认证书</span><span class="sxs-lookup"><span data-stu-id="7ed42-157">Replace the default certificate from configuration</span></span>

<span data-ttu-id="7ed42-158">Kestrel 可以使用默认 HTTPS 应用设置配置架构。</span><span class="sxs-lookup"><span data-stu-id="7ed42-158">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="7ed42-159">从磁盘上的文件或从证书存储中配置多个终结点，包括要使用的 URL 和证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-159">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="7ed42-160">在以下 appsettings.json 示例中：</span><span class="sxs-lookup"><span data-stu-id="7ed42-160">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="7ed42-161">将 `AllowInvalid` 设置为 `true`，从而允许使用无效证书（例如自签名证书）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-161">Set `AllowInvalid` to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="7ed42-162">任何未指定证书的 HTTPS 终结点（下例中的 `HttpsDefaultCert`）会回退至在 `Certificates` > `Default` 下定义的证书或开发证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-162">Any HTTPS endpoint that doesn't specify a certificate (`HttpsDefaultCert` in the example that follows) falls back to the cert defined under `Certificates` > `Default` or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "Http": {
        "Url": "http://localhost:5000"
      },
      "HttpsInlineCertFile": {
        "Url": "https://localhost:5001",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      },
      "HttpsInlineCertStore": {
        "Url": "https://localhost:5002",
        "Certificate": {
          "Subject": "<subject; required>",
          "Store": "<certificate store; required>",
          "Location": "<location; defaults to CurrentUser>",
          "AllowInvalid": "<true or false; defaults to false>"
        }
      },
      "HttpsDefaultCert": {
        "Url": "https://localhost:5003"
      },
      "Https": {
        "Url": "https://*:5004",
        "Certificate": {
          "Path": "<path to .pfx file>",
          "Password": "<certificate password>"
        }
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="7ed42-163">对任何证书节点使用 `Path` 和 `Password` 的替代方法是使用证书存储字段来指定证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-163">An alternative to using `Path` and `Password` for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="7ed42-164">例如，可将 `Certificates` > `Default` 证书指定为：</span><span class="sxs-lookup"><span data-stu-id="7ed42-164">For example, the `Certificates` > `Default` certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="7ed42-165">架构的注意事项：</span><span class="sxs-lookup"><span data-stu-id="7ed42-165">Schema notes:</span></span>

* <span data-ttu-id="7ed42-166">终结点的名称不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="7ed42-166">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="7ed42-167">例如，`HTTPS` 和 `Https` 都是有效的。</span><span class="sxs-lookup"><span data-stu-id="7ed42-167">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="7ed42-168">每个终结点都要具备 `Url` 参数。</span><span class="sxs-lookup"><span data-stu-id="7ed42-168">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="7ed42-169">此参数的格式和顶层 `Urls` 配置参数一样，只不过它只能有单个值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-169">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="7ed42-170">这些终结点不会添加进顶层 `Urls` 配置中定义的终结点，而是替换它们。</span><span class="sxs-lookup"><span data-stu-id="7ed42-170">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="7ed42-171">通过 `Listen` 在代码中定义的终结点与在配置节中定义的终结点相累积。</span><span class="sxs-lookup"><span data-stu-id="7ed42-171">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="7ed42-172">`Certificate` 部分是可选的。</span><span class="sxs-lookup"><span data-stu-id="7ed42-172">The `Certificate` section is optional.</span></span> <span data-ttu-id="7ed42-173">如果为指定 `Certificate` 部分，则使用在之前的方案中定义的默认值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-173">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="7ed42-174">如果没有可用的默认值，服务器会引发异常且无法启动。</span><span class="sxs-lookup"><span data-stu-id="7ed42-174">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="7ed42-175">`Certificate` 节同时支持 `Path`&ndash;`Password` 和 `Subject`&ndash;`Store` 证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-175">The `Certificate` section supports both `Path`&ndash;`Password` and `Subject`&ndash;`Store` certificates.</span></span>
* <span data-ttu-id="7ed42-176">只要不会导致端口冲突，就能以这种方式定义任何数量的终结点。</span><span class="sxs-lookup"><span data-stu-id="7ed42-176">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="7ed42-177">`options.Configure(context.Configuration.GetSection("{SECTION}"))` 通过 `.Endpoint(string name, listenOptions => { })` 方法返回 `KestrelConfigurationLoader`，可以用于补充已配置的终结点设置：</span><span class="sxs-lookup"><span data-stu-id="7ed42-177">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="7ed42-178">可以直接访问 `KestrelServerOptions.ConfigurationLoader` 以继续迭代现有加载程序，例如由 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> 提供的加载程序。</span><span class="sxs-lookup"><span data-stu-id="7ed42-178">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>.</span></span>

* <span data-ttu-id="7ed42-179">每个终结点的配置节都可用于 `Endpoint` 方法中的选项，以便读取自定义设置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-179">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="7ed42-180">通过另一节再次调用 `options.Configure(context.Configuration.GetSection("{SECTION}"))` 可能加载多个配置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-180">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="7ed42-181">只使用最新配置，除非之前的实例上显式调用了 `Load`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-181">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="7ed42-182">元包不会调用 `Load`，所以可能会替换它的默认配置节。</span><span class="sxs-lookup"><span data-stu-id="7ed42-182">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="7ed42-183">`KestrelConfigurationLoader` 从 `KestrelServerOptions` 将 API 的 `Listen` 簇反射为 `Endpoint` 重载，因此可在同样的位置配置代码和配置终结点。</span><span class="sxs-lookup"><span data-stu-id="7ed42-183">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="7ed42-184">这些重载不使用名称，且只使用配置中的默认设置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-184">These overloads don't use names and only consume default settings from configuration.</span></span>

### <a name="change-the-defaults-in-code"></a><span data-ttu-id="7ed42-185">更改代码中的默认值</span><span class="sxs-lookup"><span data-stu-id="7ed42-185">Change the defaults in code</span></span>

<span data-ttu-id="7ed42-186">可以使用 `ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults` 更改 `ListenOptions` 和 `HttpsConnectionAdapterOptions` 的默认设置，包括重写之前的方案指定的默认证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-186">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="7ed42-187">需要在配置任何终结点之前调用 `ConfigureEndpointDefaults` 和 `ConfigureHttpsDefaults`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-187">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });

    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.SslProtocols = SslProtocols.Tls12;
    });
});
```

## <a name="configure-endpoints-using-server-name-indication"></a><span data-ttu-id="7ed42-188">使用服务器名称指示配置终结点</span><span class="sxs-lookup"><span data-stu-id="7ed42-188">Configure endpoints using Server Name Indication</span></span>

<span data-ttu-id="7ed42-189">[服务器名称指示 (SNI)](https://tools.ietf.org/html/rfc6066#section-3) 可用于承载相同 IP 地址和端口上的多个域。</span><span class="sxs-lookup"><span data-stu-id="7ed42-189">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="7ed42-190">为了运行 SNI，客户端在 TLS 握手过程中将进行安全会话的主机名发送至服务器，从而让服务器可以提供正确的证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-190">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="7ed42-191">在 TLS 握手后的安全会话期间，客户端将服务器提供的证书用于与服务器进行加密通信。</span><span class="sxs-lookup"><span data-stu-id="7ed42-191">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="7ed42-192">Kestrel 通过 `ServerCertificateSelector` 回调支持 SNI。</span><span class="sxs-lookup"><span data-stu-id="7ed42-192">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="7ed42-193">每次连接调用一次回调，从而允许应用检查主机名并选择合适的证书。</span><span class="sxs-lookup"><span data-stu-id="7ed42-193">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span> <span data-ttu-id="7ed42-194">可以在项目的 Program.cs 文件的 `ConfigureWebHostDefaults` 方法调用中使用以下回调代码：</span><span class="sxs-lookup"><span data-stu-id="7ed42-194">The following callback code can be used in the `ConfigureWebHostDefaults` method call of a project's *Program.cs* file:</span></span>

```csharp
//using System.Security.Cryptography.X509Certificates;
//using Microsoft.AspNetCore.Server.Kestrel.Https;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ListenAnyIP(5005, listenOptions =>
    {
        listenOptions.UseHttps(httpsOptions =>
        {
            var localhostCert = CertificateLoader.LoadFromStoreCert(
                "localhost", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var exampleCert = CertificateLoader.LoadFromStoreCert(
                "example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var subExampleCert = CertificateLoader.LoadFromStoreCert(
                "sub.example.com", "My", StoreLocation.CurrentUser,
                allowInvalid: true);
            var certs = new Dictionary<string, X509Certificate2>(StringComparer.OrdinalIgnoreCase)
            {
                { "localhost", localhostCert },
                { "example.com", exampleCert },
                { "sub.example.com", subExampleCert },
            };            

            httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
            {
                if (name != null && certs.TryGetValue(name, out var cert))
                {
                    return cert;
                }

                return exampleCert;
            };
        });
    });
});
```

<span data-ttu-id="7ed42-195">SNI 支持要求：</span><span class="sxs-lookup"><span data-stu-id="7ed42-195">SNI support requires:</span></span>

* <span data-ttu-id="7ed42-196">在目标框架 `netcoreapp2.1` 或更高版本上运行。</span><span class="sxs-lookup"><span data-stu-id="7ed42-196">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="7ed42-197">在 `net461` 或最高版本上，将调用回调，但是 `name` 始终为 `null`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-197">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="7ed42-198">如果客户端未在 TLS 握手过程中提供主机名参数，则 `name` 也为 `null`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-198">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="7ed42-199">所有网站在相同的 Kestrel 实例上运行。</span><span class="sxs-lookup"><span data-stu-id="7ed42-199">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="7ed42-200">Kestrel 在无反向代理时不支持跨多个实例共享一个 IP 地址和端口。</span><span class="sxs-lookup"><span data-stu-id="7ed42-200">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

## <a name="connection-logging"></a><span data-ttu-id="7ed42-201">连接日志记录</span><span class="sxs-lookup"><span data-stu-id="7ed42-201">Connection logging</span></span>

<span data-ttu-id="7ed42-202">调用 <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging%2A> 以发出用于进行连接上的字节级别通信的调试级别日志。</span><span class="sxs-lookup"><span data-stu-id="7ed42-202">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging%2A> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="7ed42-203">连接日志记录有助于排查低级通信中的问题，例如在 TLS 加密期间和代理后。</span><span class="sxs-lookup"><span data-stu-id="7ed42-203">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="7ed42-204">如果 `UseConnectionLogging` 放置在 `UseHttps` 之前，则会记录加密的流量。</span><span class="sxs-lookup"><span data-stu-id="7ed42-204">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="7ed42-205">如果 `UseConnectionLogging` 放置于 `UseHttps` 之后，则会记录解密的流量。</span><span class="sxs-lookup"><span data-stu-id="7ed42-205">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span> <span data-ttu-id="7ed42-206">这是内置[连接中间件](#connection-middleware)。</span><span class="sxs-lookup"><span data-stu-id="7ed42-206">This is built-in [Connection Middleware](#connection-middleware).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

## <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="7ed42-207">绑定到 TCP 套接字</span><span class="sxs-lookup"><span data-stu-id="7ed42-207">Bind to a TCP socket</span></span>

<span data-ttu-id="7ed42-208"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> 方法绑定至 TCP 套接字，且 options lambda 允许 X.509 证书配置：</span><span class="sxs-lookup"><span data-stu-id="7ed42-208">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen%2A> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](samples/5.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=12-18)]

<span data-ttu-id="7ed42-209">示例使用 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions> 为终结点配置 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7ed42-209">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="7ed42-210">可使用相同 API 为特定终结点配置其他 Kestrel 设置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-210">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

## <a name="bind-to-a-unix-socket"></a><span data-ttu-id="7ed42-211">绑定到 Unix 套接字</span><span class="sxs-lookup"><span data-stu-id="7ed42-211">Bind to a Unix socket</span></span>

<span data-ttu-id="7ed42-212">可通过 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> 侦听 Unix 套接字以提高 Nginx 的性能，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="7ed42-212">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](samples/5.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

* <span data-ttu-id="7ed42-213">在 Nginx 配置文件中，将 `server` > `location` > `proxy_pass` 条目设置为 `http://unix:/tmp/{KESTREL SOCKET}:/;`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-213">In the Nginx configuration file, set the `server` > `location` > `proxy_pass` entry to `http://unix:/tmp/{KESTREL SOCKET}:/;`.</span></span> <span data-ttu-id="7ed42-214">`{KESTREL SOCKET}` 是提供给 <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> 的套接字的名称（例如，上述示例中的 `kestrel-test.sock`）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-214">`{KESTREL SOCKET}` is the name of the socket provided to <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket%2A> (for example, `kestrel-test.sock` in the preceding example).</span></span>
* <span data-ttu-id="7ed42-215">确保套接字可由 Nginx （例如 `chmod go+w /tmp/kestrel-test.sock`）进行写入。</span><span class="sxs-lookup"><span data-stu-id="7ed42-215">Ensure that the socket is writeable by Nginx (for example, `chmod go+w /tmp/kestrel-test.sock`).</span></span>

## <a name="port-0"></a><span data-ttu-id="7ed42-216">端口 0</span><span class="sxs-lookup"><span data-stu-id="7ed42-216">Port 0</span></span>

<span data-ttu-id="7ed42-217">如果指定端口号 `0`，Kestrel 将动态绑定到可用端口。</span><span class="sxs-lookup"><span data-stu-id="7ed42-217">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="7ed42-218">以下示例演示如何确定 Kestrel 在运行时绑定到的端口：</span><span class="sxs-lookup"><span data-stu-id="7ed42-218">The following example shows how to determine which port Kestrel bound at runtime:</span></span>

[!code-csharp[](samples/5.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="7ed42-219">在应用运行时，控制台窗口输出指示可用于访问应用的动态端口：</span><span class="sxs-lookup"><span data-stu-id="7ed42-219">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

## <a name="limitations"></a><span data-ttu-id="7ed42-220">限制</span><span class="sxs-lookup"><span data-stu-id="7ed42-220">Limitations</span></span>

<span data-ttu-id="7ed42-221">使用以下方法配置终结点：</span><span class="sxs-lookup"><span data-stu-id="7ed42-221">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls%2A>
* <span data-ttu-id="7ed42-222">`--urls` 命令行参数</span><span class="sxs-lookup"><span data-stu-id="7ed42-222">`--urls` command-line argument</span></span>
* <span data-ttu-id="7ed42-223">`urls` 主机配置键</span><span class="sxs-lookup"><span data-stu-id="7ed42-223">`urls` host configuration key</span></span>
* <span data-ttu-id="7ed42-224">`ASPNETCORE_URLS` 环境变量</span><span class="sxs-lookup"><span data-stu-id="7ed42-224">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="7ed42-225">若要将代码用于 Kestrel 以外的服务器，这些方法非常有用。</span><span class="sxs-lookup"><span data-stu-id="7ed42-225">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="7ed42-226">不过，请注意以下限制：</span><span class="sxs-lookup"><span data-stu-id="7ed42-226">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="7ed42-227">HTTPS 无法与这些方法结合使用，除非在 HTTPS 终结点配置中提供了默认证书（例如，使用 `KestrelServerOptions` 配置或配置文件，如本文前面的部分所示）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-227">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this article).</span></span>
* <span data-ttu-id="7ed42-228">如果同时使用 `Listen` 和 `UseUrls` 方法，`Listen` 终结点将覆盖 `UseUrls` 终结点。</span><span class="sxs-lookup"><span data-stu-id="7ed42-228">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

## <a name="iis-endpoint-configuration"></a><span data-ttu-id="7ed42-229">IIS 终结点配置</span><span class="sxs-lookup"><span data-stu-id="7ed42-229">IIS endpoint configuration</span></span>

<span data-ttu-id="7ed42-230">使用 IIS 时，由 `Listen` 或 `UseUrls` 设置用于 IIS 覆盖绑定的 URL 绑定。</span><span class="sxs-lookup"><span data-stu-id="7ed42-230">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="7ed42-231">有关详细信息，请参阅 [ASP.NET Core 模块](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="7ed42-231">For more information, see [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="listenoptionsprotocols"></a><span data-ttu-id="7ed42-232">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="7ed42-232">ListenOptions.Protocols</span></span>

<span data-ttu-id="7ed42-233">`Protocols` 属性建立在连接终结点上或为服务器启用的 HTTP 协议（`HttpProtocols`）。</span><span class="sxs-lookup"><span data-stu-id="7ed42-233">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="7ed42-234">从 `HttpProtocols` 枚举向 `Protocols` 属性赋值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-234">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="7ed42-235">`HttpProtocols` 枚举值</span><span class="sxs-lookup"><span data-stu-id="7ed42-235">`HttpProtocols` enum value</span></span> | <span data-ttu-id="7ed42-236">允许的连接协议</span><span class="sxs-lookup"><span data-stu-id="7ed42-236">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="7ed42-237">仅 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="7ed42-237">HTTP/1.1 only.</span></span> <span data-ttu-id="7ed42-238">可以在具有 TLS 或没有 TLS 的情况下使用。</span><span class="sxs-lookup"><span data-stu-id="7ed42-238">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="7ed42-239">仅 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="7ed42-239">HTTP/2 only.</span></span> <span data-ttu-id="7ed42-240">仅当客户端支持[先验知识模式](https://tools.ietf.org/html/rfc7540#section-3.4)时，才可以在没有 TLS 的情况下使用。</span><span class="sxs-lookup"><span data-stu-id="7ed42-240">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="7ed42-241">HTTP/1.1 和 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="7ed42-241">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="7ed42-242">HTTP/2 要求客户端在 TLS [应用层协议协商 (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 握手过程中选择 HTTP/2；否则，连接默认为 HTTP/1.1。</span><span class="sxs-lookup"><span data-stu-id="7ed42-242">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="7ed42-243">任何终结点的默认 `ListenOptions.Protocols` 值都为 `HttpProtocols.Http1AndHttp2`。</span><span class="sxs-lookup"><span data-stu-id="7ed42-243">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="7ed42-244">HTTP/2 的 TLS 限制：</span><span class="sxs-lookup"><span data-stu-id="7ed42-244">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="7ed42-245">TLS 版本 1.2 或更高版本</span><span class="sxs-lookup"><span data-stu-id="7ed42-245">TLS version 1.2 or later</span></span>
* <span data-ttu-id="7ed42-246">重新协商已禁用</span><span class="sxs-lookup"><span data-stu-id="7ed42-246">Renegotiation disabled</span></span>
* <span data-ttu-id="7ed42-247">压缩已禁用</span><span class="sxs-lookup"><span data-stu-id="7ed42-247">Compression disabled</span></span>
* <span data-ttu-id="7ed42-248">最小的临时密钥交换大小：</span><span class="sxs-lookup"><span data-stu-id="7ed42-248">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="7ed42-249">椭圆曲线 Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;：最小 224 位</span><span class="sxs-lookup"><span data-stu-id="7ed42-249">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack;: 224 bits minimum</span></span>
  * <span data-ttu-id="7ed42-250">有限字段 Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;：最小 2048 位</span><span class="sxs-lookup"><span data-stu-id="7ed42-250">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack;: 2048 bits minimum</span></span>
* <span data-ttu-id="7ed42-251">不禁止密码套件。</span><span class="sxs-lookup"><span data-stu-id="7ed42-251">Cipher suite not prohibited.</span></span> 

<span data-ttu-id="7ed42-252">默认情况下，支持具有 P-256 椭圆曲线 &lbrack;`FIPS186`&rbrack; 的 `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack;。</span><span class="sxs-lookup"><span data-stu-id="7ed42-252">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="7ed42-253">以下示例允许端口 8000 上的 HTTP/1.1 和 HTTP/2 连接。</span><span class="sxs-lookup"><span data-stu-id="7ed42-253">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="7ed42-254">TLS 使用提供的证书来保护连接：</span><span class="sxs-lookup"><span data-stu-id="7ed42-254">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="7ed42-255">在 Linux 上，<xref:System.Net.Security.CipherSuitesPolicy> 可用于针对每个连接筛选 TLS 握手：</span><span class="sxs-lookup"><span data-stu-id="7ed42-255">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

## <a name="connection-middleware"></a><span data-ttu-id="7ed42-256">连接中间件</span><span class="sxs-lookup"><span data-stu-id="7ed42-256">Connection Middleware</span></span>

<span data-ttu-id="7ed42-257">必要时，自定义连接中间件可以按连接为特定密码筛选 TLS 握手。</span><span class="sxs-lookup"><span data-stu-id="7ed42-257">Custom connection middleware can filter TLS handshakes on a per-connection basis for specific ciphers if necessary.</span></span>

<span data-ttu-id="7ed42-258">下面的示例针对应用不支持的任何密码算法引发 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="7ed42-258">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="7ed42-259">或者，定义 [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) 并将其与可接受的密码套件列表进行比较。</span><span class="sxs-lookup"><span data-stu-id="7ed42-259">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="7ed42-260">没有哪种加密是使用 [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) 密码算法。</span><span class="sxs-lookup"><span data-stu-id="7ed42-260">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="7ed42-261">连接筛选也可以通过 <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda 进行配置：</span><span class="sxs-lookup"><span data-stu-id="7ed42-261">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

## <a name="set-the-http-protocol-from-configuration"></a><span data-ttu-id="7ed42-262">从配置中设置 HTTP 协议</span><span class="sxs-lookup"><span data-stu-id="7ed42-262">Set the HTTP protocol from configuration</span></span>

<span data-ttu-id="7ed42-263">`CreateDefaultBuilder` 在默认情况下调用 `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` 来加载 Kestrel 配置。</span><span class="sxs-lookup"><span data-stu-id="7ed42-263">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="7ed42-264">以下 appsettings.json 示例将 HTTP/1.1 建立为所有终结点的默认连接协议：</span><span class="sxs-lookup"><span data-stu-id="7ed42-264">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="7ed42-265">以下 appsettings.json 示例将为所有指定终结点建立 HTTP/1.1 连接协议：</span><span class="sxs-lookup"><span data-stu-id="7ed42-265">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="7ed42-266">代码中指定的协议覆盖了由配置设置的值。</span><span class="sxs-lookup"><span data-stu-id="7ed42-266">Protocols specified in code override values set by configuration.</span></span>

## <a name="url-prefixes"></a><span data-ttu-id="7ed42-267">URL 前缀</span><span class="sxs-lookup"><span data-stu-id="7ed42-267">URL prefixes</span></span>

<span data-ttu-id="7ed42-268">如果使用 `UseUrls`、`--urls` 命令行参数、`urls` 主机配置键或 `ASPNETCORE_URLS` 环境变量，URL 前缀可采用以下任意格式。</span><span class="sxs-lookup"><span data-stu-id="7ed42-268">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="7ed42-269">仅 HTTP URL 前缀是有效的。</span><span class="sxs-lookup"><span data-stu-id="7ed42-269">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="7ed42-270">使用 `UseUrls` 配置 URL 绑定时，Kestrel 不支持 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7ed42-270">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="7ed42-271">包含端口号的 IPv4 地址</span><span class="sxs-lookup"><span data-stu-id="7ed42-271">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="7ed42-272">`0.0.0.0` 是一种绑定到所有 IPv4 地址的特殊情况。</span><span class="sxs-lookup"><span data-stu-id="7ed42-272">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="7ed42-273">包含端口号的 IPv6 地址</span><span class="sxs-lookup"><span data-stu-id="7ed42-273">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="7ed42-274">`[::]` 是 IPv4 `0.0.0.0` 的 IPv6 等效项。</span><span class="sxs-lookup"><span data-stu-id="7ed42-274">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="7ed42-275">包含端口号的主机名</span><span class="sxs-lookup"><span data-stu-id="7ed42-275">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="7ed42-276">主机名、`*`和 `+` 并不特殊。</span><span class="sxs-lookup"><span data-stu-id="7ed42-276">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="7ed42-277">没有识别为有效 IP 地址或 `localhost` 的任何内容都将绑定到所有 IPv4 和 IPv6 IP。</span><span class="sxs-lookup"><span data-stu-id="7ed42-277">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="7ed42-278">若要将不同主机名绑定到相同端口上的不同 ASP.NET Core 应用，请使用 [HTTP.sys](xref:fundamentals/servers/httpsys) 或反向代理服务器。</span><span class="sxs-lookup"><span data-stu-id="7ed42-278">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server.</span></span> <span data-ttu-id="7ed42-279">反向代理服务器示例包括 IIS、Nginx 或 Apache。</span><span class="sxs-lookup"><span data-stu-id="7ed42-279">Reverse proxy server examples include IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="7ed42-280">采用反向代理配置进行托管需要[主机筛选](xref:fundamentals/servers/kestrel/host-filtering)。</span><span class="sxs-lookup"><span data-stu-id="7ed42-280">Hosting in a reverse proxy configuration requires [host filtering](xref:fundamentals/servers/kestrel/host-filtering).</span></span>

* <span data-ttu-id="7ed42-281">包含端口号的主机 `localhost` 名称或包含端口号的环回 IP</span><span class="sxs-lookup"><span data-stu-id="7ed42-281">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="7ed42-282">指定 `localhost` 后，Kestrel 将尝试绑定到 IPv4 和 IPv6 环回接口。</span><span class="sxs-lookup"><span data-stu-id="7ed42-282">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="7ed42-283">如果其他服务正在任一环回接口上使用请求的端口，则 Kestrel 将无法启动。</span><span class="sxs-lookup"><span data-stu-id="7ed42-283">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="7ed42-284">如果任一环回接口出于任何其他原因（通常是因为 IPv6 不受支持）而不可用，则 Kestrel 将记录一个警告。</span><span class="sxs-lookup"><span data-stu-id="7ed42-284">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>