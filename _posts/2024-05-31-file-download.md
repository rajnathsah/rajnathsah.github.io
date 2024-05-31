---
title: Download large files faster and unzip it
date: 2024-05-31 00:00:00 +0000
excerpt: "Download file faster using python and unzip it."
classes: # wide
tags: [Python, File Download, Unzip]
toc: true
toc_sticky: true
---
## File Download
Downloading file from internet can be time consuming work, especially when file to be downloaded are large files. During my project work i come across a situation where i had to download some files from internet and unzip it. Using request library with normal approach it was taking hours. After lot of search i found an arcticle which helped me downloading file faster.

### File Download Code

```python
import asyncio
import os.path
import shutil
from random import randint
from time import sleep
import random
import time

import aiofiles
import aiohttp
from urllib.parse import urlparse
from tempfile import TemporaryDirectory


# File Download class
class FileDownloader:
    def __init__(self, urls, filename, download_path):
        self.urls = urls
        self.filename = filename
        self.download_path = download_path
        self.fast_download = None

    async def get_content_length(self, url):
        async with aiohttp.ClientSession() as session:
            async with session.head(url) as request:
                return request.content_length


    def parts_generator(self, size, start=0, part_size=50 * 1024 ** 2):
        while size - start > part_size:
            yield start, start + part_size
            start += part_size
        yield start, size


    async def download(self, url, headers, save_path, attempt=0):
        # attempt = 0
        max_attempts = 10
        timeout = aiohttp.ClientTimeout(total=6000)
        try:
            async with aiohttp.ClientSession(headers=headers, timeout=timeout) as session:
                await asyncio.sleep(1 + random.randint(0, 9))
                async with session.get(url) as request:
                    file = await aiofiles.open(save_path, 'wb')
                    await file.write(await request.content.read())
        except (aiohttp.ClientOSError,
        aiohttp.ServerDisconnectedError):
            if attempt < max_attempts:
                print(f'Attempt {attempt} failed, trying again')
                await asyncio.sleep(3 + random.randint(0, 9))
                await self.download(url, headers, save_path, attempt + 1)
            else:
                raise


    async def process(self, url):
        filename = os.path.basename(urlparse(url).path)
        # tmp_dir = TemporaryDirectory(prefix=filename, dir=os.path.abspath('.'), ignore_cleanup_errors=False)
        tmp_dir = TemporaryDirectory(prefix=filename, dir=self.download_path)
        size = await self.get_content_length(url)
        tasks = []
        file_parts = []
        for number, sizes in enumerate(self.parts_generator(size)):
            part_file_name = os.path.join(tmp_dir.name, f'{filename}.part{number}')
            file_parts.append(part_file_name)            
            tasks.append(self.download(url, {'Range': f'bytes={sizes[0]}-{sizes[1]-1}'}, part_file_name))
        await asyncio.gather(*tasks)
        download_filename = os.path.join(self.download_path, filename)

        # delete if there is existing zip file
        if os.path.exists(download_filename):
            os.remove(download_filename)

        with open(download_filename, 'wb') as wfd:
            for f in file_parts:
                with open(f, 'rb') as fd:
                    shutil.copyfileobj(fd, wfd)

    def download_file(self):
        print('slow download')
        download_file = os.path.join(self.download_path, self.filename)
        with requests.get(self.urls[0], stream=True) as r:
            r.raise_for_status()
            with open(download_file, 'wb') as f:
                for chunk in r.iter_content(chunk_size=8192):
                    f.write(chunk) 


    async def main(self, fast_download=True):
        if fast_download:
            await asyncio.gather(*[self.process(url) for url in self.urls])
        else:
            self.download_file()

if __name__ == '__main__':    
    url = "<file url>"
    start_code = time.monotonic()
    loop = asyncio.get_event_loop()    
    fileDownloader = FileDownloader(urls=[url], filename='file.txt.gz', download_path='/tmp')
    loop.run_until_complete(fileDownloader.main())
    print(f'{time.monotonic() - start_code} seconds!')
```

### File Unzip Code

```python
import os
import gzip
import time

# file unzip class
class FileUnzip:
    def __init__(self, filename, file_path, unzip_path):
        self.filename = filename
        self.file_path = file_path
        self.unzip_path = unzip_path
        self.chunk_size = 100*1024*1024 # 100MB

    def unzip(self):
        zip_file = os.path.join(self.file_path, self.filename)
        unzip_file = os.path.join(self.unzip_path, self.filename.replace('.gz', ''))

        # delete if there is existing zip file
        if os.path.exists(unzip_file):
            os.remove(unzip_file)

        with gzip.open(zip_file, 'rb') as f_in:
            with open(unzip_file, 'wb') as f_out:
                chunk = f_in.read(self.chunk_size)
                while chunk:
                    f_out.write(chunk)
                    chunk = f_in.read(self.chunk_size)

if __name__ == '__main__':
    start_code = time.monotonic()
    fileUnzip = FileUnzip(filename='file.txt.gz', file_path='/tmp', unzip_path='/tmp')
    fileUnzip.unzip()
    print(f'{time.monotonic() - start_code} seconds!')

```
