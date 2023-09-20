---
title: 页面和布局
description: 使用App 路由创建你的第一个页面和共享布局。
---

> 在开始之前，我们推荐您先阅读[路由基础](https://nextjs.org/docs/app/building-your-application/routing) 以及 [定义路由](https://nextjs.org/docs/app/building-your-application/routing/defining-routes) 这两篇文字.

Next.js 13 中的 App 路由引入了全新的文件约定，可轻松创建[页面](#pages)、[共享布局](#layouts), 和 [模板](#templates)。本页将指导您如何在 Next.js 应用程序中使用这些特殊的文件。

## 页面

一个页面是指与某个路由路径绑定的一个特定且**唯一**的用户界面。你可以通过从`page.js`文件中导出一个组件来定义页面。使用嵌套文件夹[定义路由]（https://nextjs.org/docs/app/building-your-application/routing/defining-routes）和`page.js`文件来公开访问路由。

在 `app` 目录中添加 `page.js` 文件，创建第一个页面：

<Image
  alt="page.js special file"
  srcLight="https://nextjs.org/_next/image?url=/docs/light/page-special-file.png&w=1920&q=75&dpl=dpl_3srhNWRVm1JHgkz4p8t12eqWjQJ7"
  srcDark="[/docs/dark/page-special-file.png](https://nextjs.org/_next/image?url=/docs/dark/page-special-file.png&w=1920&q=75&dpl=dpl_3srhNWRVm1JHgkz4p8t12eqWjQJ7)"
  width="1600"
  height="444"
/>

文件：`app/page.tsx`

```tsx filename="app/page.tsx" switcher
// `app/page.tsx` is the UI for the `/` URL
export default function Page() {
  return <h1>Hello, Home page!</h1>
}
```

```jsx filename="app/page.js" switcher
// `app/page.js` is the UI for the `/` URL
export default function Page() {
  return <h1>Hello, Home page!</h1>
}
```

文件：`app/dashboard/page.tsx`

```tsx filename="app/dashboard/page.tsx" switcher
// `app/dashboard/page.tsx` is the UI for the `/dashboard` URL
export default function Page() {
  return <h1>Hello, Dashboard Page!</h1>
}
```

```jsx filename="app/dashboard/page.js" switcher
// `app/dashboard/page.js` is the UI for the `/dashboard` URL
export default function Page() {
  return <h1>Hello, Dashboard Page!</h1>
}
```

> **要知道**:
>
> - 页面总是路由子树的叶子。页面总是[路由子树](https://nextjs.org/docs/app/building-your-application/routing#terminology)的[叶子节点](https://nextjs.org/docs/app/building-your-application/routing#terminology).
> - 页面可以使用 `.js`、 `.jsx` 或 `.tsx` file 文件扩展名
> - 公开访问路由段需要有一个 page.js 文件。
> - 页面默认是 [服务器组件](https://nextjs.org/docs/getting-started/react-essentials)，但也可以被设置成 [客户端组件](https://nextjs.org/docs/getting-started/react-essentials#client-components).
> - 页面可以获取数据. 请查看 [数据获取](https://nextjs.org/docs/app/building-your-application/data-fetching) 章节来了解更多信息。

## 布局

布局是在多个页面之间 **共享** 的用户页面。在导航时，布局会保留状态，保持交互性，并且不会重新渲染。布局也可以[嵌套](#nesting-layouts)。

你可以通过从 `layout.js` 文件中导出一个 React 组件来定义布局。该组件应接受一个 `children` prop，该 prop 将在呈现时填充子布局（如果存在）或子页面。

<Image
  alt="layout.js special file"
  srcLight="https://nextjs.org/_next/image?url=/docs/light/layout-special-file.png&w=1920&q=75&dpl=dpl_3srhNWRVm1JHgkz4p8t12eqWjQJ7"
  srcDark="https://nextjs.org/_next/image?url=/docs/light/layout-special-file.png&w=1920&q=75&dpl=dpl_3srhNWRVm1JHgkz4p8t12eqWjQJ7"
  width="1600"
  height="606"
/>

文件名 `app/dashboard/layout.tsx`

```tsx filename="app/dashboard/layout.tsx" switcher
export default function DashboardLayout({
  children, // will be a page or nested layout
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      {/* Include shared UI here e.g. a header or sidebar */}
      <nav></nav>

      {children}
    </section>
  )
}
```

文件名 `app/dashboard/layout.js`

```jsx filename="app/dashboard/layout.js" switcher
export default function DashboardLayout({
  children, // will be a page or nested layout
}) {
  return (
    <section>
      {/* Include shared UI here e.g. a header or sidebar */}
      <nav></nav>

      {children}
    </section>
  )
}
```

> **要知道**:
>
> - 最顶部的布局称为[根布局](#root-layout-required)。应用程序中的所有页面都共享这个必需布局。根布局必须包含 `html` 和 `body` 标记；
> - 任何的路由段都可以选择定义自己的 [布局](#nesting-layouts). 这些布局将在该段的所有页面中共享；
> - 默认情况下，路由中的布局是 **嵌套** 的。每个父布局都会使用 React `children` 属性来封装其下方的子布局；
> - 您可以使用[路由组（Route Groups）](/docs/app/building-your-application/routing/route-groups)将特定路由段选入或选出共享布局；
> - 布局默认为[服务器组件](/docs/getting-started/react-essentials)，但也可以设置为客户端组件。布局默认是 [服务器组件](/docs/getting-started/react-essentials) ，但也可以设置为 [客户端组件](/docs/getting-started/react-essentials#client-components).
> - 布局可以获取数据。请查看[数据获取](/docs/app/building-your-application/data-fetching)部分了解更多信息.
> - 父布局与其子布局之间无法传递数据。不过，您可以在路由中多次获取相同的数据，React 会[自动对请求进行去重处理](/docs/app/building-your-application/caching#request-memoization)，而不会影响性能。
> - 布局无法访问当前路由段。要访问路由段，您可以在客户端组件中使用 [useSelectedLayoutSegment](/docs/app/api-reference/functions/use-selected-layout-segment) 或 [useSelectedLayoutSegments](/docs/app/api-reference/functions/use-selected-layout-segments)。
> - 布局可以使用 .js、.jsx 或 .tsx 文件扩展名。
> - layout.js 和 page.js 文件可定义在同一文件夹中。布局将封装页面。

### 根布局 (必需)

根布局定义在 `app` 目录的顶层，适用于所有路由。通过该布局，您可以修改从服务器返回的初始 HTML。

```tsx filename="app/layout.tsx" switcher
// app/layout.tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

```jsx filename="app/layout.js" switcher
//app/layout.js
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

> **要知道**:
>
> - `app` 路径 **必须** 包含一个根布局;
> - 根布局必须定义 `<html>` 和 `<body>` 标记，因为 Next.js 不会自动创建它们。
> - 你可以使用 [内置的 SEO 支持](/docs/app/building-your-application/optimizing/metadata) 来管理 `<head>` HTML 元素，例如 `<title>` 元素。
> - 你可以使用 [路由组](/docs/app/building-your-application/routing/route-groups) 创建多个根布局。 请看这里的 [示例](/docs/app/building-your-application/routing/route-groups#creating-multiple-root-layouts)。
> - 根布局默认是[服务器组件](/docs/getting-started/react-essentials)，**不能** 设置为[客户端组件](/docs/getting-started/react-essentials#client-components)。

> **Migrating from the `pages` directory:** The root layout replaces the [`_app.js`](/docs/pages/building-your-application/routing/custom-app) and [`_document.js`](/docs/pages/building-your-application/routing/custom-document) files. [View the migration guide](/docs/app/building-your-application/upgrading/app-router-migration#migrating-_documentjs-and-_appjs).

### Nesting Layouts

Layouts defined inside a folder (e.g. `app/dashboard/layout.js`) apply to specific route segments (e.g. `acme.com/dashboard`) and render when those segments are active. By default, layouts in the file hierarchy are **nested**, which means they wrap child layouts via their `children` prop.

<Image
  alt="Nested Layout"
  srcLight="/docs/light/nested-layout.png"
  srcDark="/docs/dark/nested-layout.png"
  width="1600"
  height="606"
/>

```tsx filename="app/dashboard/layout.tsx" switcher
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return <section>{children}</section>
}
```

```jsx filename="app/dashboard/layout.js" switcher
export default function DashboardLayout({ children }) {
  return <section>{children}</section>
}
```

> **Good to know**:
>
> - Only the root layout can contain `<html>` and `<body>` tags.

If you were to combine the two layouts above, the root layout (`app/layout.js`) would wrap the dashboard layout (`app/dashboard/layout.js`), which would wrap route segments inside `app/dashboard/*`.

The two layouts would be nested as such:

<Image
  alt="Nested Layouts"
  srcLight="/docs/light/nested-layouts-ui.png"
  srcDark="/docs/dark/nested-layouts-ui.png"
  width="1600"
  height="1026"
/>

You can use [Route Groups](/docs/app/building-your-application/routing/route-groups) to opt specific route segments in and out of shared layouts.

## Templates

Templates are similar to layouts in that they wrap each child layout or page. Unlike layouts that persist across routes and maintain state, templates create a new instance for each of their children on navigation. This means that when a user navigates between routes that share a template, a new instance of the component is mounted, DOM elements are recreated, state is **not** preserved, and effects are re-synchronized.

There may be cases where you need those specific behaviors, and templates would be a more suitable option than layouts. For example:

- Enter/exit animations using CSS or animation libraries.
- Features that rely on `useEffect` (e.g logging page views) and `useState` (e.g a per-page feedback form).
- To change the default framework behavior. For example, Suspense Boundaries inside layouts only show the fallback the first time the Layout is loaded and not when switching pages. For templates, the fallback is shown on each navigation.

> **Recommendation:** We recommend using Layouts unless you have a specific reason to use Template.

A template can be defined by exporting a default React component from a `template.js` file. The component should accept a `children` prop which will be nested segments.

<Image
  alt="template.js special file"
  srcLight="/docs/light/template-special-file.png"
  srcDark="/docs/dark/template-special-file.png"
  width="1600"
  height="444"
/>

```tsx filename="app/template.tsx" switcher
export default function Template({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
```

```jsx filename="app/template.js" switcher
export default function Template({ children }) {
  return <div>{children}</div>
}
```

The rendered output of a route segment with a layout and a template will be as such:

```jsx filename="Output"
<Layout>
  {/* Note that the template is given a unique key. */}
  <Template key={routeParam}>{children}</Template>
</Layout>
```

## Modifying `<head>`

In the `app` directory, you can modify the `<head>` HTML elements such as `title` and `meta` using the [built-in SEO support](/docs/app/building-your-application/optimizing/metadata).

Metadata can be defined by exporting a [`metadata` object](/docs/app/api-reference/functions/generate-metadata#the-metadata-object) or [`generateMetadata` function](/docs/app/api-reference/functions/generate-metadata#generatemetadata-function) in a [`layout.js`](/docs/app/api-reference/file-conventions/layout) or [`page.js`](/docs/app/api-reference/file-conventions/page) file.

```tsx filename="app/page.tsx" switcher
import { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Next.js',
}

export default function Page() {
  return '...'
}
```

```jsx filename="app/page.js" switcher
export const metadata = {
  title: 'Next.js',
}

export default function Page() {
  return '...'
}
```

> **Good to know**: You should **not** manually add `<head>` tags such as `<title>` and `<meta>` to root layouts. Instead, you should use the [Metadata API](/docs/app/api-reference/functions/generate-metadata) which automatically handles advanced requirements such as streaming and de-duplicating `<head>` elements.

[Learn more about available metadata options in the API reference.](/docs/app/api-reference/functions/generate-metadata)
