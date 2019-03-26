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

import configparser
import re
import shutil
import sqlite3
import sys

try:
    from pathlib import Path
except ImportError:
    from pathlib2 import Path

if sys.version_info.major == 2:
    reload(sys)
    sys.setdefaultencoding('UTF8')


def get_zotfile_dest_and_zotero_data_dirs():
    profile_dirs = {
        'darwin': Path.home() / 'Library/Application Support/Zotero',
        'linux': Path.home() / '.zotero/zotero',
        'linux2': Path.home() / '.zotero/zotero',
        'win32': Path.home() / 'AppData/Roaming/Zotero/Zotero'
    }
    profile_dir = profile_dirs[sys.platform]

    config = configparser.ConfigParser()
    config.read('{}'.format(profile_dir / 'profiles.ini'))
    configs_loc = profile_dir / config['Profile0']['Path'] / 'prefs.js'
    configs = configs_loc.read_text()

    zotfile_dest_pat = re.compile(
        r'user_pref\("extensions.zotfile.dest_dir",\ "(?P<zotfile_dest>.+)"\);')
    zotfile_dest_dir = Path(
        zotfile_dest_pat.search(configs).group('zotfile_dest'))
    zotero_data_pat = re.compile(
        r'user_pref\("extensions.zotero.dataDir",\ "(?P<zotero_data>.+)"\);')
    zotero_data_dir = Path(zotero_data_pat.search(configs).group('zotero_data'))

    return zotfile_dest_dir, zotero_data_dir


def get_unmaintained_files(zotfile_dest_dir,
                           zotero_data_dir,
                           case_sensitive='auto'):
    attachments_local = set(p.as_posix() for p in zotfile_dest_dir.glob('**/*')
                            if p.is_file() and p.name[0] != '.')

    con = sqlite3.connect('{}'.format(zotero_data_dir / 'zotero.sqlite'))
    with con:
        cur = con.cursor()
        cur.execute('SELECT path FROM itemAttachments WHERE linkMode = 2')
        attachments_zotero = set([
            p.as_posix() for p in [
                zotfile_dest_dir / p[0].replace('attachments:', '', 1)
                for p in cur.fetchall()
            ]
        ])

    if sys.platform == 'darwin':
        import unicodedata
        attachments_zotero = set(
            list(attachments_zotero) +
            [unicodedata.normalize('NFD', p) for p in attachments_zotero])
    if case_sensitive == 'auto':
        case_sensitive = {
            'darwin': False,
            'linux': True,
            'linux2': True,
            'win32': False
        }[sys.platform]
    if not case_sensitive:
        attachments_local = set([fp.lower() for fp in attachments_local])
        attachments_zotero = set([fp.lower() for fp in attachments_zotero])
    attachments_to_remove = attachments_local - attachments_zotero

    return attachments_to_remove


def remove_unmaintained(attachments_to_remove):
    [p.unlink() for p in attachments_to_remove]
    empty_dirs = [
        p for p in zotfile_dest_dir.glob('**/*') if (not p.is_file()) and (
            not len([f for f in list(p.iterdir()) if f.name[0] != '.']))
    ]
    [shutil.rmtree('{}'.format(p), ignore_errors=True) for p in empty_dirs]


if __name__ == '__main__':
    zotfile_dest_dir, zotero_data_dir = get_zotfile_dest_and_zotero_data_dirs()
    attachments_to_remove = get_unmaintained_files(zotfile_dest_dir,
                                                   zotero_data_dir)

    try:
        import click
        print('The following files are no longer managed by Zotero:')
        print('\n'.join(['  {}'.format(p) for p in attachments_to_remove]))
        if click.confirm('Do you want remove them?', default=True):
            remove_unmaintained(attachments_to_remove)
    except ImportError:
        print(
            'The following files no longer managed by Zotero will be removed:')
        print('\n'.join(['  {}'.format(p) for p in attachments_to_remove]))
        remove_unmaintained(attachments_to_remove)

```

以上脚本同时发布在 [GithubGist - zot_rm_unmaintained_files.py](https://gist.github.com/specter119/b79dc35a6091d0fd0896a9536fbddb5a)，如有 bug 或者功能上的补充，欢迎在 gist 上 pr。同时欢迎文末留言，但如安装 python 和有了 python 如何安装 click 这类问题，还望自行搜索引擎。
