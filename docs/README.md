## docsify目录结构
```text
└── docs
    
    ├── _sidebar.md
    ├── _coverpage.md
    ├── README.md
    ├── .nojekyll
    ├──  guide.md
    └── blog
        ├── 使用服务器搭建Typecho博客.md
        ├── 使用hexo-github搭建免费个人博客.md
        └── ...
```

对应的访问页面将是:
```text
docs/README.md        => http://domain.com
docs/guide.md         => http://domain.com/guide
```


## 强调内容
适合显示重要的提示信息，语法为 `!> 内容`。
```markdown
!> 一段重要的内容，可以和其他 **Markdown** 语法混用。
```
!> 这是一段重要的内容。

## 普通提示
普通的提示信息，比如写 TODO 或者参考内容等。
```markdown
?> 完善示例
```
?> 完善示例