# GitHub 常用指令說明

## 基礎設定

```bash
# 設定使用者名稱與 Email
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 查看設定
git config --list
```

## 建立與複製專案

```bash
# 建立新專案（本地）
git init

# 複製遠端專案
git clone <repository-url>

# 複製特定分支
git clone -b <branch-name> <repository-url>
```

## 分支管理

```bash
# 查看分支
git branch              # 本地分支
git branch -r           # 遠端分支
git branch -a           # 所有分支

# 建立新分支
git branch <branch-name>

# 切換分支
git checkout <branch-name>
git switch <branch-name>

# 建立並切換分支
git checkout -b <branch-name>
git switch -c <branch-name>

# 刪除分支
git branch -d <branch-name>          # 刪除已合併
git branch -D <branch-name>          # 強制刪除

# 合併分支
git merge <branch-name>
git rebase <branch-name>
```

## 提交與修改

```bash
# 查看狀態
git status

# 查看差異
git diff
git diff --staged

# 暫存檔案
git add <file>              # 單一檔案
git add .                   # 所有檔案
git add -p                  # 互動式暫存

# 提交
git commit -m "提交訊息"
git commit --amend          # 修改最後提交

# 查看提交歷史
git log
git log --oneline           # 簡化顯示
git log -n                  # 最近 n 筆
```

## 遠端操作

```bash
# 查看遠端
git remote -v

# 新增遠端
git remote add origin <url>

# 推送
git push origin <branch-name>
git push -u origin <branch-name>  # 設定上游分支

# 拉取
git pull origin <branch-name>
git fetch origin                  # 取得但不合併

# 拉取合併
git pull = git fetch + git merge
```

## 還原與重置

```bash
# 還原檔案
git checkout -- <file>
git restore <file>

# 取消暫存
git reset HEAD <file>

# 取消提交（保留修改）
git reset --soft HEAD~1

# 取消提交（丟棄修改）
git reset --hard HEAD~1

# 還原特定提交
git revert <commit-hash>
```

## 標籤（Tag）

```bash
# 建立標籤
git tag <tag-name>
git tag -a <tag-name> -m "訊息"

# 推送標籤
git push origin <tag-name>
git push origin --tags        # 推送所有標籤

# 刪除標籤
git tag -d <tag-name>
git push origin --delete <tag-name>
```

## 儲藏（Stash）

```bash
# 儲藏目前修改
git stash
git stash save "訊息"

# 查看儲藏
git stash list

# 恢復儲藏
git stash pop                # 恢復並刪除
git stash apply              # 恢復但保留

# 刪除儲藏
git stash drop
```

## 常用快捷鍵

| 指令 | 說明 |
|------|------|
| `git status -s` | 簡短狀態 |
| `git log --graph` | 圖形化歷史 |
| `git blame <file>` | 查看檔案修改歷程 |
| `git bisect` | 二分搜尋問題提交 |

## 參考資源

- [Git 官方文件](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)