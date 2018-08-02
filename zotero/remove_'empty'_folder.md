# 删除 storage 中的“空”文件

在使用 Zotfile 的过程中，会生成一些空文件夹留在 `<数据存储位置>/storage` 中，虽然留着不占空间且没有影响，但是还是有人有删除的需求。

> 应一位感兴趣的朋友的建议，原计划是准备写一个删除 Zotfile 删除条目后遗留的附件的脚本。但是原计划的脚本里需要sqlite数据库的知识，由于没接触过，所以先写了这个，比较简单，而且不同平台下 Zotero 配置文件的读取，删除 Zotfile 遗留附件的脚本里也会用得上。原计划的脚本，有时间再写。

以下系统测试通过:

  - Windows10
  - MacOS 10.13.6
  - Manjaro 17.1.11

使用的 Python 均由 [miniconda](https://conda.io/miniconda.html) 安装，版本 3.6.6，**未经过 python 2.x 任何版本测试**。

> 笔者强烈建议任何系统装 Python 直接上 minicoda。

建议安装 [click](http://click.pocoo.org/5/)，否则**删除文件夹前不进行确认**。

```python
#!/usr/bin/env python
# coding: utf-8

from __future__ import print_function
import re
import sys
import configparser
from pathlib import Path
import shutil

profile_dirs = {
    'darwin': Path.home() / 'Library' / 'Application Support' / 'Zotero',
    'linux': Path.home() / '.zotero' / 'zotero',
    'linux2': Path.home() / '.zotero' / 'zotero',
    'win32': Path.home() / 'AppData' / 'Roaming' / 'Zotero' / 'Zotero'
}
profile_dir = profile_dirs[sys.platform]
profile_loc = profile_dir / 'profiles.ini'
config = configparser.ConfigParser()
config.read(profile_loc)
configs_loc = profile_dir / config['Profile0']['Path'] / 'prefs.js'
configs = configs_loc.read_text()
zotero_data_pat = re.compile(r'user_pref\("extensions.zotero.dataDir",\ "(?P<zotero_data>.+)"\);')
zotero_data_dir = Path(zotero_data_pat.search(configs).group('zotero_data'))
storage_dir = zotero_data_dir / 'storage'
dirs_to_remove = [
    p for p in storage_dir.iterdir()
    if (not p.is_file()) and (not len([f for f in list(p.iterdir()) if f.name[0] != '.']))
]

try:
    import click
    print('\n'.join(['The following folders containing no attachments:', *['  {}'.format(p) for p in dirs_to_remove]]))
    if click.confirm('Do you want remove them?', default=True):
        [shutil.rmtree(p, ignore_errors=True) for p in dirs_to_remove]
except ImportError:
    print('\n'.join([
        'The following folders containing no attachments will be removed:', *['  {}'.format(p) for p in dirs_to_remove]
    ]))
    [shutil.rmtree(p, ignore_errors=True) for p in dirs_to_remove]
```

以上脚本我也发布在 [GithubGist - zot_rm_empty_folders.py](https://gist.github.com/specter119/0ec043c03d0d8cbe02e83842ee7b2766)，如果有bug或者功能上的补充，欢迎在gist上给我pr。同时欢迎文末留言，但比如安装python和有了python如何安装click这类问题，还望自行搜索引擎。
