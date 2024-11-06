在使用 `cp` 命令复制文件时，默认情况下它不会显示进度。为了显示复制进度，可以使用以下几种方法：

### 方法 1: 使用 `rsync` 命令

`rsync` 是一个功能强大的文件复制工具，并且可以显示进度。

```cmd
rsync -ah --progress source_file_or_directory destination
```

- `-a`：表示归档模式，保持文件的所有属性。
- `-h`：以人类可读的格式显示文件大小（例如 KB, MB）。
- `--progress`：显示复制进度。

例如：

```cmd
rsync -ah --progress myfile.txt /mnt/ntfs/
```

### 方法 2: 使用 `cp` 和 `pv` 命令

`pv`（pipe viewer）是一个用于监控数据流进度的工具，你可以与 `cp` 命令配合使用来显示复制进度。

首先，确保你已经安装了 `pv` 工具：

```cmd
sudo apt install pv
```

然后，你可以使用以下命令将 `cp` 和 `pv` 配合起来：

```cmd
pv source_file | cp /dev/stdin destination_file
```

例如，如果你要复制文件并显示进度：

```cmd
pv myfile.txt > /mnt/ntfs/myfile.txt
```

### 方法 3: 使用 `cp` 和 `--verbose`（较少见的进度信息）

虽然 `cp` 本身没有内建的进度条，但你可以使用 `-v`（verbose）选项查看复制的文件名信息。这不会显示详细的进度条，但会显示正在复制的文件。

```cmd
cp -v source_file_or_directory destination
```

### 方法 4: 使用 `dd` 命令（适用于大文件复制）

如果你正在复制大文件，可以使用 `dd` 命令并显示进度。`dd` 是一个低级别的文件复制工具，适合用于处理大文件或磁盘映像。

```cmd
dd if=source_file of=destination_file status=progress
```

例如：

```cmd
dd if=myfile.iso of=/mnt/ntfs/myfile.iso status=progress
```

这个命令会显示复制过程的实时进度。