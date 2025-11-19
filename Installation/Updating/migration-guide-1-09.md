---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# 1.9.0 迁移指南

## 如果我使用 main/dev，如何迁移到新分支？

_**建议进行全新安装。**_ 但是，如果您希望使用现有的 SillyTavern 副本，请按照以下说明操作。

**重要！** 在做任何事情之前，请对您的安装进行*完整备份*。在此过程中，您可能会*丢失数据*，所以不要忽视这个警告。

有关更新说明，请参阅[从 1.12.0 更新到 1.12.0](/Installation/Updating/index.md#从-1120-更新到-1120)。

不确定需要备份哪些文件？请参阅此处的列表：[如何更新 SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### git 安装

1. 在您的 SillyTavern 安装文件夹中打开终端提示符（cmd、PowerShell、Termux 等）。
2. 输入 `git fetch` 然后 `git pull` 以拉取更新。
3. 您可能会丢失设置。您做了备份吗？`git switch release` 或 `git switch staging` 将分别切换您的分支
4. 如果没有错误，请跳到下一项。您可能会遇到类似这样的情况：
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   您将看到受影响文件的列表。如果您不在意这些设置文件被替换，`git switch -f release` 或 `git switch -f staging` 将设置您的分支。
   如果您想保存这些更改，请从备份中恢复。

5. 输入 `npm install` 然后 `npm run start` 以测试一切是否正常运行。
6. 尽情享用！如果需要，从备份中恢复数据。

### fatal: invalid reference: release

如果您只从旧的远程仓库（在迁移到组织仓库之前）克隆了单个分支，可能会发生这种情况。要修复此问题，您需要从新的远程仓库添加并获取分支：

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

然后从第 5 步继续。

### ZIP 安装

对您来说没有任何变化。像往常一样下载分支/发布 ZIP 即可。
