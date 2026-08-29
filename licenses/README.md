# 许可证文档

镜像许可证使用 `{mirror-id}.mdx`，例如 `debian.mdx`。

GitHub Release 项目优先使用：

```text
github-release-{org}-{repo}.mdx
```

例如 `obsproject/obs-studio` 对应：

```text
github-release-obsproject-obs-studio.mdx
```

为兼容已有内容，也会尝试 `github-release-{repo}.mdx`。MDX 文件中可以直接写一段文本，也可以使用完整的 Markdown 标题、列表和链接；文件存在时，项目详情页会自动展示许可证区域，无需修改前端映射。
