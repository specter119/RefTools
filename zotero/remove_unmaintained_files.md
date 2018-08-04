# 为 Zotero 删除无条目对应的文件链接附件

在 Zotero 中使用 Zotfile，当条目删除时，文件链接并不会一同删除。由于 Zotero 短时期并不关注这个需求，需要用户来自己实现。

> 本文中脚本从 `zotero.sqlite` 数据库读取文件链接路径，与本地实际存储的附件对比，删除数据库中不包含的附件，再删除由于删除附件后产生的新的空文件夹。

以下系统测试通过:

  - Windows10
  - MacOS 10.13.6
  - Manjaro 17.1.11

使用的 Python 均由 [miniconda](https://conda.io/miniconda.html) 安装，版本 3.6.6，**Python 2.x 无法使用**。
建议安装 [click](http://click.pocoo.org/5/)，否则**删除文件前不进行确认**，**关闭 Zotero 后才能运行该脚本**。

> 笔者强烈建议任何系统装 Python 直接上 minicoda。

```python
#!/usr/bin/env python
# coding: utf-8

from __future__ import print_function
import sys
import re
import configparser
from pathlib import Path
import shutil
import sqlite3

profile_dirs = {
    'darwin': Path.home() / 'Library/Application Support/Zotero',
    'linux': Path.home() / '.zotero/zotero',
    'linux2': Path.home() / '.zotero/zotero',
    'win32': Path.home() / 'AppData/Roaming/Zotero/Zotero'
}
profile_dir = profile_dirs[sys.platform]

config = configparser.ConfigParser()
config.read(profile_dir / 'profiles.ini')
configs_loc = profile_dir / config['Profile0']['Path'] / 'prefs.js'
configs = configs_loc.read_text()

zotfile_dest_pat = re.compile(r'user_pref\("extensions.zotfile.dest_dir",\ "(?P<zotfile_dest>.+)"\);')
zotfile_dest_dir = Path(zotfile_dest_pat.search(configs).group('zotfile_dest'))
zotero_data_pat = re.compile(r'user_pref\("extensions.zotero.dataDir",\ "(?P<zotero_data>.+)"\);')
zotero_data_dir = Path(zotero_data_pat.search(configs).group('zotero_data'))

attachments_local = set(p for p in zotfile_dest_dir.glob('**/*') if p.is_file() and p.name[0] != '.')
conn = sqlite3.connect('file:{}?mode=ro'.format(zotero_data_dir / 'zotero.sqlite'), uri=True)
c = conn.cursor()
c.execute('SELECT path FROM itemAttachments WHERE linkMode = 2')
attachments_zotero = set(zotfile_dest_dir / p[0].replace('attachments:', '', 1) for p in c.fetchall())
attachments_to_remove = attachments_local - attachments_zotero

try:
    import click
    print('\n'.join(
        ['The following files are no longer managed by zotero:', *['  {}'.format(p) for p in attachments_to_remove]]))
    if click.confirm('Do you want remove them?', default=True):
        [p.unlink() for p in attachments_to_remove]
        empty_dirs = [
            p for p in zotfile_dest_dir.glob('**/*')
            if (not p.is_file()) and (not len([f for f in list(p.iterdir()) if f.name[0] != '.']))
        ]
        [shutil.rmtree(p, ignore_errors=True) for p in empty_dirs]
except ImportError:
    print('\n'.join([
        'The following files are no longer managed by zotero will be removed:',
        *['  {}'.format(p) for p in attachments_to_remove]
    ]))
    [p.unlink() for p in attachments_to_remove]
    empty_dirs = [
        p for p in zotfile_dest_dir.glob('**/*')
        if (not p.is_file()) and (not len([f for f in list(p.iterdir()) if f.name[0] != '.']))
    ]
    [shutil.rmtree(p, ignore_errors=True) for p in empty_dirs]

```

以上脚本同时发布在 [GithubGist - zot_rm_unmaintained_files.py](https://gist.github.com/specter119/b79dc35a6091d0fd0896a9536fbddb5a)，如有bug或者功能上的补充，欢迎在gist上pr。同时欢迎文末留言，但如安装python和有了python如何安装click这类问题，还望自行搜索引擎。
