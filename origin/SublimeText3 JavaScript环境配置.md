# SublimeText3 JavaScript代码自动完成

SublimeCodeIntel插件安装

pasta_ping


SublimeText3 下的自动完成是通过SublimeCodeIntel包来实现的

- 在SublimeText3下同时点击ctrl+shift+p，输入install，选择Install Package

- 输入SublimeCodeIntel，选择进行安装

- 打开SublimeCodeIntel配置文件，Preferences->Package Settings->SublimeCodeIntel->Settings-User

- 默认的User配置文件为空，需要从Default下拷贝过来

- 找到"JavaScript"字段，可以看到JQuery字样，换之

~~~
"JavaScript": {
    "codeintel_scan_extra_dir": [],
    "codeintel_scan_exclude_dir":["/build/", "/min/"],
    "codeintel_scan_files_in_project": false,
    "codeintel_max_recursive_dir_depth": 2,
    "codeintel_selected_catalogs": ["JavaScript"]
},
~~~