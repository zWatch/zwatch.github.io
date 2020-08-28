## `QDateTime QFileInfo::lastModified() const`

返回上次修改文件的日期和本地时间。
如果文件是符号链接，则返回目标文件的时间（不是符号链接）。

可以作为文件监视器的替代方案。