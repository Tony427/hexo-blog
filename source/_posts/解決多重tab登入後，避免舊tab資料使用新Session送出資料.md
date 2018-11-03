---
title: 解決多重tab登入後，避免舊tab資料使用新Session送出資料
date: 2017-07-27 12:00:00
tags:
categories: C#
---

> 解決構想來自於MVC的AntiForgeryToken作法。
> 1. 登入時產生新的Guid，並存入Session
> 2. 前端產生hidden欄位存入Guid
> 3. post到後端時檢查目前Session中Guid與form post過來的Guid是否相符

如此一來不同Tab在post資料時，若與目前Session中GUID不相符，則進行後續導頁或登出動作

## HtmlHelper
`HtmlHelper`用來在前端使用Razor語法產生Token

```cs
using System.Web;
using System.Web.Mvc;

namespace SingleLogin.Web.Helpers
{
    public static class CustomHelpers
    {
        public static MvcHtmlString IsSingleLoginToken(this HtmlHelper helper)
        {
            string name = "SingleLoginToken"; //hidden欄位的name與id名稱
            string value = HttpContext.Current.Session["SingleLoginToken"].ToString(); //hidden的value
            return MvcHtmlString.Create(
                $"<input type=\"hidden\" id=\"{name}\" name=\"{name}\" value=\"{value}\">"
            );
        }
    }
}
```

使用方法如下：
1. 在WebConfig中加入namespace
    ```
    <namespaces>
        <add namespace="System.Web.Helpers" />
        <add namespace="System.Web.Mvc" />
        <add namespace="System.Web.Mvc.Ajax" />
        <add namespace="System.Web.Mvc.Html" />
        <add namespace="System.Web.Optimization" />
        <add namespace="System.Web.Routing" />
        <add namespace="System.Web.WebPages" />
        <add namespace="SingleLogin.Web.Helpers" />
    </namespaces>
    ```
2. View引入命名空間後使用`@Html.IsSingleLoginToken()`即可，使用方法跟AntiForgeryToken差不多

## ActionFilter
`Filter` 用來在後端Controller加上`[IsSingleLogin]`，用來驗證前端Post過來的Guid值
```cs
using System.Web.Mvc;

namespace SingleLogin.Web.ActionFilters
{
    /// <summary>
    /// 透過檢查埋入前端html的Token，比對後端Session中的token是否相同，以避免不同身分操作錯誤資料
    /// </summary>
    public class IsSingleLoginAttribute : ActionFilterAttribute
    {
        public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            string sessionVariableName = "SingleLoginToken";
            string clientVariableName = "SingleLoginToken";
            var serverSession = filterContext.HttpContext.Session[sessionVariableName].ToString();
            var clientSession = filterContext.HttpContext.Request.Form[clientVariableName].ToString();
            if (serverSession != clientSession)
            {
                filterContext.Result = new ContentResult()
                {
                    ContentType = "text/html; charset=UTF-8",
                    //用js alert後導頁登出
                    Content = "<script>alert('系統偵測到您同時以兩個帳號登入同一個瀏覽器');window.location.href = '/Member/Logout';</script>"
                };
            }
        }
    }
}
```

## 小結
Client端就可以用`@Html.IsSingleLoginToken()`跟Controller加上 `[IsSingleLogin]`tag就可以容易的加上驗證功能